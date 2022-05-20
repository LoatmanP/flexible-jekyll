---
layout: post
title: Boston Covid Dashboard
date: 2022-05-20 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: coronavirus_image_thin.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Covid-19, Coronavirus, Boston, Suffolk, Middlesex, Wastewater] #add tag
---


Covid-19 Dashboard for Middlesex and Suffolk counties in Massachusetts. 

<!DOCTYPE html>
<html>
  <head>
    <title>Streamlit for Geospatial</title>
    <style type="text/css">
      html {
        overflow: auto;
      }
      html,
      body,
      div,
      iframe {
        margin: 0px;
        padding: 0px;
        height: 100%;
        border: none;
      }
      iframe {
        display: block;
        width: 100%;
        border: none;
        overflow-y: auto;
        overflow-x: hidden;
      }
    </style>
  </head>
  <body>
    <iframe
      src="https://share.streamlit.io/loatmanp/covid_web_app/main/covid_web_app.py"
      frameborder="0"
      marginheight="0"
      marginwidth="0"
      width="100%"
      height="100%"
      scrolling="auto"
    >
    </iframe>
  </body>
</html>
