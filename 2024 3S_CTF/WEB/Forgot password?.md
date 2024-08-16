<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "Forgot password?"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/317336d9-ee83-42a0-af91-46d866fa6999)

<br>

 </br>
 <br>

 </br>
 
## app.py

```
from flask import Flask, render_template, render_template_string, request, flash, session, redirect, url_for, jsonify
from flask_mail import Message, Mail
from concurrent.futures import ThreadPoolExecutor
import os, re, hashlib, random

app = Flask(__name__)
executor = ThreadPoolExecutor(5)

if os.getenv("FLASK_ENV", "production") == "development":
    from config import Development as AppConfig
else:
    from config import Production as AppConfig

app.config.from_object(AppConfig)

mail = Mail(app)
app.config.pop("MAIL_PASSWORD")

Users = [{"username":"admin", 
          "email":"admin@3sctf.admin.com", 
          "password":os.urandom(32)},]

def send_Email(recipients:list, title, body):
    try:
        msg = Message(title, sender=app.config["MAIL_USERNAME"], recipients=recipients) # recipients must be a list
        msg.body = body
        with app.app_context():
            mail.send(msg)
        result = "success"

    except Exception as e:
        result = "fail"

    return result

def generate_verification_code():
    verification_code = random.randint(100000, 999999)
    return verification_code

@app.route("/login", methods=["GET", "POST"])
def login_page():
    if "user" not in session:
        if request.method == "GET":
            return render_template("login.html")
        else:
            try:
                email = request.form["user_email"]
                pwd = request.form["password"]
                user = list(filter(lambda user : user['email'] == email and user['password'] == pwd, Users))
                if user:
                    if user[0]["email"]=="admin@3sctf.admin.com":
                        admin_info = Users[0]
                        admin_info["password"] = os.urandom(32)
                        Users[0] = admin_info
                    session["user"] = user[0]["email"]
                    return redirect("/")
                else:
                    flash("Invalid email/password combination")
                    return render_template("login.html")
            except Exception as e:
                flash("An error occured")
                return render_template("login.html")
    else:
        return render_template_string("<script>alert('Log out first');location.href='/';</script>")   
    
@app.route("/logout", methods=["GET"])
def logout():
    if "user" in session:
        session.clear()
        return render_template_string("<script>alert('Logout successful');location.href='/login';</script>")
    else:
        return redirect("/login")

@app.route("/register", methods=["GET", "POST"])
def register_page():
    if "user" not in session:
        if request.method == "GET":
            return render_template("register.html")
        else:
            try:
                username_pattern = re.compile(r'^[a-zA-Z]+$')
                email_pattern = re.compile(r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$')
                if not re.fullmatch(username_pattern, request.form["username"]):
                    flash("Username can contain only letters")
                    return render_template("register.html")
        
                elif request.form["user_email"] in [emails.get("email") for emails in Users]:
                    flash("Email aleady exists")
                    return render_template("register.html")
        
                elif not re.fullmatch(email_pattern, request.form["user_email"]):
                    flash("Invalid email format")
                    return render_template("register.html")
        
                elif len(request.form["password"]) < 8 or len(request.form["password"]) > 16:
                    flash("Password must be 8-16 characters long")
                    return render_template("register.html")
        
                else:
                    Users.append({"username":request.form["username"], 
                                  "email":request.form["user_email"], 
                                  "password":request.form["password"]})
                    return redirect("/login")
            except Exception as e:
                flash("An error occured")
                return render_template("register.html")
    else:
        return render_template_string("<script>alert('Log out first');location.href='/';</script>")

@app.route("/reset_password", methods=["GET", "POST"])
def reset_password_page(code_sent=False):
    if "verification_info" in session:
        code_sent=True
    return render_template("reset_password.html", code_sent=code_sent)

@app.route("/api/send_verification_code", methods=["POST"])
def send_email():
    try:
        params = request.get_json()
        user_email:list = params["email"]

        if set(user_email).issubset(set([items.get("email") for items in Users])):
            username = list(filter(lambda user : user['email'] == user_email[0], Users))[0]["username"]
            code = generate_verification_code()
            title = "[3S CTF] Email Verification Code"
            body = f"Hello, {username}\n Your email verification code is {code}"
            
            task = executor.submit(send_Email, recipients=user_email, title=title, body=body)
            result = task.result()
            if result == "success":
                msg = "An email with verification code has been sent to your email"
                session["verification_info"] = hashlib.sha256((user_email[0]+str(code)).encode("UTF-8")).digest()
                return jsonify({"success":True, "msg":msg}), 201
            else:
                msg = "Error sending email"
                return jsonify({"success":False, "msg":msg}), 400

        else:
            msg = "Email not registerd"
            return jsonify({"success":False, "msg":msg}), 404
        
    except Exception as e:
        msg = "An error occured"
        return jsonify({"success":False, "msg":msg}), 400

@app.route("/verify", methods=["GET", "POST"])
def verify():
    if "verification_info" in session:
        try:
            email = request.form["email"]
            code = request.form["verification_code"]
            hash_value = hashlib.sha256((email+code).encode("UTF-8")).digest()
            verification_info = session["verification_info"]
            if hash_value == verification_info:
                new_password = str(random.randint(10000000, 99999999))
                user_info = list(filter(lambda user : user['email'] == email, Users))[0]
                idx = Users.index(user_info)
                user_info["password"] = new_password
                Users[idx] = user_info
                return render_template_string(f"<script>alert('Your new password is {new_password}');location.href='/login';</script>")
            else:
                return render_template_string(f"<script>alert('Invalid verification code');location.href='/reset_password?email={email}';</script>")
        except:
            return render_template_string("<script>alert('An error occured');location.href='/reset_password';</script>")
    else:
         return render_template_string("<script>alert('Unauthorized access');location.href='/';</script>")

@app.route("/")
def flag_page(FLAG="3S{FAKE FLAG}"):
    if "user" in session:
        if session["user"] == "admin@3sctf.admin.com":
            FLAG=os.getenv("FLAG")
        return render_template("flag.html", FLAG=FLAG)
    else:
        return redirect("/login")

if __name__=="__main__":
    app.run(host="0.0.0.0", port=5555, debug=True)

```
<br>

 </br>
 <br>

 </br>

