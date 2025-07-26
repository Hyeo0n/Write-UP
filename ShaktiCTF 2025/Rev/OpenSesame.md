<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> ShaktiCTF 2025 "OpenSesame"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.07.26 풀이

<img width="1140" height="1334" alt="image" src="https://github.com/user-attachments/assets/d76195b1-9c93-4b98-9be3-8ac15a607dbf" />

ShaktiCTF의 다음과 같은 리버싱 문제를 풀어보려고 한다.
문제파일에 들어있는 OpenSesame 파일을 살펴보려고 한다.

<br>

</br>


<img width="1336" height="84" alt="image" src="https://github.com/user-attachments/assets/96d8af78-94d9-4fbe-9b98-d974cc2b87ef" />

<img width="360" height="116" alt="image" src="https://github.com/user-attachments/assets/0b8bfb3a-3e82-4533-99ec-d9afd5ebe2ec" />

<img width="420" height="844" alt="image" src="https://github.com/user-attachments/assets/c0b58751-181d-445c-82be-4cfedd0a735b" />

file 명령어로 살펴보니, 해당 파일은 64비트 ELF 실행 파일이고, 실행하면, 입력값을 입력받고, 이를 비교하여, The thieves spot you at the cave. Run!이거나 
boop you have gold now 메시지 출 형식으로 진행되는 것 같아 보인다.

그 후, strings 명령어로 살펴보니, 성공/실패 메시지 뒤에 문자열로 보이는 :*3$" 부분이 중요해 보인다.

<br>

</br>

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int v3; // ebx
  int v4; // ebx
  int v5; // ebx
  int v6; // ebx
  int i; // [rsp+0h] [rbp-1F0h]
  int j; // [rsp+4h] [rbp-1ECh]
  int k; // [rsp+8h] [rbp-1E8h]
  int m; // [rsp+Ch] [rbp-1E4h]
  int n; // [rsp+10h] [rbp-1E0h]
  int ii; // [rsp+14h] [rbp-1DCh]
  int jj; // [rsp+18h] [rbp-1D8h]
  int kk; // [rsp+1Ch] [rbp-1D4h]
  int v16[48]; // [rsp+20h] [rbp-1D0h]
  int v17[48]; // [rsp+E0h] [rbp-110h]
  char s[56]; // [rsp+1A0h] [rbp-50h] BYREF
  unsigned __int64 v19; // [rsp+1D8h] [rbp-18h]

  v19 = __readfsqword(0x28u);
  v17[0] = 345;
  v17[1] = 312;
  v17[2] = 291;
  v17[3] = 321;
  v17[4] = 348;
  v17[5] = 315;
  v17[6] = 297;
  v17[7] = 348;
  v17[8] = 306;
  v17[9] = 369;
  v17[10] = 216;
  v17[11] = 315;
  v17[12] = 315;
  v17[13] = 147;
  v17[14] = 315;
  v17[15] = 147;
  v17[16] = 315;
  v17[17] = 147;
  v17[18] = 315;
  v17[19] = 315;
  v17[20] = 285;
  v17[21] = 282;
  v17[22] = 285;
  v17[23] = 282;
  v17[24] = 285;
  v17[25] = 285;
  v17[26] = 153;
  v17[27] = 150;
  v17[28] = 156;
  v17[29] = 153;
  v17[30] = 150;
  v17[31] = 156;
  v17[32] = 150;
  v17[33] = 285;
  v17[34] = 153;
  v17[35] = 330;
  v17[36] = 318;
  v17[37] = 144;
  v17[38] = 363;
  v17[39] = 285;
  v17[40] = 342;
  v17[41] = 153;
  v17[42] = 354;
  v17[43] = 285;
  v17[44] = 282;
  v17[45] = 285;
  v17[46] = 375;
  puts("~Speak the magic words And Enter~");
  puts("Your answer: ");
  __isoc99_scanf("%s", s);
  for ( i = 0; i < strlen(s); ++i )
    v16[i] = s[i];
  seed_one();
  for ( j = 0; j < strlen(s); ++j )
  {
    v3 = v16[j];
    v16[j] = rand() ^ v3;
  }
  seed_two();
  for ( k = 0; k < strlen(s); ++k )
  {
    v4 = v16[k];
    v16[k] = rand() ^ v4;
  }
  seed_one();
  for ( m = 0; m < strlen(s); ++m )
  {
    v5 = v16[m];
    v16[m] = rand() ^ v5;
  }
  seed_two();
  for ( n = 0; n < strlen(s); ++n )
  {
    v6 = v16[n];
    v16[n] = rand() ^ v6;
  }
  for ( ii = 0; ii < strlen(s); ++ii )
    v16[ii] = l_rotate((unsigned int)v16[ii], 3LL);
  for ( jj = 0; jj < strlen(s); ++jj )
    v16[jj] = 3 * r_rotate((unsigned int)v16[jj], 35LL);
  for ( kk = 0; kk < strlen(s); ++kk )
  {
    if ( v16[kk] != v17[kk] )
    {
      puts("The thieves spot you at the cave. Run!");
      exit(0);
    }
  }
  puts("boop you have gold now");
  return 0;
}
```

IDA로 해당 OpenSesame의 main 부분을 살펴보았다. 코드를 살펴보니, 사용자 입력 s가 `v16[i] = s[i]` 형식으로 ASCII 값이 저장되고, 

v16은 총 4번을 XOR하는데, 각각 rand() 값을 사용해 난수화하는 것으로 보인다. 그 과정에서, seed_one(), seed_two() 함수로 rand()의 시드를 초기화하는 것으로 보인다.

마지막에는 `v16[i] = l_rotate(v16[i], 3)`으로 왼쪽 회전 3비트와 `v16[i] = 3 * r_rotate(v16[i], 35)`로 오른쪽 회전 35비트 후 3배수하는 것으로 보인다. 

그 과정에서, 그 결과가 v17 배열과 일치해야 정답으로 인정되어 성공 메시지가 뜨는 것으로 보인다. 


<br>

</br>

<br>

</br>

```
import random

