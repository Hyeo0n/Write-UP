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

## Cipher.py

```
from utils import *

class GHOST:
    sbox = (
        (0xC, 0x4, 0x6, 0x2, 0xA, 0x5, 0xB, 0x9, 0xE, 0x8, 0xD, 0x7, 0x0, 0x3, 0xF, 0x1),
        (0x6, 0x8, 0x2, 0x3, 0x9, 0xA, 0x5, 0xC, 0x1, 0xE, 0x4, 0x7, 0xB, 0xD, 0x0, 0xF),
        (0xB, 0x3, 0x5, 0x8, 0x2, 0xF, 0xA, 0xD, 0xE, 0x1, 0x7, 0x4, 0xC, 0x9, 0x6, 0x0),
        (0xC, 0x8, 0x2, 0x1, 0xD, 0x4, 0xF, 0x6, 0x7, 0x0, 0xA, 0x5, 0x3, 0xE, 0x9, 0xB),
        (0x7, 0xF, 0x5, 0xA, 0x8, 0x1, 0x6, 0xD, 0x0, 0x9, 0x3, 0xE, 0xB, 0x4, 0x2, 0xC),
        (0x5, 0xD, 0xF, 0x6, 0x9, 0x2, 0xC, 0xA, 0xB, 0x7, 0x8, 0x1, 0x4, 0x3, 0xE, 0x0),
        (0x8, 0xE, 0x2, 0x5, 0x6, 0x9, 0x1, 0xC, 0xF, 0x4, 0xB, 0x0, 0xD, 0xA, 0x3, 0x7),
        (0x1, 0x7, 0xE, 0xD, 0x0, 0x5, 0x8, 0x3, 0x4, 0xF, 0xA, 0x6, 0x9, 0xC, 0xB, 0x2)
    )

    def __init__(self, key: bytes) -> None:
        self._block_size = 8
        self._round_keys = self._expand_key(key)
        self._state = []

    def _expand_key(self, key: bytes) -> list[list[int]]:
        assert len(key) == 8
        key_bits = bytes_to_bits(key)
        round_keys: list[list[int]] = []
        for i in range(0, 64, 32):
            round_keys.append(key_bits[i:i+32])
        return round_keys

    def _substitution(self, bit32: list[int]) -> list[int]:
        res: list[int] = []
        for i,j in enumerate(range(0,32,4)):
            res += int_to_bits(self.sbox[7-i][bits_to_int(bit32[j:j+4])], 4)
        return res

    def _round_function(self, round_n: int, is_enc: bool) -> None:
        round_key_idx = round_n%2

        state_hi = self._state[:32]
        state_lo = self._state[32:]

        state_lo = add_mod_2_32(state_lo, self._round_keys[round_key_idx])
        state_lo = self._substitution(state_lo)
        state_lo = rol11(state_lo)
        state_lo = xor_lst(state_lo, state_hi)

        if (is_enc and round_n == 31) or (not is_enc and round_n == 0):
            self._state[:32] = state_lo
        else:
            self._state[:32] = self._state[32:]
            self._state[32:] = state_lo  

    def _encrypt(self, plaintext: bytes) -> bytes:
        self._state = bytes_to_bits(plaintext)
        for i in range(32):
            self._round_function(i, is_enc=True)
        return bits_to_bytes(self._state)

    def _decrypt(self, ciphertext: bytes) -> bytes:
        self._state = bytes_to_bits(ciphertext)
        for i in range(31, -1, -1):
            self._round_function(i, is_enc=False)
        return bits_to_bytes(self._state)

    def encrypt(self, plaintext: bytes) -> bytes:
        assert len(plaintext)%self._block_size == 0
        ciphertext  = b''
        for i in range(0, len(plaintext), self._block_size):
            ciphertext += self._encrypt(plaintext[i:i + self._block_size])
        return ciphertext

    def decrypt(self, ciphertext: bytes) -> bytes:
        assert len(ciphertext)%self._block_size == 0
        plaintext  = b''
        for i in range(0, len(ciphertext), self._block_size):
            plaintext += self._decrypt(ciphertext[i:i + self._block_size])
        return plaintext

def test():
    import os
    trial = 0x1000
    for _ in range(trial):
        key = os.urandom(8)
        g = GHOST(key)
        plain = os.urandom(8)
        cipher = g.encrypt(plain)
        assert plain == g.decrypt(cipher)

if __name__ == '__main__':
    test()

```

