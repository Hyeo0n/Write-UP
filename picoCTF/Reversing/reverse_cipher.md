<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> picoCTF "reverse_cipher"  Write-UP </h1>
</body>
<br>
<br>
</html>

2025.05.21 풀이

<img width="972" height="266" alt="image" src="https://github.com/user-attachments/assets/821a629c-81a3-40cc-b770-20244d964bfe" />

문제 설명을 보니, 바이너리 파일과 텍스트 파일을 복구하였으니, 플래그를 reverse하라고 한다.

<br>

</br>

<img width="1280" height="297" alt="image" src="https://github.com/user-attachments/assets/ab4c9dbb-c38b-4162-b47e-61ff042a9762" />

일단 rev, rev_this 파일의 확장자를 알기 위해, file 명령어를 사용하여 살펴보았다. 그 후, text 파일인 rev_this을 출력해보니, flag 형식인 문자열이 나왔다.
근데 진짜 flag 값으로 보이지는 않는다.

<br>

</br>

<img width="1280" height="659" alt="image" src="https://github.com/user-attachments/assets/23a87837-32cf-46d1-b1f2-b12af56d3abd" />

해당 rev ELF 파일을 아이다를 통해 열고, main 함수를 살펴보았다. 해당 코드를 살펴보니, flag.txt를 읽기 모드로 열고, rev_this 파일은 쓰기 모드로 연다.
그 후, flag.txt에서 24바이트를 읽고, 이를 ptr 배열로 저장한다. 그리고 이를 3 단계로 나눠서 rev_this 파일에 출력하는 코드인 것으로 보인다. ptr배열의 0~7까지는 그대로 출력되고,
8~22까지는 홀짝 조건에 따라 조작해서 출력, 마지막 23은 그대로 출력하는 것으로 보인다.

<br>

</br>

<img width="914" height="1044" alt="image" src="https://github.com/user-attachments/assets/cce9f116-1fc5-48b8-95c7-624ed38699db" />

아까 위에서 얻은 picoCTF{w1{1wq85jc=2i0<} 을 통해, 이를 역연산하는 스크립트를 작성해보았다. 

<br>

</br>

<img width="1090" height="234" alt="image" src="https://github.com/user-attachments/assets/6cf2515d-957f-45fb-b4d7-22dc3046fd35" />

해당 스크립트를 실행해보니, flag 값을 얻을 수 있었다.

<br>

</br>

```
picoCTF{r3v3rs37ee84d27}
```
