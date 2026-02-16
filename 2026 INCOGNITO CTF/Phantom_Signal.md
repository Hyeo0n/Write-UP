<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> 2026 INCOGNITO CTF "Phantom_Signal"  Write-UP</h1>
</body>
<br>
<br>
</html>

2026.02.16 풀이

<img width="1051" height="349" alt="image" src="https://github.com/user-attachments/assets/72fa347b-71b2-47ec-b83f-1c8f5ed4b5a8" />

문제를 살펴보면, 80번 포트는 그냥 낚시처럼 보인다.
'공격 시도와는 별개로, 내부망의 장비들이 서로를 식별하는 과정에서 노이즈 신호가 섞여 있었다.'
이 부분이 힌트가 될 것으로 보인다.
<br>

</br>


<img width="1522" height="284" alt="image" src="https://github.com/user-attachments/assets/f8d9f269-6e23-4ff6-a2e6-b1a4130ef397" />

패킷 캡처가 문제파일로 제공되었는데, 다 HTTP인데 SSDP 프로토콜만 세 개 있었는데, SSDP가 장비 식별하는데 사용하는 프로토콜이라고 해서 이들을 먼저 살펴봤다. 

이 세 패킷을 살펴보면 모두 USER-AGENT 부분이 이상하다는 것을 발견할 수 있다. 

원래 USER-AGENT 부분에는 "OS명/버전 UPnP/2.0 제품명/버전" 이 담겨져 있는데 이와 관련없는 rv값이 길게 담겨져 있었다. 
<br>

</br>

```
SU5DT0dOSVRPe0gxZGQzbl8xbl90aDNfTjAxczNfUHIwdDBjMGx9
```
패킷 순서대로 이렇게 rv 값을 모았다. 암호화된 값 같길래 아무 형식으로 계속 디코딩해보니 

<img width="1184" height="1172" alt="image" src="https://github.com/user-attachments/assets/af8d0d0e-d2fa-4626-a424-1295e5678ba0" />
<br>

</br>

```
INCOGNITO{H1dd3n_1n_th3_N01s3_Pr0t0c0l}
```

플래그 값을 구할 수 있었다.
