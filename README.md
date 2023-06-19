# Unit 5 - Financial Planning

![Financial Planner](Images/financial-planner.png)

## Summary

This work is a homework entering financial planning analysis. The process has 3 parts:
1. Get historical market data
2. Manipulate the data
3. Model future outcomes using Monte Carlo Simulation 

---

## The [Monte Carlo Simulation](https://www.investopedia.com/terms/m/montecarlosimulation.asp)

The idea of the Monte Carlo Simulation is to calculate multiple rounds, randomly sampling "daily return rates" drawn out of a normal distribution around historical mean and standard deviation. It assumes the market fluctuation probability to follow a normal distribution curve mathimatically. Since the actual market probability does not follow normal distribution, due to that the market is not random random but impacted by human reactions to world's events, the Monte Carlo Simulation is a approximation in this application of ours.

The simulation calculation is not part of this homework, but a courtesy of [UofT SCS Fintech Bootcamp](https://bootcamp.learn.utoronto.ca/fintech/landing/?s=Google-Brand&pkw=%2Bu%20%2Bof%20%2Bt%20%2Bfintech&pcrid=656377915803&pmt=b&utm_source=google&utm_medium=cpc&utm_campaign=GGL%7CUNIVERSITY-OF-TORONTO%7CSEM%7CFINTECH%7C-%7COFL%7CTIER-1%7CALL%7CBRD%7CBMM%7CCore%7CGeneral&utm_term=%2Bu%20%2Bof%20%2Bt%20%2Bfintech&s=google&k=%2Bu%20%2Bof%20%2Bt%20%2Bfintech&utm_adgroupid=106469468377&utm_locationphysicalms=9000942&utm_matchtype=b&utm_network=g&utm_device=c&utm_content=656377915803&utm_placement=&gad=1&gclid=Cj0KCQjw1rqkBhCTARIsAAHz7K0yOYTbFIASPaTuRhhDs7EGyXiLAfBXK8VKAxWmH1fonsmkKzVo3zgaAhQhEALw_wcB&gclsrc=aw.ds.) - a Python Class MCSimulation, taking following arguments to initiate an instance:

* A DataFrame of market historical data from Alpaca
* A weight list for the tickers
* Rounds of simulation
* Trading days to simulate

The class provides the following methods to be of use:
* calc_cumulative_returns - Calulate cumulative returns using Monte Carlo Simulation and return a DataFrame of the "daily returns" as the rows and the simulation rounds as the columns
* plot_simulation - Plot the "daily returns" in kind line, each line represents a round of simulation
* plot_distribution - Plot the probability distribution of "daily returns" figures in the total simulation rounds
* summarize_cumulative_return - Present the statistics parameters calculated such as min, max, mean, percentiles that would appear in a box plot

This work makes use of the class.

---

## [alternative.me API to retrieve crypto information](https://alternative.me/crypto/)

[![Free Crypto API Documentation](Images/alternative.me.png)](https://alternative.me/crypto/api/)

Use the free crypto API to retrieve crypto prices:
>   pd.request.get(url).json\
>   url for Bitcoin: https://api.alternative.me/v2/ticker/Bitcoin/?convert=CAD\
>   url for Ethereum: https://api.alternative.me/v2/ticker/Ethereum/?convert=CAD

## [Alpaca API to retrieve stock and bonds information](https://alpaca.markets/)

[![alpaca](Images/alpaca.png)](https://alpaca.markets/docs/)

Steps:
1. Install alpaca package
>   pip install python-dotenv\
>   pip install alpaca-trade-api\
>   conda list alpaca-trade-api
2. Signup at [alpaca markets](https://app.alpaca.markets/login) and get API key and secret key. Store them in .env file.
3. Initiate alpaca object using the keys retrived from .env file
>   import alpaca_trade_api   
>   from dotenv import load_dotenv\
>   load_dotenv\
>   alpaca = alpaca_trade_api.REST(
>>       os.getenv(api_key),
>>       os.getenv(secret_key),
>>       api.verson='v2
>   )
4. Pull data using alpaca api
>   alpaca.get_bars(
>>      tickers_list,
>>      timeframe"1Day",
>>      start = pd.Timestamp('yyyy-mm-dd',     tz='America/New_York').isoformat(),
>>      end = pd.Timestamp('yyyy-mm-dd',tz='America/New_York').isoformat()
>   ).df
5. Combine the returned tickers' data (DataFrame indexed in timestamp-dates containing multi-columns of info under "symbol" ticker), into one df:
>   tickers_list[n]_df = returned_df.loc[returned_df['symbol']==tickers_list[n]]
>   pd.concat([tickers_list[n]_dfs],axis=1,keys=[tickers_list])

## Brief of the work

We first worked on crypto Bitcoin and Ethereum, did some simple calculation using the data got via alternative.me API.

We then pulled 5 years' daily open,high,low,close prices, volumn and trade_count using Alpaca API. One ticker is AGG (iShares Core US Aggregate Bond ETF) representing conservative investment, one is SPY (SPDR S&P 500 ETF Trust) representing more aggressive investment in stock. Using the Monte Carlo Simulation, we projected the following outcomes:
1. 40/60 starting 20K for 30 years
2. 40/60 starting 30K for 30 years
3. 10/90 starting 200K for 5 years
4. 10/90 starting 200K for 10 years

In the end, the max value at 95% confidence level are respectively 464K, 696K, 791K and 1,571K. It seems that the rich getting richer is math. The 200K investment more aggresively reaches the retirement goal much faster. Of course there are also 95% confidence level minimal return. But loss usually would be outside of our view, due to its ordinariness. Lol. 

