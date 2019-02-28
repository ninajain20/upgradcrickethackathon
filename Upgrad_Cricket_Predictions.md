---
title: "cricketdataanalysis"
output:
      html_document:
        keep_md: true
  
---
#Data Collection
We proceeded the process by collecting the relevant data for the total player for both teams.
So in order to capture the present form of all the players from both team( India and Australia) we took in account of their recent matches.
To capture the form against the opponent we took in account all the matches played by the current teams against one another.
We also consider how each ground plays a role in determining each players favorite ground to play.
The data we had covered various things.
Batsman - 
Total ball faced by the batsman,
total run scored,
Strike rate,
number of 4s and 6s,
The position the batsman came to bat,
the result of coin toss,
Bowler -
The total number of overs bowled by the bowler,
number of maiden overs,
total wickets taken,
economy,
the total  run given,

#Data Cleaning
Removed whitespaces before and after the entries,
Subsituted a common value 'NA' for all kinds of missing entries like {blanks}, {-}, {--}, {null}, 
Correction of the spellings of the names of the batsman and bowlers



Importing the Data for the Batsman in from the excel file. Removing unnecessory columns like 'Date'

```r
library(readxl)
Batsman <- read_excel("C:/Users/Arjit/Downloads/Batsman.xlsx", sheet = "Batsman", col_types = c("text", "numeric", "text", "numeric", "numeric", "numeric", "numeric", "numeric", "text", "numeric", "text", "text", "date"))
Batsman <- subset(Batsman, select = -`Start Date` )
```

Changing the categorical Variables to factors before building the model

```r
Batsman$Batsman<-factor(Batsman$Batsman)
Batsman$Pos<-factor(Batsman$Pos)
Batsman$Dismissal<-factor(Batsman$Dismissal)
Batsman$Inns<-factor(Batsman$Inns)
Batsman$Opposition<-factor(Batsman$Opposition)
Batsman$Ground<-factor(Batsman$Ground)
```

Subsituting NAs for the feilds marked as '-'

```r
Batsman$`4s`[Batsman$`4s`=="-"]<-NA
Batsman$`6s`[Batsman$`6s`=="-"]<-NA
Batsman$SR[Batsman$SR=="-"]<-NA
Batsman$Pos[Batsman$Pos=="-"]<-NA
Batsman$Dismissal[Batsman$Dismissal=="-"]<-NA
Batsman$Inns[Batsman$Inns=="-"]<-NA
Batsman$Opposition[Batsman$Opposition=="-"]<-NA
Batsman$Ground[Batsman$Ground=="-"]<-NA
Batsman$Batsman[Batsman$Batsman=="-"]<-NA
Batsman$Mins[Batsman$Mins=="-"]<-NA
Batsman$BF[Batsman$BF=="-"]<-NA
```


Changing discrete variables to numeric to tell are they are a number

```r
Batsman$Mins<-as.numeric(Batsman$Mins)
Batsman$Runs<-as.numeric(Batsman$Runs)
Batsman$BF<-as.numeric(Batsman$BF)
Batsman$SR<-as.numeric(Batsman$SR)
Batsman$`4s`<-as.numeric(Batsman$`4s`)
Batsman$`6s`<-as.numeric(Batsman$`6s`)
```

Creating Visualizations to Understand the data.


We will plot the batsman with the number of matches they have played. This will help us in knowing which players are more experienced.


```r
Batsman_Experience<-as.data.frame(table(Batsman$Batsman))

library(plotly)
```

```
## Loading required package: ggplot2
```

```
## 
## Attaching package: 'plotly'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
```

```
## The following object is masked from 'package:stats':
## 
##     filter
```

```
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
plot_ly(
  x = Batsman_Experience$Var1,y = Batsman_Experience$Freq,name = "Batsman_Experience",type = "bar")
```

<!--html_preserve--><div id="htmlwidget-4e31c72221b4f3f53b66" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-4e31c72221b4f3f53b66">{"x":{"visdat":{"25a840eb4fdb":["function () ","plotlyVisDat"]},"cur_data":"25a840eb4fdb","attrs":{"25a840eb4fdb":{"x":["Aaron Finch","Ambati Rayudu","Glenn Maxwell","KL Rahul","Marcus Stoinis","Ravindra Jadeja","Rohit Sharma","Shaun Marsh","Shikhar Dhawan","Usman Khawaja","Virat Kohli"],"y":[99,50,90,13,24,143,515,63,119,21,215],"name":"Batsman_Experience","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[],"type":"category","categoryorder":"array","categoryarray":["Aaron Finch","Ambati Rayudu","Glenn Maxwell","KL Rahul","Marcus Stoinis","Ravindra Jadeja","Rohit Sharma","Shaun Marsh","Shikhar Dhawan","Usman Khawaja","Virat Kohli"]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":["Aaron Finch","Ambati Rayudu","Glenn Maxwell","KL Rahul","Marcus Stoinis","Ravindra Jadeja","Rohit Sharma","Shaun Marsh","Shikhar Dhawan","Usman Khawaja","Virat Kohli"],"y":[99,50,90,13,24,143,515,63,119,21,215],"name":"Batsman_Experience","type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->







