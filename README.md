# Step 2: Installing Required Libraries
!pip install yfinance
!pip install matplotlib
!pip install pandas

# Step 3: Importing Libraries
import yfinance as yf
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression

# Step 4: Fetching Stock Data
ticker = 'AAPL'
stock_data = yf.download(ticker, start='2022-01-01', end='2023-01-01')

# Step 5: Exploring the Data
stock_data.head()

# Step 6: Data Visualization
stock_data['Close'].plot(title=f"{ticker} Stock Closing Prices", figsize=(10, 6))
plt.show()

# Step 7: Calculating Technical Indicators
stock_data['MA20'] = stock_data['Close'].rolling(window=20).mean()
stock_data['MA50'] = stock_data['Close'].rolling(window=50).mean()

plt.figure(figsize=(10, 6))
plt.plot(stock_data['Close'], label='Close Price')
plt.plot(stock_data['MA20'], label='20 Days MA')
plt.plot(stock_data['MA50'], label='50 Days MA')
plt.title(f"{ticker} Moving Averages")
plt.legend()
plt.show()

# Step 8: Statistical Analysis
stock_data.describe()

stock_data['Log_Return'] = np.log(stock_data['Close'] / stock_data['Close'].shift(1))
volatility = stock_data['Log_Return'].std() * np.sqrt(252)  # Annualized volatility
print(f"The annualized volatility of {ticker} stock is: {volatility:.2%}")

# Step 9: Predictive Analysis (Linear Regression)
# ...

X = stock_data[['MA20', 'MA50']]
y = stock_data['Close']

# Impute missing values in X (if any)
imputer = SimpleImputer(strategy='mean')
X_imputed = imputer.fit_transform(X)

# Fit the Linear Regression model
model = LinearRegression()
model.fit(X_imputed, y)

y_pred = model.predict(X_imputed)

# Calculate the Mean Squared Error (MSE)
mse = mean_squared_error(y, y_pred)
print(f"Mean Squared Error: {mse:.2f}")

from google.colab import drive
drive.mount('/content/drive')

# Save your CSV file to Google Drive
stock_data.to_csv('https://drive.google.com/drive/folders/1I_OG7M33qt6Z3vsNQTfzqapkzfhEy6JE')
