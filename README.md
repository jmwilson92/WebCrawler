"""
This program is intended to allow the user to search through
multiple search engines. It takes the user input and then
compares it with searches from multiple search engines at once
and gives the user the most relevant five responses ranked in order.
"""

from bs4 import BeautifulSoup
import urllib.request
import urllib.parse
def user_input():
    search = input("What would you like to search for?: ")
    quoted = urllib.parse.quote(search)
    print (quoted)
    req = urllib.request.Request("https://www.google.com/search?q=" + quoted, headers={'User-Agent':'Magic Browser'})
    goog_html = urllib.request.urlopen(req)
    print (goog_html.read())
    soup = BeautifulSoup(goog_html.read(), features='lxml')
    for link in soup.find_all('a'):
        if search in link['href']:
            print (link.get('href'))

user_input()



