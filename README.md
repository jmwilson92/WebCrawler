# WebCrawler
#This crawls through search engines and retrieves relevant links based on user's input
"""
This program is intended to allow the user to search through
multiple search engines. It takes the user input and then
compares it with searches from multiple search engines at once
and gives the user the most relevant five responses ranked in order.
"""

from bs4 import BeautifulSoup
import urllib.request
def user_input():
    search = input("What would you like to search for?: ")
    if ' ' in search:
        print ('Please only use one word')
        user_input()
    else:
        req = urllib.request.Request("https://www.google.com/search?q=" + search, headers={'User-Agent':'Magic Browser'})
        goog_html = urllib.request.urlopen(req)
        soup = BeautifulSoup(goog_html, features='lxml')
        for link in soup.find_all('a'):
            if search in link['href']:
                print(link)
user_input()
