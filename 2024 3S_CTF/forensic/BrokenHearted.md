<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "BrokenHearted"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/82caeb1b-7cc1-4123-a9e4-9c38904f6032)

<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/fb77042e-1d6d-46f8-ab23-23e6b5f9b946)

![CTF](https://github.com/user-attachments/assets/9b8cd4d2-d69d-4068-b27f-0b6bd1ed9640)

파일을 열어보니, hint.txt 랑 이미지 하나만 있었다. 
힌트를 보니, 파일 카빙을 해서 플래그를 찾으라는 것 같다. 
파일 카빙이 뭔지 찾아보니, 저장 매체가 포맷되거나 파일시스템이 손상되어 메타데이터가 없는 파일 복구를 시도하는 기법이라고 한다. 파일 카빙을 하는 방법을 찾아보니, 'foremost', 'scalpel'여러 도구를 사용하라고 한다. 그래서, foremost 도구를 설치받아서 사용해보려고 한다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/4239a076-d950-42c3-b5c5-ab6f5ad5c13e)

파일 카빙을 하는 방법을 찾아보니, 'foremost', 'scalpel'여러 도구를 사용하라고 한다. 그래서, foremost 도구를 설치받아서 사용해보려고 한다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/276729a1-7868-4177-a1f5-f6ebef079113)

찾아보니, ``foremost -t all -i`` 명령어를 통해, 파일을 추출할 수 있다고 한다.
해당 명령어를 사용하여, 실행해보았다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/31906f79-f79a-4cba-a180-bfb113747ae1)

BrokenHearted 폴더 안에 새롭게 output 폴더가 생겼는데, 살펴보니, zip 파일이 2개 있었다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/38ade82d-3e86-4d4c-b67d-308818d19786)

![image](https://github.com/user-attachments/assets/6af3232d-6949-463c-9872-f2c28bf3db71)

첫 번째 zip 파일에는 해당 이미지 1개가 들어있었고,
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/71d0f961-9c89-451c-954f-0bf815b25ba5)

![image](https://github.com/user-attachments/assets/44811c8e-dee3-45b4-89e7-bf04e5a0b9ca)

![image](https://github.com/user-attachments/assets/c360692d-ca93-4e5b-a327-85e50ef45608)

두 번째 zip 파일에는 해당 이미지가 2개 들어있었다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/a08bcece-49ee-4b32-a506-aa39c2a47354)

첫 번째 힌트는 LSB 스테가노그래피를 찾아보고, 항상 끝부분을 유심히 보라고 되어있다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/af9ccf07-7325-4d29-806c-a2ec1f5c62a1)

두 번째 힌트는 LSB 스테가노그래피을 사하라고 하는 것 같다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/ae175729-1b66-4d97-8210-d30d7cd335f3)

flag.bmp를 보니, 플래그는 오프셋 0x00000100부터 0x0005000의 범위에 있다고 하는 것 같다. 그리고 이 범위 내에서 시작이 'FE' 로 시작하는 216 바이트 길이의 데이터를 찾으면 된다고 하는 것 같다.
<br>

 </br>
 <br>

 </br>
 
이 이후부터, 해당 범위의 FE로 시작하는 데이터들을 살펴보았는데, 여기서 어떻게 접근해야될지 감이 오지 않는
