<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "BrokenHearted"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/82caeb1b-7cc1-4123-a9e4-9c38904f6032)

<br>

 </br>
 <br>

 </br>
 
<img width="718" height="553" alt="image" src="https://github.com/user-attachments/assets/3206940b-bdb4-46ea-a933-6d0c381d4af5" />

<img width="1088" height="1072" alt="image" src="https://github.com/user-attachments/assets/02b8ede2-ea37-40b7-834f-9ea4482e6e49" />

그때, 해당 CTF.jpeg 파일을 파일 카빙해서 찾으려고 시도하였었다. 파일 카빙이란 파일 자체의 바이너리 데이터를 이용해 디스크의 비활당 영역에서 파일을 복구하는 방식을 말한다. 즉, 지워졌거나, 손상되었거나, 숨겨진 파일을 디스크 이미지, 메모리 덤프, 이미지 파일 같은 바이너리에서 발굴해내는 것을 말한다. 이를 참고하여 풀이를 시도해보려고 한다.
 <br>

 </br>

 <img width="1280" height="1062" alt="image" src="https://github.com/user-attachments/assets/23d817a0-bd7e-478c-b16e-5149c7b19ad5" />

일단 먼저, CTF.jpeg를 Hxd를 통해, 살펴보았다. 살펴보니, 끝쪽에 '아마도 LSB 스테가노그래피를 볼 수 있을 것이다. 그리고 항상 끝을 주의깊게 확인해라'라고 써있었다. 또한, 중간 중간 Hint1.png, Hint2.png라고 써있는 걸 보았을 때, 아마 파일 카빙을 시도하면, 해당 이미지들이 새롭게 발견되지 않을까? 하는 생각이 들었다.
 <br>

 </br>

 <img width="920" height="252" alt="image" src="https://github.com/user-attachments/assets/6ad91d44-b8ba-4488-b18a-5ab5d91acc6e" />

파일 카빙하는 도구인 foremost를 사용해서 CTF.jpeg를 파일 카빙해보았다. 

 <br>

 </br>

 <img width="480" height="240" alt="image" src="https://github.com/user-attachments/assets/86a4d146-5c10-41df-8240-4443b771880b" />

 <img width="552" height="202" alt="image" src="https://github.com/user-attachments/assets/72450826-f013-4806-af1f-108768c26fc3" />

<img width="572" height="226" alt="image" src="https://github.com/user-attachments/assets/ea493f32-da44-46e7-aa1a-12ec2e7f686f" />

<img width="766" height="512" alt="image" src="https://github.com/user-attachments/assets/e476cc58-2751-4b89-9a86-4765ce710cce" />

<img width="1280" height="1231" alt="image" src="https://github.com/user-attachments/assets/480087bd-19b4-4900-b7e0-66963ff502f0" />

<img width="1280" height="1233" alt="image" src="https://github.com/user-attachments/assets/bff31a4b-2e57-4fa0-a98c-a6bc949c5504" />

<img width="1280" height="1235" alt="image" src="https://github.com/user-attachments/assets/5f20630c-3ee8-4103-89a1-c1eb844ce1ae" />

파일 카빙을 해보니, 새로운 파일들이 많이 생겼다. zip 디렉토리에 들어가보니, 두 개의 압축파일이 있어, 이를 풀어보니, flag.bmp와 앞서 Hxd에서 본 Hint1.png, Hint2.png가 새롭게 보였다. 

 <br>

 </br>

 
 <img width="1280" height="947" alt="image" src="https://github.com/user-attachments/assets/55f496d9-3f84-454a-b35f-62f24fd036f0" />
Hint1.png를 Hxd로 살펴보니, 끝에 숨겨진 내용이 있었다. 그래서 맨 처음, CTF.jpeg에서 맨 끝을 항상 살펴보라는게 다른 것들에도 다 끝에 숨겨진 내용이 있어서 그랬던 거구나하고 생각했다. 또한, Hint1.png에도 아까 발견한 텍스트와 똑같은 텍스트가 작성되어 있는 것을 확인할 수 있었다.
 <br>

 </br>

