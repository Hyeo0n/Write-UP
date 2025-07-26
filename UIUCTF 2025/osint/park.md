<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> UIUCTF 2025 "park"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.07.26 풀이

<img width="1144" height="922" alt="image" src="https://github.com/user-attachments/assets/d001cdc7-15aa-426d-a69a-42df1aaa236f" />

![park](https://github.com/user-attachments/assets/860c735f-a30c-48d9-9bb4-d11b76899629)


UIUCTF의 다음과 같은 osint 문제를 풀어보려고 한다. 
다음과 같은 공원 사진을 문제 파일로 받았는데, 해당 공원의 이름을 찾아야하는 것 같다.


일단 저 사진을 살펴보니, 중앙에 국기가 하나 있는데, 이는 칠레 국기라고 한다. 그 뒤에 흐릿하게 파란 깃발이 보이는데 이는 EU 깃발로 추정된다.

전체적인 건축 양식이 빨간색, 노란색 외벽들인 점을 고려하여 열심히 찾아본 결과 스톡홀름에 있는 칠레 대사관 근처 사진인 것으로 보인다. 

칠레 대사관 근처에 있는 공원을 Google Map에서 찾아보니, Tegnérlunden이라는 공원이어서 이렇게 플래그를 찾을 수 있었다.

<br>

</br>

<img width="1136" height="790" alt="image" src="https://github.com/user-attachments/assets/932a6850-b3ec-4f91-89a4-e0c04b1b86ff" />

<br>

</br>

```
uiuctf{Tegnérlunden}
```
