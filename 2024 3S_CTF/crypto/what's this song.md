<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "what's this song"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/9a826af6-8346-4988-8541-93951037d64f)

문제는 이런 문제인 것 같다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/8b14f4bb-ebbf-4a0b-81b3-175cbb35de9e)

힌트를 보니, 빈도 분석과 ASCII를 사용하면, 문제를 해결할 수 있는 것 같다. 

<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/f6848554-7e6c-4293-ac7b-5b7f61738c00)

output.txt를 살펴보니, 저 복잡한 문자열을 디코딩하면, 플래그 값을 찾을 수 있지 않을까 생각했다. 
<br>

 </br>
 <br>

 </br>
 
## malware.py
![image](https://github.com/user-attachments/assets/bec888e7-b16d-4f2c-993b-a8bde48ac13c)

이 파일을 살펴보니, 각 문자를 인코딩한 후, 매핑하고, 이를 난수를 사용하여 매핑을 무작위로 섞는 엄청 복잡해보이는 인코딩 과정을 거치는 것을 알 수 있었다. 
그래서 이를 복호화해보기 위해서, 여러 코드들을 작성해보았다
<br>

 </br>
 <br>

 </br>
 
## 1 시도
일단 정확한 시드 값을 알아내기 위해, malware salt 값을 통해 salt 값을 알아보려고 한다.
<br>

 </br>
 
![image](https://github.com/user-attachments/assets/642cf453-8bbc-49e2-b5ac-35abd3a7a47b)

위와 같은 코드를 작성하여서, 실행해보니
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/28c73fe0-47c3-4420-ad71-46fd2c11e908)

salt 값이 ``265388190964290737513642355606863780396`` 로 나왔다. 
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/94c3712d-ddac-41ea-ae57-80aa9b344227)
![image](https://github.com/user-attachments/assets/b9b81997-65ac-4f73-8290-80d5c50490ad)

얻은 seed 값을 통해, 복호화해 줄 decrypt_script 코드를 작성해보았다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/8ba18a34-20a9-4167-8e68-d40cd01f7c9b)

하지만, 제대로 복호화되지 않았다.
<br>

 </br>
 <br>

 </br>
 
## 2 시도

![image](https://github.com/user-attachments/assets/d3cbb0ba-662a-4fa5-8a17-1f0ec0715a26)

이번에는 힌트에 나왔던 빈도 분석이랑 ASCII를 사용해, output.txt 를 분석해보는 코드를 작성하였다.
<br>

 </br>
 <br>

 </br>
 
![image](https://github.com/user-attachments/assets/30914117-5b65-4eea-b134-af8520929ed9)

이번에도 역시 제대로 복호화되지 않았다..
여기서부터 어떻게 해야될지 모르겠어서, 막혔다.
