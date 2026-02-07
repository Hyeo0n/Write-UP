<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <h1> 2026 INCOGNITO CTF "The Twin Paradox"  Write-UP</h1>
</body>
<br>
<br>
</html>

2026.02.07 풀이

<img width="1530" height="1434" alt="image" src="https://github.com/user-attachments/assets/0ee5a4e4-9d42-489a-a70d-fcffc613eb31" />

```
import random
import os
from Crypto.Util.number import getPrime, isPrime, bytes_to_long

def complex_add(z1, z2): return (z1[0] + z2[0], z1[1] + z2[1])
def complex_mul(z1, z2): return (z1[0]*z2[0] - z1[1]*z2[1], z1[0]*z2[1] + z1[1]*z2[0])
def energy_norm(z): return z[0]**2 + z[1]**2
def complex_mod(z, m): return (z[0] % m, z[1] % m)

def transmit(base, exp, mod):
    def c_divmod(z1, z2):
        denom = energy_norm(z2)
        numer = complex_mul(z1, (z2[0], -z2[1]))
        q_real = (numer[0] + denom // 2) // denom
        q_imag = (numer[1] + denom // 2) // denom
        q = (q_real, q_imag)
        rem = complex_add(z1, complex_mul(q, (-z2[0], -z2[1])))
        return q, rem
    
    res = (1, 0)
    base_val = base
    while exp > 0:
        if exp % 2 == 1: _, res = c_divmod(complex_mul(res, base_val), mod)
        _, base_val = c_divmod(complex_mul(base_val, base_val), mod)
        exp //= 2
    return res

# [1] LWE Oracle
def generate_lwe_instance(secret_vector):
    LWE_MOD = getPrime(64)
    A = (random.randint(0, LWE_MOD), random.randint(0, LWE_MOD))
    
    # Error is small enough for LWE assumption, but large enough to prevent brute-force.
    Ex = random.randint(-100000, 100000)
    Ey = random.randint(-100000, 100000)
    E = (Ex, Ey)
    
    # B = A * S + E (mod M)
    AS = complex_mul(A, secret_vector)
    B = complex_mod(complex_add(AS, E), LWE_MOD)
    
    return LWE_MOD, A, B

# [2] Key Generation Logic (Now Visible!)
def generate_entangled_keys(bits, drift):
    # Public Constant G
    gx = random.getrandbits(bits // 2)
    gy = random.getrandbits(bits // 2)
    G = (gx, gy) 

    while True:
        # 1. Generate Gaussian Prime Alpha
        r = random.getrandbits(bits)
        i = random.getrandbits(bits)
        if r % 2 == 0: r += 1
        Alpha = (r, i)

        if isPrime(energy_norm(Alpha)):
            # 2. Beta is entangled with Alpha: Beta = Alpha * G + Drift
            # This logic is crucial for the challenge.
            Alpha_G = complex_mul(Alpha, G)
            Beta = complex_add(Alpha_G, drift)
            
            if isPrime(energy_norm(Beta)):
                return Alpha, Beta, G

if __name__ == "__main__":
    # --- [REDACTED SECRET CONSTANTS] ---
    FLAG = b"INCOGNITO{REDACTED}"
    
    # Secret Drift Vector S (Unknown to attacker)
    dx = random.randint(100000, 900000)
    dy = random.randint(100000, 900000)
    real_drift = (dx, dy)

    # 1. Publish LWE Instance (Leak information about Drift)
    lwe_q, lwe_a, lwe_b = generate_lwe_instance(real_drift)

    # 2. Generate RSA Keys based on Drift
    Alpha, Beta, G_const = generate_entangled_keys(512, real_drift)
    Modulus = complex_mul(Alpha, Beta)
    Exponent = 65537

    # 3. Encrypt Flag
    L = len(FLAG) // 2
    Message = (bytes_to_long(FLAG[:L]), bytes_to_long(FLAG[L:]))
    Cipher = transmit(Message, Exponent, Modulus)
    
    # Output Format
    print(f"{lwe_q=}")
    print(f"{lwe_a=}")
    print(f"{lwe_b=}")
    print(f"N={Modulus}")
    print(f"G={G_const}")
    print(f"e={Exponent}")
    print(f"C={Cipher}")
```