Here, we are trying to see a plot of the batsman vs number of Runs scored by him.

```r
batsman_runs <-data.frame(Batsman_Name = c("Rohit Sharma","Virat Kohli","Shikhar Dhawan", "Ravindra Jadeja","KL Rahul", "Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"),
Batsman_Runs = c(sum(Batsman$Runs[Batsman$Batsman =="Rohit Sharma"]),sum(Batsman$Runs[Batsman$Batsman =="Virat Kohli"]),sum(Batsman$Runs[Batsman$Batsman =="Shikhar Dhawan"]),sum(Batsman$Runs[Batsman$Batsman =="Ravindra Jadeja"]),
              sum(Batsman$Runs[Batsman$Batsman =="KL Rahul"]),sum(Batsman$Runs[Batsman$Batsman =="Ambati Rayudu"]),
              sum(Batsman$Runs[Batsman$Batsman =="Aaron Finch"]),sum(Batsman$Runs[Batsman$Batsman =="Shaun Marsh"]),
              sum(Batsman$Runs[Batsman$Batsman =="Glenn Maxwell"]),sum(Batsman$Runs[Batsman$Batsman =="Usman Khawaja"]),sum(Batsman$Runs[Batsman$Batsman =="Marcus Stoinis"])))  

batsman_runs
```

```
##       Batsman_Name Batsman_Runs
## 1     Rohit Sharma        17821
## 2      Virat Kohli        10443
## 3   Shikhar Dhawan         5069
## 4  Ravindra Jadeja         1990
## 5         KL Rahul          317
## 6    Ambati Rayudu         1637
## 7      Aaron Finch         3444
## 8      Shaun Marsh         2536
## 9    Glenn Maxwell         2327
## 10   Usman Khawaja          583
## 11  Marcus Stoinis          807
```

```r
library(plotly)

plot_ly(
  x = batsman_runs$Batsman_Name,y = batsman_runs$Batsman_Runs,name = "Batsman Runs",type = "bar")
```

<!--html_preserve--><div id="htmlwidget-ea2b0d407b9fd6d04f76" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-ea2b0d407b9fd6d04f76">{"x":{"visdat":{"25a85f053550":["function () ","plotlyVisDat"]},"cur_data":"25a85f053550","attrs":{"25a85f053550":{"x":["Rohit Sharma","Virat Kohli","Shikhar Dhawan","Ravindra Jadeja","KL Rahul","Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"],"y":[17821,10443,5069,1990,317,1637,3444,2536,2327,583,807],"name":"Batsman Runs","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[],"type":"category","categoryorder":"array","categoryarray":["Aaron Finch","Ambati Rayudu","Glenn Maxwell","KL Rahul","Marcus Stoinis","Ravindra Jadeja","Rohit Sharma","Shaun Marsh","Shikhar Dhawan","Usman Khawaja","Virat Kohli"]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":["Rohit Sharma","Virat Kohli","Shikhar Dhawan","Ravindra Jadeja","KL Rahul","Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"],"y":[17821,10443,5069,1990,317,1637,3444,2536,2327,583,807],"name":"Batsman Runs","type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->

Our plot shows us that Rohit Sharma has the highest number of Runs, followed by Virat Kohli


Similarily, we will plot for the number of fours


```r
batsman_fours=data.frame(Batsman_Name = c("Rohit Sharma","Virat Kohli","Shikhar Dhawan", "Ravindra Jadeja","KL Rahul", "Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"),
Batsman_fours = c(sum(Batsman$`4s`[Batsman$Batsman =="Rohit Sharma"]),sum(Batsman$`4s`[Batsman$Batsman =="Virat Kohli"]),sum(Batsman$`4s`[Batsman$Batsman =="Shikhar Dhawan"]),sum(Batsman$`4s`[Batsman$Batsman =="Ravindra Jadeja"]),
                                           sum(Batsman$`4s`[Batsman$Batsman =="KL Rahul"]),sum(Batsman$`4s`[Batsman$Batsman =="Ambati Rayudu"]),
                                           sum(Batsman$`4s`[Batsman$Batsman =="Aaron Finch"]),sum(Batsman$`4s`[Batsman$Batsman =="Shaun Marsh"]),
                                           sum(Batsman$`4s`[Batsman$Batsman =="Glenn Maxwell"]),sum(Batsman$`4s`[Batsman$Batsman =="Usman Khawaja"]),sum(Batsman$`4s`[Batsman$Batsman =="Marcus Stoinis"])))                          

batsman_fours
```