<img width="1280" height="903" alt="image" src="https://github.com/user-attachments/assets/160a5d91-1077-4cf5-b852-da4c9ac343e4" />
Hint2.png를 살펴보니, 이번에도 역시 끝에 숨겨진 내용이 있었다. 숨겨진 텍스트를 살펴보니, LSB를 결합해야 한다는 말이 있다.
 <br>

 </br>

 <img width="1280" height="1025" alt="image" src="https://github.com/user-attachments/assets/af37cebb-1c1b-4e93-b882-14cace957c67" />
마지막으로, flag.bmp를 살펴보니, '00000100~00005000 줄의 다른 16진수 값을 살펴보라. 플래그는 FE로 시작하고 총 216바이트로 구성된다.' 라는 힌트를 얻었다. 아마, 해당 범위 내에서 FE이로부터 시작해서 216바이트 길이의 데이터를 찾으면 된다고 하는 것 같다.
 <br>

 </br>

<img width="790" height="570" alt="image" src="https://github.com/user-attachments/assets/e30f02fa-62d0-47ce-a1b2-09049d51d712" />

<img width="1240" height="512" alt="image" src="https://github.com/user-attachments/assets/b7ab6597-a0ef-4c34-a5c0-4a0e5e877b74" />
 
찾기 기능으로 FE를 찾아보니, 바로 00000100에서 부터 시작하는 것을 찾을 수 있었다. 현재 사진에 표시된 곳까지가 216바이트 길이의 데이터이다. 여기서 어떻게 해야될 지 감이 안 와서, LSB 스테가노그래피 얘기를 계속한 것을 떠올려, 이에 관해 검색을 해보았다.
 <br>

 </br>

 <img width="1118" height="1314" alt="image" src="https://github.com/user-attachments/assets/04a3cdab-ed0f-40fc-9a44-17309cdd6e0d" />
(출처: https://hooneee.tistory.com/422)


LSB 스테가노그래피에 대해 살펴보니, 최하위 비트를 변조하는 것인데, 보통 jpeg, bmp 파일에 적용된다고 한다. 그리고 보통, 흰색을 나타내는 0xFFFFFF 값에 대해 LSB 변조를 하면 FE가 된다고 한다. FE의 LSB는 0이고, FF의 LSB는 1이며, 0과 1로 8자리, 즉 char 값을 나타낼 수 있으므로 ASCII에 대응하는 문자열을 숨길 수 있다고 한다. 

이에 따라, 각 FE와 FF를 0과 1로 변환해보려고 한다. 

```
þþÿÿþþÿÿþÿþÿþþÿÿþÿÿÿÿþÿÿþÿÿþþÿÿþþþÿÿþþþþþþÿÿþÿÿþþÿÿþþÿþÿþÿÿþÿÿÿþþÿÿÿþþÿÿþþÿÿþþþÿþÿÿþþþÿÿþÿÿÿþþÿÿþÿþÿÿÿÿÿþþÿÿþþþÿþÿÿÿþþÿÿþÿþÿÿÿÿÿþÿþÿþþÿþþÿÿþþÿþÿþþÿÿþÿÿþþÿÿþÿÿþþþÿÿþÿÿþþþÿÿÿÿþþÿþÿþÿÿÿÿÿþÿþþþÿÿþþÿþÿþÿþÿþÿÿþÿÿÿþþÿÿÿÿÿþÿÿ  

↓

001100110101001101111011011001100011000000110110011001010110111001110011001100010110001101110011010111110011000101110011010111110101001001100101001101100110110001101100011110001011111010001100101010101101110011111011
```
 <br>

 </br>
 

그리고, ASCII에 대응하는 문자열을 숨길 수 있다고 하였으니까, 이를 ASCII로 변환해보았다. 

<img width="972" height="968" alt="image" src="https://github.com/user-attachments/assets/5bf7d13d-2cac-4098-bdba-f60d7b591419" />

변환해보니, `3S{f06ens1cs_1s_Re6lly_FUn}`라는 플래그를 얻을 수 있었다.

 <br>

 </br>
 

FLAG
```
3S{f06ens1cs_1s_Re6lly_FUn}
```

