---
title: How to build your own AI personal assistant using Python
date: 2020-10-10 10:10:10 Z
permalink: "/blog/pythonPA"
categories:
- PythonDataScience
tags:
- learning
summary: An AI personal assistant is a piece of software that understands verbal or written commands and completes task assigned by the client. It is an example of weak AI that is it can only execute and perform quest designed by the user.
image: "/img/G1.png"
author: Girish Godage
layout: posts
prevurl: ""
nexturl: ""
discussion_id: 2019-10-10-PythonPA
---

## How to build your own AI personal assistant using Python

*An AI personal assistant is a piece of software that understands verbal or written commands and completes task assigned by the client. It is an example of weak AI that is it can only execute and perform quest designed by the user.*

![image info](/img/G1.png)

Want to build your own personal AI assistant like Apple Siri, Microsoft Cortana and Google assistant?

You can check out this Article to build one in a few simple steps!

With the python programming language, a script most commonly used by the developers can be used to build your personal AI assistant to perform task designed by the users.

Now, let’s write a script for our personal voice assistant using python.

## Skills:

The implemented voice assistant can perform the following task it can **open YouTube, Gmail, Google chrome and stack overflow. Predict current time, take a photo, search Wikipedia to abstract required data, predict weather in different cities, get top headline news from Times of India and can answer computational and geographical questions too**.

The following queries of the voice assistant can be manipulated as per the users need.

## Packages required:

To build a personal voice assistant it’s necessary to install the following packages in your system using the pip command.

1) **Speech recognition** — Speech recognition is an important feature used in house automation and in artificial intelligence devices. The main function of this library is it tries to understand whatever the humans speak and converts the speech to text.
   
2) **pyttsx3** — pyttxs3 is a text to speech conversion library in python. This package supports text to speech engines on Mac os x, Windows and on Linux.

3) **wikipedia** — Wikipedia is a multilingual online encyclopedia used by many people from academic community ranging from freshmen to students to professors who wants to gain information over a particular topic. This package in python extracts data’s required from Wikipedia.
   
4) **ecapture** — This module is used to capture images from your camera
   
5) **datetime** — This is an inbuilt module in python and it works on date and time
   
6) **os** — This module is a standard library in python and it provides the function to interact with operating system

7) **time** — The time module helps us to display time
   
8) **Web browser** — This is an in-built package in python. It extracts data from the web
   
9)  **Subprocess** — This is a standard library use to process various system commands like to log off or to restart your PC.
    
10) **Json**- The json module is used for storing and exchanging data.
    
11) **requests**- The request module is used to send all types of HTTP request. Its accepts URL as parameters and gives access to the given URL’S.
    
12) **wolfram alpha** — Wolfram Alpha is an API which can compute expert-level answers using Wolfram’s algorithms, knowledge base and AI technology. It is made possible by the Wolfram Language

## Implementation:
*Import the following libraries*

```code

import speech_recognition as sr
import pyttsx3
import datetime
import wikipedia
import webbrowser
import os
import time
import subprocess
from ecapture import ecapture as ec
import wolframalpha
import json
import requests
```

## Setting up the speech engine:

The pyttsx3 module is stored in a variable name engine.
**Sapi5** is a **Microsoft Text to speech engine used for voice recognition**.

The voice Id can be set as either 0 or 1,
- 0 indicates Male voice
- 1 indicates Female voice

```code
engine=pyttsx3.init('sapi5')
voices=engine.getProperty('voices')
engine.setProperty('voice','voices[0].id')

```

Now define a function **speak** which converts text to speech. The speak function takes the text as its argument,further initialize the engine.

**runAndWait**: This function Blocks while processing all currently queued commands. It Invokes callbacks for engine notifications appropriately and returns back when all commands queued before this call are emptied from the queue.

```
def speak(text):
    engine.say(text)
    engine.runAndWait()
```

## Initiate a function to greet the user:

Define a function wishMe for the AI assistant to greet the user.

The **now().hour** function abstract’s the hour from the current time.

If the hour is greater than zero and less than 12, the voice assistant wishes you with the message “Good Morning”.

If the hour is greater than 12 and less than 18, the voice assistant wishes you with the following message “Good Afternoon”.

Else it voices out the message “Good evening”

```
def wishMe():
    hour=datetime.datetime.now().hour
    if hour>=0 and hour<12:
        speak("Hello,Good Morning")
        print("Hello,Good Morning")
    elif hour>=12 and hour<18:
        speak("Hello,Good Afternoon")
        print("Hello,Good Afternoon")
    else:
        speak("Hello,Good Evening")
        print("Hello,Good Evening")
```

