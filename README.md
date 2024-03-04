# Jarvis-AI-desktop
import pyttsx3
import speech_recognition as sr
import pyaudio
import datetime
import os
import cv2
from requests import get
import wikipedia
import webbrowser
import pywhatkit as kit
import smtplib
import sys
import pyjokes


from pyttsx3 import Engine

engine: Engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
print(voices[1].id)
engine.setProperty('voices', voices[1].id)


# text to speech
def speak(audio):
    engine.say(audio)
    print(audio)
    engine.runAndWait()
# speak("hello mam,i am jarvis ,please tell me how can i help you")

#to convert voice into text
def takecommand():
    r=sr.Recognizer()
    with sr.Microphone() as Source:
        print("listening...")
        r.Pause_threshold=1
        audio=r.listen(Source,timeout=1,phrase_time_limit=5)
    try:
        print("recognizing....")
        query=r.recognize_google(audio,language='en_ln')
        print(f'user said {query}')
    except Exception as e:
        speak('say that again please...')
        return "none"
    return query
#takecommand()

#to wish
def wish():
    hour=int(datetime.datetime.now().hour)

    if hour>=0 and hour<=12:
        speak('good morning')
    elif hour>12 and hour<18:
        speak('good afternoon')
    elif hour>18 and hour<20:
        speak('good evening')
    else:
        speak('good night')
    speak('i am jarvis please tell me how can i help you?')

def sendemail(to,content):
    server=smtplib.SMTP('smtp.gmail.com',587)
    server.elho()
    server.starttls()
    server.login('your email id','your password')
    server.sendmail('your email id',to,content)
    server.close()
if __name__=='__main__':
    #speak('hello mam,i am jarvis,please tell me how can i help you')
    #takecomm
    wish()
    while True:
    #if 1:
        query=takecommand().lower()

        #logic building for task
        if 'open notepad' in query:
            npath="C:\\Windows\\system32\\notepad.exe"
            os.startfile(npath)
        elif 'open camera' in query:
            cap=cv2.VideoCapture(0)
            while True:
                ret,img=cap.read()
                cv2.imshow('webcam',img)
                k=cv2.waitKey(50)
                if k==27:
                    break;
            cap.release()
            cv2.destroyWindow()
        elif 'play music' in query:
            music_dir='E:\\music'
            songs=os.listdir(music_dir)
            os.startfile(os.path.join(music_dir,songs[1]))
        elif 'ip address' in query:
            ip=get('https://api.ipify.org').text
            speak(f'your ip address is {ip}')
        elif 'wikipedia' in query:
            speak('searching wikipedia.....')
            query=query.replace('wikipedia','')
            results=wikipedia.summary(query,sentences=2)
            speak('according to wikipedia')
            speak(results)
            print(results)
        elif 'open youtube' in query:
            webbrowser.open('www.youtube.com')
        elif 'open spotify' in query:
            webbrowser.open('www.spotify.com')
        elif 'open telegram' in query:
            webbrowser.open('www.telegram.com')
        elif 'open chrome' in query:
            speak('mam,what would i search on google')
            cm=takecommand().lower()
            webbrowser.open(f'{cm}')
        elif 'send message' in query:
            kit.sendwhatmsg('+918949114791','hii',0,0)
        elif 'play song on youtube' in query:
            kit.playonyt('ishq nachaave')
        elif 'email to jitanshu' in query:
            try:
                speak('what should i say?')
                content=takecommand().lower()
                to='jitanshu......'
                sendEmail(to,content)
                speak('email has been sent to jitanshu')
            except Exception as e:
                print(e)
                speak('sorry mam i am not able to send an email')
        elif 'you can sleep now' in query:
            speak('thanks for using me mam,have a good day')
            sys.exit()
        #to close any application
        elif 'close notepad' in query:
            speak('okay mam closing notepad')
            os.system("taskkill /f /im notepad.exe")
        #to set an alarm
        elif 'set alarm' in query:
            nn=int(datetime.datetime.now().hour)
        if nn==22:
         music_dir = 'E:\\music'
         songs = os.listdir(music_dir)
         os.startfile(os.path.join(music_dir, songs[3]))
        #speak('mam do you have any other work?')
        #to find a joke
        elif 'tell me a joke' in query:
            jokes=pyjokes.get_joke()
            speak(jokes)
        elif 'shutdown the system'in query:
            os.system('shutdown /s /t 5')
        elif 'sleep the system' in query:
            os.system('rund1132.exe powrprof.dil,SetSuspendState 0,1,0')