GHOST 클래스가 정의되어져 있는데, sbox는 8개의 테이블이 정의되어 있는데, 이는 치환 과정에서 비트들을 치환하는 역할을 하는 것으로 보이는데, 각 s-box는 16개의 값으로 이루어져 있다. 

<br>

 </br>

` __init__` 생성자 부분을 살펴보니, key는 8바이트 길이의 키를 받아 초기화하는 것으로 보이고, -block_size는 고정된 블록 크기로 8바이트를 사용하고, _round_keys는 _expand_key() 메서드를 통해 확정된 라운드 키를 저장하는 것으로 보인다. _state는 암호화/복호화 중의 상태 비트 배열을 저장할 리스트이다.

 <br>

 </br>

 _expand_key(self, key: bytes) -> list[list[int]] 부분을 보니, key는 8바이트의 키를 받아 2개의 32비트 라운드 키로 확장하는 것으로 보이고, bytes_to_bits()는 바이트 배열을 비트 배열로 변환하는 것으로 보인다. 각각 32비트씩 나누어진 키를 사용하여, 두 개의 라운드 키가 생성되는 것으로 보인다. 

 <br>

 </br>

 _substitution(self, bit32: list[int]) -> list[int] 부분을 보니, 32비트의 입력을 받아 각 4비트 블록에 대해 S-box 치환을 수행하고, 입력 비트 블록을 S-box를 통해 변환한 후, 결과를 다시 4비트로 변환하는 것으로 보인다. 

 <br>

 </br>

 _round_function(self, round_n: int, is_enc: bool) 부분을 보니, 현재 라운드 번호에 맞는 라운드 키를 선택하고, 32비트 상위 블록(state_hi)과 하위 블록(state_lo)으로 상태를 나누고, 하위 블록에 라운드 키를 더하는 것으로 보인다. 그리고, S-box 치환 및 비트 회전을 수행하시는 것으로 보인다. 그 후, XOR 연산을 통해 상위 블록과 하위 블록을 결합하고, 마지막 라운드에서는 상위 블록과 하위 블록을 뒤바꾸지 않고 암호화, 복호화를 하는 것으로 보인다.

 <br>

 </br>

 _encrypt() 과 _decrypt() 메서드는 각 블록에 대해 32라운드 동안 암호화 또는 복호화를 수행하고, encrypt()와 decrypt()는 데이터를 블록 단위로 나누어 암호화/복호화를 수행하는 것으로 보인다. 그 후, 전체 데이터는 _block_size 에 맞게 나누어 처리시키는 것으로 보인다.


<br>

 </br>

 <br>

 </br>

 ## utils.py

```
def xor_lst(a: list[int], b: list[int]) -> list[int]:
    return [x^y for x,y in zip(a,b)]

def xor_bytes(a: bytes, b: bytes) -> bytes:
    return bytes([x^y for x,y in zip(a,b)])

def int_to_bits(d: int, bits_len:int = 8) -> list[int]:
    return [int(x) for x in bin(d)[2:].zfill(bits_len)]

def bits_to_int(bits: list[int]) -> int:
    return int(''.join([str(x) for x in bits]),2)

def bytes_to_bits(m: bytes) -> list[int]:
    bits = []
    for b in m:
        bits += [int(x) for x in bin(b)[2:].zfill(8)]
    return bits

def bits_to_bytes(bits: list[int]) -> bytes:
    return bits_to_int(bits).to_bytes(len(bits)//8, 'big')

def add_mod_2_32(bit32: list[int], key32: list[int]) -> list[int]:
    return int_to_bits((bits_to_int(bit32) + bits_to_int(key32)) % (2**32), 32)

def rol11(bit32: list[int]) -> list[int]:
    return bit32[11:]+bit32[:11]
```
xor_lst(a: list[int], b: list[int]) -> list[int] 를 보니, 두 개의 비트 리스트를 받아 각 요소를 XOR 연산으로 결합한 결과를 반환하는 것으로 보인다. 

