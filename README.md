# ugtm v1.0: Generative Topographic Mapping with Python.

GTM (Generative Topographic Mapping) is a dimensionality reduction algorithm (as t-SNE, LLE, etc) created by Bishop et al. (https://www.microsoft.com/en-us/research/wp-content/uploads/1998/01/bishop-gtm-ncomp-98.pdf) and a probabilistic counterpart of Kohonen maps.

ugtm is a python package implementing GTM and GTM prediction algorithms. ugtm contains the core functions and runGTM.py is an easy-to-use program. The kernel version of the algorithm (kGTM) is also implemented. You can also generate regression or classification maps, or evaluate the predictive accuracy (classification) or RMSE/R2 (regression) in repeated cross-validation experiments.



## Prerequisites
python 2.7

and following packages:
sklearn
numpy 
matplotlib
scipy
mpld3

## Examples

### Ex 1: Load some labeled test data, make a GTM map for classification (for regression: --labeltype continuous): 

```
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype discrete --output out --model GTM
```
This will generate a pdf and an html map. For GTM, there are two possible representations: the mean position, and the mode (the position where the data point has the highest posterior probability). On the html map, each point represents the mean position, and a grey line shows which node on the 2D map corresponds to the mode.

The function uses default parameters: the size of the map (kxk) is determined automatically, m (RBF square grid = mxm) is set to sqrt(k), l (regularization coefficient) = 0.1 and w (RBF width factor) = 0.3. The options for these parameters can be changed with: --grid_size, -- rbf_grid_size, --regularization and --rbf_width_factor. Setting --regularization or --rbf_width_factor to -1 with --crossvalidate option will result in a grid search of best parameters.

Instead of --data and --labels, you can load some test data from sklearn using --usetest (s = S in 3D space, swiss = swiss roll in 3D space, iris = iris classification dataset), and generate GTM, kGTM, t-SNE or PCA maps:

```
python runGTM.py  --usetest s --labeltype continuous --output out --model GTM
python runGTM.py  --usetest s --labeltype continuous --output out --model kGTM
python runGTM.py  --usetest s --labeltype continuous --output out --model t-SNE
python runGTM.py  --usetest s --labeltype continuous --output out --model PCA
python runGTM.py  --usetest iris --labeltype discrete --output out --model GTM
python runGTM.py  --usetest iris --labeltype discrete --output out --model kGTM
python runGTM.py  --usetest iris --labeltype discrete --output out --model t-SNE
python runGTM.py  --usetest iris --labeltype discrete --output out --model PCA

```

You can also add ids with the --ids option to visualize data point ids on your html map.


### Ex 2: With PCA pre-processing (usually better with less than 100 principal components) and missing data:

```
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype discrete --output out \
--model GTM --pca --n_components 50 --missing --missing_strategy median
```

### Ex 3: Run 5-fold cross-validation repeated 10 times on GTM, linear SVM, rbf SVM or PCA k-NN classification model: 

```
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype discrete --output out \
--model GTM --crossvalidate
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype discrete --output out \
--model SVM --crossvalidate
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype discrete --output out \
--model SVMrbf --crossvalidate
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype discrete --output out \
--model PCA --crossvalidate
```
To try different parameters for GTM: --regularization -1 --rbf_width_factor -1.
To try different parameters for linear SVM: --svm_margin -1.
To try different parameters for rbf SVM: --svm_margin -1 --svm_gamma -1.
To try different parameters for PCA: --n_neighbors -1.


### Ex 4: Run 5-fold cross-validation repeated 10 times on GTM, PCA k-NN or linear SVM regression model:

```
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype continuous --output out \
--model GTM --crossvalidate
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype continuous --output out \
--model SVM --crossvalidate
python runGTM.py  --data csvlist.dat --labels csvlist.lbls  --labeltype continuous --output out \
--model PCA --crossvalidate
```

To try different parameters for GTM: --regularization -1 --rbf_width_factor -1.
To try different parameters for linear SVM: --svm_margin -1.
To try different parameters for PCA: --n_neighbors -1.

### Ex 5: Train a GTM map on a training set and project a test set:

```
python runGTM.py --model GTM --data train --test test --labels lbls  --labeltype discrete --output out
```


## Get help

```
python runGTM.py -h
```


## Author

Héléna A. Gaspar, hagax8@gmail.com, https://github.com/hagax8


