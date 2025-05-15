# EXP.NO.10-Simulation-of-Error-Detection-and-Correction-Algorithms
# AIM
To implement and demonstrate error detection and correction techniques using:<br>
1.Even Parity Check<br>
2.Checksum (16-bit)<br>
3.Hamming Code (7,4)<br>

# SOFTWARE REQUIRED
Python 3.x environment<br>
Code editor or IDE (e.g., VS Code, PyCharm)<br>
Basic knowledge of binary operations and error detection methods<br>

# ALGORITHMS
## Even Parity Check
*Encoding:<br>
   1.Count the number of 1’s in the binary data.<br>
   2.Compute parity as count % 2.<br>
   3.Append the parity bit to the original data.<br>
*Checking:<br>
   1.Count number of 1’s in the received data (including parity).<br>
   2.If count is even → valid; else → error detected.<br>
## Checksum (16-bit)<br>
*Encoding:<br>
   1.Divide data into 16-bit blocks.<br>
   2.Add all blocks using binary addition with carry wrap-around.<br>
   3.Take 1’s complement of the result as the checksum<br>
*Verification:<br>
   1.Add all blocks including the checksum.<br>
   2.Take 1’s complement.<br>
   3.If result is all 0’s → valid; else → error detected.<br>
## Hamming Code (7,4):<br>
*Encoding:<br>
    1.Insert 4 data bits: d1, d2, d3, d4.<br>
    2.Calculate 3 parity bits:<br>
       p1 = d1 ⊕ d2 ⊕ d4<br>
       p2 = d1 ⊕ d3 ⊕ d4<br>
       p3 = d2 ⊕ d3 ⊕ d4<br>
    3.Arrange as: p1 p2 d1 p3 d2 d3 d4<br>
 *Decoding:<br>
    1.Compute syndrome bits: s1, s2, s3 using parity checks.<br>
    2.Syndrome gives the error bit position (if any).<br>
    3.Correct the error if needed.<br>
    4.Extract and return the 4 data bits.<br>

# PROGRAM
```# Parity Check (Even)
def parity_check(data):
    parity = data.count('1') % 2
    return data + str(parity)

def check_parity(received):
    return received.count('1') % 2 == 0


# Checksum (16-bit blocks)
def compute_checksum(blocks):
    checksum = 0
    for block in blocks:
        checksum += int(block, 2)
        checksum = (checksum & 0xFFFF) + (checksum >> 16)  # carry around
    return bin(~checksum & 0xFFFF)[2:].zfill(16)

def verify_checksum(blocks, checksum):
    total = int(checksum, 2)
    for block in blocks:
        total += int(block, 2)
        total = (total & 0xFFFF) + (total >> 16)
    return (bin(~total & 0xFFFF)[2:].zfill(16) == '0000000000000000')


# Hamming Code (7,4)
def hamming_encode(data):  # data = 4 bits: d1 d2 d3 d4
    d = list(map(int, data))
    p1 = d[0] ^ d[1] ^ d[3]
    p2 = d[0] ^ d[2] ^ d[3]
    p3 = d[1] ^ d[2] ^ d[3]
    return f'{p1}{p2}{d[0]}{p3}{d[1]}{d[2]}{d[3]}'

def hamming_decode(code):
    code = list(map(int, code))
    s1 = code[0] ^ code[2] ^ code[4] ^ code[6]
    s2 = code[1] ^ code[2] ^ code[5] ^ code[6]
    s3 = code[3] ^ code[4] ^ code[5] ^ code[6]
    error_pos = s1 + (s2 << 1) + (s3 << 2)
    if error_pos != 0:
        code[error_pos - 1] ^= 1  # Correct the bit
    return ''.join(str(code[i]) for i in [2, 4, 5, 6])  # Return data bits


# ---- RUNNING EXAMPLES ----
print("=== Parity Check ===")
data = "1011001"
encoded = parity_check(data)
print("Encoded:", encoded)
print("Is Valid?", check_parity(encoded))

print("\n=== Checksum ===")
blocks = ['1100110011001100', '1111000011110000']
checksum = compute_checksum(blocks)
print("Checksum:", checksum)
print("Is Valid?", verify_checksum(blocks, checksum))

print("\n=== Hamming Code ===")
data4 = "1011"
encoded_hamming = hamming_encode(data4)
print("Encoded:", encoded_hamming)
# introduce 1-bit error for test
received = list(encoded_hamming)
received[2] = '0' if received[2] == '1' else '1'
received_code = ''.join(received)
print("Received with error:", received_code)
decoded = hamming_decode(received_code)
print("Decoded (corrected):", decoded)
```

# OUTPUT
![image](https://github.com/user-attachments/assets/59ed2026-588e-4ca5-baa0-4eeb2b1333d2)

 # RESULT / CONCLUSIONS
 The simulation of Error Detecion and Correction Algorithms was successfully verified
