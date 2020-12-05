# H.266



## Decoder Structure

```c++
DecLib::decode
    	|
    	V
DecLib::xDecodeSlice
    	|
    	V
DecSlice::decompressSlice
    	|
    	V
DecCu::decompressCtu
{
    if (currCU.predMode != MODE_INTRA && currCU.predMode != MODE_PLT && currCU.Y().valid())
    {
        xDeriveCUMV(currCU);
    }
    switch( currCU.predMode )
      {
      case MODE_INTER:
      case MODE_IBC:
        xReconInter( currCU );
        break;
      case MODE_PLT:
      case MODE_INTRA:
        xReconIntraQT( currCU );
        break;
      default:
        THROW( "Invalid prediction mode" );
        break;
      }
}
```



## Encoder Structure

```c++
EncLib::encode
    	|
    	V
EncGOP::compressGOP
    	|
    	V
EncSlice::compressSlice
    	|
    	V
EncSlice::encodeCtus
    	|
    	V
EncCu::compressCtu
    	|
    	V
EncCu::xCompressCU
    do
  	{
        if( currTestMode.type == ETM_INTER_ME )
        {
            xCheckRDCostInter()
        }
        else if( currTestMode.type == ETM_AFFINE )
        {
            xCheckRDCostAffineMerge2Nx2N()
        }
        else if( currTestMode.type == ETM_MERGE_SKIP )
        {
            xCheckRDCostMerge2Nx2N()
        }
        else if( currTestMode.type == ETM_MERGE_TRIANGLE )
        {
            xCheckRDCostMergeTriangle2Nx2N();
        }
        else if( currTestMode.type == ETM_INTRA )
        {
            xCheckRDCostIntra();
        }
        else if( currTestMode.type == ETM_IPCM )
        {
            xCheckIntraPCM();
        }
        else if (currTestMode.type == ETM_PALETTE)
        {
            xCheckPLT();
        }
        else if (currTestMode.type == ETM_IBC)
        {
            xCheckRDCostIBCMode();
        }
        else if (currTestMode.type == ETM_IBC_MERGE)
        {
            xCheckRDCostIBCModeMerge2Nx2N();
        }
        else if( isModeSplit( currTestMode ) )
        {
            ...
        }
        else
        {
            THROW( "Don't know how to handle mode: type = " << currTestMode.type << ", 				options = " << currTestMode.opts );
        }
	} while( m_modeCtrl->nextMode( *tempCS, partitioner ) );
 	//////////////////////////////////////////////////////////////////////////
    // Finishing CU
    // set context states
    m_CABACEstimator->getCtx() = m_CurrCtx->best;
```
