<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> ShaktiCTF 2025 "gooGOOgaaGAA"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.07.26 풀이

<img width="986" height="1296" alt="image" src="https://github.com/user-attachments/assets/cab36bf2-fc48-4139-b0e9-597c734ce70e" />

ShaktiCTF의 다음과 같은 리버싱 문제를 풀어보려고 한다. 

<img width="2330" height="768" alt="image" src="https://github.com/user-attachments/assets/ab9e1990-5ebf-4f08-8604-701a76591edb" />

문제파일에 있던 py 파일이다. 

해당 코드를 살펴보니, gaga()함수는 입력 문자열 text를 key= "IWANTMOMOS" 와 XOR 연산하여 암호화한 뒤, 미리 정의된 encrypted 리스트와 비교한다.

`text[i] ^ key[i % len(key)]` 를 수행하여 문자 하나씩 XOR그 결과를 encrypted 와 비교하는 것으로 보인다. 

<br>

</br>


```
encrypted = [':', '?', ' ', '%', ' ', '$', ',', '9', ')', '(', '+', 'c', '#', '7', '\x06', '~', '9', '\x12', '~', ' ', '\x16', '4', '4', ':', 'g', '0']
key = "IWANTMOMOS"

flag_chars = []

for i in range(len(encrypted)):
    e = encrypted[i]
    k = key[i % len(key)]
    flag_char = chr(ord(e) ^ ord(k))
    flag_chars.append(flag_char)

flag = ''.join(flag_chars)
print("output :", flag)
```
그래서 다음과 같이, 위의 XOR 연산을 역연산하는 스크립트를 작성해보았다. 
`input[i] = chr(ord(encrypted[i]) ^ ord(key[i % len(key)]))` 연산을 활용하여 기존의 flag 값을 복구해보려고 한다.

<br>

</br>

<img width="762" height="130" alt="image" src="https://github.com/user-attachments/assets/74e5fca7-94eb-4e2e-ac46-d0a2b497c173" />

스크립트를 실행해보니, flag 값을 구할 수 있었다.

<br>

</br>

```
shaktictf{b4byR3v_1s_cut3}
```

