# Training



## Frameworks

> A deep learning framework is an interface, library or a tool which allows us to build deep learning models more easily and quickly, without getting into the details of underlying algorithms. They provide a clear and concise way for defining models using a collection of pre-built and optimized components.



- TensorFlow (multiple languages)
- Keras  (run on top of TensorFlow, high-level API)

- Pytorch (flexible)
- Caffe
- Theano



## Pre-processing on data



### Datasets

- [Meta Pointer: A large collection organized by CV Datasets.](http://www.cvpapers.com/datasets.html)
- [Yet another Meta pointer](http://riemenschneider.hayko.at/vision/dataset/)
- [ImageNet](http://http://image-net.org/): a large-scale image dataset for visual recognition organized by [WordNet](http://wordnet.princeton.edu/) hierarchy
- [SUN Database](http://groups.csail.mit.edu/vision/SUN/): a benchmark for scene recognition and object detection with annotated scene categories and segmented objects
- [Places Database](http://places.csail.mit.edu/): a scene-centric database with 205 scene categories and 2.5 millions of labelled images
- [NYU Depth Dataset v2](http://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html): a RGB-D dataset of segmented indoor scenes
- [Microsoft COCO](http://mscoco.org/): a new benchmark for image recognition, segmentation and captioning
- [Flickr100M](http://yahoolabs.tumblr.com/post/89783581601/one-hundred-million-creative-commons-flickr-images): 100 million creative commons Flickr images
- [Labeled Faces in the Wild](http://vis-www.cs.umass.edu/lfw/): a dataset of 13,000 labeled face photographs
- [Human Pose Dataset](http://human-pose.mpi-inf.mpg.de/): a benchmark for articulated human pose estimation
- [YouTube Faces DB](http://www.cs.tau.ac.il/~wolf/ytfaces/): a face video dataset for unconstrained face recognition in videos
- [UCF101](http://crcv.ucf.edu/data/UCF101.php): an action recognition data set of realistic action videos with 101 action categories
- [HMDB-51](http://serre-lab.clps.brown.edu/resource/hmdb-a-large-human-motion-database/): a large human motion dataset of 51 action classes
- [ActivityNet](http://activity-net.org/): A large-scale video dataset for human activity understanding
- [Moments in Time](http://moments.csail.mit.edu/): A dataset of one million 3-second videos

- [MNIST: handwritten digits](http://yann.lecun.com/exdb/mnist/)
- [CIFAR10 / CIFAR100: 32×32 natural image dataset with 10/100 categories]( http://www.cs.utoronto.ca/~kriz/cifar.html)
- [Caltech 101: pictures of objects belonging to 101 categories](http://www.vision.caltech.edu/Image_Datasets/Caltech101/)
- [STL-10 dataset is an image recognition dataset for developing  unsupervised feature learning, deep learning, self-taught learning  algorithms. It is inspired by the [CIFAR-10 dataset](http://www.cs.toronto.edu/~kriz/cifar.html) but with some modifications](http://www.stanford.edu/~acoates//stl10/)
- [The Street View House Numbers (SVHN) Dataset](http://ufldl.stanford.edu/housenumbers/)
- [NORB: binocular images of toy figurines under various illumination and pose](http://www.cs.nyu.edu/~ylclab/data/norb-v1.0/)
- [Pascal VOC: various object recognition challenges](http://pascallin.ecs.soton.ac.uk/challenges/VOC/)
- [Labelme: A large dataset of annotated images](http://labelme.csail.mit.edu/Release3.0/browserTools/php/dataset.php)
- [COIL 20: different objects imaged at every angle in a 360 rotation](http://www.cs.columbia.edu/CAVE/software/softlib/coil-20.php)
- [COIL100: different objects imaged at every angle in a 360 rotation](http://www1.cs.columbia.edu/CAVE/software/softlib/coil-100.php)



### Own dataset

1. Install LabelImg

   For WINDOWS:

   - Install **Python3** via https://www.python.org/downloads/release/python-357/
   - Install **PyQt5** and **lxml** via **PIP**

   ```
   pip3 install pyqt5 lxml
   ```

   - Download, unzip and run the **LabelImg.exe** file via https://www.dropbox.com/s/kqoxr10l3rkstqd/windows_v1.8.0.zip?dl=1

   For LINUX:

   - Install **LabelImg** via **PIP**

   ```
   pip3 install labelimg
   ```

   - Launch **LabelImg** via the command below

   ```
   python3 labelimg
   ```



2. Annotation

   For  object detection, their are many formats for preparing and annotating  your dataset for training. The most popular formats for annotating your datasets are:

   - **.xml (Pascal VOC)**

      The annotation XML file for each image is saved using the name of the image file. For example:

     - you have images **image_1.jpg**, **image_2.jpg** …… **image_z.jpg**
     - the XML annotations file will be saved as **image_1.xml**, **image_2.xml,**…. **image_z.xml**

   - **.json (Micosoft COCO)**

   - **.txt (YOLO)**

     One row for one image;
     Row format: `image_file_path box1 box2 ... boxN`;
     Box format: `x_min,y_min,x_max,y_max,class_id` (no space).

     Here is an example:

     ```
     path/to/img1.jpg 50,100,150,200,0 30,50,200,120,3
     path/to/img2.jpg 120,300,250,600,2
     ...
     ```



## Remote interpreter (Server)

Using Pycharm (Professional)

1. Settings -> Python Console -> Environment variables:

   ```shell
   CUDA = /usr/local/cuda-10.0
   ```

   ```shell
   LD_LIBRARY_PATH = /usr/local/cuda-10.0/lib64
   ```

2. Settings -> Project Interpreter -> Add -> SSH Interpreter -> New server configuration:

   Host: xx.xx.xx.xx

   Username:xx

   -> Password: xxx

3. If successful, you may confirm in **File Transfer** and find a list of files are uploading.



## Training

Example: (keras-yolo3)

- Modify **num_classes** in **yolov3.cfg** and **class file** in `train.py` based on your own dataset.
- Modify **anchors** in **yolov3.cfg** and **anchor file** in `train.py` based on your own settings. (if applicable)



Notes: `.h5` file only contains weights not architecture.