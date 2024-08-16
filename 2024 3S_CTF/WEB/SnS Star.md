<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "SnS Star"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/aa9b0f1b-6fbc-4f5f-aae0-c801d2f5fdd6)

문제 파일 중에서 app.py를 살펴보려 한다.

<br>

 </br>
 <br>

 </br>
 
```
from flask import *
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
import base64, re, time

app = Flask(__name__)
app.secret_key = 'your_secret_key_here'

users = [{'id': 'admin', 'pw': '***secret***', 'post': {'imgname': 0, 'likes': 0, 'adminlike': 0}}]
flag = "3S{***secret***}"

def check_url(url):
    driver = None
    try:
        service = ChromeService(executable_path=ChromeDriverManager().install())
        driver = webdriver.Chrome(service=service)
    
        driver.get("http://127.0.0.1:5550/login")
        id_box = driver.find_element(By.NAME, 'id')
        pw_box = driver.find_element(By.NAME, 'pw')
        id_box.send_keys(users[0]['id'])
        pw_box.send_keys(users[0]['pw'])
        button = driver.find_element(By.NAME, 'login')
        button.click() 
        driver.implicitly_wait(3)
        
        try:
            alert = driver.switch_to.alert
            alert.accept()
        except:
            pass

        driver.get(url)
        time.sleep(10)
    except Exception as e:
        return False
    finally:
        if driver:
            driver.quit()
    return True

def check_photo(filename):
    allowed_extensions = {'jpg', 'jpeg', 'png', 'gif'}
    if '.' in filename:
        extension = filename.rsplit('.', 1)[-1].lower()
        return extension in allowed_extensions
    return False

def base64_encode(str):
    bytes = str.encode('UTF-8')
    result = base64.b64encode(bytes)
    return result.decode('ascii')

def base64_decode(str):
    str_bytes = str.encode('ascii')
    result = base64.b64decode(str_bytes)
    return result.decode('UTF-8')

@app.route('/', methods=['GET', 'POST'])
def index():
    if 'id' not in session :
        flash("로그인이 필요한 서비스입니다")
        return redirect('/login')
    user_id = session['id']
    current_user = next((u for u in users if u['id'] == user_id), None)
    if not current_user:
        return redirect('/login')
    flag_msg = flag if current_user and current_user['post']['adminlike'] else None
    return render_template('index.html', user=current_user, flag=flag_msg)

@app.route('/profile/<id>', methods=['GET','POST'])
def profile(id):
    if 'id' not in session:
        flash("로그인이 필요한 서비스입니다")
        return redirect('/login')    
    current_user = next((u for u in users if u['id'] == id), None)    
    return render_template('profile.html', user=current_user, photoname=base64_decode(current_user['post']['imgname']) if current_user['post']['imgname'] else None) 

@app.route('/image_upload', methods=['POST'])
def upload():
    if 'id' not in session:
        flash("로그인이 필요한 서비스입니다")
        return redirect('/login')
    user = next((u for u in users if u['id'] == session['id']), None)
    if not user:
        return redirect('/login')
    f = request.files['file']
    if not check_photo(request.form['photoname']) or not check_photo(f.filename):
        return '사진 파일만 올려주세요'
    filename = base64_encode(request.form['photoname'])  
    if user and f:
        f.save(filename)
        user['post'] = {'imgname': filename, 'likes': 0, 'adminlike': 0}
        flash("프로필 사진 업로드를 완료하였습니다.")
        return redirect('/')
    return redirect('/login')

@app.route('/image_update', methods=['POST'])
def update():
    if 'id' not in session:
        flash("로그인이 필요한 서비스입니다")
        return redirect('/login')
    user = next((u for u in users if u['id'] == session['id']), None)
    if not user:
        return redirect('/login')
    f = request.files['file']
    if not check_photo(request.form['photoname']) or not check_photo(f.filename):
        return '사진 파일만 올려주세요'
    filename = base64_encode(request.form['photoname'])   
    if f:
        f.save(filename)
        user['post'] = {'imgname': filename, 'likes': 0, 'adminlike': 0}
        flash("프로필 사진 업데이트를 완료하였습니다.")
    return redirect('/')


@app.route('/follow/<id>', methods=['POST'])
def follow(id):
    user = next((u for u in users if u['id'] == id), None)
    if user:
        if session['id'] == 'admin':
            user['post']['adminlike'] += 1
        else:
            user['post']['likes'] += 1
            flash(f"{user['id']}님을 팔로우합니다.")
    return redirect('/')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    else:
        if 'id' in session:
            return redirect('/logout')
        for u in users:
            if u['id'] == request.form['id'] and u['pw'] == request.form['pw']:
                app.config['SESSION_COOKIE_HTTPONLY'] = False
                session['id'] = u['id']
                flash("로그인에 성공하였습니다.")
                u['post'] = {'imgname' : None, 'likes': 0, 'adminlike' : 0 }
                return redirect('/')
        flash("아이디 혹은 비밀번호가 일치하지 않습니다.")
        return redirect('/login')

@app.route('/sign', methods=['GET', 'POST'])
def sign():
    if request.method == 'GET':
        return render_template('sign.html')
    else:
        if not re.match(r'^[a-zA-Z0-9]+$', request.form['id']):
            return '아이디는 숫자, 알파벳으로만 구성할 수 있습니다.'
        if any(u['id'] == request.form['id'] for u in users):
            flash("이미 존재하는 id 입니다.")
            return redirect('/login')
        flash("회원가입에 성공하였습니다.")
        users.append({'id': request.form['id'], 'pw': request.form['pw'], 'post': {'imgname': None, 'likes': 0, 'adminlike': 0}})
        return redirect('/login')

@app.route('/logout', methods=['GET', 'POST'])
def logout():
    session.clear()
    return redirect('/login')

@app.route('/admin_dm', methods=['GET', 'POST'])
def dm_for_admin():
    if 'id' not in session:
        return redirect('/login')
    if request.method == 'GET':
        return render_template('dm.html')
    else:
        url = f"http://127.0.0.1:5550/{request.form['url']}"
        try:
            check_url(url)
            flash("URL 접속을 완료했습니다.")
        except Exception as e:
            flash("URL 접속 중 오류가 발생했습니다.")
        return redirect('/')

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=5550)

```
코드를 살펴보니, /profile/<id> 은 프로필 페이지를 렌더링하는데, 사용자가 업로드한 프사를 Base64로 디코딩하여 보여준다.
/image_upload는 사용자가 프사를 업로드하는 곳이고, /admin_dm은 관리자가 사용자가 입력한 특정 URL로 접근할 수 있도록 한다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/b9777995-60e4-4bea-8e33-10164c399400)
![image](https://github.com/user-attachments/assets/674abbcf-c568-487d-a886-b35210ae6005)

한 번 서버에 들어가서, blu라는 계정으로 회원가입 후, 로그인하여,
<br>

 </br>

![image](https://github.com/user-attachments/assets/9327bba6-43ea-4de0-a443-dca0d38b2161)
![image](https://github.com/user-attachments/assets/e8530909-b132-44fa-b365-02a74f998244)

/profile/admin을 파라미터에 입력한 후, admin 계정을 팔로우해보았다. 
<br>

 </br>
 <br>

 </br>

문제 설명에도 나왔듯이 내가 admin 계정으로 로그인하여, 내 계정을 팔로우해야지 플래그 값을 얻을 수 있을 것 같았다.
그래서 어떻게 하면 플래그 값을 얻을 수 있을까 하다가 딱히 방법이 떠오르지 않아서, 기존의 app.py를 수정해보기로 했다.

![image](https://github.com/user-attachments/assets/c6d5b948-42c1-401e-b22f-a29d0469b230)
<br>

 </br>
 <br>

 </br>
 다음과 같이, admin 계정의 비밀번호를 임의로 1234로 바꾸고, 저장 후, 터미널을 이용하여, Flask 애플리케이션을 실행시켜, 서버에 들어가보았다. 
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/28c61720-0654-40a9-84b3-89247057e838)

일단 blu라는 계정으로 회원가입을 하고,
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/07129fd0-8da1-44b4-b199-50b34bb3e12e)
![image](https://github.com/user-attachments/assets/56d4784c-7573-4a1c-9bc5-927f46f3ef35)
admin - 1234로 로그인해보니, 잘 로그인되었다.
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/d68a589c-8c0d-404a-87de-1ea4163e1931)

그래서 /profile/blu 페이지로 들어가서, blu 계정을 로그인해보려고 했는데, 팔로우 버튼을 누르면 자꾸 맨 처음 로그인 화면으로 돌아가서, 제대로 실행되지 않았다.
여기서 어떻게 해야될지 막혀서, 플래그를 찾지 못했다.



