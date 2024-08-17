<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "Find Spade"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/8927608e-dc7b-4505-b5d1-d438a8953860)

문제 파일을 다운받아보니, find spade.exe 파일 하나가 있었고, 이를 실행시켜 보니, 이런 콘솔창이 나왔다.
<br>

 </br>
 <br>

 </br>
 
살펴보니, ♠️ 랑 같이 쓰여있는 수들이 16진수로 보였다. 그리고, 마지막에 플래그 앞에 ASCII라고 쓰여 있는 거 보니, 저 16진수들을 아스키로 반환하고, ABCDEFGHIJ에 대입하면, flag 값을 알아낼 수 있을 거 같았다. ♠️가 무슨 수를 의미하는지 알아내야 계산을 해볼 수 있을 거 같아서 찾아보니, ♠️의 유니코드 값이 0x2660이라고 한다. 이를 활용하여, 차근차근 계산을 진행해보려고 한다. 
<br>

 </br>
 <br>

 </br>

## A
A = ♠️ - 0x25eb

♠️ = 0x2660  ->  A = 0x2660 - 0x25eb = 9824 - 9707 = 117 (0x75)

![image](https://github.com/user-attachments/assets/451ea881-d907-4b77-9d1b-7915982a149c)
<br>

 </br>
 <br>

 </br>
 
## B
0x2674 - B = ♠️

B = 0x2674 - ♠️  ->  B = 0x2674 - 0x2660 = 9844 - 9824 = 20 (0x14)

![image](https://github.com/user-attachments/assets/3a72a434-a708-4105-a0c8-998bab604b09)

<br>

 </br>
 <br>

 </br>
 
## C
0x26d1 - C = ♠️

C = 0x26d1 - ♠️  ->  C = 0x26d1 - 0x2660 = 9937 - 9824 = 113 (0x71)

![image](https://github.com/user-attachments/assets/f611217a-9dd6-4e53-937b-584a5c29883c)
<br>

 </br>
 <br>

 </br>
 
## D
(D * 0x72) - 0xf10 = ♠️

D = (♠️ + 0xf10) / 0x72  

->  D = (0x2660 + 0xf10) / 0x72 = (9824 + 3856) / 114 = 13680 / 114 = 120 (0x78)

![image](https://github.com/user-attachments/assets/da7bf99b-15dc-47cb-a525-bb97b5e47203)

![image](https://github.com/user-attachments/assets/f048cd9f-9813-4877-aade-597f14d88e67)

<br>

 </br>
 <br>

 </br>
 
## E
0x25 * (E - B) + 0x195e = ♠️

E = (♠️ - 0x195e) / 0x25 + B  

-> E = (0x2660 - 0x195e) / 0x25 + 0x14 = (9824 - 6494) / 37 + 20 = 0x6E = 110 (0x6E)
<br>

 </br>
 <br>

 </br>
 
## F
바트연산자가 사용된 걸로 보인다.

0x200 * (F >> 1) + 0x1460 = ♠️

F = ((♠️ - 0x1460) / 0x200) << 1  

-> F = ((0x2660 - 0x1460) / 0x200) << 1 = ((9824 - 5216) / 512) << 1 =  18 (0x12)
<br>

 </br>
 <br>

 </br>
 
## G
0x75 * (G << 4) - 0x34720 = ♠️

G = ((♠️ + 0x34720) / 0x75) >> 4  

->  G = ((0x2660 + 0x34720) / 0x75) >> 4 = ((9824 + 214816) / 117) >> 4 = 120 (0x78)
<br>

 </br>
 <br>

 </br>
 
## H
C + H + 0x258e == ♠️

H = ♠️ - C - 0x258e  -> H = 0x2660 - 0x71 - 0x258e = 97 (0x61)
<br>

 </br>
 <br>

 </br>
 
## I
(I + 3) ^ 0x2635 = ♠️

I = (♠️ ^ 0x2635) - 3  

->  I = (0x2660 ^ 0x2635) - 3 = (9824 ^ 9781) - 3 = 82 (0x52)
<br>

 </br>
 <br>

 </br>
 
## J
(J ^ A) + 0x2644 = ♠️

J = ♠️ - 0x2644 + A 

-> J = 0x2660 - 0x2644 + 0x75 = 9824 - 9796 + 117 = 145 (0x91)
<br>

 </br>
 <br>

 </br>
 
## ASCII
ABCDEFGHIJ를 아스키로 변환해보면, ``u qxn xaR``이다. 중간에 B, F, J는 제어 문자라서 화면에 표시되지 않는 것으로 보인다. 
<br>

 </br>
 <br>

 </br>
 
```
3S{u qxn xaR}
```
이게 맞는 답인지 확인할 방도가 없어서, 맞는 답인지는 모르겠다. 
