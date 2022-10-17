# Project 1: Shark Attacks
### Author: Joan Roca Jovani

<img src="images/Screenshots/babyshark.png" width="300" height="400">
## Introduction
For the first project of the bootcamp in data analytics I will analyze a table consisting in all shark attacks reported ever.
This is a particularly interesting topic for me because when I was a child I remember watching an episode of CSI: Miami where a victim was killed by a shark.
From that moment I have always had a fear of sharks and when in 2020 I visited Miami I saw sharks from a boat and I was afraid as hell.

First I cleaned the table since it had over 25k rows and over 20 columns and many of them were not of any use.

When the dataframe was clean I proceeded to analyze it trying to prove some hypothesis and finally, I tried to find the most dangerous spot to perform an Ironman (which is a dream of mine).

The hypothesis are the following:

__1. I saw a documentary about surf and it said that the people practising is increasing year by year. I want to prove that the % of shark attacks when surfing has increased overtime.__

__2. I recently saw an article that said:__ *In Florida, sharks typically move inshore and north in the spring and summer, and offshore and south in fall and winter months. This pattern explains why shark activity is at its peak in Florida waters during April through October, which coincidentally, is also the time period that humans are more likely to be in the water.* __I would like to prove that this is accurate or not.__

__3. I want to prove that Florida is the most dangerous state in the US when it comes to sharks. Going back to the episode of CSI: Miami and the time that I saw the sharks in Miami, i have always seen Florida as the place with most sharks, I would like to prove if that is right.__

__4. I want to prove that surfing is the activity in which more shark attacks have happened.__

__5. I want to test if the 50% of the shark attacks happen to people between 18 and 30 years old.__

__6. This is not a hypothesis but I would like to find the most dangerous places to do an Ironman, which has been a goal of mine since I was a kid. This is important because I really want to to an Ironman but I don't want to put myself in a situation where I could get bitten by a shark.__


   .
## Cleaning the dataframe
Initially, the dataframe had a size of (25723, 24) and it had the following null values.
<img src="images/Screenshots/Initial%20Null%20Values.png" width="300" height="400">

We could see that there are a lot of null values in the dataframe so maybe there are many full null rows or rows with just a few non null values, so I dropped them.
I also dropped all the columns i didn't want for the analysis: 
["Unnamed: 22","Unnamed: 23","Investigator or Source", "pdf", "href formula", "href", "Case Number.1", "Case Number.2", "original order", "Name", "Location", "Injury", "Species", "Time"]

The result of the previous cleaning was this:
<img src="images/Screenshots/First%20Clean%20Null%20Values.png" width="150" height="200">

__Column Year Fix__
For this column I droped the 2 null values and converted from float to integer.
And then I only kepts the rows where the year was higher than 1960 because the records of before where not accurate.

__Column Month Fix__
I created a new column called Month so I can compare the number of attacks each month.
To do this i used the following code:
```df["Month"] = df["Case Number"].str.extract('(\.\d{2}\.)')```
```df["Month"] = df["Month"].str.extract('(\d{2})')```

Finally I droped the null values and converted the values into integers.

__Column Sex Fix__
For this one I only wanted values that were M or F so I transformed the typos into correct spelling and dropped the unknowns.

__Column Fatal (Y/N) Fix__
I did the same as with sex.

__Column Type Fix__
I did something similar here, I dropped the questionable ones and try to fit the typos into the correct categories so i just ended up with 5 categories and no null values.

__Column Activity Fix__
This was a tough one because there were 733 unique values.
I only wanted 5 categories: Surfing, Swimming, Water Sports, Fishing, Accident and Other.
To fix it I used multiple lines locating specific words and renaming the activity into one of the previous groups. With that I ended up with 107 unique values and then I put all values that were not into the main categories into other.

__I deleted Case Number and Date columns because i did not need them anymore since I had year and month.__

__Column Age Fix__
Finally I wanted to fix the age column to reduce unique values and convert them into integers so I could do a proper histogram later.
To do that I used the following code:
```search = []    
for values in df['Age']:
    try:
        search.append(re.search('\d+', values))
    except TypeError:
        search.append(np.nan)

df['New_Age'] = search
```

