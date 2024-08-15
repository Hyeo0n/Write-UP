<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1>2024 3S CTF "DUM DUM :p"  Write-UP</h1>
</body>
<br>
<br>
</html>

![image](https://github.com/user-attachments/assets/f9e22ac2-3c40-48dc-87f4-e7cf87e1bff3)

<br>

</br>
<br>

</br>

![image](https://github.com/user-attachments/assets/bcf29b90-fd03-44d9-8a87-ce99fb3cc82c)

파일 압축을 해제하니, dump.bin 파일 하나만 있었다.
이 dump.bin 파일 칼리 리눅스로 가져와서,
<br>

</br>
<br>

</br>

![image](https://github.com/user-attachments/assets/4b267198-4c44-4579-b456-cf82f3dec4c4)

``binwalk`` 명령어를 사용해서, dump.bin 파일을 탐색해보았다.
명령어를 입력하니, 압축 파일, 이미지 파일 등으로 보이는 여러 파일들이 나왔다. 

<br>

</br>
<br>

</br>

![image](https://github.com/user-attachments/assets/3cb1be52-32ea-47fa-9997-cc4900d81520)

![image](https://github.com/user-attachments/assets/7d7835a6-b77f-4275-8dd8-f51002b4c29b)

![image](https://github.com/user-attachments/assets/942b20ae-50be-4ff4-ae58-2ea53c06f8b5)

![image](https://github.com/user-attachments/assets/031f268d-07ff-437b-bb84-e9bbbb43c3fe)

``binwalk --dd ".*" `` 명령어를 사용하여서, 아까 본 파일들처럼 dump.bin 안에 있던 모든 임베디드 파일이나 데이터를 자동으로 추출하게끔 하였다.
그렇게 추출된 데이터들이 저장된 _dump.bin.extracted 파일이 생겨서, 들어가보니, 압축 파일과 해당 이미지 파일과 같은 파일들을 확인할 수 있었다.
<br>

</br>
<br>

</br>

![image](https://github.com/user-attachments/assets/768fef9a-1d80-4b04-b149-452620708d3a)

![image](https://github.com/user-attachments/assets/08c5731e-a0c2-4a48-93a4-8e671c716976)

압축 파일을 한 번 살펴보니, djye.txt 라는 파일에 플래그를 확인할 수 있었다!
<br>

</br>
<br>

</br>

```
3S{EA5Y_DUMP_F1L3_G00D_:P}
```
