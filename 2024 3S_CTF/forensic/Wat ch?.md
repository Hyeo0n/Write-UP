<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "Wat ch?"  Write-UP</h1>
</body>
<br>
<br>
</html>


![image](https://github.com/user-attachments/assets/fb2f8d31-f0c2-45a7-8b0a-da4f2037051c)

<br>

 </br>
 <br>

 </br>
 
## 1. 어플리케이션 개수

![image](https://github.com/user-attachments/assets/72885fad-7061-4acc-b1e5-f15e82d038db)

파일의 개수가 너무 많아서 셀 수가 없을 것 같아, ls -l | wc -l 라고 명령어를 입력하여, 어플리케이션 개수가 46개인 것을 알아냈다.
<br>

 </br>
 
``46``

<br>

 </br>
 <br>

 </br>
 
# 2. 3번째 리마인더 작성 시간(HH:MM:SS)

![image](https://github.com/user-attachments/assets/38b51850-f035-4eff-bb85-0d87fa50d38b)

리마인더 관련 폴더인 com.samsung.w-reminder안에 들아가 파일 하나하나 살펴보다가, reminderConsumerLog 파일을 열어보니, 여러 시간들이 나열되어 있었다. 
3번째 리마인더 작성 시간을 물어보았으니, 3번째 Start 시간을 보니, 22:45:48 이다.
<br>

 </br>
 
``22:45:48``
<br>

 </br>
 <br>

 </br>

# 3. 음악 파일명(@@@.mp3)

![image](https://github.com/user-attachments/assets/2640b2fd-0b5e-42c9-86f0-58872afedcc2)

com.samsung.w-music-player이 음악과 관련되어 있어보여서, 들어가서 여러 파일들을 살펴보다가 local_queue_data 파일을 열어보니, 아티스트가 Samsung인 Over the Horizon 이라는 음악 파일명을 찾았다.
<br>

 </br>
 
``Over the Horizon.mp3``
<br>

 </br>
 <br>

 </br>

# 4. 김소리의 거주지(countryName cityName)
<br>

 </br>

거주지와 관련된 파일이 뭐가 있을까 하다가 com.samsung.weather를 발견하여, 들어가보니, 거의 다 db 파일이라 파일이 열리지 않아 확인할 수 없었다..
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/6f90da04-c6fb-45a2-9938-892bbc8dc02d)

그래서 DB Browser for SQlite를 설치하여, 파일 하나하나 찾아보았다.
그러던 중, city와 country에 관련된 테이블들이 나와서 데이터 탐색을 해보니, 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/f48acaca-15a8-40b3-849b-f44b2b478bd1)

대한민국 하남시라고 적힌 데이터를 발견했다. 아마 김소리씨는 하남시에 거주하는 것 같다
<br>

 </br>
 
``대한민국 하남시``
<br>

 </br>
 <br>

 </br>

# 5. 1695650088에 뉴스를 발행한 author
<br>

 </br>
 
또 다시 여러 테이블을 보던 중, stories을 들어가서 살펴보았는데, 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/9480fd2d-abc2-4c5e-b2a0-7cc2c9250c15)

1695650088에 뉴스를 발행했다는 말이 이해가 안 됬었는데, 1695650088에 세계일보가 있는 것을 발견했다.
<br>

 </br>
 
``세계일보``
<br>

 </br>
 <br>

 </br>
 
```
3S{46_22:45:48_Over the Horizon.mp3_대한민국 하남시_세계일보}
```
이렇게 플래그 값을 찾을 수 있었다. 
