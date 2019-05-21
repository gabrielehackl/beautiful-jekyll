---


layout: post


title:  One bird with two stones: Regex and Beautiful Soup


---
Trying to find certain text (articles) in a text (issues, stored in a list) in XML format, saving the found texts in new files with new filenames (consisting of date and a serial number/counter) - first with Regex, then with BeautifulSoup. Both programs put out exaclty the same amount of files (articles), which is a good sign, right?

Done with *REGEX*:

~~~
import os
import re

originalPath = "C:/Users/hackl/Documents/Seminare/DH_Methodenkurs/XML/Homework/"
newPath = "C:/Users/hackl/Documents/Seminare/DH_Methodenkurs/XML/XML_Homework1bb/"
lof = os.listdir(originalPath)

for f in lof:
    with open(originalPath+f, "r", encoding="utf8") as f1:
        dataOrig = f1.read()
        issue_date = re.search(r"[0-9]{4}-[0-9]{2}-[0-9]{2}", dataOrig).group(0)
        allarticles = re.findall(r'(?s)<div3 type="article"(.*?)</div3>', dataOrig)
        counter = 0
        for article in allarticles:
            counter += 1
            text = re.sub("<[^<]+>", "", article)
            cleantext = re.sub("n=(.*)>", "", text)
            newFileName = issue_date + "_" + str(counter)
            with open(newPath+newFileName, "w", encoding="utf8") as f9:
                f9.write(cleantext)
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
