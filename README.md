"""
Search informational webpages on google to see what the most common words
are in order of their occurrence
"""

from bs4 import BeautifulSoup
import urllib.request
import urllib.parse
from collections import Counter
from string import punctuation
from nltk.corpus import stopwords
import requests
import ssl
from time import sleep
    
    


def user_input():
    goog_list = []
    relevant_list =[]
    superrelevant_list = []
    search = input("What would you like get hashtags for?: ")
        
    quoted = urllib.parse.quote(search)
    req = urllib.request.Request("https://www.google.com/search?q=" + quoted, headers={'User-Agent':'Magic Browser'})
    goog_html = urllib.request.urlopen(req)
    print("Getting Links")
    sleep(2)
    soup = BeautifulSoup(goog_html, features='lxml')
    for links in soup.find_all('a'):
        if links.has_attr('href'):
            goog_list.append(links['href'])
    for glinks in goog_list:
        if 'https://www.' in glinks:
            relevant_list.append(glinks)
    for glinkers in relevant_list:
        if "https://www.google.com/" not in glinkers:
            newsearch = "http://www.google.com" + glinkers
            superrelevant_list.append(newsearch)
    superrelevant_list.pop(0)
    print("Validating Websites, please wait a few minutes")
    for linkers in superrelevant_list:
        try:
            response = urllib.request.Request(linkers)
        except:
            print ("Was not a valid website")
        sleep(2)
        ssl._create_default_https_context = ssl._create_unverified_context
        secreq = urllib.request.Request(linkers, headers={'User-Agent': 'Mozilla/5.0'})
        sec_html = urllib.request.urlopen(secreq).read()
        sleep(2)
        soup = BeautifulSoup(sec_html, "lxml")
        text = (''.join(s.findAll(text=True))for s in soup.find_all('p'))
        c = Counter((x.rstrip(punctuation).lower() for y in text for x in y.split()))
        final_c = ([x for x in c if c.get(x) > 3]) # words appearing more than 5 times 
        nltk_words = list(stopwords.words('english'))
        for i in final_c[:]:
            if i in nltk_words:
                final_c.remove(i)
        hashtag_list = []
        for s in final_c[:]:
            s = ' #' + s
            hashtag_list.append(s)
            final_list = ' '.join(hashtag_list)
    print("Your results are on their way!")
    sleep(2)
    print(final_list)     
user_input()
