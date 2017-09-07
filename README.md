# Retrieve-Stock-History
# Retrieve stock data of "AXP" from the Yahoo finance website

import requests
import re
import json
import pandas as pd

def retrieve_quotes_historical(stock_code):
    quotes = []                         #empty list "quotes"
    url = 'https://finance.yahoo.com/quote/%s/history?p=%s' % (stock_code, stock_code)   #url with argument/address stock_code
    #print(url)
    fhandle = requests.get(url)                 # Get the file handle of "url"
    #print(fhandle.text)
    m = re.findall('"HistoricalPriceStore":{"prices":(.*?),"isPending"',fhandle.text)   #finding the search pattern from the string "text of url"
    if m:                # "m" is True if there is some content and False if empty
        quotes = json.loads(m[0])      #json.loads transfer the string "m[0]" into list/dict
        quotes = quotes[::-1]          #reversing the order/indexing of quotes(collection of list/dict)
    return [item for item in quotes if not 'type' in item]   #return only those item (list/dict) having string "type"

quotes = retrieve_quotes_historical('AXP')     #calling the function to retrieve historical data of 'AXP'
#print(quotes)
quotes_df =  pd.DataFrame(quotes)              #transfering quotes(collection of list/dict) into Dataframe
print(quotes_df)