## Setting up the command function for your AI assistant :

Define a function takecommand for the AI assistant to understand and to accept human language. The microphone captures the human speech and the recognizer recognizes the speech to give a response.

The exception handling is used to handle the exception during the run time error and,the recognize_google function uses google audio to recognize speech.

```
def takeCommand():
    r=sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio=r.listen(source)

        try:
            statement=r.recognize_google(audio,language='en-in')
            print(f"user said:{statement}\n")

        except Exception as e:
            speak("Pardon me, please say that again")
            return "None"
        return statement

print("Loading your AI personal assistant G-One")
speak("Loading your AI personal assistant G-One")
wishMe()
```

## The Main function:
The main function starts from here,the commands given by the humans is stored in the variable **statement**.

```
if __name__=='__main__':


    while True:
        speak("Tell me how can I help you now?")
        statement = takeCommand().lower()
        if statement==0:
            continue
```

If the following trigger words are there in the statement given by the users it invokes the virtual assistant to speak the below following commands.

```
if "good bye" in statement or "ok bye" in statement or "stop" in statement:
            speak('your personal assistant G-one is shutting down,Good bye')
            print('your personal assistant G-one is shutting down,Good bye')
            break
```

## Skill 1 -Fetching data from Wikipedia:

The following commands helps to extract information from wikipedia. The **wikipedia.summary()** function takes two arguments, the statement given by the user and how many sentences from wikipedia is needed to be extracted is stored in a variable **result**.

```
if 'wikipedia' in statement:
            speak('Searching Wikipedia...')
            statement =statement.replace("wikipedia", "")
            results = wikipedia.summary(statement, sentences=3)
            speak("According to Wikipedia")
            print(results)
            speak(results)

```
## Skill 2 -Accessing the Web Browsers — Google chrome , G-Mail and YouTube:

The web browser extracts data from web. The **open_new_tab** function accepts **URL** as a parameter that needs to be accessed.

The **Python time sleep function** is used to add delay in the execution of a program. We can use this function to halt the execution of the program for given **time** in seconds.

```
elif 'open youtube' in statement:
            webbrowser.open_new_tab("https://www.youtube.com")
            speak("youtube is open now")
            time.sleep(5)

        elif 'open google' in statement:
            webbrowser.open_new_tab("https://www.google.com")
            speak("Google chrome is open now")
            time.sleep(5)

        elif 'open gmail' in statement:
            webbrowser.open_new_tab("gmail.com")
            speak("Google Mail open now")
            time.sleep(5)

```

## Skill 3 -Predicting time:

The current time is abstracted from **datetime.now()** function which displays the hour, minute and second and is stored in a variable name **strTime**.

```
elif 'time' in statement:
            strTime=datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"the time is {strTime}")
```

## Skill 4 -To fetch latest news:
If the user wants to know the latest news , The voice assistant is programmed to fetch top headline news from Time of India by using the web browser function.

## Skill 5 -Capturing photo:

**The ec.capture()** function is used to capture images from your camera. It accepts 3 parameter.

**Camera index** — The first connected webcam will be indicated as index 0 and the next webcam will be indicated as index 1.

**Window name** — It can be a variable or a string. If you don’t wish to see the window, type as False.

**Save name** — A name can be given to the image and if you don’t want to save the image, type as false.

```
elif 'news' in statement:
            news = webbrowser.open_new_tab("https://timesofindia.indiatimes.com/home/headlines”)
            speak('Here are some headlines from the Times of India,Happy reading')
            time.sleep(6)

        elif "camera" in statement or "take a photo" in statement:
            ec.capture(0,"robo camera","img.jpg")
```

## Skill 6-Searching data from web:

From the **web browser** you can search required data by passing the user statement (command) to the **open_new_tab()** function.

**User**: Hey G-One, please search images of butterfly

The Voice assistant opens the google window & fetches butterfly images from web.

```
elif 'search'  in statement:
            statement = statement.replace("search", "")
            webbrowser.open_new_tab(statement)
            time.sleep(5)	
```

## Skill 7- Setting your AI assistant to answer geographical and computational questions:

Here we can use a third party API called **Wolfram alpha API** to answer computational and geographical questions.It is made possible by the Wolfram Language. The **client** is an instance (class) created for wolfram alpha. The **res** variable stores the response given by the wolfram alpha.

