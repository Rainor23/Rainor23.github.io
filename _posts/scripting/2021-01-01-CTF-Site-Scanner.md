---
title: "CTF Site Scanner (Python)"
excerpt: A simple script to grab all url's and comments from a source page.
categories:
  - scripting
  - python
  - CTF
read_time: true
words_per_minute: 820
header:
      overlay_image: /assets/imgs/hacker3.jpg
---

# Simple CTF script
___

You're doing a CTF and your wordlist isn't finding any directories... maybe they are using custom url's on purpose so that the wordlist won't work? Maybe they've hidden comments in the source code to point to a dev area? It can be annoying to search through, even more so when there is a large page with a ton of source code!

I created this script to look through html and find interesting elements, such as urls, comments and soon to be, hidden html elements. In scope of this project will be the ability for recursive searching using a word list.

__What does it do?__
This script will pull the site source code, look for any urls and comments, then parse them out.

Updates to come:
- Search for hidden html elements.
- Ability to search multiple urls and output them together in one output.

{% highlight python %}

import requests
from bs4 import BeautifulSoup, Comment
import sys
import os

#Colour list
ansi_Black = "\u001b[30m"
ansi_Red = "\u001b[31m"
ansi_Green = "\u001b[32m"
ansi_Yellow = "\u001b[33m"
ansi_Blue = "\u001b[34m"
ansi_Magenta = "\u001b[35m"
ansi_Cyan = "\u001b[36m"
ansi_White = "\u001b[37m"
ansi_Reset = "\u001b[0m"

#Windows has weird python bug, where it won't load ANSI colors unless cls is run first, so... we run cls
#os.system("cls")

if len(sys.argv) > 2:
    print("Error: Too many positional arguments! ")
    sys.exit()
if len(sys.argv) < 2:
    print('Error: You must provide a URL')
    sys.exit()
if not sys.argv[1].startswith("http://") or sys.argv[1].startswith("https://"):
    print('Error: You must provide http:// or https://')

URL = sys.argv[1]
r = requests.get(URL)
soup = BeautifulSoup(r.content, 'html.parser')

#Search URLS
href_Arr = []
for link in soup.find_all('a', href=True, text=True):
    href_Arr.append(link['href'])
print(f'{ansi_Red}Starting search.... \n')
print(f'''{ansi_Yellow}URLS found on webpage {URL}{ansi_Reset}
============================================== \n {ansi_Cyan}''')
print('\n'.join(href_Arr))
print(f"{ansi_Reset}==============================================")

#Search for comments
comments_arr = []
for comments in soup.findAll(text=lambda text:isinstance(text, Comment)):
    comments_arr.append(comments)
print(f'''{ansi_Yellow}Comments found on webpage {URL}{ansi_Reset}
============================================== \n{ansi_Green}''')
print('\n'.join(comments_arr))
print(f"{ansi_Reset}==============================================")


{% endhighlight %}

Example output (From http://RainerInfoSec.com)
![Script Output](/assets/imgs/site_script1.PNG)
