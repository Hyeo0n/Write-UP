<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "Minjuns_travel_2"  Write-UP</h1>
</body>
<br>
<br>
</html>

문제 파일을 다운받아 압축 해제해보니, 

![image](https://github.com/user-attachments/assets/ba2704e5-b7e9-4b0d-982e-9c5ef3770e54)

여러 pdf 파일이 있었다. txt 파일을 확인해보니,
<br>

 </br>
<br>

 </br>
 
![image](https://github.com/user-attachments/assets/25f9fa38-2944-4c4c-a09f-1168a969d126)

차근차근 pdf 비밀번호를 알아내면 되는 문제같다.
<br>

 </br>
 <br>

 </br>

## 1.pdf

![image](https://github.com/user-attachments/assets/be887ad3-7ca0-4560-be51-8d2b2b3c9fa3)

1번 pdf를 확인해보니, 음식점 이름을 찾는 거 같은데, 괄호에 37.503,127.0037 가 좌표 형식인 것 같아서, 구글 지도에 검색해보았다.

<br>

 </br>
 
![image](https://github.com/user-attachments/assets/4d17ff85-45da-4a8e-a1d5-9b8cdbf90111)

좌표를 찾아보니, 파미 에스테이션이라는 여러 음식점들이 있는 건물 좌표였다. 해당 건물에 모든 음식점들 이름을 띄어쓰기없이 영어 대문자로 하나하나 다음 pdf에 입력해본 결과, 

![image](https://github.com/user-attachments/assets/9cd7904c-965d-40a5-9416-c5d836420f3c)

M1F 층에 ``DIMDIMSUM``이라는 음식점을 입력해본 결과, 2.pdf 파일이 열렸다!

<br>

 </br>
 <br>

 </br>

 ## 2.pdf
 ![image](https://github.com/user-attachments/assets/85db189f-2864-46f3-a881-e2f142a051d5)

 2번째 pdf 파일을 열어보니, flag가 나와있어서, 저게 flag 값인가 했는데, 입력해보니 아니었다.. pdf 파일이 7개나 있는데 2번째만에 나오지는 않겠지.. 하고 pdf 문제를 살펴보았다. 
 사실 뭔 말인지 하나도 몰라서 그냥 복붙으로 구글링을 해보았다.
 <br>

 </br>
 <br>

 </br>

 ![image](https://github.com/user-attachments/assets/54a1a6d4-24a9-43d3-a02b-40f1c3bb73a3)

 찾아보니, 프로그래밍 언어인 '아희' 라고 한다. 위 사진의 맨 처음 블로그에 들어가보니,
 <br>

 </br>
 <br>

 </br>


 ![image](https://github.com/user-attachments/assets/0ad36358-d402-40c2-aaab-0cdff59bfc1d)

pdf와 똑같은 문자열이 구구단 2단부터 9단까지 나열한 것이라고 한다.
 <br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/da7dba29-a06d-4837-93ef-6a7a422be88f)

그래서 다음 3번째 pdf에 ``GUGUDAN``을 입력해본 결과, 파일이 열렸다!
 <br>

 </br>
 <br>

 </br>
 
 ## 3.pdf
 
 ![image](https://github.com/user-attachments/assets/4978b5ee-57c2-4bb4-b9fe-4c8ddf15de1d)

또 다시 플래그가 나온 걸 보니, 모든 pdf에 나온 플래그 값들을 조합하는 건가 보다.
다시 36.1082, 128.4188와 같은 좌표같은 형식이 나와서 구글 지도에 찾아보니,
 <br>

 </br>
 <br>

 </br>

![image](https://github.com/user-attachments/assets/6fa57222-3c85-402c-88dd-d972816c86b4)

이 건물에 있는 걸 찾아보니, 올리브영인 것을 알아냈다. 올리브영에서 선크림 종류가 수만 가지라서 어떤 선크림을 말하는 건지 감이 잘 안 왔다. 
사람들이 많이 쓰는 것을 사야겠다고 쓰여 있으니, 올리브영 사이트에 들어가서 인기 1위 상품을 찾아보니, 구달 선크림이길래 다음 pdf에 입력해보니, 아니었다. 
그래서 확대해보고, 이것저것 대비해보다가, 중앙쯤에 ``-....-....-.`` 라는 점과 선의 배열을 찾아냈다. 이게 뭔지 구글링해보니, 모스부호였고, 이를 해석해보니, BLUE라는 단어인 것을 알 수 있었다. 
 <br>

 </br>
 <br>

 </br>

![image](https://github.com/user-attachments/assets/61738874-3854-4e91-b0af-4db1df563f59)

올리브영에서 찾아보니, 누가봐도 완전 파란색인 식물나라 선크림을 발견하여서, 한글로 입력해보았다.

 <br>

 </br>
 <br>

 </br>
 
 ![image](https://github.com/user-attachments/assets/e64836a4-6209-44b9-913d-277f772d1ede)

 이걸 영자 자판으로 바꿔서 
 
 ``tlranfskfk`` 
 라고 입력하니, 다음 pdf를 열 수 있었다.

  <br>

 </br>
  <br>

 </br>
 
 ## 4.pdf
 
 ![image](https://github.com/user-attachments/assets/85e3cf20-f5ac-4646-95e3-5cf91c83dc02)

마블 영화 중에 무슨 마블 영화일까 한 번 찾아보니, 가장 최근에 개봉한 마블 영화가 '데드풀과 울버린'이라는 것을 알아냈다. 
찾아보니 이 영화 길이가 2시간 7분이라니까, 시간 계산을 해보려고 한다.
이전 pdf에서 14시 30분이라고 했으니, 선크림 쇼핑을 대충 30분? 정도 했다고 생각하고, 15시에다가 2시간 7분을 더하면 17시 7분이 되지 않을까 하여, 입력해보니 아니었다. 
이전에 올리브영 좌표 찾아보다가, 근처에 메가박스가 있었던 게 기억이 나서 해당 메가박스의 7월 27일 상영시간표를 확인해보려고도 해봤는데, 예전 꺼라 나오지 않았다. 여기서 뭘 더 어떻게 해야될지 진짜 모르겠어서, 그냥 아까 선크림 쇼핑 시간을 30분으로 잡았는데, 이를 10분 15분 20분 25분 순으로 5분씩 늘려가면서 입력해보았다.

  <br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/0ca00247-9512-4fe8-b61f-aec2c02944a8)

그러다가 운이 좋게 선크림 쇼핑 시간을 25분으로 어림잡아 17시 2분으로 계산하여 입력해보니, 운이 좋은건지 어처구니 없게도 다음 pdf 파일이 열렸다...
``1702``

 <br>

 </br>
  <br>

 </br>
 
 ## 5.pdf

 ![image](https://github.com/user-attachments/assets/2a39b454-1351-4f33-9b5b-4f9db75a5e4a)

 해당 사진에 숫자라고는 다 세 자리이어서..포켓몬을 사실 잘 모르는 나에게는 너무 낯설고, 어디서부터 접근해야될지 모르겠어서,
   <br>

 </br>
 <br>

 </br>

 ![image](https://github.com/user-attachments/assets/7704c36e-ccfe-4341-8484-6bdcd2aec6a0)

 일단 포켓몬 친구들부터, 인터넷에 검색하다가 나무위키를 보는 중에, 각 포켓몬마다 4자리 고유번호? 같은 숫자가 있는 것을 발견하였다. 
 하나하나 찾아보니
 '타부자고'라는 친구는 1000
 '체리꼬'라는 친구는 0421
 '코코리'라는 친구는 0231
 '가디안'이라는 친구는 0282
 '레시라무'라는 친구는 0643
 '레지스틸'라는 친구는 0379 인 것을 알아냈다. 이 모두를 더해보니, ``2956`` 이라서 이를 입력해보니, 다음 파일이 열렸다. 

<br>

 </br>
 <br>

 </br>

 ## 6.pdf
 ![image](https://github.com/user-attachments/assets/d0f39d5d-451e-47df-9b4c-a186470bfe98)

![image](https://github.com/user-attachments/assets/666ea286-b01c-49bd-be4c-9b74dbc689ed)

이게 뭘까 구글링을 타고 타고, 열심히 해본 결과 프로그래밍 언어 "Brainfuck"이라는 언어였다. 
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/cc9c4fcb-537e-4b54-9d19-3eef485874df)

번역기를 사용해서, 번역해본 결과 크라임씬을 보면서 문제를 만들고 있다는 재미난 메세지가 해석되었다. 

``CRIMESCENE`` 을 입력해보니, 다음 pdf가 열렸다. 

<br>

 </br>
 <br>

 </br>

 ## 7.pdf

![image](https://github.com/user-attachments/assets/5c0c0a53-7bf0-4a80-8ebe-bf07a7474a32)

이제 모든 플래그 값들을 다 합쳐서
<br>

 </br>
 
```
3S{thank_you_for_participating_this_3sctf_wjfeoahtEofuakwcnj}
```
라는 플래그를 얻을 수 있었다. 
