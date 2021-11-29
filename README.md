# Don't Get Rekt
<img src="images/rekt.jpg" alt="rekt" width="400" height="800"/>

This is bad okay.

## Project Goals

Our Project goal was to analyse trends using Bitcoin liquidation data. 
We would examine the relationships between liquidations, price & futures data, volume and use technical analysis to attempt to arrive at reliable market signals.

## Cleaning of Data
<img src = "images/fintechmeme.png" alt = "lol" width="500" height = "1000"/>
Cleaning of data feat. TA Hero Gunawan

## Visualisation of Data

### Process of Visualisation

Visualsation of Data was done with mostly hvplot, however the imported library Altair was used as it provided a more efficient way of combined plots together.
Documentation of Altair can be found [here.](https://pypi.org/project/altair/)

The installation is simply 

<code> 
    
    pip install altair
  
</code>

For example, bar graphs can easily be made with negative values containing different colours to notate whether the value was negative or positive.
The code is simply a conditional value on whether the value is negative or positive. 

<code> 
   
    import altair as alt
    
    source = trade

    alt.Chart(source).mark_bar().encode(
    
    x="date",
    
    y="percentchange",
    
    color=alt.condition(
        
    alt.datum.percentchange > 0,
        
    alt.value("steelblue"),# The positive color
       
    alt.value("orange")  # The negative color
    )

    ).properties(height = 700, width=1500)' 
    
</code>

<img src = "images/historicpercent.png" alt = "bar" width = "900" height = "500"/>


Altair also allows for easy combining of graphs. Combining a line, scatter, area, and area graphs into a simple graph was a simple defining of the plot, followed by the addition numeric function.


<code>
    
    base = alt.Chart(source).encode(x='date')

    bar = alt.Chart(source).mark_bar().encode(
    
    x="date:T",
    
    y="volume in 10M USD",
    
    color=alt.condition(
        
    alt.datum.y > 0,
        
    alt.value("steelblue"),  # The positive color
        
    alt.value("steelblue")  # The negative color
    
    )

    ).properties(width=1500)


    line =  base.mark_line(color='black').encode(
    
    y='tradecount in 10,000s'

    )

    line2 = base.mark_area(color = 'red').encode(
    
    y='percentchange'

    )

    (bar + line + line2).properties(width=1500, height = 900)
    
</code> 
    
<img src = "images/mid.png" alt = "bar" width = "900" height = "500"/>

### Plots 

<img src = "images/historicohlc.png" alt = "historicohlc" width = "900" height = "300"/>

An OHLC representation of Bitcoin prices in USD from Binance from the 1/8/2017 to 31/10/2021. Even though this can be easily found from an exchange, the representation here allows for future slicing of data.

A simple hvplot was used to create this graph. 


<code>
    
    ohlc = prices1.hvplot.ohlc(ylabel = "close", grid = True, xaxis = None, width = 1500)
    
    
</code>
    


Using the RangeToolLink plugin from the Holoviews Library, we are able to create a combined representation along with volume, and a small scrollable overlay capable of zooming.


<img src = "images/combined.png" alt = "combined" width = "1300" height = "500"/>


<code>
    
    from holoviews.plotting.links import RangeToolLink
    
    ohlc = prices1.hvplot.ohlc(ylabel = "close", grid = True, xaxis = None, width = 1500)

    overview = prices1.hvplot.ohlc(yaxis = None, height = 150, width = 1500)

    volume1 = prices1.hvplot.step("date", "volume", height = 100, xaxis = None, width = 1500)

    RangeToolLink(overview.get(0), ohlc.get(0))

    layout = (ohlc + overview + volume1).cols(1)

    layout.opts(merge_tools = False)

</code>

## Plotting Price & Volume data 

<img src = "images/early.png" alt = "early" width = "900" height = "600"/>
    
And early representation of our data, from the period of August 2017 to December 2018. From this graph we can see that as trading count increases, volume increases in correlation. The change in price also reflects this. This shows shallow market depth and a lack of liquidity, a sign of an immature market. This allows for the "whales" to control the swings through coordinated sells and buys. 
    
<img src = "images/mid.png" alt = "mid" width = "900" height = "600"/>
    
A representation of the bear market following the 2017-2018 bull cycle. This graph shows the trade volume from 2018 - 2020. The results are similar here, with the largest spike being March 14, where the price of Bitcoin dropped a staggering 40% in a single day. 
    
    
<img src = "images/late.png" alt = "late" width = "900" height = "600" />
    
2020 onwards to October 2021. This bull cycle shows a maturing market compared to the 2017 bull. The swings are smaller with larger trade volume. This shows greater market depth and liquidity. 
Here a scatter + line plot was used to plot the price % change. An area graph would have covered the data slightly.
A simple addition of code was used to add a scatter to the line to see the points more clearly.

<code> 
    
    volume_5 = vol3.loc["2021-05-20":"2021-10-31"]

    volume_5.reset_index(inplace = True)

    source = volume_5

    base = alt.Chart(source).encode(x='date')


    bar = alt.Chart(source).mark_bar().encode(
   
    x="date:T",
   
    y="volume in 100M USD",
    
    color=alt.condition(
        
    alt.datum.y > 0,
       
    alt.value("steelblue"),  # The positive color
       
    alt.value("steelblue")  # The negative color
   
    )

    ).properties(width=1500)


    line =  base.mark_line(color='black').encode(
   
    y='tradecount in 100,000s'

    )

    point2 = base.mark_line(color='black').encode(
    
    y='tradecount in 100,000s'

    )
    

    line2 = base.mark_line(color = 'red').encode(
   
    y='percentchange'

    )    

    point = base.mark_point(color = 'red').encode(
    
    y='percentchange'

    )

    (bar + line + line2 + point).properties(width=1500, height = 900) 
    
</code>
    
### **On Balance Volume**

On Balance Volume or OBV is an indication of buying and selling pressure and is used to measure positive and negative volume flow.

It is used to look for divergences between OBV and price to predict price movements or used to confirm price trends.

### **Stochastic Oscillator** 

The Stochastic Oscillator is a momentum indicator comparing the closing price of a security to a range of prices over a certain period.

It is a popular technical indicator for generating overbought and oversold signals
It looks at current price and compares to highest high and lowest low

The Stochastic Oscillator is range-bound between 0 and 100. Traditionally, readings over 80 are considered in the overbought range, and readings under 20 are considered undersold. 

Changes in the Stochastic Oscillator can also provide clues on future trend shifts.


Signals are created when %K and %D intersect that may signify a change in momentum of trading. 

### OBV v Price for the period 8 December 2018 - 15 March 2020
<img src = "images/OBV_period_3.png" alt = "late" width = "1300" height = "600" />


When looking at the trend for the period 8 December 2018 - 16 March 2020, OBV trends upwards as the market recovers all time low of December 2018. Key support levels are reached from January 2019 into April 2021 around the USD4,000 mark. Once this support level was broken in April, this was the trigger for a steep upward trend.

### Stochastic Oscillator for the period 8 December 2018 - 15 March 2020
<img src = "images/oscillator_period_3.PNG" alt = "late" width = "1500" height = "400" />

This upward trend was also confirmed in the Stochastic Oscillator where there was no contest in buying momentum when looking at the oversold ranges (i.e. BTC does not encounter selling pressure) until July 2021, but not before increasing 330% to USD13,500.

As selling pressure mounted, eventually BTC became oversold and this lead to a drop in the price leading into 2020 where a large downward movement was caused by the lower lows and the start of the COVID-19 pandemic.

### **Limitations** 
Both On Balance Volume and the Stochastic Oscillator have been known to produce false signals. This is when a trading signal is generated by the indicators, yet the price does not actually flow through, which can end up as a losing trade. 

Large spikes in OBV on a single day can throw off the indicator for quite a while, but the spike in volume may not be indicative of a trend. 

We can use price trend as a filter, where signals are only taken if thet are in the same direction as the trend. 

## Conclusion
From these visual representations we can gather that the market is maturing, and that volatility while still high should be less than the previous years. This is due to a rise in liquidity and more institutional interest in cryptocurrencies. However, this also means that the benefits of extremely volatile upswings will also be lessened, and that the days of Bitcoin going from $1 to $20,000 are incredibly unlikely to return. On the positive side (unless you are short BTC), it means that the bear market will be less volatile, as evident the 2018 bear market caused an almost -90% change to Bitcoin's price, while the bear market of 2021 moved the price ~-50%.

## Can these technical indicators be used as market signals to forecast future Bitcoin prices?

### OBV
OBV can be used as a leading indicator of market pressure to determine when the large players are taking positions. However the results need to be filtered to follow the price trend. 

. Stochastic Oscillator
The Stochastic Oscillator can be used to determine price momentum and if a stock is overbought or oversold. Similarly as with OBV, the results require filtering to only take up those signals that follow the price. 

### Sharpe Ratio
The Sharpe ratio indicates how well an equity investment performs in comparison to the rate of return.
<img src = "images/Sharpe Ratio, Standard Deviation.png" alt = "late" width = "1300" height = "600" />

In our analysis of the Bitcoin pricing over the last four years, while separating the Bitcoin cycle into historical moments of its price, the Sharpe Ratio seem to appear really low, which would lead to the observation that investing in Bitcoin is a very risky.

### Standard Deviation
Even though it has been observed that the price of Bitcoin appears to be very risky, hence the value of the Sharpe Ratio being very low, it would appear that the Standard Deviation of the price changes of Bitcoin is, to our surprise, very low, being at roughly 0.05, implying that even though the risk of investing in Bitcoin is potentially high, the price change does not change drastically, leading to the possibility of the price of Bitcoin following a predictive course.
    
    


     
   
