# Project Summary
<b>Scraped product name and average customer rating from Amazon website to track the ratings over time.</b>
<ul>
<li>Utilized web hosting connection and the BeautifulSoup library for web scraping process.</li>
<li>Find() was used to scrape name of the product, current average customer rating, and date.</li>	
<li>Created and populated a csv file with scraped data (headers and initial row) using the csv library. </li>
<li>Automated scraping and data appending at user-specified intervals using the time library.</li>
<li>Allowed user to gain the capability to closely monitor fluctuations or changes in product ratings over time without visiting the website.</li>
</ul>

```python
# %%
%cd "C:\Users\wodnj\Desktop\Projects"

# %%
#Importing Libraries

from bs4 import BeautifulSoup
import requests
import time
import datetime
import csv
import pandas as pd

#Hosting connection to the website
URL = 'https://www.amazon.com/Essential-Math-Data-Science-Fundamental/dp/1098102932/ref=sr_1_4?crid=1R22UQ4DY0DSH&dib=eyJ2IjoiMSJ9.IRIcA3ASAZDz-4QeoiNg13QAN-g2oHLoVDGgmH_ay_SGT-ZwmV8P_rmCdPSAe4WyqIMea-7FjIqkh3gELYeqQaXBq6m253t5-JkjKlZ1zZyAj3G1ajgpqPtZb5-Fh0IRcKZmchCL0m49tq7159i7DQTjWsjhIls0ICJNkYGFa8SE_5mIbCCSq_EUJ2kLvKVpOWRMopzAARleLPcty9cL80bWbrob8u9piBIc-4AKl_aVrL-r7Kmgmbm7wnSexdJSWpUZ-uZ7EAIN2dounpmffzOAIA5_1yNBH3_qPQrjTUs.cOMP_MWu_X4C19iQbiDk5hii5Qcfa1D9fVRExl6hShk&dib_tag=se&keywords=data+science+textbook&qid=1710085397&sprefix=data+science+textboo%2Caps%2C258&sr=8-4'

#headers obtained from https://httpbin.org/get
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36" , "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

#Using BeautifulSoup library to scrape the Amazon webpage, then assigning the title(item's name),average_rating(Customer's),today(date) variables.
page = requests.get(URL, headers = headers)

soup1 = BeautifulSoup(page.content, "html.parser")

soup2 = BeautifulSoup(soup1.prettify() , 'html.parser')

result = soup2.find("div", id = 'averageCustomerReviews')

average_rating = result.find(class_ = "a-size-base a-color-base").get_text().strip()

title = soup2.find(id = 'productTitle').get_text().strip()

today = datetime.date.today()

print(title)
print(average_rating)
print(today)

header = ['Title' , 'Rating' , 'Date']
data = [title, average_rating ,today]

#Creating the CSV , then inserting the header and the first row of data 
with open('AmazonData.csv' , 'w' , newline = '' , encoding = 'UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)

#outcome of the cell above after the addition
pd.read_csv(r"C:\Users\wodnj\Desktop\Projects\AmazonData.csv")

#Now appending data into the AmazonData.csv
with open('AmazonData.csv' , 'a+' , newline = '' , encoding = 'UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)

#rating_check function created and included the codes above in order to utilize the time library.
def rating_checker():
    URL = 'https://www.amazon.com/Essential-Math-Data-Science-Fundamental/dp/1098102932/ref=sr_1_4?crid=1R22UQ4DY0DSH&dib=eyJ2IjoiMSJ9.IRIcA3ASAZDz-4QeoiNg13QAN-g2oHLoVDGgmH_ay_SGT-ZwmV8P_rmCdPSAe4WyqIMea-7FjIqkh3gELYeqQaXBq6m253t5-JkjKlZ1zZyAj3G1ajgpqPtZb5-Fh0IRcKZmchCL0m49tq7159i7DQTjWsjhIls0ICJNkYGFa8SE_5mIbCCSq_EUJ2kLvKVpOWRMopzAARleLPcty9cL80bWbrob8u9piBIc-4AKl_aVrL-r7Kmgmbm7wnSexdJSWpUZ-uZ7EAIN2dounpmffzOAIA5_1yNBH3_qPQrjTUs.cOMP_MWu_X4C19iQbiDk5hii5Qcfa1D9fVRExl6hShk&dib_tag=se&keywords=data+science+textbook&qid=1710085397&sprefix=data+science+textboo%2Caps%2C258&sr=8-4'
    
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36" , "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}
    
    page = requests.get(URL, headers = headers)
    
    soup1 = BeautifulSoup(page.content, "html.parser")
    
    soup2 = BeautifulSoup(soup1.prettify() , 'html.parser')
    
    result = soup2.find("div", id = 'averageCustomerReviews')

    average_rating = result.find(class_ = "a-size-base a-color-base").get_text().strip()
    
    title = soup2.find(id = 'productTitle').get_text().strip()
    
    import datetime
    today = datetime.date.today()
    
    import csv
    header = ['Title' , 'Rating' , 'Date']
    data = [title, average_rating ,today]
    
    with open('AmazonData.csv' , 'a+' , newline = '' , encoding = 'UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)

#time library's sleep() used to append the updated rating every 12 hours (60 seconds * 60 minutes * 12 hours)
while(True):
    rating_checker()
    time.sleep(43200)

#This line of code is for demonstration purposes. Here,I will be setting the sleep time to 3 seconds and update the rating of the product for total of 30 seconds (10 times) and display the output.
while(True):
    rating_checker()
    time.sleep(5)

#after running for 30 seconds (Since the rating will not change in an interval of 30 seconds, all rows each added at 3 second interval are same.)
pd.read_csv(r"C:\Users\wodnj\Desktop\Projects\AmazonData.csv")


```

<b>Source: </b> [Amazon Product Link](https://www.amazon.com/Essential-Math-Data-Science-Fundamental/dp/1098102932/ref=sr_1_4?crid=1R22UQ4DY0DSH&dib=eyJ2IjoiMSJ9.IRIcA3ASAZDz-4QeoiNg13QAN-g2oHLoVDGgmH_ay_SGT-ZwmV8P_rmCdPSAe4WyqIMea-7FjIqkh3gELYeqQaXBq6m253t5-JkjKlZ1zZyAj3G1ajgpqPtZb5-Fh0IRcKZmchCL0m49tq7159i7DQTjWsjhIls0ICJNkYGFa8SE_5mIbCCSq_EUJ2kLvKVpOWRMopzAARleLPcty9cL80bWbrob8u9piBIc-4AKl_aVrL-r7Kmgmbm7wnSexdJSWpUZ-uZ7EAIN2dounpmffzOAIA5_1yNBH3_qPQrjTUs.cOMP_MWu_X4C19iQbiDk5hii5Qcfa1D9fVRExl6hShk&dib_tag=se&keywords=data+science+textbook&qid=1710085397&sprefix=data+science+textboo%2Caps%2C258&sr=8-4')
