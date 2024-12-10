# Trading-Bot-template

This is the template for a trading bot for Binance Futures, have support of websockets.

## Users should edit the indicator file in:

`/indicators/index.js`

The function in that file should return a object that has signal inside it

`return { content : 'BUY'}` or `return { content : 'SELL'}`,
The bot would place the orders based on the signals returned from the indicators.

# **How It Works**

## **Introduction**
This is a cryptocurrency trading bot designed to interact with Binance Futures via the Binance API. It connects to Telegram using the Telegram Bot API and provides automated trading based on technical indicators. Users can issue commands through Telegram to manage trades for a single cryptocurrency.

---

## **Setup**
1. **Environment Configuration**:  
   The bot uses `dotenv` to load environment variables, including your **Telegram Bot API key** and other sensitive information.

2. **Key Libraries**:
   - `Telegraf`: To interact with Telegram.
   - Binance API client: For executing trades and retrieving market data.
   - WebSocket: For streaming real-time market data.
   - Custom Functions: Includes helper functions for balance retrieval (`balanceGiver`) and candlestick data analysis (`klineDataGiver`).

---

## **Features**

### **1. Welcome Message**
- When a user starts the bot with `/start`, they receive a warm welcome message explaining the bot's functionality:
  > "I can trade cryptos automatically using Super Trend. Ready to make stonks! 🤑"

### **2. Balance Check**
- **Command**: `/balance`  
  Retrieves and displays the user's USDT balance:  
  - **Available Balance**  
  - **Total Balance**

### **3. Open Trade Configuration**
- **Command**: `/openposition <coin> <timeframe> <amount> <leverage>`  
  Configures trade parameters:  
  - **Coin Name**: e.g., `BTC`
  - **Timeframe**: e.g., `15m` for 15-minute candlesticks.
  - **Amount**: USDT amount to trade.
  - **Leverage**: Leverage multiplier (e.g., 4x).

  Once configured, the bot confirms the parameters and allows users to modify the coin or timeframe using:
  - `/changetime <new_timeframe>`
  - `/changecoin <new_coin>`

### **4. Place Trade**
- **Command**: `/confirm`  
  After confirmation, the bot:
  1. Retrieves candlestick data and calculates the quantity of the coin to trade.
  2. Opens a WebSocket connection for real-time market data updates.
  3. Uses the indicator function (`Indicator`) to generate **BUY** or **SELL** signals:
     - Opens a market position when a valid signal is received.
     - Monitors open positions and checks for signal reversals:
       - If the signal changes from BUY to SELL or vice versa, it closes the current position and opens a new one with doubled quantity.
  4. Closes the WebSocket if the position is completely exited.

### **5. Help Command**
- **Command**: `/help`  
  Provides a list of all available commands and their descriptions.

### **6. Error Handling**
- Catches errors gracefully and logs them for debugging purposes.

---

## **Workflow Summary**
1. **User Interaction**:
   - Configure trades using `/openposition`.
   - Confirm trades with `/confirm`.
2. **Data Processing**:
   - Retrieves candlestick data via `klineDataGiver`.
   - Streams market data using Binance WebSocket.
   - Evaluates trade signals with a custom **Indicator** function.
3. **Trading Logic**:
   - Opens or reverses positions based on the indicator's signals.
   - Calculates precise quantities using Binance’s asset precision.

---

## **Limitations**
- The bot currently supports only a single coin at a time.
- Requires accurate parameters (coin, timeframe, leverage) to function properly.
- Assumes prior setup of Binance API keys and access to futures trading.

---

This bot is a straightforward yet powerful tool for automated trading using Telegram, making it easy to configure and monitor trades without leaving the app.
