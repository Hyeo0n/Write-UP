<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "Untitled" (WEB) Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/d6053db2-2d3b-4cf2-89f8-289d12a47302)
 <br>

 </br>
해당 문제를 해결해보기 위해, 문제 파일을 다운받아서 살펴보았다. 
<br>

 </br>

![image](https://github.com/user-attachments/assets/2950495c-8113-4a89-8338-42488429c690)

> flag.php
<br>

 flag.php 파일을 살펴보니, 플래그 값을 정의하고 있는 것을 확인할 수 있었다. 실제 flag 값은 "**REDACTED**"로 감추어져 있는 것을 확인할 수 있었다.  
<br></br><br></br>

 ![image](https://github.com/user-attachments/assets/812a7d59-93de-42b0-8ed5-f5ce2ecc9b21)

> index.php
<br>
 다음 index.php 파일을 살펴보니, 사용자가 입력한 'test' 파라미터를 처리하는 내용인 것을 알 수 있었다. preg_match 함수 부분을 보니, 문자열을 검사하는데, 괄호 안에 담긴 'flag', 'etc', 'test', 'passwd' 등등이 하나라도 포함되는지 검사하고, 포함하면 "hi"를 출력하게 하는 코드임을 알 수 있었다.
 위의 문자열 검사 조건과 phpinfo가 아닌 나머지 경우들은, 일단 입력받은 값에서 공백을 제거시킨다. 그리고 include문 뒤에 현재 위치 (./)에다가 앞서 공백을 제거한 값을 사용하여 PHP 파일을 포함시켜 실행시키고, flag 변수를 출력하는 코드인 것을 알 수 있었다. 말이 조금 온전치 못한 거 같은데, 예를 들어 입력받은 값이 A B C면, ABC로 공백을 제거하고, ./ABC.php처럼 된다는 뜻이다!
<br>
</br>
코드를 살펴보니, 주소창에 입력하는 URL의 'test' 파라미터 값으로, 존재하는 파일명을 입력하되, 앞서 살펴본 'flag', 'etc', 'test' 등 단어를 포함하지 않는 값을 입력하면, $flag 변수가 출력되어, 플래그 값을 알아낼 수 있을 것 같았다. 이것 저것 입력해보니, 계속 hi 창만 엄청 떴다. 그러다가, 어차피 나중에 공백 처리 되니까 그냥 한 칸만 띄어써보자 하고,
<br>
 </br>
 
`http://3s-ctf.kro.kr:20010/?test=fl ag`
<br>
</br>
을 입력해보니, 
<br>

 </br>
 
![image](https://github.com/user-attachments/assets/c0f0da08-840a-410c-a9e1-f26459f8bd0e)
<br>
 </br>
 화면 창에 flag로 추정되는 값이 출력되었다.
  <br>

 </br>
 
 ![image](https://github.com/user-attachments/assets/2db6bf0d-31b4-4e2d-aba4-1437c50b72cf)
  <br>
 </br>
 최종적으로 해당 `3S{re_re_re_r3_Re_re_RE_re_re_GEx}`
가 flag 값인 것을 확인할 수 있었다!
