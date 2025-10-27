<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> 2025 K17 CTF "radioactive"  Write-UP </h1>
</body>
<br>
<br>
</html>


<img width="986" height="1004" alt="image" src="https://github.com/user-attachments/assets/f4d54c62-fcc0-436f-b174-ae83b25bd8da" />

<img width="1536" height="2048" alt="image" src="https://github.com/user-attachments/assets/ee86db64-6bbc-4319-b774-aa8db2471d6e" />

제공된 사진을 살펴보면, 경비 철조망과, "This is a telecommunication facility" 문구 등이 있고, 숲 속에 있는 기지국 같은 시설을 보인다. 이 해당 특정 통신 시설에 관해서 찾으면 플래그 값을 찾을 수 있을 것으로 보인다. 

그 옆에 간판을 살펴보면, NSA site number 같은 것이 적혀져 있는 것을 확인할 수 있다. 그리고 해당 간판에 쓰여져 있는 rfnsa.com.au 사이트를 볼 수 있다.
해당 사이트는 호주 정부와 통신사들이 공동으로 운영하는 호주 전국 모바일 기지국 공개 DB 사이트라고 한다. 
해당 사이트에서 밑에 쓰여있는 NSA site number : 2154006으로 이동하면, RFNSA ID 2154006의 좌표는 (-33.71721503, 150.9871099)으로 나온다. 
<br>
이를 소수점 3째 자리 반올림으로 올려서 
<br>

```
K17{-33.717, 150.987}
```
<br>

다음과 같은 플래그를 입력해보면, 성공하였다
<br>

<img width="984" height="1180" alt="image" src="https://github.com/user-attachments/assets/c6f29e55-eb01-454f-98ed-52457361f452" />