```search_2 = []    
for value in df['New_Age']:
    try:
        search_2.append(value.group())
    except AttributeError:
        search_2.append(np.nan)

df["New_Age"] = search_2
```
Then I turned into Int64 since it had null values and I couldn't turn it into normal integers.

### Final Result:
<img src="images/Screenshots/FinalDtypes.png" width="150" height="200">
<img src="images/Screenshots/FinalNullValues.png" width="150" height="200">

<img src="images/Screenshots/finaltable.png" width="600" height="200">

## Visualizing and Hypothesis

__Atacks overtime:__
We can see that the number of attacks has increased overtime, this might be because more people is going to the beach or because the older data is less reliable than the newer data.
We can also see that even if the number of attacks has increased the proportion of fatal attacks has decreased, this might be because of better medical infrastructure.
<img src="images/Screenshots/Attacksovertime.png" width="500" height="300">

__Hypothesis 1: The % of attacks when surfing has increased overtime.__
It is crystal clear that the % of attacks when surfing has increased from the 1960s to these days. The reason I believe it to be is that as I stated in the intro of the hypothesis surf is growing sport and more poeple is practising it.

<img src="images/Screenshots/Attacksovertimeactivity.png" width="500" height="300">

__Hypothesis 2: The shark season in Florida is between April and October.__
To get the graph i created a sub dataframe where area == Florida and then a bar chart with the months.
And after looking at the data I can agree with the hypothesis. There is a big increase from March to April, a big drop from October to November and September is the month with the most attacks.
Furthermore, none of the months in the shark season has fewer attacks than any of the rest of the months.

<img src="images/Screenshots/Attacksfloridamonth.png" width="500" height="300">

__Hypothesis 3: Florida is the most dangerous state in the US when it comes to sharks.__
It is crystal clear that Florida is by far the most dangerous state in the United States when it comes to sharks. This is because there are many of them in the Caribbean waters.

<img src="images/Screenshots/Attacksstates.png" width="500" height="300">

__Hypothesis 4: Surfing is the activity with the most shark attacks.__
It is confirmed that surfing is the activity with the most attacks, but just for men. Women suffer more attacks when swimming than when surfing, which is an interesting insight.
Also interesting to see that very few women suffer attacks when fishing, which may be because the practise is more extended among men.

<img src="images/Screenshots/Attacksactivity.png" width="500" height="300">

__Hypothesis 5: 50% of the attacks are suffered by people between 18 and 30.__
The hypothesis is wrong, but some ideas were right. The 25th percentile is exactly 18 years old which was what I expected, the issue is that the 75th percentile is 37 years old, which is over what I expected.
If we analyze the age distribution by activities we can see that even if the attacks when surfing and swimming decrease after 30 years old, fishing and water sports decrease a little bit later.
Another thing that I think should be said is that even if the number of attacks goes down after 30 years old, the number of fatal accidents does not decrease as much, this might be because younger people have more chances of surviving.
<img src="images/Screenshots/Attacksage.png" width="500" height="300">
<img src="images/Screenshots/ageactivity.png" width="500" height="300">
<img src="images/Screenshots/Boxplotattacksage.png" width="500" height="300">
<img src="images/Screenshots/agedescriptive.png" width="200" height="200">

__Hypothesis 6: Finding the most dangerous places to swim so I can avoid them when doing my Ironman.__
I created a new dataframe where the only activity was swimming and the only sex was male because this will be the situation I will face.
Then I plotted a histogram of ages and I could see that people my age (23) are the one of the ones suffering more attacks, not great news so far.
When plotting the countries it is crystal clear that the 3 places to avoid are USA, Australia and South Africa, so I will be sticking to Europe and the Mediterranean.
If we took a closer look at the US we can see that the main states to avoid are Florida, South Carolina, Hawaii (unfortunate since the most unique Ironaman is there), North Carolina, Texas and California.
So the day that I decide to do the Ironman, I certainly know where not to go.
<img src="images/Screenshots/swimage.png" width="500" height="300">
<img src="images/Screenshots/swimcountries.png" width="500" height="300">
<img src="images/Screenshots/swimstates.png" width="500" height="300">

## Conclusion
It has been the first time that I have worked with Python in such a project and it has been very challenging. Some hypothesis were accurate and some were not but the most important thing was to get my hands on the Python for a analytics project for the first time and start analyzing data with it.