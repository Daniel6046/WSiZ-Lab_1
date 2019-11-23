# WSiZ-Lab_1
#!/usr/bin/env python
# coding: utf-8

# In[16]:



import numpy as np
import requests
import pandas as pd
import matplotlib.pyplot as plt

#Waluty i daty kursów
currency1 = 'AUD'
currency2 = 'GBP'
date_from = '2019-08-01'
date_to = '2019-08-31'

#Zadanie 1
def currency(currency,beg,end):
    url = 'http://api.nbp.pl/api/exchangerates/rates/A/' + currency + "/" + date_from + "/" + date_to + "/"
    currency_req = requests.get(url)
    currency_data = currency_req.json()
    return currency_data['rates']

#Zadanie 2
rate1 = currency(currency1,date_from,date_to)
rate2 = currency(currency2,date_from,date_to)

rate_dataframe1 = pd.DataFrame.from_dict(rate1).head(31)
rate_dataframe2 = pd.DataFrame.from_dict(rate2).head(31)

#Zadanie 3
plot_data1 = rate_dataframe1.set_index(['effectiveDate'])['mid']
plot_data2 = rate_dataframe2.set_index(['effectiveDate'])['mid']

# Zadanie 4
correlation = np.corrcoef (plot_data1, plot_data2)[0][1]

# Zadanie 5 
plt.plot(plot_data1, 'r--', plot_data2,'b--')
plt.ylim(ymin=0)
plt.title('Korelacja {} do {} = {}'.format(currency1, currency2, correlation))
plt.ylabel('Wartość w PLN')
plt.xlabel('Data kursu')
plt.legend([currency1, currency2], loc='lower right')
plt.show()
