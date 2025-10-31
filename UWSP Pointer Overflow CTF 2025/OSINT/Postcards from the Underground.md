<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> UWSP Pointer Overflow CTF 2025 "Postcards from the Underground"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.10.31 풀이

<img width="794" height="1410" alt="image" src="https://github.com/user-attachments/assets/2bc0be6d-c521-4ddd-8817-863c926927cd" />

해당 문제에서 나온 사진의 To Die 4라는 번호판의 차는 검은색 영구차인 것으로 보인다. 이러한 영구차를 이끄는 사람이 특이한 클럽의 리더라고 한다. 
이 클럽의 이름이 플래그라고 한다. 

해당 영구차를 이끄는 특이한 클럽은 아마 영구차 동호회지 않을까 싶어서, 외국의 영구차 동호회(Hearse Club) 여러 개를 서치해보았다.
여러 개를 하나하나 답에 넣어보다가, 미국 일리노이주의 Hardcore Hearse Club을 찾게 되었다. 이를 CTF 규칙으로 변환하여 입력해보니 플래그를 찾을 수 있었다.
<br>
<br>

```
poctf{uwsp_h4rdc0r3_h34r53_club}
```

