# Domain-monitor
## Python - This python code is written to monitor the domains and send the status report to the owner. Most of us depends upon the external monitoring tools to monitor the domains in our server. This python code can be set as a cron instead of these external monitoring tools. 

```python
import requests
import threading
import time

urls = [
  'https://freevideolectures.com',
  'https://www.guru99.com/',
  'https://linuxjourney.com',
  'https://www.quora.com',
  'https://www.tecmint.com',
  'https://linuxsurvival.com',
  'https://www.tutorialspoint.com',
  'https://www.computerworld.com',
  'https://hackernoon.com',
  'https://www.howtoforge.com',
  'https://asdfg.com',
  'https://www.ubuntupit.com/',
  'https://www.techworm.net',
  'https://www.2daygeek.com/',
  'https://www.linux.org',
  'https://linuxacademy.com',
  'https://www.cyberciti.biz',
  'https://itsfoss.com',
  'https://blog.feedspot.com',
  'https://www.techradar.com',
  'https://phoenixts.com/',
  'https://training.linuxfoundation.org',
  'https://www.reallinuxuser.com/',
  'https://linuxconfig.org',
  'https://sourceforge.net',
  'https://www.makeuseof.com'
]


Status = {}

def fetchStatus(url):
  
  try: 
    response = requests.get(url,timeout=2)
    if response.status_code == requests.codes.OK:
      Status[url] = 'The domain is online'
    else:
      Status[url] = 'The domain is offline'
  except:
    
    Status[url] = 'Error while accessing the domain'

  
threadList = []


for url in urls:
  
  t = threading.Thread(target=fetchStatus,args=(url,))
  t.start()
  threadList.append(t)
  

for thread in threadList:
  thread.join()

for item in Status:
    c = Status[item]
    print('{:20} {}'.format(item,c))
    
with open('result.txt','w') as fh:
  
  for url in Status:
      
      status = Status[url]
      fh.write('{:5} {}\n'.format(status,url))
```

## The output of the this code will looks like the following.

### https://www.computerworld.com The domain is online
https://www.cyberciti.biz The domain is online
https://training.linuxfoundation.org The domain is online
https://blog.feedspot.com The domain is online
https://hackernoon.com The domain is online
https://linuxconfig.org The domain is online
https://www.ubuntupit.com/ The domain is online
https://www.guru99.com/ The domain is online
https://linuxacademy.com The domain is online
https://www.linux.org The domain is online
https://www.makeuseof.com The domain is online
https://www.tecmint.com The domain is online
https://www.quora.com The domain is online
https://www.howtoforge.com The domain is online
https://www.techworm.net The domain is online
https://linuxsurvival.com The domain is online
https://itsfoss.com  The domain is online
https://www.tutorialspoint.com The domain is online
https://phoenixts.com/ The domain is online
https://www.2daygeek.com/ The domain is offline
https://linuxjourney.com The domain is online
https://www.techradar.com The domain is online
https://sourceforge.net The domain is online
https://freevideolectures.com The domain is online
https://www.reallinuxuser.com/ The domain is online
https://asdfg.com    Error while accessing the domain

## Now we can send this report as a text file to the owner's email account. I have used the modules smtplib and email for writing this code.

```python
import smtplib
import email


USERNAME = 'arjun.muralidharan@vipointsolutions.net'
PASSWORD = '************'


report = email.message.EmailMessage()

report['From'] = USERNAME
report['Subject'] = 'Scanning Report attached'
report['To'] = [ 'user@gmail.com','client@gmail.com' ]

with open('result.txt','r') as fh:
  data = fh.read()
  
report.add_attachment(data,subtype='text',filename='report.txt')

with smtplib.SMTP_SSL('smtp.gmail.com',465) as smtp:
  smtp.login(USERNAME,PASSWORD)
  smtp.send_message(report)
```
