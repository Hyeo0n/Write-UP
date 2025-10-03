<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> picoCTF "WinAntiDbg0x100"  Write-UP </h1>
</body>
<br>
<br>
</html>

2025.05.21 풀이

<img width="1216" height="498" alt="image" src="https://github.com/user-attachments/assets/381a42a8-fe23-4f0e-abdb-513d40626413" />

문제 설명을 살펴보니, 해당 문제는 안티 디버깅에 관련된 것이라고 한다.   
또한, exe 파일은 cmd 창으로 실행시킬 수 있고, 디버거를 통해 디버깅하여 문제를 풀 수 있을 것으로 보인다.

<br>

</br>

<img width="1280" height="352" alt="image" src="https://github.com/user-attachments/assets/495de0e3-f36f-4df5-bd32-1d057b667d0b" />

해당 exe를 cmd 창에서 실행시켜보니, 문제를 시작하고 싶으면 디버거를 사용해보라고 한다. 

<br>

</br>

<img width="1280" height="926" alt="image" src="https://github.com/user-attachments/assets/4a960d7a-e617-4e4c-83c9-3306cd5ca735" />

디버거로 실행시키니, 얼마 안가 바로 종료되서 로그창을 살펴보았다. 디버거가 감지되었다는 문구가 뜨며, 종료된 것으로 보인다.

<br>

</br>

<img width="1280" height="952" alt="image" src="https://github.com/user-attachments/assets/37469b9a-cc79-49ba-ae72-7103f5524a28" />

 디버거에서 문자열 참조를 검색해서 Oops! The Debugger was detected. 문자열을 검색한 후, 그 곳으로 이동하였다.

<br>

</br>

<img width="1280" height="623" alt="image" src="https://github.com/user-attachments/assets/3c548086-b2a4-406a-86a0-eaf024f9ea26" />

이동하니, IsDebuggerresent 결과가 0이면 je가 성공하여 0012161B로 이동하여, flag를 준비하고 출력하는 부분으로 이어지는 것으로 보인다.
근데 현재 결과가 1이므로, 해당 주소로 분기되지 않아서 flag 값을 얻지 못하는 것을 확인할 수 있다.

<br>

</br>

<img width="1280" height="864" alt="image" src="https://github.com/user-attachments/assets/a99c31b4-1bd6-4787-8edb-c8cc88e85ba9" />

<img width="1280" height="845" alt="image" src="https://github.com/user-attachments/assets/5e64129a-8a1e-40a6-9adb-171bbb55901f" />

따라서 해당 Oops! 문자열 위에 je 를 jmp로 변경시켜주었다.

<br>

</br>

<img width="1280" height="379" alt="image" src="https://github.com/user-attachments/assets/25e65e02-7ee7-46ce-a8ec-e58d27376dee" />

변경시키고, 실행시켜보니 flag 값을 얻을 수 있었다.


<br>

</br>

```
picoCTF{d3bug_f0r_th3_Win_0x100_e70398c9}
```