```
##       Batsman_Name Batsman_fours
## 1     Rohit Sharma          1445
## 2      Virat Kohli           979
## 3   Shikhar Dhawan           631
## 4  Ravindra Jadeja           155
## 5         KL Rahul            25
## 6    Ambati Rayudu           139
## 7      Aaron Finch           356
## 8      Shaun Marsh           232
## 9    Glenn Maxwell           233
## 10   Usman Khawaja            54
## 11  Marcus Stoinis            59
```


```r
plot_ly(
  x = batsman_fours$Batsman_Name,y = batsman_fours$Batsman_fours,name = "Batsman_fours",type = "bar")
```

<!--html_preserve--><div id="htmlwidget-3da38d00a5fabe008911" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-3da38d00a5fabe008911">{"x":{"visdat":{"25a8631d1bb6":["function () ","plotlyVisDat"]},"cur_data":"25a8631d1bb6","attrs":{"25a8631d1bb6":{"x":["Rohit Sharma","Virat Kohli","Shikhar Dhawan","Ravindra Jadeja","KL Rahul","Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"],"y":[1445,979,631,155,25,139,356,232,233,54,59],"name":"Batsman_fours","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[],"type":"category","categoryorder":"array","categoryarray":["Aaron Finch","Ambati Rayudu","Glenn Maxwell","KL Rahul","Marcus Stoinis","Ravindra Jadeja","Rohit Sharma","Shaun Marsh","Shikhar Dhawan","Usman Khawaja","Virat Kohli"]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":["Rohit Sharma","Virat Kohli","Shikhar Dhawan","Ravindra Jadeja","KL Rahul","Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"],"y":[1445,979,631,155,25,139,356,232,233,54,59],"name":"Batsman_fours","type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->

Similarily, for sixes


```r
library(plotly)


batsman_sixes <-data.frame(Batsman_Name = c("Rohit Sharma","Virat Kohli", "Shikhar Dhawan", "Ravindra Jadeja","KL Rahul", "Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"),
                          Batsman_Sixes = c(sum(Batsman$`6s`[Batsman$Batsman =="Rohit Sharma"]),sum(Batsman$`6s`[Batsman$Batsman =="Virat Kohli"]),sum(Batsman$`6s`[Batsman$Batsman =="Shikhar Dhawan"]),sum(Batsman$`6s`[Batsman$Batsman =="Ravindra Jadeja"]),
                                           sum(Batsman$`6s`[Batsman$Batsman =="KL Rahul"]),sum(Batsman$`6s`[Batsman$Batsman =="Ambati Rayudu"]),
                                           sum(Batsman$`6s`[Batsman$Batsman =="Aaron Finch"]),sum(Batsman$`6s`[Batsman$Batsman =="Shaun Marsh"]),
                                           sum(Batsman$`6s`[Batsman$Batsman =="Glenn Maxwell"]),sum(Batsman$`6s`[Batsman$Batsman =="Usman Khawaja"]),sum(Batsman$`6s`[Batsman$Batsman =="Marcus Stoinis"])))                         

plot_ly(
  x = batsman_sixes$Batsman_Name,y = batsman_sixes$Batsman_Sixes,name = "Batsman Runs",type = "bar")
```

<!--html_preserve--><div id="htmlwidget-bc015a462b605ee1879c" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-bc015a462b605ee1879c">{"x":{"visdat":{"25a875cd13c0":["function () ","plotlyVisDat"]},"cur_data":"25a875cd13c0","attrs":{"25a875cd13c0":{"x":["Rohit Sharma","Virat Kohli","Shikhar Dhawan","Ravindra Jadeja","KL Rahul","Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"],"y":[429,114,62,36,5,29,76,33,74,8,34],"name":"Batsman Runs","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[],"type":"category","categoryorder":"array","categoryarray":["Aaron Finch","Ambati Rayudu","Glenn Maxwell","KL Rahul","Marcus Stoinis","Ravindra Jadeja","Rohit Sharma","Shaun Marsh","Shikhar Dhawan","Usman Khawaja","Virat Kohli"]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":["Rohit Sharma","Virat Kohli","Shikhar Dhawan","Ravindra Jadeja","KL Rahul","Ambati Rayudu","Aaron Finch","Shaun Marsh","Glenn Maxwell","Usman Khawaja","Marcus Stoinis"],"y":[429,114,62,36,5,29,76,33,74,8,34],"name":"Batsman Runs","type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->



Creating Dummy Variables of Categorical Function to use in Regression

```r
Batsman<-data.frame(model.matrix(~ .,Batsman))
```





Splitting the data int Testing and Training and applying split as 0.75.


```r
require(caTools) 
```

```
## Loading required package: caTools
```

