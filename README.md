Machine Learning in Stock Price Prediction


Types of Models


Machine learning models, particularly those based on transformer architectures like GPT-3, have gained popularity in stock price prediction. These models excel at understanding patterns and relationships in sequential data.

Benefits and Limitations
Benefits

Sequence Understanding: Transformers can capture complex relationships in sequential data, such as stock prices over time.
Feature Extraction: These models can automatically extract relevant features from data.
Limitations

Interpretability: Transformer models can be challenging to interpret, making it hard to understand the reasoning behind predictions.
Resource Intensive: Training and using large language models can require significant computational resources.
For our stock price prediction model, let us make use of a XGBoost Regressor model to predict the stock price of GOOGLE (GOOGL). We will download the last 4 years worth of data from yahoo finance and feed it into our regression model. Let’s start.

Code Breakdown
Let’s delve into the implementation of our stock price prediction model using a simple linear regression approach. We’ll break down each section of the code for clarity.

Importing Libraries
In this section, we install required libraries and import necessary modules for data processing and modeling. Make sure to remove ! before pip while installing through terminal or command prompt.

User-Defined Functions
In this section, we define several user-defined functions to encapsulate specific tasks. These functions make the code modular and enhance readability. Let’s explore each of the function.

download_stock_data(ticker, start_date, end_date) : This function downloads historical stock price data for a specified stock ticker within a given time range using the Yahoo Finance API.
ticker: The stock ticker symbol (e.g., 'AAPL' for Apple Inc.).
start_date: The starting date for the historical data (in the format 'YYYY-MM-DD').
end_date: The ending date for the historical data (in the format 'YYYY-MM-DD').
The function uses the yf.download method from the Yahoo Finance library to fetch the data and returns the adjusted closing prices ('Adj Close') as a Pandas DataFrame.

Downloading the Data Set
Here, we use the user-defined function yf.download to fetch historical stock prices for a specific ticker and time period.

Visualise the Data Set
Before proceeding further, let us also visualise the data that we would have downloaded directly using the yf.download(ticker, start=start_date, end=end_datecode.

Data Preprocessing and Feature Engineering
Once we successfully collected historical stock data using the yfinance library, the next crucial step was to prepare the dataset for modeling. The raw dataset included daily stock information like Open, High, Low, Close prices, and Volume. To keep the model focused on the most relevant information, we selected only these key features.

To make our model smarter, we created additional features that can help capture price trends and short-term movements:

High-Low: the daily range of price movement, computed as the difference between the high and low prices.
Close-Open: the net gain or loss during a trading session.
5day_MA and 10day_MA: simple moving averages over 5 and 10 days, respectively, which are commonly used in technical analysis to identify trends.
Since the moving average features use a rolling window, they naturally introduce some missing values at the beginning. We addressed this by dropping any rows with NaN values to ensure our data was clean and ready for modeling.

To predict the next day’s closing price, we created a new column Target, which is simply the Close price shifted one step into the future. This setup transforms our time-series data into a supervised learning problem where the model uses today’s features to predict tomorrow’s price.

Train-Test Split: Preserving Time Series Integrity
When working with time-series data like stock prices, it’s vital to respect the chronological order of the data. Unlike typical datasets where data points are independent and can be randomly shuffled before training, time-series data relies on past information to predict the future. Therefore, shuffling the dataset before splitting can leak future information into the training set, leading to overly optimistic results.

In this project, after preparing our feature set and target variable (i.e., the next day’s closing price), we used Scikit-learn’s train_test_split function with an important configuration — we set shuffle=False. This ensures the model trains on past data and is tested on future data, just as it would behave in a real-world scenario.

We split the dataset into 80% training data and 20% testing data. The training set allows the XGBoost model to learn the patterns and relationships from historical price movements, while the test set helps evaluate how well the model generalizes to unseen, future data.

This approach simulates how a model would perform in production when predicting the next day’s price using only past information — making the evaluation both realistic and reliable.

Model Selection: XGBoost Regressor
For the prediction task, we chose the XGBoost Regressor, a powerful and efficient gradient boosting algorithm. XGBoost has become a go-to tool for many Kaggle winners and data scientists due to its speed, performance, and ability to handle overfitting effectively through built-in regularization.

Before training, we split our dataset into training and testing sets using an 80–20 split. Importantly, we disabled shuffling to preserve the chronological order of the stock data, which is essential for time-series forecasting.

We initialized the XGBoost Regressor with 100 estimators and trained it on the historical features. After training, the model was used to predict the closing prices on the test set. These predictions were then compared with the actual values to evaluate the model’s performance using metrics like Mean Squared Error (MSE) and R-squared (R²) Score.

R-squared (R²):
R-squared is a statistical measure that represents the proportion of the variance in the dependent variable that is explained by the independent variables in a regression model. It is a scale from 0 to 1, where:

R² = 0: The model does not explain any of the variability in the dependent variable.
R² = 1: The model perfectly explains all the variability in the dependent variable.
Mathematically, R-squared is calculated as:

R² = 1 — SSR/SST

Where:

SSR is the sum of squared residuals (the difference between the observed and predicted values).
SST is the total sum of squares (the difference between the observed values and the mean of the dependent variable).
R-squared is a useful metric, but it has a limitation. It tends to increase as more independent variables are added to the model, even if those variables are not actually improving the model’s performance. This is where Adjusted R-squared comes into play.

Comparing Actual vs Predicted Prices
Once the XGBoost model was trained and made predictions on the test data, the next step was to evaluate its performance visually and numerically. A straightforward and effective way to do this is by comparing the actual closing prices to the predicted ones.

To achieve this, we created a new DataFrame called comparison, which holds both the actual prices (y_test) and the model’s predicted prices. This structure not only helps in inspection but also serves as a foundation for plotting the results.

We focused the visualization on the last 20 predictions to keep the graph clean and interpretable. Using matplotlib, we plotted both the actual and predicted closing prices on the same chart. This side-by-side visual comparison gives us an intuitive sense of how well the model is tracking the true market behavior.

If the predicted line closely follows the actual line, it means our model is doing a good job capturing the underlying trends and short-term variations in the stock’s closing prices.

This plot acts as a qualitative evaluation tool, supplementing the numerical metrics (like MSE or R²) and helping stakeholders understand the model’s performance in a visual and tangible way.

Conclusion
In conclusion, stock price prediction models, especially those leveraging machine learning, can provide valuable insights for investors. However, they come with their set of challenges, including market uncertainties and model limitations.

Remember, while these models enhance decision-making, they should be used as complementary tools alongside comprehensive market analysis and financial understanding. Happy investing!
<img width="995" height="450" alt="image" src="https://github.com/user-attachments/assets/a754c63a-13b9-4c29-8146-927528c88ae4" />
<img width="847" height="515" alt="image" src="https://github.com/user-attachments/assets/61ed19a5-4d32-4bba-9005-46dea4683302" />

