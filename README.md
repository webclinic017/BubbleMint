# BubbleMint, a ML-driven cryptocurrency trading bot
* This program runs on Coinbase Pro, if you would like to use it as an investment tool, be sure to have a Coinbase Pro account and insert all necessary API information in ```authCredentials.py```. 
* This program uses 4 different machine learning algorithms to generate buy and sell signals for a given asset and time interval. 
* A pre-trained model for Bitcoin/USD is available and ready to use.

## How it works
* The file ```subroutines.py``` contains all the methods used to calculate the different technical indicators of a given asset.
* The file ```gen_transform.py``` reduces the dimensionality of the dataset by selecting the top 200 features using Random Forest. 
* The reduced dataset is then used to train 4 different machine learning algorithms, K-nearest neighbor classifier, random forest classifier, gaussian naive bayes classifier and gradient boosting classifier.
*  The 4 models' outputs are combined using a weighted average, and the final outputs are used as raw predictions. Below shows the raw predictions on the ```BTC/USD``` pair.\
\
![raw](https://user-images.githubusercontent.com/86272122/139788759-5549fe69-1c03-4d94-86c8-39582657bd08.png)

## Prediction Processing
* The raw outputs from the ensembled model have too many buy/sell signals in the same reneral area. 
* To combat this, every time a buy signal is received, it won't immediately trigger a buy action, but rather sets up a stop-loss and take-profit margin that centers at the previous closing price.
* The margins are set up according to the risk tolerance and multiplier settings in ```trader.py```.
* If a new buy signal is received before price breaks the margin, then a new margin will be set at the previous closing price.
* The buy action will only be executed when prices eventually crosses either the stop-loss or take-profit. 
* The same operation is done on sell signals. Blow shows the same predictions after the prediction processing.\
\
![stepped](https://user-images.githubusercontent.com/86272122/139789031-068c1a99-db77-45bb-972f-750db1c31000.png)

## Data Labeling
* Historic prices are first transformed into chunks of equal sizes, the minimum and maximum for each chunk is considered a buy and sell label respectively. 
* To visualize the profit and percent gains for a large range of chunk sizes, execute ```python general_test.py```. 
* Different assets often require different chunk sizes, the default chunk size is ```320```.

## Installation and usage
* This program requires the packages ```sklearn```, ```termcolor```, ```imblearn``` as well as ```cbpro```.
* To install, clone this repo via ```git clone https://github.com/SnowCheetos/BubbleMint.git .```
* A pre-trained model for ```BTC/USD``` is ready for use. To use the model, execute ```python trader.py```.
* Make sure to have inserted all the API information in ```authCredentials.py``` for Coinbase Pro.
* If you would like to train the model on an asset other than BTC, type ```python train.py``` and enter the asset.
* You are encouraged to change the function parameters to what works best.

## Testing
To visualize performances of each model, execute ```python general_test.py```. To visualize the performance of the ensembled model, execute ```python ensembled_test.py```.
