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
  
<code/>

For example, bar graphs can easily be made with negative values containing different colours to notate whether the value was negative or positive.
The code is simply a conditional value on whether the value is negative or positive. 
