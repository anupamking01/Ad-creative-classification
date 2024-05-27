# Identifying Ad Images
Code to differentiate ad from non-ad images based on the geometry of the image (if available) as well as phrases occurring in the URL, the image's URL and alt text, the anchor text, and words occurring near the anchor text.

## Summary of Strategy and Results

### Strategy

The challenges to building a good model were:  

1. Non-random missing data in the continuous variables.
2. A large number of features given the size of the sample (1,558 vs 3,279).
3. The overwhelming majority of the features are sparse.
4. There is only a moderate number of observations.
5. High class imbalance.

I tackled these challenges by:

1. Turning the continuous variables into binary variables where missing values is a feature.
2. Using algorithms robust to unfavorable feature to observation ratios -- like random forest, which will only use a sample of the features per model fit.
3. Using variance threshold based feature selection and regularized models to avoid introducing too many uninformative sparse features to the model.
4. Avoiding more data-hungry cross-validation strategies like nested cross-validation.
5. Adjusting class weights and using sampling techniques like SMOTE and Tomek Link removal.

### Results

ROC curves on the test data:

![alt text](https://github.com/anupamking01/Ad-creative-classification/ROC_Best.png "AUC ROC on Test Data of Best Models")

The above strategies resulted in highly predictive models. The best iteration of each model explored had an AUC ROC of 0.95 or greater. The best model was the logistic classifier with a 1:1 class weight, the feature variance threshold set to drop only zero variance features, and no sampling-based class imbalance corrections. 

The performance of the best model was highly stable. The standard deviation of the validation fold AUC ROC was 0.012, a tiny fraction of the average AUC ROC.

The most important features in the dataset seemed reasonable. Listed by order of importance (identified using random forest), the top five are:

1. ancurl*adclick
2. ancurl*adid
3. origurl*misfits2
4. url*static.wired.com
5. ancurl*http+www

Features 1, 2, & 5 seem to be ad attributes and 3 & 4 seem to be the url of the ad source. 

Many more model training approaches were left off of the table. For example, only L2 (ridge) regularization was used for the logistic classifier and SVM were not used (to save time on first iteration of training). However, since the first iteration of training yielded models with an AUC ROC of 0.99, further refinement of the model training process seemed unnecessary.


jimmy.charite@gmail.com

## License

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/jcharit1/Identifying-Ad-Images/blob/master/License.md) file for details
