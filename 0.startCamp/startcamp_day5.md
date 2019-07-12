# STARTCAMP_DAY5

- gitignore : git에 올리지 않을 파일을 적는다.
- get() : 가져오기
- post () : 보내기
- update() : 리필
- delete() : 삭제
- sorted() : 오름차순 정렬하기





## app.py

```python
from flask import Flask, request, render_template
from decouple import config
import requests
import random
from bs4 import BeautifulSoup

app = Flask(__name__)

api_url = "https://api.telegram.org"
token = config("TELEGRAM_TOKEN")
chat_id = config("CHAT_ID")
naver_id = config("NAVER_ID")
naver_secret = config("NAVER_SECRET")

@app.route("/write")
def write():
    return render_template("write.html")

@app.route("/send")
def send():
    msg = request.args.get('msg')
    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
    res = requests.get(url)
    return render_template("send.html")

@app.route(f"/{token}", methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = (data.get('message').get('from').get('id')) # 사용자 id 가져옴
    user_msg = (data.get('message').get('text')) # 사용자 텍스트 가져옴
    
    if data.get('message').get('photo') is None:
        if user_msg == "점심메뉴":
            menu_list = ["삼계탕", "철판낙지볶음밥", "물냉면"]
            result = random.choice(menu_list)
        elif user_msg == "로또":
            lotto_list = list(range(1,46))
            result = sorted(random.sample(lotto_list,6))
        elif user_msg[0:2]== "번역": #0부터2미만까지
            # 번역 안녕하세요 저는 누구입니다.
            raw_text = user_msg[3:] #3부터 쭉
            papago_url = "https://openapi.naver.com/v1/papago/n2mt"
            data = {
                "source":"ko",
                "target":"en",
                "text":raw_text

            }
            header = {
                'X-Naver-Client-Id' : naver_id,
                'X-Naver-Client-Secret' : naver_secret
            }
            res = requests.post(papago_url,data=data,headers=header)
            translate_res = res.json()
            translate_result = translate_res.get('message').get('result').get('translatedText')
            print(res.text)
            result = translate_result 
        
        elif user_msg == "뭐할까?":
            play = ["코인노래방", "LOL", "치맥", "카페", "잠자기"]
            result = random.choice(play)
        elif user_msg == "실시간검색어":
            response = requests.get("http://naver.com").text
            soup = BeautifulSoup(response,"html.parser")
            naver = soup.select_one("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k")
            nav = [naver]
            result = random.choice(nav)
        else:
            result = user_msg
    else:
        #사용자가 보낸 사진을 찾는 과정
        result="asdf"
        file_id = data.get('message').get('photo')[-1].get('file_id')
        file_url = (f"{api_url}/bot{token}/getFile?file_id={file_id}")
        file_res = requests.get(file_url)
        file_path = file_res.json().get('result').get('file_path')
        file = f"{api_url}/file/bot{token}/{file_path}"
        print(file)

    #사용자가 보낸 사진을 클로버로 전송
        res = requests.get(file, stream=True)
        clova_url = "https://openapi.naver.com/v1/vision/celebrity"
        header = {
                'X-Naver-Client-Id' : naver_id,
                'X-Naver-Client-Secret' : naver_secret
        }

        clova_res = requests.post(clova_url, headers=header, files={'image':res.raw.read()})
        
        if clova_res.json().get('info').get("faceCount"):
            # 누구랑 닮았는지 출력
            celebrity = clova_res.json().get('faces')[0].get('celebrity')
            name = celebrity.get('value')
            confidence = celebrity.get('confidence')
            result = f"{name}일 확률이{confidence*100}%입니다."
        else:
            #사람이 없음
            result = "사람이 없습니다."

        # print(clova_res.text)

    res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={result}"
    requests.get(res_url)

    return '', 200


if __name__ == "__main__":
    app.run(debug=True)
```



 ### write.html

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="/send">
        <input type="text", name="msg">
        <input type="submit">
    </form>
</body>
</html>
```



### send.html

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>성공적으로 메세지가 전달되었습니다</h1>
</body>
</html>
```



## test.py

```python
import requests
from decouple import config
token = config('TELEGRAM_TOKEN') #sopoo_bot
url = f"https://api.telegram.org/bot{token}/" #f 를 붙여 문자열 생성
user_id = config("CHAT_ID")

# send_url = f"{url}sendMessage?chat_id={user_id}&text=하이하이"
# requests.get(send_url)
ngrok_url = "https://ckdghdus.pythonanywhere.com"
webhook_url = f"{url}setWebhook?url={ngrok_url}/{token}"
print(webhook_url)

```



### gitignore

```python
.env

# Created by https://www.gitignore.io/api/python,windows,visualstudiocode
# Edit at https://www.gitignore.io/?templates=python,windows,visualstudiocode

### Python ###
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
pip-wheel-metadata/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
.hypothesis/
.pytest_cache/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# Jupyter Notebook
.ipynb_checkpoints

# IPython
profile_default/
ipython_config.py

# pyenv
.python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# celery beat schedule file
celerybeat-schedule

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/

### VisualStudioCode ###
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

### VisualStudioCode Patch ###
# Ignore all local history of files
.history

### Windows ###
# Windows thumbnail cache files
Thumbs.db
Thumbs.db:encryptable
ehthumbs.db
ehthumbs_vista.db

# Dump file
*.stackdump

# Folder config file
[Dd]esktop.ini

# Recycle Bin used on file shares
$RECYCLE.BIN/

# Windows Installer files
*.cab
*.msi
*.msix
*.msm
*.msp

# Windows shortcuts
*.lnk

# End of https://www.gitignore.io/api/python,windows,visualstudiocode
```



### .env

```python
TELEGRAM_TOKEN='883822782:AAGR8D4gOVxQjLAgDTQre07aoL_Uip5XIhg'
CHAT_ID='822223745'
NAVER_ID='z8gRiEoU8rq7dYLMgJ_p'
NAVER_SECRET='lc7VdyiESt'
```

