<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> Dreamhack CTF "secure-mail"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.05.21 풀이

<img width="1280" height="346" alt="image" src="https://github.com/user-attachments/assets/d23ec43a-2fd5-4b46-bd41-1b1b51d8961f" />

해당 비밀번호 칸에 아무 값이나 입력해보니, Wrong이라는 알림창이 뜬다. 비밀번호를 알아내면 flag 값을 얻을 수 있을 것으로 보인다. 

<br>

</br>

<img width="1280" height="553" alt="image" src="https://github.com/user-attachments/assets/618c84dc-bd8a-41c9-a970-5a394ebaa463" />

개발자 도구로 코드를 확인하였다.
그 중 함수 0x9a220을 살펴보니, 사용자가 입력한 비밀번호가 맞는지 확인하고,
맞으면 무언가를 출력하고, 틀리면 Wrong 알림을 띄우는 부분임을 알 수 있다.
file = [..] 에 들어있는 배열은 암호화된 바이너리 데이터로 보이며,
decrypt라는 메서드로 해당 file 배열을 복호화하고, 비밀번호를 확인하는 것으로 보인다.
이를 브루트포싱을 사용하여, 해결해보려고 한다.

<br>

</br>

<img width="1280" height="1372" alt="image" src="https://github.com/user-attachments/assets/d735a163-3dc9-45c1-bac3-930ef7b4aac0" />

해당 코드는 스크립트 실행 시 바로 동작하도록 설계되었다.
생년월일 추측 범위를 설정하고,
알림창이 뜨는 것을 가로채고, wrong이 아닌 메시지가 오면 맞았다고 
판단하게끔 작성하였다.
그리고 맞는 비밀번호가 나오면, error를 일으켜 루프를 종료시키도록 하였다.
또한, 잘못된 형식의 날짜를 필터링하기 위해, 실제로 존재하는 날짜만 입력되도록
하는 부분도 작성해주었다.

<br>

</br>

<img width="1178" height="1112" alt="image" src="https://github.com/user-attachments/assets/f36a9611-4a3c-44b6-83d6-bd5e89a76d63" />

비밀번호가 960229인 것을 알아낼 수 있다.

<br>

</br>

<img width="1186" height="648" alt="image" src="https://github.com/user-attachments/assets/da8df85f-a4c1-4b13-9fe6-e8e7d381d4d8" />

비밀번호를 입력하면, 해당 이미지가 출력된다

<br>

</br>

```
DH{Brutef0rce_th3_secur3_mail}
```
