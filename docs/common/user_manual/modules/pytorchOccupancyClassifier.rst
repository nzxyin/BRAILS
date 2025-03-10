.. _lbl-pytorchOccupancyClassifier:

Pytorch Occupancy Classifier
========================

The Pytorch Occupancy Image Classifier is a subclass of Pytorch generic image classifier. It can be used for inference and fine-tuning.

.. container:: toggle
         
   .. container:: header

       **Methods**

   #. **init**
      
      #. **modelName** Name of the model default = 'transformer_occupancy_v1'
      #. **imgDir** Directories for training data
      #. **download** Dowbload the pre-trained roof type classifier
      #. **resultFile** Name of the result file for predicting multple images. default = preds.csv
      #. **workDir** The working directory default = tmp
      #. **printRes** Show the probability and prediction default=True      

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

This class implements an occupancy classifier which is a subsclass of Pytorch generic classifier. It will download the pretrained model for inference. It can also be used for fine-tuning the pre-trained model if training data is provided.

Example
-------

The following is an example, in which an occupancy classifier is created.

The image dataset for this example contains street view images categorized according to occupancy.

The dataset can be downloaded `here <https://zenodo.org/record/6502302/files/occupancy_val.zip>`_.

When unzipped, the file gives the 'occupancy_val'. You need to set ''imgDir'' to the corresponding directory.  The occupancy_val directory contains the images for inference:


.. code-block:: none 

    occupancy_val
    │── class_1
    │       └── *.jpg
    │── class_2
    |      └── *.jpg



Construct the image classifier 
-------------------------------

.. code-block:: none 

    # import the module
    from brails.modules.PytorchOccupancyClassClassifier.OccupancyClassifier import PytorchOccupancyClassifier

    # initialize the classifier, give it a name and a directory
    occupancyClassifier = PytorchOccupancyClassifier(modelName='transformer_occupancy_v1', download=True)


Classify Images Based on Model
------------------------------

Now you can use the trained model to predict the (occupancy) class for a given image.

.. code-block:: none 

    # If you are running the inference from another place, you need to initialize the classifier firstly:
    from brails.modules.PytorchOccupancyClassClassifier.OccupancyClassifier import PytorchOccupancyClassifier

    occupancyClassifier = PytorchOccupancyClassifier(modelName='transformer_occupancy_v1', download=True)
                                            
    # define the paths of images in a list
    imgs = ["./occupancy_val/RRE/35856.jpg", "./occupancy_val/RRE/44325.jpg"]

    # use the model to predict
    predictions_dataframe = occupancyClassifier.predictMultipleImages(imgs)


The predictions will be written in preds.csv under the working directory.


Fine-tune the model for transfer learning. You need to provide the training data.
---------------

.. code-block:: none 

    from brails.modules.PytorchOccupancyClassClassifier.OccupancyClassifier import PytorchOccupancyClassifier

    occupancyClassifier = PytorchOccupancyClassifier(modelName='transformer_occupancy_v1', download=True,  imgDir='./occupancy_val/)


    # train the base model for 5 epochs with an initial learning rate of 0.01. 
    
    occupancyClassifier.train(lr=0.01, batch_size=64, epochs=5)


It is recommended to run the above example on a GPU machine.

Please refer to https://github.com/rwightman/pytorch-image-models for supported models. You may need to first install timm via pip: pip install timm. Currently, BRAILS only provides pre-trained roof type model based on Transformer.


