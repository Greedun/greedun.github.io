---
layout: post
date: 2024-11-04
title: "Summary_Result_Pwn2Own 2024 Ireland"
tags: [Com1, Pwn2Own, ]
categories: [Summary, ]
---



## 1. Pwn2Own 결과 크롤링 코드


(크롤링 코드)



{% raw %}
```python
import requests
from bs4 import BeautifulSoup
import sys

"""
# 0. zdi blog에서 pwn2own ireland 2024에 관련한 RESULT 확인
base_url = "https://www.zerodayinitiative.com/blog/"
html = requests.get(base_url).text
soup = BeautifulSoup(html, 'html.parser')

div_selection = soup.find("div", {"class":"section"})
#print(div_selection)

div_all = div_selection.find_all("div")

for d in div_all:
    if "id=\"post" in d:
        title = d.find("div",{"class":"contentBlockCopy"})
        image = d.find("div",{"class":"contentBlockImage"})
        print(title, image)
    
    
    if "PWN2OWN IRELAND" in title and "DAY" in title:
        next_url = image.find("a")
        print(next_url)
        print()
        sys.exit()
    

print("=")
"""

url = "https://www.zerodayinitiative.com/blog/2024/10/24/pwn2own-ireland-2024-day-three-results"
# 1. 내부 페이지 안에 값 크롤링

html = requests.get(url).text
soup = BeautifulSoup(html, 'html.parser')

# 특정 요소 선택
div_element = soup.find("div", {"class": "sqs-layout sqs-grid-12 columns-12"})

# (1) 개요
div_init = div_element.find_all("div", {"class": "sqs-block html-block sqs-block-html"})
# 1-1
content = div_init[0].find("p")
#print(content.get_text(strip=True))
#print()
# 1-2
start = div_init[1].find("p")
#print(start.get_text(strip=True))
#print()

# (2) 리더보드 사진


# (3) 진행결과 내용 수집
list_process_result = div_element.find_all("div", {"class": "sqs-block markdown-block sqs-block-markdown"})
list_tried = []

tried = ''
target = ["SUCCESS","COLLISION", "FAILURE"]
for p in list_process_result:
    content = p.get_text(strip=True)
    port_tried = content.split(".")
    for t in port_tried:
        if len(t) != 0:
            if target[0] in t:
                #print("test1")
                list_tried.append(tried)
                tried = t
                
            elif target[1] in t:
                #print("test2")
                list_tried.append(tried)
                tried = t
            elif target[2] in t:
                #print("test3")
                list_tried.append(tried)
                tried = t
            continue
        tried += t
#print(len(list_tried))

frame = """
<
    team : 
    activity : 
    point : 
>
"""

file_name = url.split('/')[-1]
file_path = "./docs/" + file_name + ".txt"
print(file_path)
with open(file_path,'w') as f :

    for t in list_tried:
        if t == '':
            continue
        
        f.write(t)
        f.write(frame)
        f.write("\n\n")
        print(t,end='')
        print(frame)
        print('\n')
```
{% endraw %}



⇒ 디스코드 봇까지 알려주는 자동화 만들까 했지만 패스


→ 일단 day4까지 크롤링 완료



## 2. Result



### 2-1. Full Scheduel



### 2-2. Day1



### 2-3. Day2



### 2-4. Day3



### 2-5. Day4



## 3. Summary

