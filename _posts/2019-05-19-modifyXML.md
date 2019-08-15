---


layout: post


title: Modifying XML



---


Trying to find certain text (articles) in a text (issues, stored in a list) in XML format, saving the found texts in new files with new filenames (consisting of date and a serial number/counter) - first with Regex, then with BeautifulSoup.

Done with *REGEX*:

~~~
import os #importing the OS module
import re #importing the Regex module

originalPath = "C:/Users/hackl/Documents/Seminare/DH_Methodenkurs/XML/Homework/" #to define where the original documents are stored
newPath = "C:/Users/hackl/Documents/Seminare/DH_Methodenkurs/XML/XML_Homework1bb/" #to define where the modified or newly created documents are going to be stored
lof = os.listdir(originalPath) #creating a list of files of all the files in the folder definded by the variable 'orignalPath'

for f in lof: #looping through the list of files
    with open(originalPath+f, "r", encoding="utf8") as f1: #opening every single file
        dataOrig = f1.read() #reading the data stored in the file
        issue_date = re.search(r"[0-9]{4}-[0-9]{2}-[0-9]{2}", dataOrig).group(0) #finding the date in the format yyyy-mm-dd using regex and saving that group in the variable 'issue_date'
        allarticles = re.findall(r'(?s)<div3 type="article"(.*?)</div3>', dataOrig) #making a list of all the articles found by looking for the tag 'div3 type'
        counter = 0 #universal counter
        for article in allarticles: #looping through the list of articles
            counter += 1 #counting the number of articles
            text = re.sub("<[^<]+>", "", article) #getting rid of all the tags in the data
            cleantext = re.sub("n=(.*)>", "", text) #cleaning up the text some more
            newFileName = issue_date + "_" + str(counter) #using the issue date and the counter to dreate new and unique file names for the articles
            with open(newPath+newFileName, "w", encoding="utf8") as f9: #writing one file for every article
                f9.write(cleantext) #using the data stored in the variable 'cleantext'
~~~

Done with *BeautifulSoup*:

~~~
import os
import re
from bs4 import BeautifulSoup

originalPath = "C:/Users/hackl/Documents/Seminare/DH_Methodenkurs/XML/Homework/"
newPath = "C:/Users/hackl/Documents/Seminare/DH_Methodenkurs/XML/XML_Homework1b/"
lof = os.listdir(originalPath)

for f in lof:
    with open(originalPath+f, "r", encoding="utf8") as file:
        dataOrig = BeautifulSoup(file.read(), "html.parser")
        issue_date = dataOrig.find_all("date", limit = 2)[1]
        issue_date = issue_date.get("value")
        articles = dataOrig.find_all("div3", {"type":"article"})
        counter = 0
        for i in articles:
            counter += 1
            data = i.get_text()
            newFileName = issue_date + "_" + str(counter) + ".txt"
            with open(newPath+newFileName, "w", encoding="utf8") as f9:
                f9.write(data)
~~~