주어진 문제 파일 prob.py 코드를 살펴보면, 가우스 정수 연산을 다루고 있는 것을 알 수 있다.

(x, y)는 `x + yi`로 두고 함수 `complex_mul`로 가우스 정수 곱을 구현하였다. (x,y)를 하나의 수처럼 사용해 계산한다.


그 후, `secret_vector(S)= (dx, dy)`와 같은 작은 무작위 비밀값을 만든다. 이는 원래 키 생성 구조를 살짝 틀어놓는 drift 역할을 한다. 

`AS = complex_mul(A, secret_vector)
B = complex_mod(complex_add(AS, E), q)` 코드를 살펴보면, 이 drift 값을 그대로 공개하지 않고, LWE(Learning With Errors) 형태로 드러낸다. 여기서 LWE는 원래는 정답에 작은 오차를 섞어서 숨기는 방식을 말한다.

즉 큰 소수 q와 랜덤 A를 뽑고, 작은 노이즈인 E를 더해서 `B ≡ A ⋅ S + E(mod q)`를 만든 뒤에, (q,A,B)를 공개한다. 


일단 다음과 같은 과정을 보았을 때 우리가 해당 문제를 풀려면 drift S를 복구해야 한다. 
위의 식을 풀어보면 `A ⋅ S + E = B + kq` 다음과 같은 식이 된다. k는 mod 연산 때문에 날라간 건데, 이를 k 자리에 하나씩 넣어보면서 원래의 정수 식으로 복구하는 방식으로 접근하였다.
이게 가능한 이유는 S가 만약 크다면 어떤 k가 맞는지 구분이 되지 않지만, 이 문제에서는 S가 아주 작고, E도 범위가 작으므로 k를 찾을 수 있다.

그와 동시에 S가 문제에서 준 범위(10만~90만) 안에 들어오고 residual 𝐸 = 𝐵′ − 𝐴 ⋅ 𝑆도 작으면 k 값을 확정할 수 있다. 

문제에서 공개키는 N = 알파 * 베타 형태인데, 베타는 사실 알파에 drift를 살짝 더해서 만든 값이라고 이해하면 되서, 둘은 거의 같은 구조로 묶여있다고 보면 된다. 

그래서 drift S를 알게 되면 N으로부터 알파를 반복 계산으로 쉽게 찾아낼 수 있고 그에 따라 베타도 구할 수 있게 되면서, RSA 모듈러를 직접 인수분해할 수 있게 된다. 

또한 문제에서 일반적으로 암호화할때 `transmit()` 함수를 사용하므로, 복호화때도 같은 방식으로 돌리면 답을 얻을 수 있다

<br>

</br>
<br>

</br>



