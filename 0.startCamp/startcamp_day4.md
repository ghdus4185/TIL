# STARTCAMP_DAY4



## flask

- flask를 사용해서 나만의 주소를 만들어보자
- 만들어진 주소를 base로 두고 html을 만들어보자



## 단축키

! + tab : html 구조 자동 생성

<!-- : html에서 주석 달기



### 나의 첫번째 웹페이지
```python
@app.route("/")                          # route 는 경로를 받아주는 것 , / : 최상단을 의미한다. 서버의 루트 주소를 의미
def hello():                             # hello함수 정의
    return "Hello World!"                # 결과 Hello World 

-----------------------------------------------------------------------------

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>안녕하세요</title>
</head>
<body>
    <h1>여기는 나의 첫번쨰 웹페이지</h1>
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```



## 점심 메뉴 선택 + 이미지

```python
import random
@app.route("/lunch")
def lunch():
    menu = {
        "짜장면" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRDwCVx1BzwRDkn4Ru-C4WVxB7D42OPLqVxotnrs8vywFHZRmEK",
        "짬뽕" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSezu9IiSd-pAEFpvbBTqZ8nQVncMwRV8oIz2xuxpqRO4d7PRVL",
        "볶음밥" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTjK-n6Ve_4mU9sw-r3BX-oQ4WlNJo3pTOpsaC75rNlQLv39WFO",    
    }

    menu_list = list(menu.keys()) # ["짜장면","짬뽕","스파케티"]
    pick = random.choice(menu_list)
    img = menu[pick] # dictionary에 있는 key 접근은 []로 한다.
    return render_template("lunch.html", pick=pick, img=img)
-----------------------------------------------------------------------------

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h3>오늘의 점심은 {{pick}} 어떠세요??</h3>
    <img src="{{img}}" alt="">
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```



### ping-pong : 첫 번째 html에서 입력 ,두 번째 html에서 출력

```python
@app.route('/ping')
def ping():
    return render_template("ping.html")

@app.route('/pong')
def pong():
    user_input = request.args.get("test")
    return render_template("pong.html", user_input=user_input)
-----------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>여기는 핑입니다.</h1>
    <form action="/pong">
        <input type="text" name=test>
        <input type="submit">
    </form>
</body>
</html>
----------------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>여기는 퐁입니다.</h1>
    사용자가 방금 입력한 데이터는
    <p>{{user_input}}</p>
    입니다.
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```





## 내 주소에서 검색어를 입력받아 네이버 화면을 뛰우기

```python
@app.route("/naver")
def naver():
    return render_template("naver.html")

------------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="https://search.naver.com/search.naver">
        <input type="text" name="query">
        <input type="text" name="hihi">
        <input type="submit">
    </form>
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```



## 입력받은 정보를 아스키코드로 바꿔주기

```python
@app.route("/text")
def text():
    return render_template("text.html")

import requests
@app.route("/result")
def result():
    raw_text = request.args.get('raw')     # args는 dictionary 요청에 의한 딕셔너리값중에서 raw만 뽑아서 raw_text에 담을꺼야
    url = "http://artii.herokuapp.com/make?text="
    res = requests.get(url+raw_text).text # 리퀘스트한테 get을 시킨다.
    return render_template("result.html", res=res)
---------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>입력한 글자를 아스키 코드로 바꿔줍니다.</h1>
    <form action="/result">
        <input type="number", name="raw">
        <input type="submit", value="변경!!!">   <!--확인버튼 역활-->
    </form>
</body>
</html>

---------------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>결과는 다음과 같습니다.</h1>
    <pre>{{res}}</pre>
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```



## 내가 만든 게임

```python
@app.route("/game")
def game():
    return render_template("game.html")

import random
@app.route("/game_r")
def game_r():
    cha = {
        "게임천재" : "https://opgg-com-image.akamaized.net/attach/images/20190226083413.642488.jpeg",
        "코딩천재" : "https://img.sbs.co.kr/newimg/news/20180601/201188704_700.jpg" ,
        "변태" : "https://i.ytimg.com/vi/Ghs6gdOVC4c/maxresdefault.jpg",
        "순진함" : "https://t1.daumcdn.net/cfile/tistory/0355B03A50AC7CCA0C",
        "게임바보" : "https://img.insight.co.kr/static/2018/09/20/700/p617yveo924u758a3k78.jpg"}
    
    cha_list = list(cha.keys()) 
    pick = random.choice(cha_list)
    img = cha[pick] 
    return render_template("game_r.html", pick=pick, img=img)

--------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
        <h1>신이 주신 당신의 능력은??</h1>
        <form action="/game_r">
            <input type="text", name="raw">
            <input type="submit", value="변경!!!">   <!--확인버튼 역활-->
</body>
</html>
-------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
        <h1>신이 주신 당신의 능력은??</h1>
        <h2>{{pick}}</h2>
        <img src="{{img}}" alt="">
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```





## 로또 게임

```python
@app.route("/lotto_result")
def lotto_result():
    #사용자가 입력한 정보를 가져오기
    numbers = request.args.get('numbers').split()
    user_numbers = []
    for n in numbers:
        user_numbers.append(int(n)) 

    #user_numbers = [1,2,3,4,5,6]
    #로또 홈페이지에서 정보를 가져오기
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=866"
    res = requests.get(url)
    lotto_numbers = res.json()

    winning_numbers = [] #비어있는 리스트
    for i in range(1,7): #1이상 7미만 까지 i 를 돌린다.
        winning_numbers.append(lotto_numbers[f'drwtNo{i}']) # f스트링을 쓴다. #dowNo1~6까지의 정보를 받아와서 비어있는 리스트에 넣는다
    bonus_number = lotto_numbers['bnusNo']
    
    result = "1등"

    matched = set(user_numbers) & set(winning_numbers) # 교집합 비교하는 것

    if len(matched) == 6:
        result = "1등"
    elif len(matched) == 5:
        result = "3등"
    elif len(matched) == 4:
        result = "4등"
    elif len(matched) == 3:
        result = "5등"
    else :
        result = "꽝"
    

    return render_template("lotto_result.html", u=user_numbers, w=winning_numbers, b=bonus_number, r=result)

-----------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="/lotto_result">
        <input type="text" name=numbers>
        <input type="submit">
    </form>
</body>
</html>
---------------------------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <pre>{{u}}</pre>
    <pre>{{w}}</pre>
    <pre>{{b}}</pre>
    <pre>{{r}}</pre>
</body>
</html>

if __name__ == '__main__':
    app.run(debug=True)
```

