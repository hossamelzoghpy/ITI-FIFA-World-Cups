# FIFA-World-Cups
By using python and their libraries , Power Bi I created visualizations on fifa world cups dataset
<br>

### By:
 - Hossam Asaad Elzoghpy
<p align="center">
		<img  src="https://user-images.githubusercontent.com/51371174/189508562-ad6458d9-abc7-4a9d-a620-66f69d108351.png">
	</a>

## 🎯 Contributors

<a href="https://github.com/IIAndrewII/ITI-Covid_19-project/graphs/contributors">
<img src="https://contrib.rocks/image?repo=hossamelzoghpy/dockerfile-task" />
</a>

## 🍀 Sponsors

This project exists thanks to:  **information technology institute(iti)** and **pluralsight**

</p>
<p align="center">
		<img width="10%" src="https://user-images.githubusercontent.com/81884621/188256478-03c82499-b8dc-425c-a160-5ebab2cd2350.png">
	</a>
		<img width="20%" src="https://seekvectorlogo.com/wp-content/uploads/2022/02/pluralsight-vector-logo-2022.png">
	</a>

</p>

## Introduction

The FIFA World Cup, often simply called the World Cup, is an international association football competition contested by the senior men's national teams of the members of the Fédération Internationale de Football Association (FIFA), the sport's global governing body. The championship has been awarded every four years since the inaugural tournament in 1930, except in 1942 and 1946 when it was not held because of the Second World War. The current champions are France, who won their second title at the 2018 tournament in Russia.
<hr>

	
[<img src="https://user-images.githubusercontent.com/51371174/189513327-fb0f9740-caaa-48e5-99e2-6222944bcddd.jpg">](https://www.youtube.com/watch?v=41oRqH65Eb8&ab_channel=RptimaoTV2)
	


## Context
The FIFA World Cup is a global football competition contested by the various football-playing nations of the world. It is contested every four years and is the most prestigious and important trophy in the sport of football.

## Content
The World Cups dataset show all information about all the World Cups in the history, while the World Cup Matches dataset shows all the results from the matches contested as part of the cups.

## Acknowledgements
This data is courtesy of the FIFA World Cup Archive website.

## Tools

- [Jupyter Notebook](https://jupyter.org/install)
- [Power BI](https://powerbi.microsoft.com/en-us/)
- [GIT](https://www.youtube.com/watch?v=ACOiGZoqC8w&list=PLDoPjvoNmBAw4eOj58MZPakHjaO3frVMF&ab_channel=ElzeroWebSchool)
- [How To Create Readme File](https://www.makeareadme.com/)

## Data
<p >
		World Cups
		<img src="https://user-images.githubusercontent.com/51371174/189510966-7a1dc99e-e789-40eb-a0f3-c3b62b8d6da6.png">
		<br>
		World Cup Players
		<img src="https://user-images.githubusercontent.com/51371174/189511026-b5fa156a-0502-4b41-81a3-ebdfa76c108d.png">
		<br>
		World Cup Matches
		<img src="https://user-images.githubusercontent.com/51371174/189511043-6a460d77-e2ac-4422-a175-a00a17f493b8.png">
	
</p>

## Relation Model

	
<img src="https://user-images.githubusercontent.com/51371174/189513604-88350085-5eaf-4160-aa7f-68101093dc83.png">


## Usage

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
import plotly as py
import cufflinks as cf
from plotly.offline import iplot
py.offline.init_notebook_mode(connected=True)
cf.go_offline()
```
## Important EDA
### This Graph show the goal frequency by year
```python
world_cups.plot(kind = "line",x="Year",y = "GoalsScored")
plt.show()
```
<img src="https://user-images.githubusercontent.com/51371174/189511721-a0a4af3c-e3ce-4583-b60e-04c83d2b4d35.png">
<br>

### This Graph show the Countries winne, runner up, third

```python
winner= world_cups['Winner'].value_counts()
runnerUp= world_cups['Runners-Up'].value_counts()
thirdPlace=world_cups['Third'].value_counts()
winnerPlaces=pd.concat([winner,runnerUp,thirdPlace],axis=1)
winnerPlaces.fillna(0,inplace=True)
winnerPlaces=winnerPlaces.astype(int)
winnerPlaces.iplot(kind='bar',xTitle = 'Teams',yTitle='Count',title='FIFA World Winner')
```
<img src="https://user-images.githubusercontent.com/51371174/189511816-3d9506dd-1652-4785-985b-23565221b046.png">

### This Graph show the Distribution of minutes in which goals occur

```python
#minutes in which he made the goal
def min_goals(text):
    events = text.split("' ")
    goals = [ e.replace("'","") for e in events if e and e[0] == 'G']
    return [int(g[1:]) for g in goals]

plt.hist(players['Event'].fillna("").map(min_goals).sum())
plt.ylabel('probability')
plt.xlabel('minute')
plt.title('Distribution of minutes in which goals occur')
```
<img src="https://user-images.githubusercontent.com/51371174/189511888-3392a025-7fb4-4813-92c6-41c8686ccb22.png">


### This Graph show the Matches With Highest Number Of Attendance

```python
top10M = matches.sort_values(by = 'Attendance',ascending = False)[:10]
top10M['VS'] = top10M['Home Team Name'] + " VS " + top10M['Away Team Name'] 
plt.figure(figsize = (12,9))
ax = sns.barplot(y = top10M['VS'],x = top10M['Attendance'])
sns.despine(right = True) 

plt.ylabel('Teams')
plt.xlabel('Attendance')
plt.title('Matches With Highest Number Of Attendance')


for i ,s in enumerate(top10M['Stadium'] + ",  Date: " + top10M['Datetime']):
    ax.text(2000,i,s,fontsize = 12,color = 'white')
plt.show()
```
<img src="https://user-images.githubusercontent.com/51371174/189511994-8d445dcc-e698-4fe8-8699-468ee3bb6df2.png">

### This Graph show the Home Team VS Away Team

```python
def func(matches):
    if matches['Home Team Goals'] > matches['Away Team Goals']:
        return "Home Team Win"
    if matches['Home Team Goals'] < matches['Away Team Goals']:
        return "Away Team Win"
    return "Draw"
matches['OutComes'] = matches.apply(lambda x: func(x), axis=1)
mat = matches['OutComes'].value_counts()
plt.figure(figsize = (6,6))
mat.plot.pie(autopct = '%1.0f%%',colors = sns.color_palette('winter_r'), shadow = True)
c = plt.Circle((0,0),0.4,color = 'white')
plt.gca().add_artist(c)
plt.title('Results Of: Home Team VS Away Team')
plt.show()

```
<img src="https://user-images.githubusercontent.com/51371174/189512092-550b9637-41c5-40c5-aabd-258c5d244fc5.png">

## Dashboard Using Power BI
- [Link Dashboard](https://app.powerbi.com/links/V90kXP9s_Y?ctid=ce1a9a08-aeda-4331-ae1b-7bdefa6b2dc4&pbi_source=linkShare&bookmarkGuid=ab340f74-5cb6-4be1-b0e4-38ea91914b95) 
<img src="https://user-images.githubusercontent.com/51371174/189515625-7b415c09-1af0-487b-b219-311cab4fcd10.png">

## What's News:
### Building a Model to Predict who will win the next World Cup

