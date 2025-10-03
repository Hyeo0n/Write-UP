<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> picoCTF "WinAntiDbg0x300"  Write-UP </h1>
</body>
<br>
<br>
</html>

2025.05.21 풀이

<img width="1208" height="702" alt="image" src="https://github.com/user-attachments/assets/3fb6dd63-de16-4745-9b73-76c078f02127" />

<img width="654" height="298" alt="image" src="https://github.com/user-attachments/assets/f8e06887-250a-4588-8b0b-87d505a7bd1e" />

해당 exe 파일을 실행시켜보니, Oops! Debeugger Detected라면서 종료된다.

<br>

</br>

<img width="1280" height="1017" alt="image" src="https://github.com/user-attachments/assets/3b0a1f1f-aa8b-4f3f-92d1-02bdf7566d56" />

또한, 해당 파일을 Hxd를 통해, 살펴보니 UPX가 있는 것을 보아 패킹된 것으로 보인다.

<br>

</br>

<img width="1280" height="272" alt="image" src="https://github.com/user-attachments/assets/ade80417-a8fb-40d9-b9e0-0de89a9ef80d" />

이를 upx를 통해, 패킹을 풀어주었다.

<br>

</br>

<img width="1280" height="617" alt="image" src="https://github.com/user-attachments/assets/b432d549-7355-4deb-a0e9-2a7f13bec024" />

IDA로 살펴보니, 디버거의 여부를 살피고, Oops! 문자열을 출력하는 것을 확인할 수 있다.

<br>

</br>

<img width="1280" height="976" alt="image" src="https://github.com/user-attachments/assets/a8d3d451-cfab-4c9f-9348-4fcd8d8d11d2" />

<img width="1280" height="675" alt="image" src="https://github.com/user-attachments/assets/622c2d80-f3e1-4dc9-a304-41dc055da69f" />

해당 Oops! 문자열을 검색해서 찾은 후, 이동하였다.

<br>

</br>

<img width="1280" height="593" alt="image" src="https://github.com/user-attachments/assets/5843b524-933f-41ff-8303-cb6e0904f972" />

<img width="1280" height="567" alt="image" src="https://github.com/user-attachments/assets/cf3a4163-f53b-4c2d-93df-0391242f41fd" />

해당 je 부분을 jmp로 수정하여, 조건을 만족하지 않더라도 무조건 분기하게끔 수정한다.

이렇게 수정한 후, 관리자 권한으로 실행해보니,


<img width="1130" height="548" alt="image" src="https://github.com/user-attachments/assets/e255073f-3883-4db2-9616-ccf36b8c1dc7" />

그리고 이를 따라서, 실행해보니, 다음과 같은 창이 뜨면서 flag값을 얻을 수 있었다.

<br>

</br>

```
picoCTF(WindOws_antid3bg_0x300_da7fdd01}
```

