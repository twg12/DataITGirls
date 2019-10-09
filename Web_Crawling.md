# 웹크롤링
```py
import requests
from bs4 import BeautifulSoup

res = requests
```
```py
import requests
from bs4 import BeautifulSoup
from datetime import datetime
import pandas as pd
import re
```


## 네이버 검색 웹크롤링
1. In notebook:
```py
res = requests.get('https://news.v.daum.net/v/20190927154409162') #여기서 링크는 웹크롤링하고 싶은 페이지 첨가할 것
res.content
```

2. Parser 사용: 데이터 형태를 인식, 인간이 해석할 수 있는 형식으로 정리해주는 기능
```py
soup = BeautifulSoup(res.content,'html.parser')
soup
```

3. 부분부분 크롤링해보기
```py
title = soup.find('title')
title
```
```py
print(title.get_text())
```

4. 어디서 줍줍해온 웹크롤링 자료: 알아서 정리해서 응용할 것.
```py
soup = BeautifulSoup(response,"html.parser")
results = soup.select("#section_body .photo a")

for result in results:
    print("제목: ", result.attrs["title"])
    url_article = result.attrs["href"]
    response = urllib.request.urlopen(url_article)
    soup_article = BeautifulSoup(response, "html.parser")
    content = soup_article.select_one("#articleBodyContents")
    
    output = ""
    for item in content.contents:
        stripped = str(item).strip()
        if stripped == "":
            continue
        if stripped[0] not in ["<", "/"]:
            output += str(item).strip()
    output.replace("&apos;", "")
    print(output.replace("본문 내용TV플레이어",""))
    
    time.sleep(5)

title_text=[]
link_text=[]
source_text=[]
date_text=[]
contents_text=[]
result={}

#엑셀로 저장하기 위한 변수
RESULT_PATH ='D:/Projects/Web_crawling'  #결과 저장할 경로
now = datetime.now() #파일이름 현 시간으로 저장하기

def date_cleansing(test):
    try:
        #지난 뉴스
        #머니투데이  10면1단  2018.11.05.  네이버뉴스   보내기  
        pattern = '\d+.(\d+).(\d+).'  #정규표현식 
    
        r = re.compile(pattern)
        match = r.search(test).group(0)  # 2018.11.05.
        date_text.append(match)
        
    except AttributeError:
        #최근 뉴스
        #이데일리  1시간 전  네이버뉴스   보내기  
        pattern = '\w* (\d\w*)'     #정규표현식 
        
        r = re.compile(pattern)
        match = r.search(test).group(1)
        #print(match)
        date_text.append(match)

def contents_cleansing(contents):
    first_cleansing_contents = re.sub('<dl>.*?</a> </div> </dd> <dd>', '', 
                                      str(contents)).strip()  #앞에 필요없는 부분 제거
    second_cleansing_contents = re.sub('<ul class="relation_lst">.*?</dd>', '', 
                                       first_cleansing_contents).strip()#뒤에 필요없는 부분 제거 (새끼 기사)
    third_cleansing_contents = re.sub('<.+?>', '', second_cleansing_contents).strip()
    contents_text.append(third_cleansing_contents)
    #print(contents_text)

def crawler(maxpage,query,sort,s_date,e_date):

    s_from = s_date.replace(".","")
    e_to = e_date.replace(".","")
    page = 1  
    maxpage_t =(int(maxpage)-1)*10+1   # 11= 2페이지 21=3페이지 31=4페이지  ...81=9페이지 , 91=10페이지, 101=11페이지
    
    while page <= maxpage_t:
        url = "https://search.naver.com/search.naver?where=news&query=" + query + "&sort="+sort+"&ds=" + s_date + "&de=" + e_date + "&nso=so%3Ar%2Cp%3Afrom" + s_from + "to" + e_to + "%2Ca%3A&start=" + str(page)
        
        response = requests.get(url)
        html = response.text
 
        #뷰티풀소프의 인자값 지정
        soup = BeautifulSoup(html, 'html.parser')
 
        #<a>태그에서 제목과 링크주소 추출
        atags = soup.select('._sp_each_title')
        for atag in atags:
            title_text.append(atag.text)     #제목
            link_text.append(atag['href'])   #링크주소
            
        #신문사 추출
        source_lists = soup.select('._sp_each_source')
        for source_list in source_lists:
            source_text.append(source_list.text)    #신문사
        
        #날짜 추출 
        date_lists = soup.select('.txt_inline')
        for date_list in date_lists:
            test=date_list.text   
            date_cleansing(test)  #날짜 정제 함수사용 
        
        #본문요약본
        contents_lists = soup.select('ul.type01 dl')
        for contents_list in contents_lists:
            #print('==='*40)
            #print(contents_list)
            contents_cleansing(contents_list) #본문요약 정제화
        
        
        #모든 리스트 딕셔너리형태로 저장
        result= {"date" : date_text , "title":title_text ,  "source" : source_text ,"contents": contents_text ,"link":link_text }  
        print(page)
        
        df = pd.DataFrame(result)  #df로 변환
        page += 10
            
    
    # 새로 만들 파일이름 지정
    outputFileName = '%s-%s-%s  %s시 %s분 %s초 merging.xlsx' % (now.year, now.month, now.day, now.hour, now.minute, now.second)
    df.to_excel(RESULT_PATH+outputFileName,sheet_name='sheet1')
    
        
    
    # 새로 만들 파일이름 지정
    outputFileName = '%s-%s-%s  %s시 %s분 %s초 merging.xlsx' % (now.year, now.month, now.day, now.hour, now.minute, now.second)
    df.to_excel(RESULT_PATH+outputFileName,sheet_name='sheet1')
    ```


