# Visualstudio
## 정보를 담은 txt file 만들기(정렬 안됨)
```python
f = open("student.txt",'w') # student.txt를 연다. w(write)라는 방식으로 
f.write("안녕하세요") # 안녕하세요를 txt 파일안에 적는다.
f.close() # 닫는다.
```

```python
import requests
from bs4 import BeautifulSoup


url = "https://finance.naver.com/marketindex/exchangeList.nhn"
res = requests.get(url).text
soup = BeautifulSoup(res,"html.parser")

tr = soup.select('tbody > tr')
with open("exchange.txt",'w') as f:  # open함수를 f라고 하겠다, 
    							# w(write 앞에서부터 덮어쓰기),r(read읽기)a(add추가)
    for r in tr:
        f.write(r.select_one('.tit').text.strip()) 
        f.write(r.select_one('.sale').text + "\n")  # \n는 줄바꿈
```

### CSV(정렬 쉬움)으로 코인 정보보기
```python
import requests 							#requests 내장함수 불러오기
from bs4 import BeautifulSoup				#BeautifulSoup 불러오기
import csv									#csv 불러오기

url = "https://www.bithumb.com/" 			#크롤링할 주소를 URL에 저장한다.
res = requests.get(url).text				#원하는 text만 response에 저장한다.
soup = BeautifulSoup(res,"html.parser")		#BeautifulSoup함수를 이용해서 이쁘게 바꿔준다.

tr = soup.select('tbody > tr')				# ???
with open("bitsum_coin.csv",'w', encoding="utf-8", newline="") as f: # 
    csv_writer = csv.writer(f)
    for r in tr: 							#for문을 통해서 
        print(r.select_one('.blind').text.strip() + r.select_one('.sort_real').text)
        row = [r.select_one('.blind').text.strip(), r.select_one('.sort_real').text]
									        #한줄에 2개씩 넣겠다.
        									# class를 확인해서 ('.class')를 적어준다.
        csv_writer.writerow(row) 			#writerow함수를 이용해서 row를 write 한다.
```

### CSV에 정보 넣어서 만들기 

``` python
import csv

lunch = {
    "BBQ" : "123123",
    "중국집" : "544658",
    "교촌" : "157538"
}


with open("lunch.csv", 'w', encoding="utf-8", newline="") as f : # 인코딩은 파일형식을 바꿔주는것. 우리는 utf를 많이 씀
    csv_writer = csv.writer(f)    # 파일을 연걸 너가 write

    for item in lunch.items():
        csv_writer.writerow(item)   #csv_writer야 item좀 write
```





## HTML

```PYTHON
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="./intro.css">
    </head>
    <body>

    <h1>HTML</h1>
    <h1 class="blue">CSS</h1>
    <h2 class="blue">HyperText Markup Language</h2>
    <a href="https:www.naver.com">네이버</a>
    <a href="https:www.google.com">삼성전자</a>
<!--<태그이름 속성명 = "속성값" 속성명2="속성값2">내용</태그이름>-->

<h3>우리가 공부한것</h3>
<ol>
    <li> <strong><i>파이썬</i></strong></li>
    <li>HTML</li>
    <li id="git" class="blue">Git</li>
   </ol>    
  </body>
</html>
```

## 앞에 만든 HTML과 css를 link로 연결시키기

```python
/* 여기는 css 파일입니다. */

h1{
    background-color:rgb(149, 238, 149);   
}
a{
    color:rgb(49, 155, 102);
}
.blue{
    background-color:rgb(24, 154, 206);
}
#git {
    background-color:greenyellow;
}
a {
    font-family: verdana;
    font-size: 200px;
  }
</style>
```






## 명령어

- ctrl + /  : 주석처리
- shift  + tap : 들여쓰기



#### 크롤링

크롤링 할 떄 class = '___' 을 홈페이지에서 확인해서 r.select_one으로 선택한 정보를 print하고 csv_writer.writer(row)를 통해서 csv파일을 만든다.



css 는  끝에 :를 붙여준다.



## Github에 자료 저장

git init 												# git 사용 on
git add . 											 # 현재 폴더 추가
git commit -m "My profile"			 # 이름 만들기
git remote add origin https://github.com/ghdus4185/ghdus4185.github.io.git / 올릴 홈페이지
git push origin master					# Github에 올림



## 용어정리
- HTML : Hyper Text Markup Language 뼈대(구조) 구성, 문서랑 문서랑 연결되어있다.
- CSS : 표현, 색깔 디자인
- JAVAscript : 동적인 역할 수행
- href : Hyper reference 참조