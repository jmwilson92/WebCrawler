"""
Search informational webpages to see what the most common words
are in order of their occurrence
"""

from bs4 import BeautifulSoup
import urllib.request
import urllib.parse
from collections import Counter
from string import punctuation
from nltk.corpus import stopwords



def web_search():
    websearch = input("Use this format: www.yoursite.com .Enter a website to find out what the most common word is? ")
    newsearch = "http://" + websearch
    try:
        response = urllib.request.Request(newsearch)
    except:
        print ("Was not a valid website")
        web_search()
    goog_req = urllib.request.Request(newsearch)
    goog_html = urllib.request.urlopen(goog_req).read()
    soup = BeautifulSoup(goog_html, "lxml")
    text = (''.join(s.findAll(text=True))for s in soup.findAll('p'))
    c = Counter((x.rstrip(punctuation).lower() for y in text for x in y.split()))
    final_c = ([x for x in c if c.get(x) > 5])
    processed_word_list = []
    stopWords = set(stopwords.words('english'))
    for word in final_c:
        word = word.lower() 
        if word not in stopWords:
            processed_word_list.append(word)
    print(processed_word_list)
    anothersearch()
    
            
def startprogram():
    choice = input(" Would you like to search a website? yes or no? ")
    newchoice = choice.lower()
    if newchoice == "yes":
        web_search()
    elif newchoice == "no":
        print ("Ok bye")
        startprogram()
    else:
        print("That is not a valid response")
        startprogram()
def anothersearch():
    choice = input(" Would you like to search another website? yes or no? ")
    newchoice = choice.lower()
    if newchoice == "yes":
        web_search()
    elif newchoice == "no":
        print ("Ok bye")
        startprogram()
    else:
        print("That is not a valid response")
        anothersearch()


startprogram()
