<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> 2026 INCOGNITO CTF "The Missing Image Fragment"  Write-UP</h1>
</body>
<br>
<br>
</html>

2026.02.16 풀이

<img width="1884" height="1284" alt="image" src="https://github.com/user-attachments/assets/f7706259-9acc-4bd7-aad5-5e7acbb2f000" />

제공된 이미지 파일을 autopsy 툴로 살펴보니, jpg 파일 다섯 개를 추출할 수 있었다.

다섯 개 중 2개는 손상된 jpg 파일로 보인다. 

<br>

</br>

<img width="1286" height="602" alt="image" src="https://github.com/user-attachments/assets/0039ac0a-a671-4441-9d4c-2ed270446c88" />

<img width="1288" height="584" alt="image" src="https://github.com/user-attachments/assets/6eaefa84-1130-4ec5-b8ed-91e040458131" />

<img width="1276" height="722" alt="image" src="https://github.com/user-attachments/assets/ada64108-ef75-47ec-87d1-5113aeaad2e3" />

<img width="764" height="912" alt="image" src="https://github.com/user-attachments/assets/2f10cab1-4318-4581-9b4e-4a101f2afa93" />

해당 이미지 파일들을 HxD로 헥스값을 살펴보았는데, RKIR439.jpg에서 나머지 이미지에는 없는 ASCII 뒤에 `aGlkZGVuXzIwMjY=` 부분을 발견하였다. 

`aGlkZGVuXzIwMjY=` 해당 값을 base64로 디코딩해보면 `hidden_2026`이라는 값이 나온다. 
<br>

</br>

그리고 문제가 
```
# The Missing Image Fragment

본 외장 저장장치는 특정 직원이 사용하던 장치입니다.
수거 과정에서 일부 이미지 파일이 삭제된 흔적이 확인되었습니다.
삭제된 데이터의 내용은 현재 확인되지 않았습니다.
```
였으므로 디스크 이미지 내에 더 삭제된 데이터가 있을 것으로 생각해 HxD로 살펴보았다.
<br>

</br>

<img width="1298" height="676" alt="image" src="https://github.com/user-attachments/assets/48ef421f-5436-4476-bc8f-e74fafc947a0" />

<img width="1298" height="464" alt="image" src="https://github.com/user-attachments/assets/53fb76df-2a6e-49b2-8c89-5e3bcaf902c3" />

그러다가 ZIP 시그니처인 PK를 발견하였고, 이를 카빙해보았다.
<br>

</br>

<img width="1592" height="830" alt="image" src="https://github.com/user-attachments/assets/75f1db30-2a3b-43ff-b1e3-bfd9bf11abb9" />

<img width="1608" height="950" alt="image" src="https://github.com/user-attachments/assets/8126e757-db6e-4569-8aed-1a44bc98eda5" />

이 압축파일을 보니 안에 secret.jpg가 있엇고, 압축 풀려면 비밀번호가 걸려 있어서 위에서 구한 `hidden_2026`을 입력해보니, 압축이 풀렸다.
<br>

</br>

![secret](https://github.com/user-attachments/assets/597b75ca-39c7-448b-8b0f-dbe68438df6b)

```
INCOGNITO{EVID-68eb-4ac5-hd19}
```
플래그 값을 얻을 수 있었다.