```r
set.seed(123)
sample = sample.split(Batsman,SplitRatio = 0.75)
Batsman_train =subset(Batsman,sample ==TRUE)
Batsman_test=subset(Batsman, sample==FALSE)
```

Building a simple linear regression model to predict the runs

```r
Batsman_model<-lm(Runs~.,data=Batsman_train)


k<-as.data.frame(subset(Batsman_test, select = - Runs ))
y<-predict.lm(Batsman_model,k)

mean(y-Batsman_test$Runs) *mean(y-Batsman_test$Runs) #Mean Squared Error as a metric to evaluate the model
```

```
## [1] 0.1653363
```

We will now try building a random forest regressor model. This will also help us decide the important features for variable sleection

```r
require(randomForest)
```

```
## Loading required package: randomForest
```

```
## randomForest 4.6-14
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

```r
fit=randomForest(Runs~.,data=Batsman_train,importance=T)
y2<-predict(fit,k)
mean(Batsman_test$Runs-y2)*mean(Batsman_test$Runs-y2)
```

```
## [1] 0.03424071
```
We can see that the mean sqaured error has reduced a lot upon using random forest for building the model.

Now, we will look at the important features


```r
library(caret)
```

```
## Loading required package: lattice
```

```r
importance <- varImp(fit, scale=FALSE)
varImpPlot(fit)
```

![](Upgrad_Cricket_Predictions_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
Till now we have studied the data using the historic data. Now, we build our own data according to the upcoming match details.This data is stored in parameter_Batsman.

Reading the data and converting the datatypes.

Creating the dummy variables.



```r
library(readxl)

parameter_Batsman <- read_excel("C:/Users/Arjit/Downloads/parameter_Batsman (1).xlsx",sheet = "Batsman", col_types = c("text", "text", "text", "text", "text"))
View(parameter_Batsman)

parameter_Batsman$Ground<-factor(parameter_Batsman$Ground)
parameter_Batsman$Opposition<-factor(parameter_Batsman$Opposition)
parameter_Batsman$Batsman<-factor(parameter_Batsman$Batsman)
parameter_Batsman$Pos<-factor(parameter_Batsman$Pos)
parameter_Batsman$Inns<-factor(parameter_Batsman$Inns)

parameter_Batsman<-data.frame(model.matrix(~ .,parameter_Batsman))
```



Fitting a random forest regressor model to the data and predicting who will score the highest number of Runs.

```r
set.seed(123) #To produce reproducible results

fit_final<-randomForest(Runs~GroundHyderabad..Deccan. + GroundMohali +GroundNagpur +GroundRanchi+Oppositionv.India+BatsmanAmbati.Rayudu+BatsmanGlenn.Maxwell + BatsmanKL.Rahul+BatsmanMarcus.Stoinis+BatsmanRavindra.Jadeja+ BatsmanRohit.Sharma+BatsmanShaun.Marsh+ BatsmanUsman.Khawaja+BatsmanVirat.Kohli +Pos2+ Pos3+ Pos4+ Pos5 +Pos6 +  Inns2,data = Batsman)

##fitted a random forest model 
k_final<-parameter_Batsman[c("GroundHyderabad..Deccan.","GroundMohali","GroundNagpur","GroundRanchi","Oppositionv.India","BatsmanAmbati.Rayudu","BatsmanGlenn.Maxwell","BatsmanKL.Rahul","BatsmanMarcus.Stoinis","BatsmanRavindra.Jadeja","BatsmanRohit.Sharma","BatsmanShaun.Marsh","BatsmanUsman.Khawaja","BatsmanVirat.Kohli","Pos2","Pos3","Pos4","Pos5","Pos6","Inns2")] ##New test data to provide to the model to predict results

parameter_Batsman$y_final=predict(fit_final,k_final) #Predicting Results
parameter_Batsman$y_final==max(parameter_Batsman$y_final) #Selecting the parameters with predicted highest number of runs
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
## [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [34] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

Our model shows the value as TRUE for 16th Row. This means that the new data we gave (According to the upcoming match) has been predicted by our model and the 16th Row has the highest number of Runs.

We can see the corresponding value of 'Batsman Name' to find out who is scoring the maximum runs


```r
(parameter_Batsman[16,]==1)
```

```
##    X.Intercept. GroundHyderabad..Deccan. GroundMohali GroundNagpur
## 16         TRUE                    FALSE        FALSE         TRUE
##    GroundRanchi Oppositionv.India BatsmanAmbati.Rayudu
## 16        FALSE             FALSE                FALSE
##    BatsmanGlenn.Maxwell BatsmanKL.Rahul BatsmanMarcus.Stoinis
## 16                FALSE           FALSE                 FALSE
##    BatsmanRavindra.Jadeja BatsmanRohit.Sharma BatsmanShaun.Marsh
## 16                  FALSE                TRUE              FALSE
##    BatsmanUsman.Khawaja BatsmanVirat.Kohli  Pos2  Pos3  Pos4 Pos5  Pos6
## 16                FALSE              FALSE FALSE FALSE FALSE TRUE FALSE
##    Inns2 y_final
## 16 FALSE   FALSE
```
As it is Showing True only infront of Rohit Sharma, according to our model, Rohit Sharma will have the maximum runs = 79.78106 or 78 as shown in the next chunk

