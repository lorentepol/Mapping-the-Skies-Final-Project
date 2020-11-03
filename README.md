<img src="https://bit.ly/2VnXWr2" alt="Ironhack Logo" width="100"/>

# Mapping the Skies
*Pol Lorente Gay*

*Ironhack Data Analytics Full-time June 2020*

## Content
- [Project Description](#project-description)
- [Hypotheses / Questions](#hypotheses-questions)
- [Dataset](#dataset)
- [Cleaning](#cleaning)
- [Analysis](#analysis)
- [Model Training and Evaluation](#model-training-and-evaluation)
- [Conclusion](#conclusion)
- [Future Work](#future-work)
- [Workflow](#workflow)
- [Organization](#organization)
- [Links](#links)

## Project Description
This Project created a Supervised Machine Learning Model in order to classify sky objects (stars/galaxies/quasars) according to their light received in Earth by the corresponding telescope.

## Hypotheses / Questions
* A telescope may receive up to 200 GB data per night, and there are billions of sky objects to be observed and mapped. Having an algorithm that classifies the light data received by type of object could speed up all the process and save time.
* Could light data from millions of objects be classified correctly with a Machine Learning model?

## Dataset
* The data was got from the Sloan Digital Sky Survey, whose purpose is to map the entire observable universe. It is believed that they will finish by 2060.
* The data is publicly available was obtained through an SQL query executed in their website, which joins 2 tables and extract the data in csv format. As the query allowed to download up to 500,000 rows, the dataset contains data from 500,000 objects.


## Cleaning
From the dataset, only the light data columns needed to be used, the rest where instrumental data and position data (where the object was located in the sky). Therefore these columns were kept for analysis and preprocessing.

The columns are the following:
* **u**: Light magnitude for ultraviolet light.
* **g**: Light magnitude for green light.
* **r**: Light magnitude for red light.
* **i**: Light magnitude for near-infrared light.
* **z**: Light magnitude for far-infrared light.
* **redshift**: Value that determines the light increase in wavelength (shifted to red-infrared) since it leaves the object until it reaches Earth. It indicates the speed the object is moving away from us.

Also 5 objects had some light value missing. Therefore these rows were dropped.

## Analysis
First the percentage value of each class was evaluated in order to split the dataset into Train and Test sets as soon as possible, in order to avoid analysis and information contamination from test set.
The percentage value of each class (Star, Galaxy, QSO (*Quasi-Stellar Object, or Quasar*)) was 40%, 50%, 10%. For QSO the dataset is slightly unbalanced, therefore the train-test split was done using stratification.
As the dataset had 500,000 objects. I find it convenient to have 10,000 rows as a test set (2%), not the regular 20%. This is because if a dataset is large enough you can train the ML model with almost all the data, and it will still remain a fair absolute amount of objects in the test set.

Analysing the train set I found that all the light features except redshift were strongly correlated with each other. I first planned to drop some of the columns, but finally I decided to create 4 new columns that were the difference between one column to the next (u-g, g-r, r-i, i-z). That value is known as Color Index. Also Redshift was completely skewed to the right, with almost all values below 0.01 but having values up to 6. Thus, I recomputed the column with a logarithm formula to try compensate this skewness.

Finally, I used Standard Scaling before model training.

## Model Training and Evaluation
For model Training I first tried 10 models with default parameters using Scikit Learn. I used cross validation and computed several metrics, including Accuracy, Precision, Recall, F1 and ROC-AUC. I finally used F1 Macro since it is the best indicator for unbalanced multiclass datasets.

Once trained and cross-validated, I tried the 3 best models and fine-tuned them with a Grid Search, this time computing only F1 Macro.

## Conclusion
The final selected model was a Random Forest. 
I could reach a F1 score of 0.99 for the test set, which is really impressive. 
In fact, for almost all models I could reach a F1 ober 95%, and I wanted to know why these high values.
I computed the feature importance for the random forest, and I found the most important feature to classify objects was the redshift, with 52%.
Plotting the redshift (with logarithm) along with sky coordinates using spherical coordinates helped distinguish in a fist glance the objects, and demonstrate that indeed the universe is expanding (the furthest objects more redshifted they are, thus moving away from us at really high speeds).

## Future Work
For future improvements I would like to perform an unsupervised algorithm for anomaly detection, or for clustering the object in order to have a second overall subclassification (dwarf stars, supergiants, spiral galaxies, elliptical galaxies etc).
Also I would feed the model with more data, maybe twisting the balance of the classes in order to test the model in these circumstances.


## Organization

The repo is organised as follows:
* **data** folder : includes the raw data, the train and test sets and the SQL query used in SDSS to obtain the data.
* **code** folder : includes two Jupyter Notebooks, one for Cleaning, Splitting and Analysis (*Exploratory Data Analysis.ipynb*) and the other for Model Testing (*Modelling.ipynb*)
* **gif** folder : includes animations made with the model

## Links

[Slides](https://slides.com/pollorente/deck-78bfed)  

