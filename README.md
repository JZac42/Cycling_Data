# Cycling_Data
## Capstone Project: Cyclistic Usage Data over 12 Months
### Description 
Through Coursera, I've been pursuing the Google Data Analytics Professional Certificate. This is the course's final capstone assignment for me. I will be examining a public dataset for a made-up corporation in this case study, which is being provided by the course. I'll be analyzing data with the R programming language and creating visualizations with Tableau. The dataset included information about rides such as start and end timings, ride lengths, user types (members or casual users), and the day of the week from various months in 2022 and 2023. Data cleaning, data combining, calculating journey lengths, and examining ride trends for various user groups were the project's main goals.
### The Objective
By turning casual users into yearly members, Cyclistic, a bike-share company with 5,824 bicycles and 692 stations in Chicago, hopes to boost profitability. The team wants to comprehend how annual members and casual riders use the service differently, why casual riders might buy annual memberships, and how digital media affects marketing strategies. This analysis aims to offer insightful guidance for the upcoming marketing program. The company wants to know how casual and annual members use the service differently, what can help make more casual riders into annual members, and what sorts of digital media would have an impact, if any.
### The Tools Used
Originally, I was going to use spreadsheets (Excel, Google Sheets, Libra Calc, etc.) to clean and do preliminary analysis. Since each month would have been a separate sheet, there would have been at least another combined sheet for cleaning and transforming, and the sheer amount of data in each month would have created a 13-page spreadsheet that takes literally minutes to load. It was clearly not practical, so I switched to using RStudio to clean and perform analysis. Then I exported the data set to Tableau Public for visualizations.
The Process: Loading, Cleaning, and Transforming
First, we need to load all the relevant packages; this means making sure that both RStudio and RTools are active and up-to-date. From there, the packages are:
```
	install.packages("tidyverse")
	install.packages("lubridate")
	install.packages("janitor")
	install.packages("dplyr")
	library(tidyverse)
	library(lubridate)
	library(janitor)
	library(dplyr)
```
After that we then load the data. We will be loading raw data directly since spreadsheets proved to be slow and difficult to use. 
```
	bikedat1 <- read.csv("202205-divvy-tripdata.csv")
	bikedat2 <- read.csv("202206-divvy-tripdata.csv")
	bikedat3 <- read.csv("202207-divvy-tripdata.csv")
	bikedat4 <- read.csv("202208-divvy-tripdata.csv")
	bikedat5 <- read.csv("202209-divvy-tripdata.csv")
	bikedat6 <- read.csv("202210-divvy-tripdata.csv")
	bikedat7 <- read.csv("202211-divvy-tripdata.csv")
	bikedat8 <- read.csv("202212-divvy-tripdata.csv")
	bikedat9 <- read.csv("202301-divvy-tripdata.csv")
	bikedat10 <- read.csv("202302-divvy-tripdata.csv")
	bikedat11 <- read.csv("202303-divvy-tripdata.csv")
	bikedat12 <- read.csv("202304-divvy-tripdata.csv")
```
Now  we combine the data sets into one for cleaning and general use, this perservives the raw data in case it needs to be used later or reloading/reworking becomes needed. 
```
biketotal <- rbind(bikedat1, bikedat2, bikedat3, bikedat4, bikedat5, bikedat6, bikedat7, bikedat8, bikedat9, bikedat10, bikedat11, bikedat12)
```
The data is ready to be cleaned, first part of cleaning is to remove irrelevent data, or null spaces.
```
  biketotal <- janitor::remove_empty(dat = biketotal,which = c("cols"))
	biketotal <- janitor::remove_empty(dat = biketotal,which = c("rows"))
	dim(biketotal)
  biketotal <- biketotal %>%  
  	select(-c(start_lat, start_lng, end_lat, end_lng, start_station_name, start_station_id, end_station_name, end_station_id))
```
Next is to standardize the formating, such as dates, times, starts and finishes, etc. Converting start and end at to proper date format and adding sperate columns for each to allow better analysis.
```
	biketotal$started_at <- lubridate::as_datetime(biketotal$started_at)
	biketotal$ended_at <- lubridate::as_datetime(biketotal$ended_at)
	biketotal <- biketotal %>%
  mutate(
    date = as.Date(started_at),
    month = month(started_at),
    day = day(started_at),
    year = year(started_at),
    day_of_week = wday(started_at, label = TRUE),
    start_time = paste(hour(started_at), minute(started_at), sep = ":"),
    end_time = paste(hour(ended_at), minute(ended_at), sep = ":"))
```
Now we transform the data to get usable measurements for analysis and visualization, getting times for each use in minutes and hours making sure to round off decimals for readablity. 
```
	biketotal$length_min <- difftime(biketotal$ended_at, biketotal$started_at, units = "mins")
	biketotal$length_min <- round(as.numeric(biketotal$length_min), 3)
	biketotal$length_hr <- difftime(biketotal$ended_at, biketotal$started_at, units = "hours")
	biketotal$length_hr <- round(as.numeric(biketotal$length_hr), 2)
```
Finally the data is ready to out put to Tableau for visualization and manipulation for analysis.
```
	write.csv(biketotal, file = "cycle_data_tableau.csv", row.names = FALSE)
```
### Visualizations and Dashboard
Using Tableau these are the individual visualizations that I created for this case study.
![Piechart](https://github.com/JZac42/Cycling_Data/assets/140922648/6b650269-ac12-4709-a12b-ab1889242482)
![Linegraph](https://github.com/JZac42/Cycling_Data/assets/140922648/e6e60bf6-c95f-4228-b023-4615abfdd079)
![BarChart](https://github.com/JZac42/Cycling_Data/assets/140922648/fa2dc307-7484-427d-acb5-d8aaa9f06545)
![Circleplot](https://github.com/JZac42/Cycling_Data/assets/140922648/54646ef5-8d95-4ac8-ab1d-5a724e4a260b)
I then combined these to create a Dashboard that allows for seeing all the information at once in a convient and easy to understand way.
![Dashboard](https://github.com/JZac42/Cycling_Data/assets/140922648/40e60ff7-031f-483c-b14f-a532962aa6b5)
### The Conclusion and Recommendations
It is very clear that while annual members use the bike share more often per month that causal users ride for much longer periods of time when they do use it. To have the greatest imapact on converting casual to members you would want to target any events or discount campaigns at this time. Especially on Sundays, which is when casual user use spikes the most. I would recommend gamifying the usage, create a leadership board based on usage, or offer points and small prizes to those that reach milestones. If you make a template that is shareable on social medias for this it will further encourage usage. This along with targeted discounts on specific days and months, will drive both casual and member usage up. In short:
* Targets Seasonal and Weekend Discounts and promotional campaigns
* Gamification, Leadership Boards, points for use, accessories as prizes, etc. will drive both casual and member use
* Social media posting, allow milestones of usage to be shared and posted to create buzz