```r
max(parameter_Batsman$y_final) #maximum value of runs 
```

```
## [1] 79.78106
```


Similarily,
For predicting the highest number of fours

```r
fit_final_1<-randomForest(X.4s.~GroundHyderabad..Deccan. + GroundMohali +GroundNagpur +GroundRanchi+Oppositionv.India+BatsmanAmbati.Rayudu+BatsmanGlenn.Maxwell + BatsmanKL.Rahul+BatsmanMarcus.Stoinis+BatsmanRavindra.Jadeja+ BatsmanRohit.Sharma+BatsmanShaun.Marsh+ BatsmanUsman.Khawaja+BatsmanVirat.Kohli +Pos2+ Pos3+ Pos4+ Pos5 +Pos6 +  Inns2,data = Batsman)

#fitted the model


k_final<-parameter_Batsman[c("GroundHyderabad..Deccan.","GroundMohali","GroundNagpur","GroundRanchi","Oppositionv.India","BatsmanAmbati.Rayudu","BatsmanGlenn.Maxwell","BatsmanKL.Rahul","BatsmanMarcus.Stoinis","BatsmanRavindra.Jadeja","BatsmanRohit.Sharma","BatsmanShaun.Marsh","BatsmanUsman.Khawaja","BatsmanVirat.Kohli","Pos2","Pos3","Pos4","Pos5","Pos6","Inns2")]
#new data to be given for predictions
parameter_Batsman$y_final_1=predict(fit_final_1,k_final) #Predicting for fours
parameter_Batsman$y_final_1==max(parameter_Batsman$y_final_1) #Selecting row with highest number of fours predicted
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [34] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```
It is showing the true value for row number 14

```r
(parameter_Batsman[14,]==1) #looking at the parametersin row 14
```

```
##    X.Intercept. GroundHyderabad..Deccan. GroundMohali GroundNagpur
## 14         TRUE                    FALSE        FALSE         TRUE
##    GroundRanchi Oppositionv.India BatsmanAmbati.Rayudu
## 14        FALSE             FALSE                FALSE
##    BatsmanGlenn.Maxwell BatsmanKL.Rahul BatsmanMarcus.Stoinis
## 14                FALSE           FALSE                 FALSE
##    BatsmanRavindra.Jadeja BatsmanRohit.Sharma BatsmanShaun.Marsh
## 14                  FALSE               FALSE              FALSE
##    BatsmanUsman.Khawaja BatsmanVirat.Kohli  Pos2 Pos3  Pos4  Pos5  Pos6
## 14                FALSE               TRUE FALSE TRUE FALSE FALSE FALSE
##    Inns2 y_final y_final_1
## 14 FALSE   FALSE     FALSE
```
As it is Showing True only infront of Virat Kohli, according to our model, Virat Kohli will have the maximum fours = 6.432789 or 6 as shown in the next chunk

```r
max(parameter_Batsman$y_final_1) #Maximum number of fours
```

```
## [1] 6.432789
```

For predicting the highest number of sixes

```r
fit_final_2<-randomForest(X.6s.~GroundHyderabad..Deccan. + GroundMohali +GroundNagpur +GroundRanchi+Oppositionv.India+BatsmanAmbati.Rayudu+BatsmanGlenn.Maxwell + BatsmanKL.Rahul+BatsmanMarcus.Stoinis+BatsmanRavindra.Jadeja+ BatsmanRohit.Sharma+BatsmanShaun.Marsh+ BatsmanUsman.Khawaja+BatsmanVirat.Kohli +Pos2+ Pos3+ Pos4+ Pos5 +Pos6 +  Inns2,data = Batsman)

#fitted the model
k_final<-parameter_Batsman[c("GroundHyderabad..Deccan.","GroundMohali","GroundNagpur","GroundRanchi","Oppositionv.India","BatsmanAmbati.Rayudu","BatsmanGlenn.Maxwell","BatsmanKL.Rahul","BatsmanMarcus.Stoinis","BatsmanRavindra.Jadeja","BatsmanRohit.Sharma","BatsmanShaun.Marsh","BatsmanUsman.Khawaja","BatsmanVirat.Kohli","Pos2","Pos3","Pos4","Pos5","Pos6","Inns2")]

#new data
parameter_Batsman$y_final_2=predict(fit_final_2,k_final) #predcitions for number of sixes
parameter_Batsman$y_final_2==max(parameter_Batsman$y_final_2) #fetching row with highest number of sixes
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [34]  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```
It is showing the true value for row number 34

