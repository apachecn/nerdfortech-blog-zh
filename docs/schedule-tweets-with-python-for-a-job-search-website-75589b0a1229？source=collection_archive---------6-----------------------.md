# 用 Python 为求职网站安排推文

> 原文：<https://medium.com/nerd-for-tech/schedule-tweets-with-python-for-a-job-search-website-75589b0a1229?source=collection_archive---------6----------------------->

在这篇文章中，我们将使用 Python 为我们的求职网站编写代码，我们已经使用 Python 和 Django 编写了代码。我们已经发布了一系列文章，涵盖了使用 Python 和 Django 从头构建一个全功能 Web 应用程序的各个阶段。

在前两篇文章中，我们讨论了:

*   [从网站和 PDF 文档中删除数据](https://skolo-online.medium.com/scrap-data-from-website-and-pdf-document-for-django-app-fa8f37010085)
*   [带 Tweepy 的 Python 推特机器人分步指南](https://skolo-online.medium.com/step-by-step-guide-to-python-twitter-bot-with-tweepy-in-15min-f3c8b50a5429)

在本文中，我们将结合前两篇文章的内容，构建一个 twitter 机器人，它将发布我们从网络报废活动中收集的数据。

# 本教程的原因

像 twitter 和 facebook 这样的社交媒体网站在世界访问量最大的网站中排名前五。社交媒体很受消费者欢迎，因此是接触潜在受众的好地方。

![](img/b877439600812bb7439ae9faaa34151f.png)

社交媒体也是根据兴趣、群体和标签来组织的。这使得它成为一个营销的好地方，因为你可以将你的信息瞄准一个特定的标签或受众，而不是一个寻找任何可能遇到它的人的广告牌。

因此，当我们开始推出我们的求职平台时，我们也要关注如何利用社交媒体接触更广泛的受众，这一点很重要。理想情况下，我们希望在社交媒体上接触到人们，这样我们就可以开发我们的 web 应用程序。

## 重新确定 Web 报废代码的用途

我们已经编写了代码来删除我们需要获取工作数据的网站，但我们现在需要重新利用这些代码来删除我们需要的推文。使用同一个网站，创建一个新文件，再次为推文废弃数据:

```
from bs4 import BeautifulSoup
from datetime import date
import requests
import json
import configheaders =  {'User-agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:61.0) Gecko/20100101 Firefox/61.0'}
baseUrl = '[https://careers.vodafone.com'](https://careers.vodafone.com')
url = "[https://careers.vodafone.com/key/vodacom-vacancies.html?q=vodacom+vacancies](https://careers.vodafone.com/key/vodacom-vacancies.html?q=vodacom+vacancies)"#Read a JSON File
def readJson(filename):
        with open(filename, 'r') as fp:
            return json.load(fp)#Write a JSON file
def writeJson(filepath, data):
    with open(filepath, 'w') as fp:
        json.dump(data, fp)def shortenUrl(linkUrl):
    linkRequest = {
      "destination": linkUrl,
      "domain": { "fullName": "messages.careers-portal.co.za" },
      #"slashtag": "",
    }requestHeaders = {
      "Content-type": "application/json",
      "apikey": config.rebrand_api_key,
    }r = requests.post("[https://api.rebrandly.com/v1/links](https://api.rebrandly.com/v1/links)",
        data = json.dumps(linkRequest),
        headers=requestHeaders)if (r.status_code == requests.codes.ok):
        link = r.json()
        return link["shortUrl"]
    else:
        return ''#Scan the website
def jobScan(link):
    the_job = {}jobUrl = '{}{}'.format(baseUrl, link['href'])
    the_job['urlLink'] = shortenUrl(jobUrl)job=requests.get(jobUrl, headers=headers)
    jobC=job.content
    jobSoup=BeautifulSoup(jobC, "html.parser")to_test = jobSoup.find_all("div", {"class": "joblayouttoken displayDTM"})if to_test == []:
        return None
    else:title = jobSoup.find_all("h1")[0].text
        the_job['title'] = titlethe_divs = jobSoup.find_all("div", {"class": "joblayouttoken displayDTM"})country = the_divs[1].find_all("span")[1].text
        the_job['location'] = countrydate_posted = the_divs[2].find_all("span")[1].text
        the_job['date_posted'] = date_postedfull_part_time = the_divs[3].find_all("span")[1].text
        the_job['type'] = 'Full Time'contract_type = the_divs[4].find_all("span")[1].text
        the_job['contract_type'] = contract_typereturn the_jobr = requests.get(url, headers=headers)
c = r.content
soup = BeautifulSoup(c, "html.parser")#Modified these lines here . . . .
table=[]
results = soup.find_all("a", {"class": "jobTitle-link"})
[table.append(x) for x in results if x not in table]#run the scanner and get all the jobs . . . .
final_jobs = []
for x in table:
    job = jobScan(x)
    if job:
        final_jobs.append(job)#Save the data in a Json
filepath = '/absolute-path-to-folder/vodacom.json'
writeJson(filepath, final_jobs)
```

U **RL 缩短**

在将工作页面添加到 dict 对象之前，我们添加了用于缩短工作页面 URL 的特定代码，该代码保存在我们的 JSON 中。由于字符限制，URL 缩短对 twitter 至关重要——一条推文只能有 160 个字符。

我使用了一个名为 Rebrandly([https://www . rebrandy . com)/](https://www.rebrandly.com/)的服务—您可以在这里创建一个新帐户，并在代码中使用它来缩短 URL。

**保存数据**

在页面底部，我们对这段代码所做的是:将数据保存在一个 json 文件中。这样，我们就可以定期运行这段代码，总是检查最新的可用作业，并刷新保存在文件中的数据。tweeting 代码只需要访问保存的文件，就可以获得最新的数据。

## 从 JSON 数据创建 Tweet

我们将分两个阶段创建推文:

*   清理网站上的数据，并将其串在一起，以创建推文消息
*   将 tweep 消息与适当的 hashtags 连接起来，并创建 Tweepy API 对象来发送 tweep

**数据处理，创建推文**

```
import jsondef readJson(filename):
        with open(filename, 'r') as fp:
            return json.load(fp)def grabVodacomJobs():
    statuses = []
    filepath = 'absolute-path-to-folder/vodacom.json'
    jobs = readJson(filepath)for job in jobs:
        status = 'Vodacom-{}{} Apply {}'.format(job['title'], job['contract_type'], job['urlLink']).replace('\n', '').replace('  ',' ')
        statuses.append(status)return statuses
```

**用标签建立推文信息**

不要忘记将上面的文件导入到 tweeting 文件中，我们需要访问从数据库中获取数据的函数。

```
# -*- coding: utf-8 -*-import tweepy
import config
import random
import tweet_dataimport schedule
import time def createTheAPI():
    auth = tweepy.OAuthHandler(config.api_key, config.api_secret)
    auth.set_access_token(config.access_token, config.access_token_secret)
    api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)
    return apivodaJobs = tweet_data.grabVodacomJobs()
hashtags = ["#hashtag1", "hashtag2", "hashtag3", "hashtag4", "hashtag5"]def createVodacomTweet():
    while True:
        msg = random.choice(vodaJobs)hashtags = getTags()
        tags = random.sample(hashtags, 2)if len(msg) < 110:
            msg += ' '+tags[0]
            msg += ' '+tags[1]if len(msg) < 160:
            return msg
            break def sendVodacomTweet():
    api = createTheAPI()
    tweet = createVodacomTweet()
    api.update_status(tweet)
    print('Just tweeted: {}'.format(tweet)) #The Scheduling is happening below: schedule.every().day.at("10:30").do(sendVodacomTweet)
while True:
    schedule.run_pending()
    time.sleep(1)
```

我们使用在文件顶部导入的随机函数来(1)从数据库发送的列表中选择一条随机 tweet ,( 2)从 hashtags 列表中选择 2 个随机样本。只要你需要，你可以列出这个列表，随机选择的标签也可以防止你连续发送两条相同的推文。

我们还使用 python 时间表库来创建任务的每日时间表——有多种方法可以使用，直接从时间表网站——https://pypi.org/project/schedule/

文档中的代码:

```
import schedule
import time

def job():
    print("I'm working...")

schedule.every(10).seconds.do(job)
schedule.every(10).minutes.do(job)
schedule.every().hour.do(job)
schedule.every().day.at("10:30").do(job)
schedule.every(5).to(10).minutes.do(job)
schedule.every().monday.do(job)
schedule.every().wednesday.at("13:15").do(job)
schedule.every().minute.at(":17").do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

完整视频教程: