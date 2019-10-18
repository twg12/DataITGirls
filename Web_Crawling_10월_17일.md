pandas: datareader (네이버 주식정보 판다스로 옮기기)

## Part 1: Web Crawler
Web scraping
*  `Client`(aka. code) asks `server` to send `results`

Fetching Webpages
* Crawler를 이용하는 것과 본질 같다

How Web Works
* Name resolution: IP 주소 찾기
* TCP: IP위 Protocol (신뢰성 있는 통신 가능케함)
    * 패키지 단위로 들어오는데 그런 데이터를 정리 시켜줌
* Certificate, public key exchange
    * 내가 누군지 암호로 확인 등
* HTTP Requests, Response
    * 요청한 기능, 반응
* Pharsing: 해석하는 과정, 어떻게 정보들을 표시할 것인가

Robots.txt (알아두면 좋은 것)
* Web server에 이 파일을 넣으면 크롤러들이 들어와서 어떤 것들을 가져가도 될지 안될지 보고 크롤링한다.
* 강제성은 없으나 규약이다

## Part 2: HyperText Transfer Protocol (HTTP)
Hyperlink
* Interchangeable하게 이용하지만 사실 hyperlink를 포함하는 text를 hypertext라고 한다

HTTP Request
* Get: 읽기, 가져오기
* Key-value pair: 서버요청을 보낼 때 쓰인다
* Postman: 요청을 보내서 low level에서 요청의 모습과 결과의 raw 모습을 볼 수 있도록 돕는 프로그램

실습 1
* 새로운 Request -> collection
* Get http://www.google.com (오른쪽 위 코드 누르기)
* www.google.com?q=검색어
* www.google.com?hl=언어-나라 
    * hl은 host language를 뜻함
    * 예) ko-kr

Post란?
* Get와 다르게 쓰기 or 업데이트 같은 느낌  
    * 서버값 변경
    * Form 작성하고 보낼 때
* Mobile인척 웹 크롤링
    * Postman: headers로 들어가서 ㄱㄱ

실습 2 (Do in vsc commandline)
* import requests
* work cd 검색어 / workon 검색어 / pip install requests
* url = 'http:// ㅡ '
    * requests.get(url)
    <response [200]>
    * resp = requests.get(url)
    resp: <response [200]>
    * dr(resp)
    * resp.text.split('\n')[n:n]
        * [:]는 source code따라 다르니 유의할 것
* newspaper 3k: 라이브러리
    * pip install newspaper3k
* newspaper.readthedocs.io
    * 웹사이트 실습에 사용
* dir(article)
    * article.title > 페이지 제목이 딸려옴
    * article.summary
    * article.text
    <a href = "___">Link</a>
    ```
    requests.get()
    HTML phrasing
    finding all lists
    ```
    while queue: 
        crawl()

