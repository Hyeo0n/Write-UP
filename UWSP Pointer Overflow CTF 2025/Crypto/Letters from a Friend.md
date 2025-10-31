<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> UWSP Pointer Overflow CTF 2025 "Letters from a Friend"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.10.31 풀이

<img width="720" height="1186" alt="image" src="https://github.com/user-attachments/assets/d857265c-cc5f-4271-bad5-21cc2aec56a6" />

해당 문제를 읽어보면,  Blaise와 Friedrich라는 이름이 언급되는데, 이는 Blaise de Vigenère와 Friedrich Kasiski로 비네제르 암호 문제인 것으로 보인다.

또, 6글자 키를 사용하고, 암호문 안에는 특정 단어가 반복적으로 등장한다고 한다. 그리고 각 문장은 그 단어로 끝난다고 한다. 
암호문의 형태를 살펴보니, 비네제르 암호의 전형적인 구조인 것을 미루어보았을 때, 이 문제가 비네제로 암호 문제인 것으로 보인다.
키가 6글자라고 했는데, 문제 설명이나 제목에서 나오는 `friend`라는 글자를 키라고 가정해보고 복호화를 시도해보았다.
<br>
<br>

```
def vigenere_decrypt(ciphertext, key):
    plaintext = ""
    key = key.upper()
    key_index = 0
    for char in ciphertext:
        if char.isalpha():
            # A=0 ~ Z=25로 변환
            offset = ord('A')
            c_val = ord(char.upper()) - offset
            k_val = ord(key[key_index % len(key)]) - offset

            # 복호화: (C - K) mod 26
            p_val = (c_val - k_val) % 26
            plaintext += chr(p_val + offset)

            key_index += 1
        else:
            plaintext += char
    return plaintext

cipher = """IVIVS UNVVH ZBKIQ IAGNY WTRBT LIVRZ JCTJE LJELM ZDIVI RRZKI QIAGF ELXUH DRZIZ BKIQI AGYYM JYDLK MBGLX TIRVF FCTCB XKIQI AGHFV ZRUYK PEGDS UAYOP NKQXS UNVVH VKTGM XULXN IWAWY FWLNU IWZMR QIBIW VVPZX PHVAZ ORRUJ ZAINV NVZAU HSKPI EHXIM TRDYV LABUI JNVVH SU"""
key = "FRIEND"

print(vigenere_decrypt(cipher, key))
```
그래서 다음과 같이 비제네르 암호를 복호화하는 스크립트를 작성해보았다. 알파벳을 숫자로 매핑하고, mod를 사용해서 바꾸게 하였다.
<br>
<br>

<img width="2334" height="194" alt="image" src="https://github.com/user-attachments/assets/be7ef41e-24a8-4873-a8d5-82cf01153321" />

이를 돌려보니까, 복호화된 값이 나왔다. 이를 자연스럽게 띄어쓰기를 하여 원문으로 바꿔보면, 
<br>
<br>

```
DEAR FRIEND,
MY FRIEND, I HOPE YOU ARE WELL.
FRIEND, I MADE A NEW FRIEND, AND THEY ARE MY FRIEND.
THE FLAG TEXT IS "CAN I CALL YOU FRIEND".
CONVERT THAT AND SUBMIT IT.
I HOPE THIS WASN'T TOO HARD, FRIEND.
KASISKI + VIGENERE IS EASIER WHEN THERE ARE REPEATED WORDS, FRIEND.
```
복호화된 값을 통해, 플래그를 `CAN I CALL YOU FRIEND`인 것으로 보인다.
<br>
<br>

이를 해당 CTF 규칙에 맞게 변환하여 플래그 값을 얻을 수 있다. 

```
poctf{uwsp_c4n_1_c4ll_y0u_fr13nd}
```