## 네이버 API 실시간 검색
1. https://developers.naver.com/apps/#/myapps/W9vSp_O_lSvI8RgBCNL2/overview 
    * API-Key와 비밀번호 발급
2. In notebook:
```py
client_key = 'W9vSp_O_lSvI8RgBCNL2'
client_secret = 'nb1gYd649o'

url = 'https://openapi.naver.com/v1/search/news.json?query=피앤지'
header_params = {"X-Naver-Client-Id":client_key, "X-Naver-Client-Secret":client_secret}
response = requests.get(url, headers = header_params)

print(response.text)
```
3. In notebook: header를 입력해서 보안 절차를 걸침
```py
import requests
from bs4 import BeautifulSoup
headers = {
    'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',
    'accept-encoding' : 'gzip, deflate, br',
    'accept-language' : 'ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7',
    'cache-control' : 'max-age=0',
    'cookie' : 'NNB=3UQWYKSBASRFO; npic=RaKCiSYTBUmJupjHoaheVtMMN48+z+7443qDM6dmY5VMuzNi+dHg+TUBj0aF3qEDCA==; NGE2YjFlZWQtZGMyOS00NTJkLTk3ZjktNjI5OTUwZWJkMGVj_d20gGEw3=true; ASID=dc46470d0000015a478bf14000000045; _ga=GA1.2.902872023.1474298405; OWMzMzk0ZTQtM2U3Ni00OTk1LThhMWItN2M4ZTZjMDljYTBi_jH066zaQ=true; MWUxNTc0YzktM2EwNS00MmE3LTk0NWQtMjUzN2E5NGExZWYy_aBZxbHEt=true; N2RiNzA5YmQtNDJhZi00MWI0LTk3NDQtYzZlZTIyMjgxOWRk_jJuqhpbS=true; __gads=ID=45b3976df8d09b28:T=1537157380:S=ALNI_MbraQoct2O0ZBpNt92FfsgNBc8LvQ; nsr_acl=1; nx_ssl=2; nid_inf=1265552978; NID_AUT=tuxIbNQim3pius4Is/Uu6urTz1RW1uXG9uS39vflmLP1nxDnl351NG7qTodAqGxB; NID_JKL=NtOhusmkk8KBrV3fuLEI+sA2cR2coFCxk6slBvSOyEs=; NID_SES=AAABarN7ByP1N9ZFeMn6UFOGS7Iybi/+XT6+3HkJPjZFk3sOz6pcOpjzbgayhzwsKRGKMHVZ0gOdw0h7jheeCtCl/rARY1EDkX99LoiQpW0ioKPziONi+wq1eVfS3i0GIXqVfkTaAHrFzJU64K5ZyLXITaKIT3bTEfac5N1JxnaQLXSHS9kNFZNPlEXK1O7Mtsxos0U57DH1A/+oe01R6r4qt4sOVi2GEt+wKdjC9wFTphnE39C6oIHf8FUWA5ZW2z8qRvOQTkaFr8ALm/9FrjbIF3HqVuPR2JHkTZNFwDXlBYKwlqdo3mGlFi84oXVDoucRhwIw/0eyFjbZuL93CiO0e+yVHX2/y5q8/YAOYLUnXnPIlzQD5e9D3jDy7D64eDaAtLxH84YcP5ufLGopIuVacDLMm0clbU13eYt8aeSYLXTQnsZAvN3tUj3j6VaXki/zV+Q2XYsD5gccrs8Phs0Nx7cf0Yi6OCOsIlsXawDT0IqY; page_uid=UiEdxdp0Yihssc5UpJRsssssttw-026438; BMR=s=1570345335780&r=https%3A%2F%2Fm.blog.naver.com%2FPostView.nhn%3FblogId%3Dchumy%26logNo%3D60213874814%26proxyReferer%3Dhttps%253A%252F%252Fwww.google.com%252F&r2=https%3A%2F%2Fwww.google.com%2F; _datalab_cid=50000000',
    'referer' : 'https://datalab.naver.com/opendata.naver',
    'sec-fetch-mode' : 'navigate',
    'sec-fetch-site' : 'same-origin',
    'sec-fetch-user' : '?1',
    'upgrade-insecure-requests' : '1',
    'user-agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36'
}

res = requests.get('https://datalab.naver.com/keyword/realtimeList.naver', headers=headers)
```
    * `accept` 등의 headers는 F12 > networks에서 realtimeList.naver의 scheme 아래서 찾을 것

