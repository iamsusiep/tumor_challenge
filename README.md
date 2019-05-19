# tumor_challenge

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


