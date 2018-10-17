"""
Search informational webpages to see what the most common words
are in order of their occurrence
"""

from bs4 import BeautifulSoup
import urllib.request
import urllib.parse
from collections import Counter
from string import punctuation


def web_search():
    websearch = input("Enter a website to find out what the theme is? ")
    try:
        response = urllib.request.Request(websearch)
    except:
        print ("Was not a valid website")
        web_search()
    goog_req = urllib.request.Request(websearch)
    goog_html = urllib.request.urlopen(goog_req).read()
    soup = BeautifulSoup(goog_html, "lxml")
    text = (''.join(s.findAll(text=True))for s in soup.findAll('p'))
    c = Counter((x.rstrip(punctuation).lower() for y in text for x in y.split()))
    final_c = ([x for x in c if c.get(x) > 5])
    print (final_c)
    web_search()
            
    
web_search()
        
               




