## Putting it together

### The Intermediate SQL Tutorial

#### The Apple stock prices dataset

For the first few lessons, you'll be working with [Apple](https://www.apple.com/) stock price data. The data was pulled from [Google Finance](https://www.google.com/finance/) in January 2014. There's one row for each day (indicated in the `date` field). `open` and `close` are the opening and closing prices of the stock on that day. `high` and `low` are the high and low prices for that day. `volume` is the number of shares traded on that day. Some data has been intentionally removed for the sake of this lesson. Check it out for yourself:

```sql
SELECT * FROM tutorial.aapl_historical_stock_price
```

