# Detecting cancerous cells in gigapixel images

## demo/presentation:
YouTube link:

Presentation Link: https://docs.google.com/presentation/d/1E3XWvqCxJy-t4xHPb5mD3t1sU6h2WGekTyDEKKRZanA/edit?usp=sharing

## data_extract.ipynb:
We mounted google drive to colab. In order to run the code, you must move drive folder with few slides and tumor masks prepared in advance with ASAP to "Colab Notebooks" folder in your drive. (https://drive.google.com/drive/folders/1rwWL8zU9v0M27BtQKI52bF6bVLW82RL5).

Runs end-to-end for extracting data at zoom levels 1-5, with zoom level 1 being most zoomed in and 5 most zoomed out.
This dataset, generated in folder "/Colab Notebooks/Data" is used to train model at each zoom level.

Also generates data separately for multi-zoom level training, such that slices centered at specific pixel are extracted. 
This dataset, generated in folder "/Colab Notebooks/data_zoom" is used to train model at each zoom level.

In addition, the notebook contains code for undersampling the training data in the end.

It will take ~15 to 20 hours to run the notebook.

**Some outputs are missing because I had to run lower zoom levels on local, due to many people accessing folder with .tiff slides**

## models.ipynb:

## heatmap.ipynb:
Generates a heatmap for any keras model trained on the slide dataset. Given a slide, it uses a sliding window over the entire slide and uses the model to generate a prediction over the window. It uses a color gradient to show which areas the model has predicted to have the highest probability of containined tumour, and areas that have a predicted low probability.

It takes about 30 minutes to generate a heatmap, depending on the zoom level and model used.

![Example of heatmap](https://www.google.com/url?sa=i&source=images&cd=&ved=2ahUKEwjrhKPJgqjiAhVydt8KHZwhCqQQjRx6BAgBEAQ&url=https%3A%2F%2Fnews.nationalgeographic.com%2F2018%2F05%2Fanimals-cats-training-pets%2F&psig=AOvVaw2UbHUpAlQkaUKn6Zoxcqvy&ust=1558369944186575)


