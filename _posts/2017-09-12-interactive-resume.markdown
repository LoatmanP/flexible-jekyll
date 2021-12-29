---
layout: post
title: Building an Interactive Resume with Plotly
date: 20121-12-29 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: interactive_resume.png # Add image post (optional)
tags: [Productivity, Resume, Interactive Resume, CV, Website] # add tag
---

## Import the required Python libraries

```python
import plotly.express as px
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
```

## Build the Interacticve Resume

```python
present = datetime.today().strftime('%Y-%m-%d') #Use if this is your current job

#Create a pandas dataframe for each job, Task can be company or job title--it will become a tooltip when cursor hovers over era of timeline
df = pd.DataFrame([
    dict(Task="job_1", Start='yyyy-MM-dd', Finish='yyyy-MM-dd', Resource=1),
    dict(Task="job_2", Start='yyyy-MM-dd', Finish='yyyy-MM-dd', Resource=1),
    dict(Task="job_3", Start='yyyy-MM-dd', Finish='yyyy-MM-dd', Resource=1),
    dict(Task="job_4", Start='yyyy-MM-dd', Finish=present, Resource=1) # Finish = present denotes current job
    #keep adding jobs until your timeline is complete!

])

fig = px.timeline(df, x_start="Start", x_end="Finish", y="Resource", color="Start",
hover_name = "Task",  width=900,
                  color_discrete_sequence=px.colors.diverging.delta
                  , opacity=.7
                  , title="<b>Interactive Resume: Work History</b>" #change the title of your interactive resume
                    ,hover_data={"Start": False,
                              "Finish": False,
                              "Task": False,
                              "Resource": False}

                  )

fig.update_layout(

        showlegend=False 
        ,yaxis_visible=False 
        ,yaxis_showticklabels=False 
        ,paper_bgcolor="#FFFFFF"
        ,plot_bgcolor = "#FFFFFF"
        ,xaxis = dict(
        showgrid=False
        ,rangeslider_visible=False
        ,side ="bottom"
        ,tickmode = 'array'
        ,ticks="outside"
        ,zeroline=True
        ,showline=True
        ,ticklen=20
        ,tickfont=dict(
            family='Serif',size=22,color="#100700")),
        hoverlabel = dict(
            bgcolor="white",
            font_size=16,
            font_family="Rockwell")

)

fig.update_xaxes(range=['df.Start.min()', present]) #create the timeline range of your interactive resume

#current or last job details, here Start and Finsih need to be in 'yyyy-MM-dd' format denotes start time of current/last job
fig.add_annotation(ax=0, ay=-200, #ay to change the height distance of text away from the timeline
                   x = pd.Timestamp('Start') + (pd.Timestamp(present) - pd.Timestamp('Start'))/2, y = 1.4, #center the text 
            text="your job title or company you work for", #text that will appear above the era of the timneline
            arrowhead=6,
           arrowsize=2,
           arrowwidth=1

                   )

#add another job, Start and Finsih need to be in 'yyyy-MM-dd' format denotes start time of job
fig.add_annotation(ax=0, ay=-100,
                   x = pd.Timestamp('Start') + (pd.Timestamp('Finish') - pd.Timestamp('Start'))/2, y = 1.4,
            text="your job title or company you work for",
            arrowhead=6,
           arrowsize=2,
           arrowwidth=1

                   )
#add another job, Start and Finsih need to be in 'yyyy-MM-dd' format denotes start time of job
fig.add_annotation(ax=0, ay=-150,
                   x = pd.Timestamp('Start') + (pd.Timestamp('Finish') - pd.Timestamp('Start'))/2, y = 1.4,
            text="your job title or company you work for",
            arrowhead=6,
           arrowsize=2,
           arrowwidth=1

                   )
#add another job, Start and Finsih need to be in 'yyyy-MM-dd' format denotes start time of job
fig.add_annotation(ax=0, ay=-75,
                   x = pd.Timestamp('Start') + (pd.Timestamp('Finish') - pd.Timestamp('Start'))/2, y = 1.4,
            text="your job title or company you work for",
            arrowhead=6,
           arrowsize=2,
           arrowwidth=1
 
 #keep adding annotations until your timeline is complete
 
 fig.show() #check out your interactive reusme!
```
## Publish it to your website using [Plotly Chart Studio](https://chart-studio.plotly.com/)

### Step 1: Create or login to Plotly Chart Studio 

![signup]({{site.baseurl}}/assets/img/step_1_studio.png)

### Step 2: Install Chart Studio
```console
pip install chart_sutdio
```
or 
```console
$ sudo pip install chart_studio
```
### Step 3: Upload your interactive resume to Chart Studio

```python
 import chart_studio
 username = 'your user name'
 api_key = 'your API key'
 chart_studio.tools.set_credentials_file(username=username, api_key=api_key)
 py.plot(dig, filename= 'fig',auto_open=True)
```
### Step 4: Share your figure, send it as a link, embed it into your own website
Your interactive resume will now have a link that you can share

Embed that link into an iframe
```html
<iframe width="900" height="800" frameborder="0" scrolling="no" src="the link to your figure here"></iframe>
```
Below is my interactive resume using the code above
<iframe width="900" height="400" frameborder="0" scrolling="no" src="//plotly.com/~pal1234/3.embed"></iframe>