```r
(parameter_Batsman[34,]==1) #looking at parameters of row 34
```

```
##    X.Intercept. GroundHyderabad..Deccan. GroundMohali GroundNagpur
## 34         TRUE                    FALSE         TRUE        FALSE
##    GroundRanchi Oppositionv.India BatsmanAmbati.Rayudu
## 34        FALSE             FALSE                FALSE
##    BatsmanGlenn.Maxwell BatsmanKL.Rahul BatsmanMarcus.Stoinis
## 34                FALSE           FALSE                 FALSE
##    BatsmanRavindra.Jadeja BatsmanRohit.Sharma BatsmanShaun.Marsh
## 34                  FALSE                TRUE              FALSE
##    BatsmanUsman.Khawaja BatsmanVirat.Kohli  Pos2  Pos3  Pos4  Pos5  Pos6
## 34                FALSE              FALSE FALSE FALSE FALSE FALSE FALSE
##    Inns2 y_final y_final_1 y_final_2
## 34 FALSE   FALSE     FALSE     FALSE
```
As it is Showing True only infront of Rohit Sharma, according to our model, Rohit Sharma will have the maximum fours =  2.707413 or 3 as shown in the next chunk

```r
max(parameter_Batsman$y_final_2) #maximum number of sixes
```

```
## [1] 2.707413
```

Same Process will be followed for Bowlers data

Here again we will import the data, Change the datatypes, replace the 'NA' values

```r
library(readxl)
Bowler <- read_excel("C:/Users/Arjit/Downloads/Batsman.xlsx", sheet = "Bowler", col_types = c("text", "numeric", "numeric", "numeric", "numeric", "numeric", "numeric", "numeric", "text", "text", "date"))

#Imported the data

Bowler <- subset(Bowler, select = -`Start Date` )

Bowler$Bowler<-factor(Bowler$Bowler)
Bowler$Pos<-factor(Bowler$Pos)
Bowler$Mdns<-factor(Bowler$Mdns)
Bowler$Inns<-factor(Bowler$Inns)
Bowler$Opposition<-factor(Bowler$Opposition)
Bowler$Ground<-factor(Bowler$Ground)

Bowler$Overs[Bowler$Overs=="-"]<-NA
Bowler$Runs[Bowler$Runs=="-"]<-NA
Bowler$Wkts[Bowler$Wkts=="-"]<-NA
Bowler$Pos[Bowler$Pos=="-"]<-NA
Bowler$Mdns[Bowler$Mdns=="-"]<-NA
Bowler$Inns[Bowler$Inns=="-"]<-NA
Bowler$Opposition[Bowler$Opposition=="-"]<-NA
Bowler$Ground[Bowler$Ground=="-"]<-NA
Bowler$Bowler[Bowler$Bowler=="-"]<-NA
Bowler$Econ[Bowler$Econ=="-"]<-NA
```
Plotting the data to see the relationship between the parameters

This plot shows us the number of matches played by any bowler. This helps us see the most experienced players.

```r
Bowler_Experience<-as.data.frame(table(Bowler$Bowler))

library(plotly)

plot_ly(
  x = Bowler_Experience$Var1,y = Bowler_Experience$Freq,name = "Bowler Experience",type = "bar")
```

<!--html_preserve--><div id="htmlwidget-f901f08e6cd773d508b5" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-f901f08e6cd773d508b5">{"x":{"visdat":{"25a823c2204b":["function () ","plotlyVisDat"]},"cur_data":"25a823c2204b","attrs":{"25a823c2204b":{"x":["Adam Zampa","Bhuvneshwar Kumar","Glenn Maxwell","Jasprit Bumrah","Kane Richardson","Kuldeep Yadav","Mohammed Shami","Nathan Coulter-Nile","Nathan Lyon","Pat Cummins","Ravindra Jadeja","Yuzvendra Chahal"],"y":[33,102,84,44,18,39,56,22,38,17,143,40],"name":"Bowler Experience","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[],"type":"category","categoryorder":"array","categoryarray":["Adam Zampa","Bhuvneshwar Kumar","Glenn Maxwell","Jasprit Bumrah","Kane Richardson","Kuldeep Yadav","Mohammed Shami","Nathan Coulter-Nile","Nathan Lyon","Pat Cummins","Ravindra Jadeja","Yuzvendra Chahal"]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":["Adam Zampa","Bhuvneshwar Kumar","Glenn Maxwell","Jasprit Bumrah","Kane Richardson","Kuldeep Yadav","Mohammed Shami","Nathan Coulter-Nile","Nathan Lyon","Pat Cummins","Ravindra Jadeja","Yuzvendra Chahal"],"y":[33,102,84,44,18,39,56,22,38,17,143,40],"name":"Bowler Experience","type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->

