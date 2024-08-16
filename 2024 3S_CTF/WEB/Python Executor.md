<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "Python Executor"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/b0ab82d1-e9c2-4012-b0bc-189182c421e0)

문제 파일을 살펴봐야 겠다.
<br>

 </br>
 <br>

 </br>
 
## app.py

![image](https://github.com/user-attachments/assets/43ad99dc-5db4-4f90-b31c-2a920d6bad5c)

'My_Injection_filter' 는 명령어 인젝션을 방지하기 위해 차단해야 하는 특정 문자열의 리스트이다.  
'$','{','}','|','&',';','\n','!','?','=','*','`',',','"','flag','system'와 같은 문자열을 필터링한다.
'index' 함수에서 'base_command'에 사용자가 제공한 파일명을 추가하는 부분이 보인다. 
<br>

 </br>

문제 파일을 살펴보니, flag.png 파일이 있던데, 이를 읽어오도록 하면 flag 값을 얻을 수 있지 않을까 생각했다. 그래서 서버에 요청을 보내 플래그를 출력하게끔 하는 밑에와 같은 python 코드를 작성해보았다.

![image](https://github.com/user-attachments/assets/13e89a8b-eb63-41b6-b1ed-27d406f4b541)

<br>

 </br>
 <br>

 </br>
 
근데, '$','{','}','|','&',';','\n','!','?','=','*','`',',','"','flag','system'와 같은 문자열은 필터링되기 때문에, 이 필터링을 우회하기 위해, 위에서 작성한 코드를 유니코드로 변환해보기로 하였다.

![image](https://github.com/user-attachments/assets/e3652b0a-77a7-44dc-b2c4-f36e0dc95314)
변환한 후, 이 파일을 실행시켜보니, 

<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/902fb9d3-e2d8-43ba-a653-d9e608ac2186)

? 하나만 출력되고, 딱히 바뀌는 것이 없었다...
