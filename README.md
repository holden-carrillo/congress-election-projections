# Congress Election Projections
This project aims to forecast the results of the 2024 US House and Senate elections. Data was used from the MIT Election Lab, Ballotpedia, and FiveThirtyEight. 
* Note that this was last updated August 16th, so the data referenced in this file may be outdated. 

## House Projections
Because there are 435 House races per election cycle, it's difficult to treat each individual race uniquely. There's little to no polling data in a vast majority of districts, and fast-changing demographics and voting patterns can only be accounted for to such an extent. Because of this, the projected results for each district are based on past performances of each party relative to the rest of the country during recent elections. To find the projected performance of a given party in a given district relative to the party's average, I took data from the past ten elections and used a recursive EWMA formula with a weight of 0.73 (optimized from the 2022 election).

Now that we have vote shares for each party relative to national averages, we need estimated performances from each party on a national level. I took FiveThirtyEight's [generic ballot polling averages]([url](https://projects.fivethirtyeight.com/polls/generic-ballot/2024/?ex_cid=abcpromo)) to estimate what results would look like in November. These national polls can be projected to each individual race with the relative data we have.  

To turn projected vote totals into probabilities of winning the seat, I used a multiple logistic regression model from data in 2020, calculated through Desmos. This gave me the formula of y ~ 1 / 1 + e^(-27.54x), which displays that candidates win 50% of the time when their projected vote share is tied with the next best candidate, roughly 70% of the time when their projected vote share is 3.1 points above the next best candidate, and roughly 90% of the time when their projected vote share is 8 points above the next best candidate.

The last step was to simulate, as I used a random number generator for each seat to determine who would win in a given simulation. 1,000 simulations found that democrats would gain a majority of seats ~ 96.5% of the time (with an average of 220.83 seats). The model seems especially bullish for democrats, especially given that democrats still get a majority of seats in ~ 75% of simulations where the generic ballot polls are tied. 

## Senate Projections
The basic methodology for the senate projections follows the House ones, but because there are significantly less races, it's much easier to account for the individual candidates. This gave me the freedom to create two separate models: an empiric-based one (similar to the House model), and a polls-based one. For the polls-based model, with consistent and reliable polling data for each race, I used another EWMA model (with weight of 0.76) to find polling averages for each candidate. I also found that it's best to only use polls from August-November, given that before that period there's still uncertainty on who nominees will be (this also means that as of now I can only make projections for ~ 1/3 of races, since there's not enough polling data elsewhere).

Both models have their advantages and limitations, and I found that the most accurate results came with a 0.16 weight for the empiric model and 0.84 weight for the polls one. This gives us a single projected vote share for each candidate, and the same steps in the House model can be applied here.

The simulation results so far, while limited, are also optimistic for democrats, giving them favorable chances in many contested seats. But for both chambers, it's still too early to make any decisive predictions.

## Limitations and Future Additions
The biggest limitation of the House projections is that they don't account for redistricting. A work around for this could be to look at specific precinct data, but I couldn't find any up to date, accessible data that showed which precincts have been redistricted. As the election gets closer, this data could be moved to an R Shiny App using Leaflet or another package to display the projections on a district-by-district map. While I'm content about the initial results overall, there's still a lot that can be done.
