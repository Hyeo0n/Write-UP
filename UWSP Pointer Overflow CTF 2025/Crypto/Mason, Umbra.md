<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> UWSP Pointer Overflow CTF 2025 "Mason, Umbra"  Write-UP</h1>
</body>
<br>
<br>
</html>

2025.10.31 풀이

<img width="724" height="338" alt="image" src="https://github.com/user-attachments/assets/204215c3-17aa-4bee-8747-89a4890e57aa" />

해당 문제는 쉬프트 암호 문제라고 한다. 앞부분의 ZyMDP 부분은 poctf 일 것으로 보이니, 이를 생각해보면 ZyMDP를 뒤로 10칸 이동시키면 맞아떨어진다.

그래서 ZyMDP{E GCZ_4V mR3W157 5_P1 b 3} 를 뒤로 옮겨가면 poctf{uwsp_4l_ch3m157_5_f1r3} 가 나온다!!
<br>
<br>

```
poctf{uwsp_4l_ch3m157_5_f1r3}
```
