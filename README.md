# Final-Project-Statistical-Modelling-with-Python

## Table of Contents
*(CTRL+LMB) to open in new tab*
* [Data](/data/)
    *[PowerPoint](https://docs.google.com/presentation/d/1BEtuGz384fHrowA51wvoreLauoCn2Os_hlGUewdCNeU/edit?usp=sharing)
* Notebooks
    * [city_bikes](/notebooks/city_bikes.ipynb)
    * [yelp_foursquare_EDA](/notebooks/yelp_foursquare_EDA.ipynb)
    * [joining_data](/notebooks/joining_data.ipynb)
    * [model_building](/notebooks/model_building.ipynb)

## Project/Goals
* Organization
    * More pushes to GitHub.
    * Cleaner Markdown files.
    * More comments in code.
    * code readability over code conciseness.
* Develope Statistical Modeling Skills
    * While I can learn to use modules for statistical modeling quickly I don't quite have an intuitive comprehension of how to apply everything.
    * In other words I can do many things in the world of statistical modeling but I have a poor fundemental understanding of the things I am doing.
    * This naturally harms my confidence working with statistical modeling so I am aiming to build a stronger comprehension by the end of the project.

## Process
1. Skim over the assignment.md to get a good idea of what direction the project is going in. Fill out Goals in README.md and begin!

2. I think my first step was going to citybikes and looking around the map on their website to see what cities were available and feeling out how many stations were in the cities I was interested in. I wanted a city that had a ton of stations because it would be a challenge to handle all the data and it would provide deep data. I settled on Berlin.
3. I used the documentation to figure out how the API worked then completed the notebook.
4. On the next notebook. Foursquare has excellent documentation so it was really easy to figure out. After playing around with some requests I settled on the data I wanted and loaded it. Once it was loaded I did some EDA and had a lot of fun ploting details of the stations or places on a map of Berlin. I spent quite a bit of time developing a deeper context to the data. For example I looked up external data like bike traffic heatmaps, I used an API to generate a car traffic heatmap and I used google maps streetview at notable points on the plotted maps. I didn't spend much time with Yelp as I wasn't impressed by the data it provided.
5. I made my POI table have a column that is the lat and long of the station it's linked to. This made it easy to digest data relevant to a specific station and also made it really easy to join them in the joining_data notebook. Setting up the SQL database was also very straightforward.
6. Starting the modeling, my data was going to be actually applied so this is where I did the cleaning. Making sure distance is within my radius, lowercasing everything
7. I had a very depressing correlation heatmap and for a bit I wasn't confident at all that I could make a somewhat successful model but I started unpacking the data by adding binary columns tied to each station. For example 'near_restaurant' 0 if it isn't and 1 if it is. I zoned out and added about ~80 and I was happy to see some better colors on my new heatmap. After that I just prepped it to be fit, made an automatic backwards elim function and challenged myself to automate a forward selection function too.
8. After that I dissected the values of the summary, did a couple plots, and tweaked some things to try get a better model. I also experimented with a couple other regression models but neither did a better job.
9. My classification model isn't anything to write home about but it did the best it could and was interesting to analyze regardless. I tried getting it to predict whether a station would have lots of bikes or few. It had a very hard time.

## Results
**APIs**

During this project I found the foursquare API was miles better than the Yelp api in every respect. There are some fields that Yelp has that Foursquare doesn't (and vice versa) but foursquare returned I think nearly 8 times the POIs for each station and more relevant information like 'popularity' measured in foot traffic. On top of this I didn't think Yelp offered any really useful data that Foursquare didn't

**Model Results**

I had really low expectations for my model when I first started that notebook but after 'unpacking'/expanding a ton of data I was pleasantly surprised with the correlations I saw. As I said in my notebook I don't think the model would be valuable for making predictions but reflecting on the insights/patterns it picked up was really valuable. I do think with some time, some different reggressions to try, and more manipulation of the data I could get it to a point where it's accurate enough to make predictions well.

My stretch classification model was awful but was neat to analyze how it tried to make itself useful. I tried a couple different things to improve it but I didn't spend a ton of time as it seemed like the data would have to be quite a bit different for something meaningful to come out of it.

I think the main problem here is my dataset was filled with tons and tons of bike stations with 4 bikes and few with 20+. [Here's a plot showing this](https://media.discordapp.net/attachments/1063653051602321462/1069017577587757129/map_1.PNG?width=1153&height=661) (big and bright markers mean more bikes). You could see how much confusion / contradiction this led my poor models to in their residual plots.

Also I got way more comfortable working with statistics models and grew my fundemental understanding so I'm really happy about that.

## Challenges 
* I go over this in much greater detail in [yelp_foursquare_EDA](/notebooks/yelp_foursquare_EDA.ipynb) but I was very mindful of potential overlap in POIs. Basically I played around with the radius I pulled POIs from to balance data loss and data overlap. The root of this problem is both APIs don't return POIs ordered by how close they are to the origin.
* On this topic there's also the element of verticality. If you're in an area with a large many story office building there's potential to have the problem I stated above return only/mostly businesses in that office which are likely similiar or even the same category and neglecting other, different POIs in the radius. I approached this in the same way through experimenting with the radius and visualizing the response.
* I wanted to use Pandas as much as possible for database transformation, cleaning, and partitioning. I'm pretty confident with my SQL queries and my head will often default to expressing what I want to do with a database in a SQL query. I wanted to strengthen my Pandas so I decided to not allow myself to use SQL as a 'crutch' and find ways to do what I wanted in Pandas.
* I set a few challenges for myself as well.
    * Using custom classes to increase readability (I really liked how this turned out and also let me construct code quicker once set up). I left a comparison solution where I didn't use a custom class in city_bikes and my classes are in yel_foursquare.
    * Automating Forward selection for picking model columns.
    * I had the idea to compare car traffic to bike traffic and it was tricky finding an api that was free with decent documentation to provide this. I ended up going with Tom Tom and once I got everything working it was fun to play around with and I was impressed with what it had to offer.

## Future Goals

When my automatic forward selection function ran for the first time I was very happy. Then I was immediatley bummed out when I saw that forward selection performs so poorly when it has to handle tons of independent, not too meaningful variables. The first R value it grabbed and brought forward was the highest selection. From there it steadily declined.

When this happened though I got a couple ideas for how I could make it more relevant / useful with large sets of variables.

The one I most wanted to explore was different ways I could integrate my automatic backwards elimination function to work in harmony with my forward selection function.

I only have rough ideas but I think the best is that I could have my final output from the backwards elim feed into the forward selection as a starting point to see if there's any moves it could make rising the r value at the expense of a p value.