<br>

 </br>
 
xor_bytes(a: bytes, b: bytes) -> bytes 부분을 보니, 두 바이트 배열의 각 바이트를 XOR 연산으로 결합한 결과를 반환하는 것으로 보인다. 

<br>

 </br>

 int_to_bits(d: int, bits_len: int = 8) -> list[int] 부분을 보니, 정수를 이진수 비트 리스트로 변환하는 것으로 보인다. 정수를 2진수로 변환한 후, 지정된 비트 길이만큼 리스트로 반환하는 것으로 보인다. 

 <br>

 </br>

 bits_to_int(bits: list[int]) -> int 부분을 보니, 비트 리스트를 정수로 변환하고, 비트 리스트를 이진수 문자열로 바꾼 후, 이를 정수로 변환하는 것으로 보인다.

  <br>

 </br>

 bytes_to_bits(m: bytes) -> list[int] 부분을 보니, 바이트 배열을 비트 리스트로 변환하는 것으로 보인다. 바이트 배열의 각 바이트를 8비트씩 분리하여 비트 리스트로 만들어 반환하는 것 같다.

 <br>

 </br>

 bits_to_bytes(bits: list[int]) -> bytes 부분을 보니, 비트 리스트를 바이트 배열로 변환하는 것으로 보인다. 비트 리스트를 정수로 변환한 후, 이를 바이트로 변환하여 반환하는 것 같다.

  <br>

 </br>
 
add_mod_2_32(bit32: list[int], key32: list[int]) -> list[int] 부분을 보니, 두 32비트 비트 리스트를 더한 후, 2^32로 나눈 나머지를 계산하여 반환하는 것 같다. 

 <br>

 </br>
 
rol11(bit32: list[int]) -> list[int] 부분을 보니, 32비트 비트 리스트를 11비트 왼쪽으로 회전시키는 것으로 보인다. 이 파일 함수들은 주로 비트 연산과 데이터 변환을 하는 것 같은데, 이 함수들은 cipher.py에서 암호화와 복호화 과정의 비트 처리, 키 확정, 라운드 함수에서 많이 사용되는 것으로 보인다..

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

![image](https://github.com/user-attachments/assets/42cc042d-e9e0-4cd0-ba55-8fb041c5a281)

위처럼 코드를 작성하였다. prob.py를 실행하여 얻은 값을 encrypted_flag_hex 변수에 암호화된 플래그 값으로 지정하고, XOR 연산을 사용하여 원래 키를 복구하고, GHOST 암호 알고리즘으로 암호화된 플래그를 복호화하게끔 작성해보았다. 
<br>

 </br>
 
![image](https://github.com/user-attachments/assets/a74df9f0-1f24-466c-8904-0c17ddbde7c2)

이를 실행해보니, 어떤 문자열이 나오기는 하는데, 기존의 드림핵 플래그 형식의 값은 나오지 않는다... 그 후에도, 내가 작성한 파이썬 코드를 몇 번 수정해보았지만, 값이 계속 알 수 없는 형태의 문자열로만 출력되었다... 어디서부터 잘못되었는지 감이 잘 안 잡혀서, 나중에 다른 분들의 라이트업을 보고 방향성을 다시 잡아야 할 것 같다.