# 주어진 v17 암호화된 값 배열
v17 = [
    345, 312, 291, 321, 348, 315, 297, 348, 306,
    369, 216, 315, 315, 147, 315, 147, 315, 147,
    315, 315, 285, 282, 285, 282, 285, 285, 153,
    150, 156, 153, 150, 156, 150, 285, 153, 330,
    318, 144, 363, 285, 342, 153, 354, 285, 282,
    285, 375
]

# Rotate 함수
def l_rotate(val, n):
    n = n % 32
    return ((val << n) | (val >> (32 - n))) & 0xFFFFFFFF

def r_rotate(val, n):
    n = n % 32
    return ((val >> n) | (val << (32 - n))) & 0xFFFFFFFF

# 회전 복호화
def reverse_rotations(encrypted_vals):
    reversed_vals = []
    for val in encrypted_vals:
        val //= 3
        val = l_rotate(val, 35)
        val = r_rotate(val, 3)
        reversed_vals.append(val & 0xFFFFFFFF)
    return reversed_vals

# XOR 복호화
def reverse_xors(encrypted_vals):
    SEED_ONE = 0x4F347
    SEED_TWO = 0x59334

    random.seed(SEED_TWO)
    temp = [v ^ random.randint(0, 0x7FFFFFFF) for v in encrypted_vals]

    random.seed(SEED_ONE)
    temp = [v ^ random.randint(0, 0x7FFFFFFF) for v in temp]

    random.seed(SEED_TWO)
    temp = [v ^ random.randint(0, 0x7FFFFFFF) for v in temp]

    random.seed(SEED_ONE)
    final = [v ^ random.randint(0, 0x7FFFFFFF) for v in temp]

    return final

# 전체 복호화 진행
stage1 = reverse_rotations(v17)
ascii_vals = reverse_xors(stage1)
recovered = ''.join(chr(c) for c in ascii_vals if 0 <= c <= 255)

print("최종 :", recovered)
```
그래서 위에 연산은 역산하는 스크립트를 작성해보았다. 

<br>

</br>

<img width="1106" height="118" alt="image" src="https://github.com/user-attachments/assets/588a1c0f-e29b-47a8-a396-51b0e2cbc65a" />
<img width="976" height="704" alt="image" src="https://github.com/user-attachments/assets/3d959643-4841-4686-99ba-a34398417e1a" />

이를 실행해보니, 플래그 값을 얻을 수 있었다.

<br>

</br>

```
shaktictf{Hii1i1i1ii_^_^__3243242_3nj0y_r3v_^_}
```
