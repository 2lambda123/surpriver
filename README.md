<p align="center">
  <img width="350" src="figures/black_logo.png">
</p>

# Surpriver - Find High Moving Stocks before they Move
Find high moving stocks before they move using anomaly detection and machine learning. Surpriver uses machine learning to look at volume + price action and infer unusual patterns which can result in big moves in stocks.

### Files Description
| Path | Description
| :--- | :----------
| surpriver | Main folder.
| &boxur;&nbsp; dictionaries | Folder to save data dictionaries for later use. 
| &boxur;&nbsp; figures | Figures for this github repositories.
| &boxur;&nbsp; stocks | List of all the stocks that you want to analyze.
| data_loader.py | Module for loading data from yahoo finance.
| detection_engine.py | Main module for running anomaly detection on data and finding stocks with most unusual price and volume patterns.
| feature_generator.py | Generates price and volume return features as well as plenty of technical indicators.

## Usage
### Packages
You will need to install the following package to train and test the models.
- [Scikit-learn](https://scikit-learn.org/)
- [Numpy](https://numpy.org/)
- [Tqdm](https://github.com/tqdm/tqdm)
- [Yfinance](https://github.com/ranaroussi/yfinance)
- [Pandas](https://pandas.pydata.org/)
- [Scipy](https://www.scipy.org/install.html)
- [Ta](https://github.com/bukosabino/ta)

### Predictions for Today
If you want to go ahead and directly get the most anomalous stocks for today, you can simple run the following command to get the stocks with the most unusual patterns. We will dive deeper into the command in the following sections.

```
python detection_engine.py --top_n 25 --min_volume 5000 --data_granularity_minutes 60 --history_to_use 14 --is_load_from_dictionary 0 --data_dictionary_path 'dictionaries/data_dict.npy' --is_save_dictionary 1 --is_test 0 --future_bars 0
```
This command will give you the top **25 stocks** that had the highest anomaly score in the last **14 bars** of **60 minute candles**. It will also store all the data that it used to make predictions in the **dictionaries/data_dict.npy** folder. Below is a more detailed explanation of each parameter.
- **top_n**: The total number of most anomalous stocks you want to see.
- **min_volume**: Filter for volume. Any stock that has an average of volume lower than this value will be ignored.
- **data_granularity_minutes**: Data granularity to use for analysis. The available options are *1min, 5min, 15min, 30min, 60min*.
- **history_to_use**: Historical bars to use to analyze the unusual and anomalous patterns.
- **is_save_dictionary**: Whether to save the stock data that is used for analysis in a dictionary or not. Enabling this would save you time if you want to do some further analysis on the data.
- **data_dictionary_path**: Dictionary path where data would be stored.
- **is_load_from_dictionary**: Whether to load the data from dictionary or download it from yahoo finance directly. You can use the dictionary you saved above here for multiple runs.
- **is_test**: You can actually test the predictions by leaving some of the recent data as future data and analyzing whether the most anomalous stocks moved the most after their predictions. If this value is 1, the value of **future_bars** should be greater than 5.
- **future_bars**: These number of bars will be saved from the recent history for testing purposes.

Here is an output of how a single prediction looks like. **Please note that negative scores indicate higher anomalous and unusual patterns while positive scores indicate normal patterns. The lower the better**.

```
Last Bar Time: 2020-08-25 11:30:00-04:00
Symbol: TGA
Anomaly Score: -0.018
Today Volume: 19.11K
Average Volume 5d: 144.85K
Average Volume 20d: 127.31K
Volatility 5bars: 0.006
Volatility 20bars: 0.010
Future Absolute Sum Price Changes: 79.23 (This only appears if **is_test** is set to 1.)
```

### Test on Historical Data
If you are suspicious of the use of Machine Learning and Artificial Intelligence in trading, you can actually test the predictions from this tool on historical data. The two most important command line arguments for testing are **is_test** and **future_bars**. If the former one is set to 1 and the later one is set to anything more than 5, the tool will actually leave that amount of data for analysis purposes and use the data prior to that for anomalous predictions. Next, it will look at that remaining data to see how well the predictions did. Here is an example of a scatter plot from the following command.

```
python detection_engine.py --top_n 25 --min_volume 5000 --data_granularity_minutes 60 --history_to_use 14 --is_load_from_dictionary 0 --data_dictionary_path 'dictionaries/data_dict.npy' --is_save_dictionary 1 --is_test 1 --future_bars 25
```

<p align="center">
  <img src="figures/correlation_plot.png">
</p>
