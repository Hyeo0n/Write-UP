<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "sick xss"  Write-UP</h1>
</body>
<br>
<br>
</html>

문제를 다운받으니, 여러 파일들이 있었다. 
<br>

 </br>
 <br>

 </br>
 
## app.js

```
const express = require('express');
const cors = require('cors');
const BP = require('body-parser');
const puppeteer = require('puppeteer')
const cookieParser= require('cookie-parser');
const app = express();
const session = require('express-session');
const bodyParser = require('body-parser');
const qs = require('querystring');

app.set('view engine', 'ejs');
app.use(express.urlencoded({extended:true,}));
app.use(cookieParser());

app.use(session({
    secret: '##@#@#@#@##@#@#@##@',
    resave:false,
    saveUninitialized:true
}));

if(!process.argv[2]){
    console.log("ERROR");
    return -1;
}

const port = process.argv[2];
const bot_URL = 'http://127.0.0.1:'+port;

const bot = async (report, flag) => {


    const cookies=[
        {name: "flag", value: flag, domain: "127.0.0.1:"+port}
    ]

    const browser = await puppeteer.launch({
        headless: true,
        //executablePath: "/usr/bin/google-chrome-stable",
        args: ["--no-sandbox"],
      });
    
    const page = await browser.newPage();
    await page.setCookie(...cookies);

    await page.goto(bot_URL+'/echo');
    await setTimeout(()=>{},300);

    await page.waitForSelector("#echo");
    await page.evaluate((re)=>{
        document.querySelector("#echo").value=re;
    },report);

    await page.waitForSelector("#submit");
    await page.click("#submit");
    await setTimeout(()=>{},100);

    await page.waitForNavigation();
    await page.close();
    await browser.close();

}


var db = [
    {'row':0, 'id': 'admin','title':'TEST','contents':'test'}

]

app.get('/', (req, res) =>{
    
    res.render('index')
})

app.post('/echo', (req,res)=>{
    const filter_string = /\s|document|cookie|alert|on|script|`|{|=|;|<|>/i;
    const more_string = /^[a-z]{4,}$/;

    if(req.body.echo){
     if(req.body.echo.match(filter_string) != null || req.body.echo.match(more_string)){
        res.send('Filtered');
        return 0;
        }
        res.send("<script>"+req.body.echo+"</script>");
    }
    else
        res.send('?echo=input_echo_string')
})

app.get('/echo',(req,res)=>{
    res.render('echo');
})


app.post('/bot', (req,res)=>{
    if(!req.body.report){
        res.render('bot');
        return 0;
    }

    bot(req.body.report,'3S{**REDACTED**]').then(()=>{
        res.send("<script>alert('GOOD!');history.back();</script>");
    }).catch((e)=>{
        console.log(e)
        res.send("<script>alert('NOPE!');history.back();</script>");
    })
    
})
app.get('/bot',(req,res)=>{
    res.render('bot');
})


app.listen(port, ()=>{
    console.log("Server started:"+port)
})
```
코드를 살펴보자
<br>

 </br>
 <br>

 </br>
 
/bot 엔드포인트는 report를 제출하는 페이지를 렌더링하는데, 플래그는 이것과 관련이 있어보이고, 여기에 접근해보려고 한다. 플래그는 `bot` 함수 호출 시 전달되는 것 같다. /echo 엔드포인트는 입력된 문자열을 필터링하여 스크립트로 반환하는 것 같았다. 이를 통해 XSS 공격을 시도해보면 플래그 값을 얻을 수 있지 않을까 생각하였다.
또한, 플래그가 bot 함수 내부에서 쿠키로 설정되는 것을 확인할 수 있다.
근데 /echo 페이지 코드를 보니,

```
app.post('/echo', (req,res)=>{
    const filter_string = /\s|document|cookie|alert|on|script|`|{|=|;|<|>/i;
    const more_string = /^[a-z]{4,}$/;
    if(req.body.echo){
        if(req.body.echo.match(filter_string) != null || req.body.echo.match(more_string)){
            res.send('Filtered');
            return 0;
        }
        res.send("<script>"+req.body.echo+"</script>");
    } else {
        res.send('?echo=input_echo_string');
    }
});

```
위에 여러 문자열들이 필터링되는 것 같았다. report 값에 필터링이 되지 않게 우회하는 스크립트를 넣어야 할 것 같다는 생각이 들었다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/51a1b080-1e09-4185-8e14-4e330fdfa075)

위 사진처럼, /bot 페이지에 필터링되지 않게 수정한 페이로드를 입력하고, 

<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/69d0a4f4-26eb-48b3-a616-6a64746b32ea)

위와 같은 /log 엔트포인트로 들어오는 요청을 받아 플래그 값이 있는 쿠키 값을 콘솔에 출력하돌고 하는 코드를 작성하여, 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/0c4e0897-9b08-432b-bb62-a3ebdc48bc75)

이를 실행시켜 보니, 웹 브라우저에서는 Cookie received 라고 쿠키를 전달받았다고 되어 있으나, 콘솔 창에서는 cookie : undefined이라고만 나오고 찾고자 하는 값이 나오지 않았다...
