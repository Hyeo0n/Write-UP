<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "It's my secret"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/2d72627d-99c5-43c0-af3e-247446580784)

<br>

 </br>
안드로이드 문제인 It's my secret 문제를 풀어보려고 한다. 일단 위 문제 사진처럼 녹스 애뮬레이터를 사용해야 되는데, 기존의 SSR을 쓰면서 녹스를 활용했던 터라 바로 녹스에서 이 APK 파일을 실행시켜 보았다.

<br>

 </br>
 
![image](https://github.com/user-attachments/assets/f10c4f24-1770-4081-97d9-496cef36febe)

<br>

 </br>
 실행시켜 보니, 해당 화면이 뜨면서, 여러 문구들이 나오기 시작했다.
 이제 이 APK 파일을 분석해보려고 한다.
 <br>

 </br>

![image](https://github.com/user-attachments/assets/e5efafde-c968-428b-9a95-99a8e9914eac)

<br>

 </br>
 jadx-gui 도구를 이용하여, 해당 APK 파일을 살펴보았다. 뭔가 기존의 보았었던 APK 파일보다 훨씬 파일들이 많았고, 코드가 복잡했다. 그래서 그냥 모든 코드들을 차근차근 읽어보면서, flag 값과 관련된 코드 값부터 찾아보자 하고 코드들을 천천히 보기 시작하였다. 

<br>

 </br>
 
![image](https://github.com/user-attachments/assets/1140d849-9f85-47ba-a920-998714f450ad)

 <br>

 </br>
Mainactivity 부분을 살펴보니, 아까 녹스에서 앱을 실행시켰을 때 보았던 문구들을 발견하였고, 그 밑을 살펴보니, There is secretactivity 라는 문구가 출력되는 부분을 발견하였다. 아까 앱을 실행시켰을 때, There is secretactivity 이 문구는 발견하지 못했었고, secret이라니까 flag와 관련된 값이지 않을까? 하고 바로 secretactivity 내용을 살펴보았다.   

 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/18991a6f-294b-4187-9cc9-0edffd8c00fd)


 <br>

 </br>
 바로 secretactivity를 살펴보니, 
 
 ``SF{It's_your_first_step_toward_Android_hacking}`` 
 이라고 flag 같은 값이 나온 것을 확인할 수 있었다. 
