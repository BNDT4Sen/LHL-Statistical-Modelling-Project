# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
My goal for this project was to determine if there was a significant relationship between the capacity of bike stations and the quantity and quality of nearby eateries in the city of Vienna.

## Process

### Starting with the CityBikes API
Firstly I took a look at the CityBikes API, and tried to get a feel for it. I brushed up on some of the API content and started to figure out how to make the requests that I would need. The more difficult part of this step was sorting through the Json response. It took me a good while to understand how to parse through it properly, but once I was comfortable with that I created my first DataFrame for this project. 

### Yelp and FourSquare
After struggling a decent bit with the CityBikes API requests, I did not have too many troubles here. Throughout my testing I remained cognisant of the limits that FourSquare and Yelp placed on my API keys, and was narrowly able to sneak under the cap. For the requests themselves, I decided on a radius of 500m rather than 1000m. Vienna is a fairly concentrated town with a lot of biking, and I wanted to mitigate excessive overlap. From the FourSquare requests I gathered the venue names, distance from the bike station, and the primary category. Yelp had a bit more information available, and so in addition to the previously mentioned items I also grabbed average rating and the number of reviews. Yelp and FourSquare each got their own separate DataFrames.

### Joining the DataFrames
I first standardized the columns between the Yelp and FourSquare DataFrames. After joining them together I merged with the City Bikes DataFrame. The result was a nice enough looking table, but of course the ratings and review columns had plenty of nulls. I did my best to remove the duplicates that I could, but slight variations in distances and coordinates made it practically impossible to do so with (my knowledge of) code.

### SQLite & Visualizations
Some histograms were made to determine the distributions of the number of bike station slots, average ratings from Yelp, as well as how the distance information differed between Yelp and FourSquare. I found that Yelp's returned venues are much more evenly distributed across the radius than FourSquare's, which are highly concentrated near the center of the circle. I also ported the finalized combined DataFrame into a local SQLite 3 database on my machine. 

## Results
Many of the locations sourced from Yelp had a highly variable number of reviews. This may cause inaccuracies in the results of the regression model. That is not even to mention that anything sourced from FourSquare completely lacks rating and review data, immediately disqualifying over a third of the rows in the final DataFrame. Simply not using these columns wouldn't be a big deal if there was plenty of other information to analyze, but there really wasn't. Only distance and primary category were left over. And this leads into another problem: distances and coordinates were inconsistent between Yelp and FourSquare when referring to the same venues. Even some of the names were spelt differently. Additionally, some small cafe franchises are clustered within the same area. This makes properly filtering out duplicates between Yelp and FourSquare unrealistic. I ended up filtering all null values from the final DataFrame- and that includes all of the FourSquare information.

I'm not all that impressed with the resulting 0.007 adjusted r-squared. Distance appeared to have a very minor negative correlation with the number of slots in a bike station, and this was expected. Interestingly, venues with higher average ratings were also negatively correlated with the number of slots in bike stations. Without more extensive data, no real insights can be drawn from this. The final independent variable that I modelled was the number of reviews, and this was slightly positively correlated with the number of slots. This outcome was also expected, as higher traffic areas are expected to see more people, and ultimately, more reviewers.

## Challenges 
I struggled a bit to get the hang of API calls for City Bikes, but trying to make sense of the Json responses was my biggest time sink in this project. Some of the functions for creating the DataFrames seemed to be getting away from me, but with a bit of organization they worked out alright. The modelling part of the project was by far the least time intensive, but it was also a little disappointing to see the rather poor outcome of all of that data collection/cleaning.

## Future Goals
Possibly searching for an alternative API to Yelp and FourSquare with more extensive and reliable information would have made the modelling part of this project much more interesting, although I'm not sure to what capacity that exists. For data visualization, I wanted to make a circular histogram to represent the radius based on several variables (source of the data, category of venue, etc.). I had no clue where to start with that but I'm sure something along those lines is possible with one of the Python packages. 