The following plot shows us the number of wickets by each of the bowlers

```r
Bowler_Wickets <-data.frame(Bowler_Name = c("Nathan Lyon","Pat Cummins","Ravindra Jadeja","Kuldeep Yadav","Kane Richardson","Glenn Maxwell","Adam Zampa","Yuzvendra Chahal","Nathan Coulter-Nile","Mohammed Shami","Jasprit Bumrah","Bhuvneshwar Kumar"),
Bowler_Plus = c(sum(Bowler$Wkts[Bowler$Bowler =="Nathan Lyon"]),sum(Bowler$Wkts[Bowler$Bowler =="Pat Cummins"]),sum(Bowler$Wkts[Bowler$Bowler 
              =="Ravindra Jadeja"]),sum(Bowler$Wkts[Bowler$Bowler =="Kuldeep Yadav"]),sum(Bowler$Wkts[Bowler$Bowler =="Kane Richardson"]),
              sum(Bowler$Wkts[Bowler$Bowler =="Adam Zampa"]),sum(Bowler$Wkts[Bowler$Bowler =="Bhuvneshwar Kumar"]),
              sum(Bowler$Wkts[Bowler$Bowler =="Yuzvendra Chahal"]),sum(Bowler$Wkts[Bowler$Bowler =="Nathan Coulter-Nile"]),
              sum(Bowler$Wkts[Bowler$Bowler =="Glenn Maxwell"]),sum(Bowler$Wkts[Bowler$Bowler =="Jasprit Bumrah"]),sum(Bowler$Wkts[Bowler$Bowler =="Mohammed Shami"])))

library(plotly)

plot_ly(
  x = Bowler_Wickets$Bowler_Name,y = Bowler_Wickets$Bowler_Plus,name = "Bowler Wicket",type = "bar")
```

<!--html_preserve--><div id="htmlwidget-7ebda769bc32d3e50242" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-7ebda769bc32d3e50242">{"x":{"visdat":{"25a8a8157ef":["function () ","plotlyVisDat"]},"cur_data":"25a8a8157ef","attrs":{"25a8a8157ef":{"x":["Nathan Lyon","Pat Cummins","Ravindra Jadeja","Kuldeep Yadav","Kane Richardson","Glenn Maxwell","Adam Zampa","Yuzvendra Chahal","Nathan Coulter-Nile","Mohammed Shami","Jasprit Bumrah","Bhuvneshwar Kumar"],"y":[77,18,170,62,27,40,114,71,38,46,78,102],"name":"Bowler Wicket","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[],"type":"category","categoryorder":"array","categoryarray":["Adam Zampa","Bhuvneshwar Kumar","Glenn Maxwell","Jasprit Bumrah","Kane Richardson","Kuldeep Yadav","Mohammed Shami","Nathan Coulter-Nile","Nathan Lyon","Pat Cummins","Ravindra Jadeja","Yuzvendra Chahal"]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":["Nathan Lyon","Pat Cummins","Ravindra Jadeja","Kuldeep Yadav","Kane Richardson","Glenn Maxwell","Adam Zampa","Yuzvendra Chahal","Nathan Coulter-Nile","Mohammed Shami","Jasprit Bumrah","Bhuvneshwar Kumar"],"y":[77,18,170,62,27,40,114,71,38,46,78,102],"name":"Bowler Wicket","type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":[]}</script><!--/html_preserve-->
Changing the data types of the variables, splitting the data 


```r
#Changing the Data types
Bowler$Runs<-as.numeric(Bowler$Runs)
Bowler$Wkts<-as.numeric(Bowler$Wkts)
Bowler$Econ<-as.numeric(Bowler$Econ)
Bowler$Mdns<-as.numeric(Bowler$Mdns)
Bowler$Overs<-as.numeric(Bowler$Overs)
Bowler<-data.frame(model.matrix(~ .,Bowler))

Bowler <- subset(Bowler, select = - X.Intercept. )

#Splitting Data in the Ratio 0.8

require(caTools) 
set.seed(123)
sample = sample.split(Bowler,SplitRatio = 0.80)
Bowler_train =subset(Bowler,sample ==TRUE)
Bowler_test=subset(Bowler, sample==FALSE)
```

Creating a simple linear regression model and looking at the mean squared error

```r
Bowler_model<-lm(Wkts~.,data=Bowler_train)


k<-as.data.frame(subset(Bowler_test, select = - Wkts ))
y<-predict.lm(Bowler_model,k)

mean(y-Bowler_test$Wkts)*mean(y-Bowler_test$Wkts)
```

```
## [1] 0.001599208
```
Mean Squared Error given by this model is 0.001599208

Now creating a random forest model and looking at the mean squared error. Our aim is to decrease the mean squared error

