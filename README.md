# Trend Forecasting
This project is an effort to predict fashion trends over time using machine learning. Currently, We are able to classify different types of clothing (Shirts, Skirts, Pants, Dress) using images webscraped from chictopia.com, instagram and training a model built using Keras.

# Avantages of Predictiong fashion trends
* Allow retailers to enhance their logistics for storage/shipping of clothes.
* Right now, retailers use sales along with results from fashion shows/blogs to determine what clothes would be useful for fashion.
* This project could add a new dimension of analytics for such retailers (being able to use social media to predict demand possibly).

## How is this project set up
Currently, this project is split into several files to make prototyping easier. Current workflow:

`webscraper.py --> csvDownload.py --> preprocessing.py --> FashionTrends.py`

* `webscraper.py` webscrapes chictopia.com for images and tags.
* `csvDownload.py` actually downloads the scraped images which are fed into.
* `preprocessing.py` to perform image preprocessing.
* Finally, `FashionTrends.py` is run to actually train the model and test the model.


## Details on how it works
We used [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) to parse webpages. Then, We downloaded the images and start preprocessing.

### Preprocessing
If `preprocessing.py` started with this image:

![Original Image](/imgs/original.jpg "Original Image")

We removed the background (everything but humans) using [DeepLabv3+](https://github.com/bonlime/keras-deeplab-v3-plus).

![Removed Background](/imgs/removed.jpg "Removed Background")

After removing the background, We then resized the image and perform k-means clustering using [KMeans from Scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html). Number of clusters currently is 4.

![Resized and Clustered](/imgs/128size.jpg "Resized and Clustered")

Finally, We saved the resized image along with a 2D NumPy array where each 2D location is the corresponding cluster value at that location based on cluster size and starting at 0 for largest cluster. This is then used for training.

### Training
`FashionTrends.py` first creates a pandas dataset using the CSVs from `webscraping.py` and then constructs a 3D numpy array of the arrays created from `preprocessing.py`. After this, training occurs.

Our current model is a 'Knowledge Enchanced Recurrent Network'  followed by a softmax layer. Optimizer is Adam. For specifics, look at (/FashionTrends/FashionTrends.py#L99).

Result after training:

![Results](/imgs/result.jpeg "Results")

These results are pretty good considering a small and potentially noisy dataset (~1200 images). For further improvements, We might use a GAN (Generational-Adversarial Network) which has been proven to do better for fashion classification and add more images to the dataset.
