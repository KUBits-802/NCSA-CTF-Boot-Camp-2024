# Programming / Challenge1
**NCSA CTF Bootcamp 2024**<br/>
**Programming**<br/>
**Challenge 1**
## Step 1:
```bash
[siph:...ts/CTF_COPY/Programming/ch1]$ unzip xor.zip
Archive:  xor.zip
  inflating: xor.py
```
## Step 2:
source code:
```python
#!/usr/bin/env python3

def xor_cipher(data, key):
    encrypted_data = []
    key_length = len(key)

    # XOR each character of the data with the corresponding key byte
    for i in range(len(data)):
        encrypted_char = chr(ord(data[i]) ^ key[i % key_length])  # Use key as byte array
        encrypted_data.append(encrypted_char)

    # Join the encrypted characters into a final string
    encrypted_text = ''.join(encrypted_data)

    return encrypted_text


# Key in hexadecimal format
key_hex = ""
key = bytes.fromhex(key_hex)  # Convert hex key to byte array

# Plaintext message
# p = "xxxxxxxxxxxxx3942.043lb77g6c3g3m4bdll64em`aagee3ge(xxxxxxxxxxxxxxxx"
# k = Range of 1000 to 9999

# Decrypt using XOR cipher
decrypted_text = xor_cipher(plaintext, key)
print("Decrypted text:", decrypted_text)
```
จากที่ผมดูใน comment เดาได้ว่า p น่าจะเป็น plaintext แล้ว k น่าจะเป็น key ดังนันเราสามารถเขียนโปรแกรม bruteforce เพื่อถอดรหัสได้
## Step 3:
```python
#!/usr/bin/env python3


def xor_cipher(data, key):
    encrypted_data = []
    key_length = len(key)

    # XOR each character of the data with the corresponding key byte
    for i in range(len(data)):
        encrypted_char = chr(
            ord(data[i]) ^ key[i % key_length]
        )  # Use key as byte array
        encrypted_data.append(encrypted_char)

    # Join the encrypted characters into a final string
    encrypted_text = "".join(encrypted_data)

    return encrypted_text


# Key in hexadecimal format
key_hex = ""
key = bytes.fromhex(key_hex)  # Convert hex key to byte array

# Plaintext message
# p = "xxxxxxxxxxxxx3942.043lb77g6c3g3m4bdll64em`aagee3ge(xxxxxxxxxxxxxxxx"
# k = Range of 1000 to 9999

# Decrypt using XOR cipher

plaintext = "xxxxxxxxxxxxx3942.043lb77g6c3g3m4bdll64em`aagee3ge(xxxxxxxxxxxxxxxx"

for key in range(1000, 9999):
    decrypted_text = xor_cipher(plaintext, bytes.fromhex(f"{key}"))
    if "flag{" in decrypted_text:
        print("Decrypted text:", decrypted_text)
```
```bash
[siph:...ts/CTF_COPY/Programming/ch1]$ python3 xor.py
Decrypted text: -------------flag{eaf97bb2c6f2f8a7199ca085442[REDACTED]}----------------
```
*written by [Siraphat-ohm](https://github.com/Siraphat-ohm)*