```
from Crypto.Util.number import inverse, long_to_bytes
import math

# --- [1] ---
LWE_MOD = 13723961949211994657
LWE_A = (2888994149235268084, 9609593431197676929)
LWE_B = (2525832076166463274, 3137161026398176073)
N = (600550557176713090187716619245463005374872228656562592268219623356873810466559389870158481809402681612641006642127530103171396492661160580687712922248325996513679058580952490946148224697645648623204832961366515052672740643857744854993346673215524041215179965289847327587097771415129469094953887116024269679942315251612274979610245852077189279983223347123850554296850654418036269562750, 8455430966298311256436470274540219589023229057656681616158629011488325458648100386676551729789389412824900511481607369264562409149426750799241462792878107143293394913968037763848158916301878890320548992762704094676664272378483240799334621558499971007202613285107359067195240457039024838772916835544433168250442378066725297842789700537402890438512115049004773772732370239256137521577469)
G = (33755229244675391005684548301587247766425062278039606317042997406353807948152, 38704294803516622531797897179381328418972951286228612662530642796747772997597)
C = (2971709492431076785470179493234024415424443100619870153408911288766996276495044570824065536714613859380678487078584596754912385454554412105917163812419021948602954693710638499639318123434716833975183139901605403680857212755589986219041847111336179354293089063577768338855455148152485265112156984958538389794106283588691515193671662595763755839644480970913331815819943394391721404648188, 178790374256750525740336642013676090816302116276667739369898734637187611734969852275264396969063310581532622793532269446593304206309137201830771083786593248150945048197060421926266162382743929883312575647065746550366871180120374241947396803188255361913528304253760686242563360100272012607772877080640352792097525884170133493287752956474824540312686245190618743152868378445170869433613)
e = 65537

# --- [2] ---
def g_mul(z1, z2): return (z1[0]*z2[0] - z1[1]*z2[1], z1[0]*z2[1] + z1[1]*z2[0])
def g_add(z1, z2): return (z1[0] + z2[0], z1[1] + z2[1])
def g_sub(z1, z2): return (z1[0] - z2[0], z1[1] - z2[1])
def g_norm(z): return z[0]**2 + z[1]**2
def g_divmod(z1, z2):
    d = g_norm(z2)
    n = g_mul(z1, (z2[0], -z2[1]))
    q = ((n[0] + d // 2) // d, (n[1] + d // 2) // d)
    return q, g_sub(z1, g_mul(q, z2))

def g_isqrt(z):
    a, b = z
    norm = math.isqrt(a**2 + b**2)
    x = math.isqrt((norm + a) // 2)
    y = math.isqrt((norm - a) // 2)
    if b < 0: y = -y
    for dx in range(-2, 3):
        for dy in range(-2, 3):
            cand = (x + dx, y + dy)
            if g_mul(cand, cand) == z: return cand
    return (x, y)

# --- [3] Drift(S) 복구 ---
# A*S + E = B (mod M) => S = (B-E) * A^-1 (mod M)
# 가우시안 정수 모듈러 역원 구하기
def g_inv_mod(z, m):
    det = g_norm(z) % m
    det_inv = inverse(det, m)
    return ((z[0] * det_inv) % m, (-z[1] * det_inv) % m)

m = LWE_MOD
A_inv = g_inv_mod(LWE_A, m)

s_raw = g_mul(LWE_B, A_inv)
dx, dy = s_raw[0] % m, s_raw[1] % m

if dx > m // 2: dx -= m
if dy > m // 2: dy -= m

S = (dx, dy)
print(f"[*] Recovered Drift: {S}")

# --- [4] 복호화 ---
D = g_add(g_mul(S, S), g_mul((4, 0), g_mul(G, N)))
sqrt_D = g_isqrt(D)
alpha, _ = g_divmod(g_sub(sqrt_D, S), g_mul((2, 0), G))
beta, _ = g_divmod(N, alpha)

phi = (g_norm(alpha) - 1) * (g_norm(beta) - 1)
d = inverse(e, phi)

def g_pow(base, exp, mod):
    res = (1, 0)
    while exp > 0:
        if exp % 2 == 1: _, res = g_divmod(g_mul(res, base), mod)
        _, base = g_divmod(g_mul(base, base), mod)
        exp //= 2
    return res

msg = g_pow(C, d, N)
flag = long_to_bytes(msg[0]) + long_to_bytes(msg[1])
print(f"\n[+] Flag: {flag.decode().strip()}")
```


<img width="1472" height="190" alt="image" src="https://github.com/user-attachments/assets/1c259632-f061-4d6f-9028-4c98f758cd6c" />


위에서 말한 방식으로 복호화를 진행하는 익스플로잇 코드를 다음과 같이 짜서 돌리니 flag 값을 얻을 수 있었다
<br>

</br>


```
INCOGNITO{H4rd_Latt1c3_CVP_w1th_Aff1ne_Tr4nsf0rm_Is_Sup3r_C00l!}
```
