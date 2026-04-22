# R-Final-Project
R final project for the MTECH data technology course

## This code shows us the averave change in lap time from the previous lap for drivers who made a pit stop.
df %>% 
  filter(PitStop == 1) %>% 
  summarise(avg_positions_lost <- mean(LapTime_Delta, na.ra=T))
### On average a drivers lap time will increase by 0.362 seconds if they make a pitstop.

## This code shows the average lap time delta by compound taken from racers in the top 10 positions.
df %>% 
  filter(Position <= 10) %>% 
  group_by(Compound) %>% 
  summarise(avg_tire_deg = mean(LapTime_Delta, na.ra=T))
### This is the output: 
  Compound     avg_tire_deg
  <chr>               <dbl>
1 HARD                0.339
2 INTERMEDIATE        3.25 
3 MEDIUM             -0.600
4 None              -17.1  
5 SOFT               -1.65 
6 WET                12.6  
### This can help us know what average laptimes to expect when using a certain type of compound.

## Using a 2 tailed T-Test we will test to see if a driver making a pit stop is related to the lap number.
## Null Hypothesis: If a driver makes a pit stop it does not effect what lap number they pitted on.
## Alternative Hypothesis: If a driver makes a pit stop it will effect what lap number they pitted on.

as.character(df$PitStop)
alpha = 0.05
results <- t.test(LapNumber ~ PitStop, data=df)
p <- results$p.value
if(p <= alpha){
  print("Rejected")
} else {
  print("Failed to reject")
}

### The null hypothesis was rejected, this means that there is a relationship bettween a driver making a pit stop and the lap number they pitted at.
### Finding the trend in that relationship can help to predict other teams pit stop window.

## This image shows us the the trends of laptimes as a race prgresses, it is seperated by compound to display each tyres unique trend.
![Trends in laptime over race progress](Trends_in_laptime_over_race_progress.png)
### This can help us to predict how tyre degradation and pit stop windows.

## Our next visual goes along with the previous one. It shows the overall average laptime for each tyre compound.
![Average laptime by tyre compound](Average_laptime_by_tyre_compound.png)
### This is helpful in giving us a benchmark laptime to compare degredation to.

## The final image shows us the overall average cumulative tyre degradation year by year for the time periods we have.
![Average cumulative degradation by year](Average_cumulative_degradation_by_year.png)
### Observing these trends we can see what year had the best degradation management, which can help in future decisions about car upgrades.