```r
set.seed(123)
require(randomForest)
fit=randomForest(Wkts~.,data=Bowler_train,importance=T)
y2<-predict(fit,k)
mean(Bowler_test$Wkts-y2)*mean(Bowler_test$Wkts-y2)
```

```
## [1] 0.0006857448
```
Mean Squared Error given by this model is 0.0006857448 which is very much lessor than the previous model. That is why this is a better model.

The following plot will show us the important variables which will help us in feature selection.

```r
library(caret)
importance <- varImp(fit, scale=FALSE)
varImpPlot(fit)
```

![](Upgrad_Cricket_Predictions_files/figure-html/unnamed-chunk-32-1.png)<!-- -->
Our analysis was based on the historic data. Now we will use the data for the current match and build the model.

```r
#Reading the data

library(readxl)
parameter_Bowler <- read_excel("C:/Users/Arjit/Downloads/parameter_Batsman (1).xlsx", sheet = "Bowler", col_types = c("text",  "text", "text", "text", "text"))
parameter_Bowler$Ground<-factor(parameter_Bowler$Ground)
parameter_Bowler$Opposition<-factor(parameter_Bowler$Opposition)
parameter_Bowler$Bowler<-factor(parameter_Bowler$Bowler)
parameter_Bowler$Pos<-factor(parameter_Bowler$Pos)
parameter_Bowler$Inns<-factor(parameter_Bowler$Inns)

#Creating dummy Variables

parameter_Bowler<-data.frame(model.matrix(~ .,parameter_Bowler))

set.seed(123)

#Building the model
fit_final <-randomForest(Wkts~ GroundHyderabad..Deccan. + GroundMohali+GroundNagpur  + GroundRanchi  +Oppositionv.India+BowlerGlenn.Maxwell + BowlerJasprit.Bumrah+BowlerKane.Richardson+BowlerPat.Cummins+BowlerRavindra.Jadeja+BowlerYuzvendra.Chahal+Pos2 +Pos3 +Pos4+Pos5 +Pos6 +Inns2+BowlerKuldeep.Yadav+BowlerMohammed.Shami+BowlerNathan.Lyon,data = Bowler,importance=T)

#Putting in the new data

k_final<-parameter_Bowler[c( "GroundHyderabad..Deccan.","GroundMohali" ,  "GroundNagpur" , "GroundRanchi" ,    "Oppositionv.India", "BowlerGlenn.Maxwell" , "BowlerJasprit.Bumrah", "BowlerKane.Richardson" , "BowlerKuldeep.Yadav", "BowlerMohammed.Shami", "BowlerNathan.Lyon" ,   "BowlerPat.Cummins", "BowlerRavindra.Jadeja","BowlerYuzvendra.Chahal" , "Pos2" , "Pos3" ,"Pos4" ,  "Pos5","Pos6", "Inns2")]

#Predicting data for number of wickets as dependent variable 

parameter_Bowler$y_final=predict(fit_final,k_final)

#Fetching the row with highest number of wickets
parameter_Bowler$y_final==max(parameter_Bowler$y_final)
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [34] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [45] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE
```

It shows 'TRUE' for row 51

```r
parameter_Bowler[51,]==1 #fetching all the parameters for row 51
```

```
##    X.Intercept. GroundHyderabad..Deccan. GroundMohali GroundNagpur
## 51         TRUE                    FALSE        FALSE        FALSE
##    GroundRanchi Oppositionv.India BowlerGlenn.Maxwell BowlerJasprit.Bumrah
## 51        FALSE             FALSE               FALSE                FALSE
##    BowlerKane.Richardson BowlerKuldeep.Yadav BowlerMohammed.Shami
## 51                 FALSE                TRUE                FALSE
##    BowlerNathan.Lyon BowlerPat.Cummins BowlerRavindra.Jadeja
## 51             FALSE             FALSE                 FALSE
##    BowlerYuzvendra.Chahal Pos2  Pos3  Pos4  Pos5  Pos6 Inns2 y_final
## 51                  FALSE TRUE FALSE FALSE FALSE FALSE  TRUE   FALSE
```


As it is showing 'TRUE' for Kuldeep Yadav, Our model predicts that Kuldeep Yadav will have the highest number of wickets that is 2 wickets as shown in the next chunk.


```r
max(parameter_Bowler$y_final) #max number of wickets
```

```
## [1] 2.216886
```
##Predictions

## Winner of the Series : INDIA
##As the highest number of wickets, runs, fours, and sixes are by Indian Batsman and Bowlers, 

##Series Output: 4-0 (India,Australia)

##Highest Run Scorer : Rohit Sharma

##Highest Wicket Taker: Kuldeep Yadav

##Maximum Sixes : Rohit Sharma

##Maximum Fours : Virat Kohli
