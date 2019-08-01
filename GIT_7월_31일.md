# 7월 31일

## Gistory
* .index: file 이름 -누르면 수정 내역이 뜬다

반방향 암호: 단어를 긴 코드(sha1)으로 바꿀 수 있지만 그 반대로는 불가능하다.

## Command Line
* ls > [filename].txt: 새로운 파일 생성
* 무한반복으로 업데이트
```sh 
while true
do
curl -H "User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) AppleWebKit/537.36 (KHTML, like>  Gecko) Chrome/44.0.2403.89 Safari/537.36" https://datalab.naver.com/keyword/realtimeList.naver > naver.html
git commit -am "update naver"
sleep 5
done
```
* vi [filename].txt: 새로운 파일을 만든다.
    * i: 입력 모드
    * ctrl + c: 명령어 모드
        * : 을 누르면 입력 가능
        * w 을 누르면 저장
        * q 를 누르면 나간다

## Backup
* Make repository on GitHub
* git remote add origin [SSH_Address]
* git remote -v: check repository details
* git push --set-upstream: remote 저장소가 누군지를 세팅한다

## 비밀번호 세팅
1. ssh-keygen
2. Enter * 3
3. cd [저장주소]
4. ls -al
5. cat id_rsa.pub
    * pub: public
    * password has been created

## Command Line (협력)
* Pull = 불러내다
* Push = 저장하다
* Pull = Fetch + Merge