코드를 살펴보니, 이메일 전송 기능을 활성화하고, Users 리스트에 사용자 정보가 포함되어 있고, send_Email 함수를 통해, 지정된 수신자에게 이메일을 전송하는 것을 알 수 있다. 또한, generate_verification_code 함수를 통해, 6자리의 무작위 인증 코드를 생성하여 반환하는 것을 알 수 있다. /login 페이지 코드 부분을 살펴보면,
사용자가 입력한 이메일과 비밀번호가 Users 리스트와 비교되고, 관리자 계정으로 로그인하면, 비밀번호가 매번 무작위로 재설정되는 것을 알 수 있다. 그 후, 사용자가 인증되면 세션에 사용자 정보를 저장하고, 홈 페이지로 이동하는 것을 알 수 있다. 
/register 부분을 보면, 회원가입하는 부분인데, 유효한 사용자 이름인지 확인하는 등 여러 확인 절차를 거치고, Users 리스트에 추가하는 것을 알 수 있다. 
/api/send_verification_code 부분은 이메일이 Users 리스트에 존재하면, 인증 코드를 생성하여 이메일로 전송하는 것을 알 수 있다. 
<br>

 </br>
 <br>

 </br>
 
```
@app.route("/")
def flag_page(FLAG="3S{FAKE FLAG}"):
    if "user" in session:
        if session["user"] == "admin@3sctf.admin.com":
            FLAG=os.getenv("FLAG")
        return render_template("flag.html", FLAG=FLAG)
    else:
        return redirect("/login")

```
 
 이 부분을 살펴보니, 사용자가 세션에 로그인되어 있고, 관리자 계정 (admin@3sctf.admin.com) 으로 로그인했을 경우, FLAG 환경 변수를 사용하여, 실제 플래그 값을 표시하도록 하는 것을 알 수 있다. 그렇지 않으면 가짜 플래그 값을 표시하는 것을 알 수 있다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/0e845faa-8862-4d03-812a-13892fd2212c)

플래그를 찾으려면, admin@3sctf.admin.com인 관리자 계정으로 로그인해야 된다. 하지만, 비밀번호가 무작위로 생성된 것이기 떄문에, 이를 얻어야 할 것 같다. 그 후, 로그인 후 홈 페이지에 실제 플래그가 바로 출력될 것 같다. 
그래서 관리자 계정 비밀번호를 재설정하면되지 않을까 하여서, http://3sctf.kro.kr:5555/api/send_verification_code 엔드포인트로 POST 요청을 보내고, ctfm4il@gmail.com 이메일 계정에서 인증 코드를 확인하려고 하였는데,
ctfm4il@gmail.com 계정으로도 딱히 로그인할 방도가 없어서, 여기서 막혀버렸다.

