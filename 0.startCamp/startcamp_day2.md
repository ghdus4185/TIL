# startcamp_day2

## Gitbash, visualstudiocode, Github 사용법

### Git bash 명령어

- ls : 현재 디렉토리(폴더)에 내용을 나열함 <ls>    
- cd : 현재 작업하는 디렉토리(폴더)를 변경함 <cd .. , cd TIL>
- echo : 문자열 출력
- touch : 파일 만들기 <touch day1.md, touch text.md >
- rm  : 파일 지우기
- rm -r : 폴더 지우기
- .  : 현재 폴더 <파일명 앞에 있으면 숨김 파일>
- .. : 상위 폴더
  - import os
  - os.chdir(r"C:\Users\student\startcamp\students") <- 여기서 r 은 raw
- git ignore 를 visualstudio에 만들고(.gitignore) 전체 복사한것을 전체 입력해서 실행해준다.



## VisualstudioCode

- import webbrowser : webbrowser 함수 불러오기

- pip install webbrowser : webbrowser 함수 다운

- webbrowser.open("naver.com") : 주소 열기

- webbrowser.open_new_tab("google.com"): 새주소 열기

#### 국내증시

  ``` python
  pip install bs4 // bs4 설치
  
  import requests
  from bs4 import BeautifulSoup
  
  res = requests.get("https://finance.naver.com/sise/").text 
    // naver 국내증시 정보를 받아옴
  soup = BeautifulSoup(res) // BeautifulSoup을 통해서 보여줌
  print(soup)
  
  
  
  ```

#### 실시간 검색어
```python
import requests
from bs4 import BeautifulSoup

response = requests.get("http://naver.com").text
soup = BeautifulSoup(response,"html.parser")
naver = soup.select_one("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k")

print(naver.text)
```



#### 파일명 바꾸기

```python
import os

os.chdir(r"C:\Users\student\startcamp\students") #  r(raw) 날것의 의미
for filename in os.listdir("."):
    os.rename(filename, filename.replace("SAMSUMG_","SSAFY_"))
```

```python
import os

os.chdir(r"C:\Users\student\startcamp\students") #  r(raw) 날것의 의미
for filename in os.listdir("."):
    os.change(filename, ("SAMSUMG_") + filename)
```



#### 환율 보기

```
import requests
from bs4 import BeautifulSoup

url = "https://finance.naver.com/marketindex/exchangeList.nhn"
res = requests.get(url).text
soup = BeautifulSoup(res,"html.parser")
#tbody = soup.select_one('tbody')
#tr = tbody.select('tr')
tr = soup.select('tbody > tr')
for r in tr:
    print(r.select_one('.tit').text.strip()) #text는 문자열을 뽑아온것 
    # strip 함수는 문자열 왼쪽과 오른쪽을 날려주는 함수
    print(r.select_one('.sale').text) #.이 클래스임
```



#### 파일 자동으로 만들기

```python
from faker import Faker
import os

f = Faker('ko_KR') # 한국어 버전

for i in range(100):
    filename = f"{i}_{f.name()}.txt"
    cmd = f"touch {filename}"
    os.system(cmd)
```



#### 리스트 검색하기

```python
import webbrowser

url = "https://search.naver.com/search.naver?sm=top_hty&fbm=1&ie=utf8&query="
my_keywords = ["JIAN","IU","SANA"]
for my_keyword in my_keywords:
    webbrowser.open(url + my_keyword)
```






## Github (버전관리시스템)
-  git init  :  git을 통해 관리 가능
- git add . : 현재 폴더 안에 있는 모든 파일과 모든 폴더를 git에 추가해서 관리
- git commit -m"제목" : git에 올라갈 세부제목을 정함
- git remote add origin : 
- mv : 파일을 이동시키기/이름바꾸기
- github 사이트에서 + 버튼 누르고 new repository 클릭
- git push origin master : github에 정보 저장
- git clone 복사한 주소 : 다른 사람이 올린 파일을 다운
- git pull origin master : 내가 올린 파일을 다른 사람이 수정한걸 다운
- add : 커밋할 목록에 추가
- commit  : 커밋 만들기
- push : 현재까지의 역사(commits)가 기록되어 있는 곳에 새로 생성한 commits 반영하기
- 관리 잘된 github 예시 -> https://github.com/milooy/TIL 또는 TIL 검색해서 잘한 사람꺼 보기