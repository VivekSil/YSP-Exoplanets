# Rough Overview of project

* YSP-ExoplanetsThe original dataset from the exoplanet archive is in the exoplanets.csv file
 
* Initially I removed all columns that contained the limit or error of a measurement eg st_radvlim(Radial Velocity Limit Flag)
I then removed all columns that had more than 50% missing data and was left with about 80 columns.
Of these columns I found that some were unrelated to whether a planet were habitable or not (like the number of time series made on a particular planet), these unrelated columns were removed and are listed in the unimportantFeatures.txt file.

* The datasetmaker.py file takes the exoplanets dataset, and removes all columns that were mentioned in the above para. It also removes any planet(row) that has more than 25% missing data. It was found that dropping all rows having missing data in the st_optband olumn significantly reduces the total amount of missing data while discarding minimum number of planets. Hence the datasetmaker.py file also removes all planets that have missing data in the st_optband column.
The resulting dataset is saved to selectedFeatures.cs

* The selectedFeatures.csv dataset still has some missing values. Hence the mice package in R was used to impute these values. This is done using the Impute.R file. The resulting imputed data is saved to imputedData in the dataset folder. This takes care of most of the mising data but some columns still have missing data hence we use the FixMiceImputed.py file to handle these remaining missing data using simple imputation. The resulting data is stored in the simpleImputedMiceRf file.

* The modelExplore folder contains the training results of 5 models (KNeighbours, RandomForest, catboost, Perceptron and SVC) and their feature importances. The feature importance of each of these models are stored in the impFeats folder where for example the catboost82 file contains the feature importance of the best catboost model having an accuracy of 82%.

* In this folder the feat.py file gives the 25 most important features based on these feature importance files. The methodology by which it determines overall importance is given below:
1. Take the feature importance file for a given model and sort the features in ascending order by their given feature importance. The score of each feature is:
 its position in sorted order(starting from 0) + the model accuracy.
2. repeat the above step for all feature importance files and add the feature scores. 
3. the top 25 highest scoring features are most important overall.

The resulting 25 most overall important features are stored in the featureImportance file
..