```
elif 'ask' in statement:
            speak('I can answer to computational and geographical questions  and what question do you want to ask now')
            question=takeCommand()
            app_id="Paste your unique ID here "
            client = wolframalpha.Client('R2K75H-7ELALHR35X')
            res = client.query(question)
            answer = next(res.results).text
            speak(answer)
            print(answer)
```

To access the wolfram alpha API an unique App ID is required which can be generated by the following ways:

1. Login to the official page of wolfram alpha and create an account if you do not possess one.

![image info](/img/python/datascience/img/6/1.png)

2. Sign in using your wolfram ID

![image info](/img/python/datascience/img/6/2.png)


3. Now you will view the homepage of the website. Head to the account section in the top right corner where you see your email. In the drop down menu, select the My Apps (API) option.

![image info](/img/python/datascience/img/6/3.png)

4. You will see this following window, now click Get APP_ID button

![image info](/img/python/datascience/img/6/4.png)

5. Now you will get the following dialog box, give a suitable name and description and click the App ID button, an App ID will be generated and this is an unique ID. Using the App Id use can access the Wolfram alpha API.

![image info](/img/python/datascience/img/6/5.png)


**Human:** Hey G-One ,what is the capital of California?

**G-One Voice assistant:** Sacramento, United States of America


## Skill 8- Extra features:

It would be interesting to program your AI assistant to answer the following questions like what it can and who created it,isn't it?

```
elif 'who are you' in statement or 'what can you do' in statement:
            speak('I am G-one version 1 point O your personal assistant. I am programmed to minor tasks like'
                  'opening youtube,google chrome, gmail and stackoverflow ,predict time,take a photo,search wikipedia,predict weather' 
                  'In different cities, get top headline news from times of india and you can ask me computational or geographical questions too!')


        elif "who made you" in statement or "who created you" in statement or "who discovered you" in statement:
            speak("I was built by Girish")
            print("I was built by Girish")
```
## Skill 9- To forecast weather:

Now to program your AI assistant to detect weather we need to generate an API key from Open Weather map.

Open weather map is an online service which provides weather data. By generating an API ID in the official website you can use the APP_ID to make your voice assistant detect weather of all places whenever required. The necessary modules needed to be imported for this weather detection is json and request module.

The **city_name variabl**e takes the command given by the human using the **takeCommand()** function.

The **get** method of **request** module returns a **response** object. And the **json** methods of response object converts json format data into python format.

The variable **X** contains list of nested dictionaries which checks whether the value of ‘COD’ is 404 or not that is if the city is found or not.

The values such as temperature and humidity is stored in the main key of variable **Y**.

```
elif "weather" in statement:
            api_key="Apply your unique ID"
            base_url="https://api.openweathermap.org/data/2.5/weather?"
            speak("what is the city name")
            city_name=takeCommand()
            complete_url=base_url+"appid="+api_key+"&q="+city_name
            response = requests.get(complete_url)
            x=response.json()
            if x["cod"]!="404":
                y=x["main"]
                current_temperature = y["temp"]
                current_humidiy = y["humidity"]
                z = x["weather"]
                weather_description = z[0]["description"]
                speak(" Temperature in kelvin unit is " +
                      str(current_temperature) +
                      "\n humidity in percentage is " +
                      str(current_humidiy) +
                      "\n description  " +
                      str(weather_description))
                print(" Temperature in kelvin unit = " +
                      str(current_temperature) +
                      "\n humidity (in percentage) = " +
                      str(current_humidiy) +
                      "\n description = " +
                      str(weather_description))

```

**Human:** Hey G-One ,I want to get the weather data

**G-One:** What is the city name?

**Human:** Himachal Pradesh

**G-One:** Temperature in kelvin unit is 301.09 , Humidity in percentage is 52 and Description is light rain.

## Skill 10- To log off your PC:

The subprocess.call() function here is used to process the system function to log off or to turn off your PC. This invokes your AI assistant to automatically turn off your PC.

```
    elif "log off" in statement or "sign out" in statement:
            speak("Ok , your pc will log off in 10 sec make sure you exit from all applications")
            subprocess.call(["shutdown", "/l"])
			
    time.sleep(3)

```

Hurray , We have finally built our own AI voice assistant . Further you can still add more functionalities to your AI voice assistant to perform more task.

Check out my GitHub profile for code:

https://github.com/girishgodage/ai-personal-assistant

Happy Coding !!
