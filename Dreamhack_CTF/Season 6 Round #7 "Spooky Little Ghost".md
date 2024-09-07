<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 Dreamhack Season 6 Round "Spooky Little Ghost" Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/88b133a0-db9f-42fb-82b3-9551d09ec4d8)

암호학 문제인 Spooky Little Ghost를 풀어보려고 한다. 문제 설명을 보니까, 유령을 대충 피하라는 내용 같았다.

<br>

 </br>
 
![image](https://github.com/user-attachments/assets/dac7f7bc-4662-4bd8-b079-429b18f1b5e8)

파일을 다운받아 보니, 여러 파이썬 파일들이 있었다. 이 파일들을 차근차근 살펴보려고 한다. 

<br>

 </br>

 <br>

 </br>

 ## prob.py
 
```
#! /usr/bin/env python3
import os
from cipher import GHOST
from utils import *

def menu() -> int:
    print('1. encrypt')
    print('2. decrypt')
    print('3. flag')
    print('4. exit')
    return int(input('> '))

if __name__ == '__main__':
    with open('flag', 'rb') as f:
        flag = f.read()

    key = os.urandom(8)
    g = GHOST(key)

    while True:
        i = menu()
        if i == 1:
            msg = input('plaintext(hex)> ')
            enc = g.encrypt(bytes.fromhex(msg))
            print('ciphertext(hex)>', enc.hex())
        elif i == 2:
            enc = input('ciphertext(hex)> ')
            msg = g.decrypt(bytes.fromhex(enc))
            print('plaintext(hex)>', msg.hex())
        elif i == 3:
            new_key = xor_bytes(key, bytes.fromhex('deadbeefcafebabe'))
            new_g = GHOST(new_key)
            enc_flag = new_g.encrypt(flag)
            print('encrypted_flag(hex)> ', enc_flag.hex())
        else:
            break

```

<br>

 </br>

 일단 menu() 함수를 보니까, 사용자에게 암호화, 복호화, 플래그를 암호화하여 출력, 종료 이렇게 네 가지 옵션을 제공하는 것으로 보인다. 

 <br>

 </br>
 
 그 후 메인 함수를 살펴보니, 프로그램이 실행되면, 먼저 flag 파일을 바이너리로 읽어오는 것으로 보인다. 이 flag는 플래그 값을 저장한 파일로 보이며, flag 변수에 저장되는 것으로 보인다. 그 다음, 8바이트 길이의 무작위 키가 생성되고 key 변수에 저장되고 난 후, GHOST(key)로 GHOST 암호 알고리즘 객체 g가 생성되는 것으로 보인다. 아마, 이 객체는 암호화와 복호화 쪽에서 활용되지 않을까 하는 생각이 든다. 
 <br>

 </br>
 
 그 후, while로 쓰인 메뉴 실행 루프 코드 부분을 살펴보니, 첫 번째, 암호화에서는 사용자가 입력한 값을 받는 것으로 보인다.  bytes.fromhex() 함수는 처음 보아서, 이 함수가 무엇인지 찾아보니, 입력받은 값을 이용해 바이트로 변환하는 함수라고 한다. 그리고, 아까 위에서 생성한 g 객체를 활용하는 것으로 보이는 g.encrypt()가 입력값을 암호화하는 것으로 보인다. 그리고, 뒤에 print 함수를 통해, 결과가 다시 문자열로 변환되어 출력되는 구조로 보인다.
 <br>

 </br>
 
 두 번째, 복호화에서는 사용자가 입력한 암호문을 전달받아서 바이트로 변환한 후, 다시 객체 g를 활용한 g.decrypto()로 복호화하여 출력하는 것으로 보인다.
 <br>

 </br>

 세 번째, 플래그 암호화 부분을 살펴보니, 원래의 key 변수가 `deadbeefcafebabe`라는 문자열의 16진수 값을 xor_bytes() 함수로 사용되어 지는 것으로 보인다. 이 함수가 무엇인지 찾아보니, XOR 연산을 수행하는 함수였다. 그럼, 이 부분은 원래의 key와 저 문자열의 16진수 값을 XOR 연산을 진행하여, 새로운 new_key로 만드는 것으로 보인다. 또한, 그 새로운 키를 GHOST 객체를 다시 생성하고, 이 객체로 플래그를 암호화한 후 암호화된 플래그를 16진수로 출력되는 것으로 보인다. 
 네 번재는 이 구문을 종료시키는 부분으로 보인다. 

 <br>

 </br>
 
<br>

 </br>
 
 ## flag
 prob.py를 실행하여 암호화된 플래그의 16진수 값을 얻고, 이를 통해, XOR 연산을 사용하여 원래 키를 복구하고, GHOST 암호 알고리즘으로 암호화된 플래그를 복호화해내서, 플래그를 얻어내면 될 것 같다고 생각했다. 
 그래서, 암호화된 플래그의 16진수 값을 활용하여, 플래그를 복구해보는 python 코드를 작성해보려고 한다. 
 작성해보기에 앞서, 일단 prob.py의 3번 과정을 실행해서, 암호화된 플래그 값을 얻어보려고 한다.

<br>

 </br>

 ![image](https://github.com/user-attachments/assets/7d90b274-561e-4892-8e30-4838e1f8a149)

prob.py의 3번 과정을 실행해보니, 암호화된 플래그가 `b96cf913c355b69d639eb4a930a475afa4045b7e905a0c8cc5571b20aa46d8e0639eb4a930a475afa4045b7e905a0c8cc5571b20aa46d8e0812d284dcceaa390` 인 것을 확인했다. 이제 이를 활용하여 플래그를 복구해내는 파이썬 코드를 작성해보려고 한다. 

<br>

 </br>

위처럼 코드를 작성하였다. prob.py를 실행하여 얻은 값을 encrypted_flag_hex 변수에 암호화된 플래그 값으로 지정하고, XOR 연산을 사용하여 원래 키를 복구하고, GHOST 암호 알고리즘으로 암호화된 플래그를 복호화하게끔 작성해보았다. 
<br>

 </br>
 
![image](https://github.com/user-attachments/assets/a74df9f0-1f24-466c-8904-0c17ddbde7c2)

이를 실행해보니, 어떤 문자열이 나오기는 하는데, 기존의 드림핵 플래그 형식의 값은 나오지 않는다... 그 후에도, 내가 작성한 파이썬 코드를 몇 번 수정해보았지만, 값이 계속 알 수 없는 형태의 문자열로만 출력되었다... 