4. In notebook: 
```py
soup = BeautifulSoup(res.content, 'html.parser')
soup
```

5. In notebook:
```py
rank_list = soup.select(".rank_list.v2")[3].select('.list_area')
for rank in rank_list:
    print(rank.text)
```
    * 여기서 print(rank.text)의 text 뒤에 .split() 혹은 .strip()을 적용할 경우 리스트로 정리될 것. 

6. In notebook: 정리된 형태로 실시간 검색어 발표
```py
rank_list = soup.select(".rank_list.v2")[0].select('.list_area')
rank_result_list = []
for rank in rank_list:
    rank_result = rank.text.split()
    rank_result_list += [{"rank":rank_result[0], "value":rank_result[1]}]
    
print(rank_result_list)
print("-"*100)
print('실시간 급상승 검색어 "[]"입니다.'.format(rank_result_list[0]["rank"], rank_result_list[0]["value"]))
```

7. In notebook:
```py
from selenium import webdriver
from bs4 import BeautifulSoup
driver = webdriver.Chrome('D:/데잇걸즈/chromedriver')
driver.implicitly_wait(3)
driver.get('https://datalab.naver.com/keyword/realtimeList.naver')
html = driver.page_source
soup = BeautifulSoup(html, 'html.parser')

rank_list = soup.select(".rank_list.v2")[0].select('.list_area')
rank_result_list = []
for rank in rank_list:
    rank_result = rank.text.split()
    rank_result_list += [{"rank":rank_result[0], "value":rank_result[1]}]

print(rank_result_list)
print("-"*100)
print('실시간 급상승 검색어 {}위는 "{}"입니다.'.format(rank_result_list[0]["rank"], rank_result_list[0]["value"]))
```