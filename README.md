# Detecting cancerous cells in gigapixel images

## demo/presentation:

YouTube Link: hhttps://youtu.be/bY1WoEXrLpc

Presentation Link: https://docs.google.com/presentation/d/1E3XWvqCxJy-t4xHPb5mD3t1sU6h2WGekTyDEKKRZanA/edit?usp=sharing

Models (.h5) and plot files: https://drive.google.com/open?id=1zxjpTnfmtFbW-vQAe-VYYIk5yhEVu-nV

Dataset: https://drive.google.com/open?id=19jDzBJ0S5Rxz1V3a0_c2HYPAaRVVnWXl


Github Link: https://github.com/iamsusiep/tumor_challenge

## data_extract.ipynb:
We mounted google drive to colab. In order to run the code, you must move drive folder with few slides and tumor masks prepared in advance with ASAP to "Colab Notebooks" folder in your drive. (https://drive.google.com/drive/folders/1rwWL8zU9v0M27BtQKI52bF6bVLW82RL5).

Also, the models must be generated with models.ipynb or from drive containing our saved models (https://drive.google.com/open?id=1zxjpTnfmtFbW-vQAe-VYYIk5yhEVu-nV) must be moved to the same "Colab Notebooks" folder in order to use load_model(). This is necessary for running heat_maps.ipynb.

Runs end-to-end for extracting data at zoom levels 1-5, with zoom level 1 being most zoomed in and 5 most zoomed out.
This dataset, generated in folder "/Colab Notebooks/Data" is used to train model at each zoom level.

Also generates data separately for multi-zoom level training, such that slices centered at specific pixel are extracted. 
This dataset, generated in folder "/Colab Notebooks/data_zoom" is used to train model at each zoom level.

In addition, the notebook contains code for undersampling the training data in the end.

It will take ~15 to 20 hours to run the notebook.

**Some outputs are missing because I had to run lower zoom levels on local, due to many people accessing folder with .tiff slides**

## models.ipynb:
The models notebook contains the code for creating and training all our models. Our main models use a transfer learning model using InceptionV3 architecture (pre-trained on imagenet dataset) and a custom CNN model. We tried and tested many variants of these two main models with different zoom levels (namely level 2, 3, 4, 5).

Finally we settled on these four models:

1. A pre-trained InceptionV3 model on imagenet and trained on our zoom level 2 data.
After including the InceptionV3 model as the base layer, we set further training of its layers to False. We added a couple of Dense layers followed by a Dropout layer with dropout factor set to 0.4 and finally a Dense layer with sigmoid activation for binary classification. This model is our most complex model with paramters numbers as follows:
Total params: 25,999,297, Trainable params: 4,196,513, Non-trainable params: 21,802,784

2. A custom CNN model trained on zoom level 2 data.
This model contains 4 pairs of Conv2D and MaxPooling2D layers followed by a Flatten layer. In Conv2D, the number of filters were 32, 64, 128 and 128 respectively and all 4 layers had a kernel size of 3x3. The MaxPooling layers had a pool size of 2x2. After the Flatten layers, we added two Dense layers with relu activation and a Dropout layer between them with a drop factor of 0.25. Finally the last layer was a Dense layer with sigmoid activation for binary classification. The complexity of this model was lower than pre-trained InceptionV3 with parameter numbers as follows: Total params: 4,451,905, Trainable params: 4,451,905, Non-trainable params: 0

3. A custom CNN model trained on zoom level 3 data.
We tried this model to be fairly simple in terms of its architecture. While this custom CNN models follows closely the previous CNN model but it doesn't have the last two Dense layers which makes it less complex. The architecture includes 4 sets of Conv2D and MaxPooling2D layers with the same number of filters, kernel size and pool size as before. Then we added a Flatten layers followed by a final Dense layer with sigmoid activation for binary classification. The parameter numbers are as follows: Total params: 273,601, Trainable params: 273,601, Non-trainable params: 0

4. Combining 2 zoom levels - Custom CNN (Zoom level 2) and Custom CNN (Zoom level 3)
In this model architecture, we took two input pathways with 2 models and combined them at the end with a couple of Dense layers. Both the input paths were simple CNN models with 4 pairs of Conv2D and MaxPooling2D layers followed by a Flatten layer. The number of filters, kernel size and pool size are the same as before. We combined the output of these 2 models with a concatenate layer followed by a Dense layer and a final Dense layer with sigmoid activation for binary classification. The parameter numbers are as follows: Total params: 612,741, Trainable params: 612,741, Non-trainable params: 0

The other models in the notebook such as a custom CNN model trained on zoom level 4 was just used for experimenting and is not to be considered as part of our final model set.

## heatmap.ipynb:
This notebook has code to generate the confusion matrix for our trained models. We load a model from its corresponding saved .h5 file using the load_model function from Keras library. The .h5 file contains the model architecture, all the weights and the state of the optimizer. A confusion matrix is a table that is often used to describe the performance of a classification model (or "classifier") on a set of test/val data for which the true values are known.

Generates a heatmap for any keras model trained on the slide dataset. Given a slide, it uses a sliding window over the entire slide and uses the model to generate a prediction over the window. It uses a color gradient to show which areas the model has predicted to have the highest probability of containined tumour, and areas that have a predicted low probability.

It takes about 30 minutes to generate a heatmap, depending on the zoom level and model used.

![Example of heatmap](https://i.ibb.co/6sQvStp/download-5.png)

## compute_metrics.nb:
This was a mathematica notebook used to generate the various metries to evaluate our model. The function defined takes in the counts of True Positives, True Negatives, False Positives and False Negatives and outputs the 6 different metrics that we used in evaluating our model. The 6 are:

* Sensitivity/Recall
* Specificity
* Precision
* Negative Predictive Value
* F1-Score
* MCC
## Other Information
Some outputs are missing, because we ran on local, or different notebook. 
We didn't have time to complete re-running on the notebook.
We ran multi-zoom on personal NVIDIA GPU desktop by connecting colab to local runtime, as shown (https://research.google.com/colaboratory/local-runtimes.html).
