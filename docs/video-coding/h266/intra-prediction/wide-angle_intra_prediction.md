# Wide-angle intra prediction modes



**Initialization**

```c++
static_vector<ModeInfo, FAST_UDI_MAX_RDMODE_NUM> uiHadModeList;
static_vector<double, FAST_UDI_MAX_RDMODE_NUM> CandCosctList;
static_vector<double, FAST_UDI_MAX_RDMODE_NUM> CandHadList;

static_vector<ModeInfo, FAST_UDI_MAX_RDMODE_NUM> uiRdModeList;
```



#### First round of SATD for normal angular modes

```c++
for( int modeIdx = 0; modeIdx < numModesAvailable; modeIdx++ )
{
    uint32_t       uiMode = modeIdx;
    Distortion		minSadHad = 0;
    
    // Skip checking extended Angular modes in the first round of SATD
    if( uiMode > DC_IDX && ( uiMode & 1 ) )
    {
        continue;
    }
    
    bSatdChecked[uiMode] = true;
    pu.intraDir[0] = modeIdx;
    
    initPredIntraParams(pu, pu.Y(), sps);
    if( useDPCMForFirstPassIntraEstimation( pu, uiMode ) )
    {
        encPredIntraDPCM( COMPONENT_Y, piOrg, piPred, uiMode );
    }
    else
    {
        predIntraAng( COMPONENT_Y, piPred, pu);
    }
    // Use the min between SAD and HAD as the cost criterion
    // SAD is scaled by 2 to align with the scaling of HAD
    minSadHad += std::min(distParamSad.distFunc(distParamSad)*2, 													distParamHad.distFunc(distParamHad));
    
    // NB xFracModeBitsIntra will not affect the mode for chroma that may have already been pre-estimated.
    m_CABACEstimator->getCtx() = SubCtx( Ctx::MipFlag, ctxStartMipFlag );
    m_CABACEstimator->getCtx() = SubCtx( Ctx::ISPMode, ctxStartIspMode );
    m_CABACEstimator->getCtx() = SubCtx(Ctx::IntraLumaPlanarFlag, ctxStartPlanarFlag);
    m_CABACEstimator->getCtx() = SubCtx(Ctx::IntraLumaMpmFlag, ctxStartIntraMode);
    m_CABACEstimator->getCtx() = SubCtx( Ctx::MultiRefLineIdx, ctxStartMrlIdx );
    
    uint64_t fracModeBits = xFracModeBitsIntra(pu, uiMode, CHANNEL_TYPE_LUMA);
    double cost = ( double ) minSadHad + (double)fracModeBits * sqrtLambdaForFirstPass;
    
    DTRACE(g_trace_ctx, D_INTRA_COST, "IntraHAD: %u, %llu, %f (%d)\n", minSadHad, fracModeBits, cost, uiMode);
    
    updateCandList( ModeInfo( false, false, 0, NOT_INTRA_SUBPARTITIONS, uiMode ), cost,uiRdModeList,  CandCostList, numModesForFullRD );
    updateCandList( ModeInfo( false, false, 0, NOT_INTRA_SUBPARTITIONS, uiMode ), double(minSadHad), uiHadModeList, CandHadList,  numHadCand );
}
```



#### Second round of SATD for extended angular modes

```c++
for (int modeIdx = 0; modeIdx < numModesForFullRD; modeIdx++)
{
    unsigned parentMode = parentCandList[modeIdx].modeId;
    if (parentMode > (DC_IDX + 1) && parentMode < (NUM_LUMA_MODE - 1))
    {
        for (int subModeIdx = -1; subModeIdx <= 1; subModeIdx += 2)
        {
            unsigned mode = parentMode + subModeIdx;
            if (!bSatdChecked[mode])
            {
                pu.intraDir[0] = mode;
                initPredIntraParams(pu, pu.Y(), sps);
                if (useDPCMForFirstPassIntraEstimation(pu, mode))
                {
                  encPredIntraDPCM(COMPONENT_Y, piOrg, piPred, mode);
                }
                else
                {
                  predIntraAng(COMPONENT_Y, piPred, pu );
                }

                // Use the min between SAD and SATD as the cost criterion
                // SAD is scaled by 2 to align with the scaling of HAD
                Distortion minSadHad = std::min(distParamSad.distFunc(distParamSad)*2, distParamHad.distFunc(distParamHad));

                // NB xFracModeBitsIntra will not affect the mode for chroma that may have already been pre-estimated.
                m_CABACEstimator->getCtx() = SubCtx( Ctx::MipFlag, ctxStartMipFlag );
                m_CABACEstimator->getCtx() = SubCtx( Ctx::ISPMode, ctxStartIspMode );
                m_CABACEstimator->getCtx() = SubCtx(Ctx::IntraLumaPlanarFlag, ctxStartPlanarFlag);
                m_CABACEstimator->getCtx() = SubCtx(Ctx::IntraLumaMpmFlag, ctxStartIntraMode);
                m_CABACEstimator->getCtx() = SubCtx( Ctx::MultiRefLineIdx, ctxStartMrlIdx );

                uint64_t fracModeBits = xFracModeBitsIntra(pu, mode, CHANNEL_TYPE_LUMA);

                double cost = (double) minSadHad + (double) fracModeBits * sqrtLambdaForFirstPass;

                updateCandList( ModeInfo( false, false, 0, NOT_INTRA_SUBPARTITIONS, mode ), cost, uiRdModeList,  CandCostList, numModesForFullRD );
                updateCandList( ModeInfo( false, false, 0, NOT_INTRA_SUBPARTITIONS, mode ), double(minSadHad), uiHadModeList, CandHadList,  numHadCand );

                bSatdChecked[mode] = true;
            }
        }
    }
}
```

