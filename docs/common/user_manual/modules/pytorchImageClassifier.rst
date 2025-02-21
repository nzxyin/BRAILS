.. _lbl-pytorchImageClassifier:

Pytorch Image Classifier
========================

The Pytorch Generic Image Classifier is a class that can be used for creating user defined classifier. It can be used for training and evaluating the model.

.. container:: toggle
         
   .. container:: header

       **Methods**

   #. **init**
      
      #. **modelName** Name of the model default = 'rooftype_resnet18_v1'
      #. **imgDir** Directories for training data
      #. **valimgDir** Directories for validation data
      #. **random_split** Ratio to split the data into a training set and a validation set if validation data is not provided.
      #. **resultFile** Name of the result file for predicting multple images. default = preds.csv
      #. **workDir** The working directory default = tmp
      #. **printRes** Show the probability and prediction default=True      
      #. **printConfusionMatrix** Whether to print the confusion matrix or not default=False    

   #.  **train**

      #. **lr1**: default=0.01
      #. **epochs**: default==10
      #. **batch_size**: default==64
      #. **plot**: default==False
     
   #. **predictOneImage**
   
      #. **imagePath**: Path to a single image

   #. **predictMultipleImages**
  
      #. **imagePathList**: A list of image paths
      #. **resultFile**: The name of the result filename default=None
                   
   #. **predictOneDirectory**

      #. **directory_name**: Directory for saving all the images
      #. **resultFile**: The name of the result filename default=None
                   

Description
----------

This class implements the abstraction of an image classifier, it can be first used to train the classifier. Once trained, the classifier can be used to predict the class of each image given a set of images. This class can also be used to load a pre-trained model to make predictions. 

Example
-------

The following is an example, in which a classifier is created and trained.

The image dataset for this example contains satellite images categorized according to roof type.

The dataset can be downloaded `here <https://zenodo.org/record/6231341/files/roofType.zip>`_.

When unzipped, the file gives the 'roofType'. You need to set ''imgDir'' to the corresponding directory.  The roofType directory contains the images for training:


.. code-block:: none 

    roofType
    │── class_1
    │       └── *.png
    │── class_2
    |      └── *.png
    │── ...
    |
    └── class_n
           └── *.png



Construct the image classifier 
-------------------------------

.. code-block:: none 

    # import the module
    from brails.modules import PytorchImageClassifier

    # initialize the classifier, give it a name and a directory
    roofClassifier = PytorchImageClassifier(modelName='rooftype_resnet18_v1', imgDir='/home/yunhui/SimCenter/train_BRAILS_models/datasets/roofType/')


Train the model
---------------

.. code-block:: none 

    # train the base model for 5 epochs with an initial learning rate of 0.01. 
    roofClassifier.train(lr=0.01, batch_size=64, epochs=5)


It is recommended to run the above example on a GPU machine.

Please refer to https://github.com/rwightman/pytorch-image-models for supported models. You may need to first install timm via pip: pip install timm.


Classify Images Based on the Model
------------------------------

Now you can use the trained model to predict the (roofType) class for a given image.

.. code-block:: none 

    # If you are running the inference from another place, you need to initialize the classifier firstly:
    from brails.PytorchGenericModelClassifier import PytorchImageClassifier
    roofClassifier = PytorchImageClassifier(modelName='rooftype_resnet18_v1')
                                            
    # define the paths of images in a list
    imgs = ["/home/yunhui/SimCenter/train_BRAILS_models/datasets/roofType/flat/TopViewx-76.84779286x38.81642318.png",   
         "/home/yunhui/SimCenter/train_BRAILS_models/datasets/roofType/flat/TopViewx-76.96240924000001x38.94450328.png"]

    # use the model to predict
    predictions_dataframe = roofClassifier.predictMultipleImages(imgs)


The predictions will be written in preds.csv under the working directory.

.. note::
    The generic image classifier is intended to illustrate the overall process of model training and prediction.
    The classifier takes an image as the input and will always produce a prediction. 
    Since the classifier is trained to classify only a specific category of images, its prediction is meaningful only if 
    the input image belongs to the category the model is trained for.
