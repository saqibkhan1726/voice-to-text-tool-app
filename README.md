# voice-to-text-tool-app
import datetime
import random
from tkinter import Image
import webbrowser
import os
import pyttsx3
import speech_recognition as sr
import pyaudio
import wikipedia
import smtplib
import pyjokes
import requests
import subprocess
import ctypes
from decouple import config
from email.message import EmailMessage
import time
import ecapture as ec
from pywikihow import WikiHow, search_wikihow
import pywhatkit
import pywhatkit as kit
import pyaudio
from vosk import Model, KaldiRecognizer
import sys
import PyPDF2
import pyautogui
import wolframalpha

engine = pyttsx3.init('sapi5')
voices = engine.getProperty("voices")
x = random.randint(0, 1)
print(voices[x].id)
engine.setProperty("voice", voices[x].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wish_me():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour < 12:
        speak("Good Morning!")
    elif 12 <= hour < 18:
        speak("Good Afternoon!")
    else:
        speak("Good Evening! sir")
        print("Computer: Good Evening! sir")
    if x == 0:
        speak(f"I am your assistant Jarvis. How can i help you?\n")
    elif x == 1:
        speak("I am your assistant Zira. How can i help you?\n")
    else:
        speak("I am your assistant Zira. How can i help you?\n")


def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...\n")
        r.pause_threshold = 1
        audio = r.listen(source, 0, 5)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language="en-in")
        print(f"User said: {query}\n")

    except Exception as er:
        print(er)
        print("Sorry I can't hear that.")
        return "None"
    return query.lower()

def GoogleSearch(term):
    query = term.replace("jarvis","")
    query = query.replace("what is", "")
    query = query.replace("how to", "")
    query = query.replace("what do you mean by", "")
    query = query.replace("who is the", "")
    

    Query = str(term)
    pywhatkit.search(Query)

    if "how to" in Query:
        m_res = 1
        how_to_func = search_wikihow(query=Query,max_results=m_res)
        assert len(how_to_func) == 1
        how_to_func[0].print()
        speak(how_to_func[0].summary)
        # print(how_to_func[0].summary)

    
    else:
        search = wikipedia.summary(Query, 2)
        speak(f": According to Your Search : {search}")
        print(f": According to Your Search : {search}")
def pdf_rer():
    book = open("C:\\Users\\sonas\\Documents\\SIST_BE_40731086_Saqib_Khan.pdf","rb")
    pR = PyPDF2.PdfFileReader(book)
    p = pR.numPages
    speak(f"Total Number of pages in this book is {p}")
    print(f"Total Number of pages in this book is {p}")
    speak("sir please say the page number i have to read")
    pg = int(takeCommand())
    pa = pR.getPage(pg)
    text = pa.extractText()
    speak(text)
    print(text)

def Cal_day():
        day = datetime.datetime.today().weekday() + 1
        Day_dict = {1: 'Monday', 2: 'Tuesday', 3: 'Wednesday',4: 'Thursday', 5: 'Friday', 6: 'Saturday',7: 'Sunday'}
        if day in Day_dict.keys():
            day_of_the_week = Day_dict[day]
            speak(day_of_the_week)
            print(day_of_the_week)
        
        return day_of_the_week

def Task():
    clear = lambda: os.system('cls')

    clear()
    wish_me()
    while True:
        # if 1:
        query = takeCommand().lower()


        if "wikipedia" in query:
            speak("Searching Wikipedia...")
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to wikipedia: ")
            print(results)
            speak(results)

        elif "search google" in query:
            query = query.replace("search google","")
            GoogleSearch(query)

        elif "youtube search" in query:
            from Features import YouTubeSearch
            YouTubeSearch(query)
            

        # elif "open google" in query:
        #     webbrowser.open("www.google.com")

        elif "open stackoverflow" in query:
            webbrowser.open("www.stackoverflow.com")

        elif "open duckduckgo" in query:
            webbrowser.open("www.duckduckgo.com")

        # elif "quran" in query:
        #     webbrowser.open("https://www.youtube.com/playlist?list=PLuM8gIEBe3i6iGPwZyFDu7ElEHWmyGm-v")

        # elif "audio to text" in query:
            # audiototext()

        elif "play music" in query:
            music_direc = "C:\\Users\\91950\\Music\\music_direc"
            songs = os.listdir(music_direc)
            print(songs)
            y = random.randint(0, len(songs) - 1)
            # print(y)
            os.startfile(os.path.join(music_direc, songs[y]))

        elif "play video" in query:
            vid_dir = "C:\\Users\\sonas\\Videos\\Video"
            vid = os.listdir(vid_dir)
            print(vid)
            z = random.randint(0, len(vid) - 1)
            # print(y)
            os.startfile(os.path.join(vid_dir, vid[z]))

        elif "time" in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            print(strTime)
            speak(f"sir the time is {strTime}")
        
        elif "day" in query:
            Cal_day()

        elif "open code" in query:
            codePath = "D:\\PyCharm Community Edition 2021.1.3\\bin\\pycharm64.exe"
            os.startfile(codePath)

        elif "open whatsapp" in query:
            whatsPath = "C:\\Users\\91950\\AppData\\Local\\WhatsApp\\WhatsApp.exe"
            os.startfile(whatsPath)

        elif "open tor" in query:
            torPath = "D:\\Tor Browser\\Browser\\firefox.exe"
            os.startfile(torPath)
        
        # elif "my location" or " where am i" in query:
            # from Features import my_location
            # my_location()

        elif "where is" in query:
            from Features import GoogleMaps
            place = query.replace("where is","")
            GoogleMaps(place)

        elif 'how are you' in query:
            speak("I am fine, Thank you")
            speak("How are you, Sir")
 
        elif 'fine' in query or "good" in query:
            speak("It's good to know that your fine")
        
        elif "read the book" in query:
            pdf_rer()
        
        elif 'joke' in query:
            speak(pyjokes.get_joke())
            print(pyjokes.get_joke())

        elif "don't listen" in query or "stop listening" in query:
            speak("for how much time you want to stop jarvis from listening commands")
            a = int(takeCommand())
            time.sleep(a)
            print(a)
        
        elif 'change background' in query:
            ctypes.windll.user32.SystemParametersInfoW(20,0,"Location of wallpaper",0)
            speak("Background changed successfully")

        elif 'lock window' in query:
            speak("locking the device")
            ctypes.windll.user32.LockWorkStation()

        elif 'shutdown system' in query:
            speak("Hold On a Sec ! Your system is on its way to shut down")
            subprocess.call('shutdown / p /f')

        elif "restart" in query:
            subprocess.call(["shutdown", "/r"])
             
        elif "hibernate" in query:
            speak("Hibernating")
            subprocess.call("shutdown / h")
 
        elif "log off" in query or "sign out" in query:
            speak("Make sure all the application are closed before sign-out")
            time.sleep(5)
            subprocess.call(["shutdown", "/l"])

        elif "camera" in query or "take a photo" in query:
            ec.capture(0, "Jarvis Camera ", "img.jpg")
        elif "quit" in query:
            sys.exit()
