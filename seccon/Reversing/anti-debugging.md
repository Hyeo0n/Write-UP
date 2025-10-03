<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> seccon 2016 "anti-debugging"  Write-UP </h1>
</body>
<br>
<br>
</html>

2025.05.21 풀이

<img width="1074" height="372" alt="image" src="https://github.com/user-attachments/assets/f2edb27a-cbf1-421b-8bef-0e5751804721" />

해당 exe를 실행시켜보면, password를 입력하라는 창이 뜬다.

<br>

</br>

<img width="1280" height="906" alt="image" src="https://github.com/user-attachments/assets/9035ccea-c7e9-4c02-bb5d-f68f681d269d" />

이를 디버거로 살펴보니, "I have a pen." 이 password인 것을 확인할 수 있다.
근데 비밀번호를 눌러도 실행이 되지 않아서, 다시 디버거로 살펴보았다

<br>

</br>

<img width="1280" height="714" alt="image" src="https://github.com/user-attachments/assets/130c641c-b720-4852-b071-1af8252e027e" />

<img width="1280" height="1012" alt="image" src="https://github.com/user-attachments/assets/8e6fbae4-76b7-48ff-8014-a636bd5fc925" />

<img width="1280" height="373" alt="image" src="https://github.com/user-attachments/assets/7bc95f20-052f-4b46-90ae-43f45154194f" />

일단 IsDebuggerPresent 부분을 찾아서, 해당 부분 구현 부분의 반환값을 0이 되도록 변경하였다.

<br>

</br>

<img width="1280" height="632" alt="image" src="https://github.com/user-attachments/assets/a18236b1-65c7-4d34-9f4c-7dcf771ffd7e" />

<img width="1280" height="654" alt="image" src="https://github.com/user-attachments/assets/4f00ab2c-c399-4861-8ab0-ca4145deccb3" />

<img width="1280" height="583" alt="image" src="https://github.com/user-attachments/assets/437a55cb-71c9-42a8-af28-6c606e12a342" />

또한, NtGlobalFlag도 손보기 위해, 해당 부분을 찾아가서, je를 jmp로 변경하여서 우회하도록 하였다.

<br>

</br>

<img width="1280" height="565" alt="image" src="https://github.com/user-attachments/assets/04e6c32e-156e-45c7-8e5d-02e4f9dcb1b8" />

바로 밑에, CheckDebuggerPresent() 부분 또한 발견하여서, 이 부분도 jmp로 우회해주었다.

<br>

</br>

<img width="1280" height="493" alt="image" src="https://github.com/user-attachments/assets/374ad801-2619-45ae-8d21-9b1973f93e97" />

<img width="1280" height="629" alt="image" src="https://github.com/user-attachments/assets/acef3efb-1f7f-404e-9677-9d1b80d7ab3a" />

그 후, 이어서 GetTickCount로 타이밍 기반 탐지 하는 부분 또한 jmp로 우회해주었다.

<br>

</br>

<img width="1280" height="670" alt="image" src="https://github.com/user-attachments/assets/09025232-db70-40c3-a039-c1c7fae2498e" />

또 바로 이어서, ProcmonDebugLogger, Ollydbg, ImmunityDebugger, IDA, Wireshark, VMWare 등 여러 디버깅 툴들이 실행하는지 확인하는 탐지 부분이 있어서
이 또한 다 jmp로 수정하여 우회해주었다.

<br>

</br>

<img width="1280" height="439" alt="image" src="https://github.com/user-attachments/assets/8fc5a8a4-5731-4a4d-a4ec-1e9f28901185" />

그리고 0으로 나누기를 실행해서 강제로 예외 발생을 시키지 않고, 디버그 탐지 후 종료하지 않고, 디코딩된 문자열이 메시지박스를 잘 호출하도록 점프문을 수정해주었다.

<br>

</br>

<img width="986" height="274" alt="image" src="https://github.com/user-attachments/assets/b3f4ff52-1a87-4c13-9f1f-7e630869a886" />

<img width="330" height="172" alt="image" src="https://github.com/user-attachments/assets/27a673b4-9537-497d-99cf-c475b5885973" />

이를 실행해보니, password를 입력하니 비밀번호가 나왔다고 뜨고, flag 값을 가지고 있는 메시지 박스를 확인할 수 있었다,

<br>

</br>

```
SECCON{check_Ascii85}
```
