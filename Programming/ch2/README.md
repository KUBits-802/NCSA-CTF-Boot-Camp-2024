# Programming / Challenge 2
**NCSA CTF Bootcamp 2024**<br/>
**Programming**<br/>
**Challenge 2**
## Step 1:
```bash
[siph:...ts/CTF_COPY/Programming/ch1]$ unzip challenge.zip
Archive:  challenge.zip
  inflating: challenge/key.png
  inflating: challenge/vigenere.py
  inflating: challenge/2006-honda-pilot.jpg
```
เมื่อแตกไฟล์จะได้ folder ชื่อ challenge และใน folder จะมีอยู่ 2 ไฟล์คือ vigenere.py , key.png, 2006-honda-pilot.jpg
```python
# challenge/vigenere.py
def decode_vigenere(encoded_text, key):
    result = ""
    key_length = len(key)
    key_as_int = [ord(i) - ord('a') for i in key.lower()]
    for i in range(len(encoded_text)):
        if encoded_text[i].isalpha():
            offset = ord('a') if encoded_text[i].islower() else ord('A')
            value = (ord(encoded_text[i]) - offset - key_as_int[i % key_length]) % 26
            result += chr(value + offset)
        else:
            result += encoded_text[i]
    return result

plaintext = "hhtc{l66430331413m9k40wzbky05j2069c22}"
remote_key = "CWTBIU545"

decoded_message = decode_vigenere(plaintext, remote_key)
print(f"Decoded Message: {decoded_message}")

#p = "??????????????????????????????"
#r_k = "UPPERCASE ONLY"  #FCCID 
```
## Step 2:
จากใน code จะเห็นว่าไม่มี plaintext และ key ที่ให้มาผมเลยคิดว่าเลยลองไปหาจากรูปที่ให้มาผมเลยลองหาจากรูป 2006-honda-pilot.jpg 
```bash
[siph:...Y/Programming/ch1/challenge]$ strings 2006-honda-pilot.jpg
...
...
...
2&I-
1dR'
1ZGB~
2&I)(
S&I$
1dI`?
^>fL
"43?
X\p>
plaintext = hhtc{l66430331412m9k40wzbky05j2069c22}
```
เราก็จะเจอ plaintext แล้วต่อไปเราต้องหา key ผมเดาว่า key น่าจะอยู่ที่ รูป key ผมเลยเอารูปไป search [<u>yandex</u>](https://yandex.com) ก็จะเจอรูปนี้

<img src="https://ae03.alicdn.com/kf/Hdc1627d2049042078846894708b50eaea.jpg"></img>

```python
#r_k = "UPPERCASE ONLY"  #FCCID 
ตรง comment บอกว่า FCC ผมเลยคิดว่า key น่าจะเป็น CWTWBIU545
```
## Step 3:
```python
def decode_vigenere(encoded_text, key):
    result = ""
    key_length = len(key)
    key_as_int = [ord(i) - ord('a') for i in key.lower()]
    for i in range(len(encoded_text)):
        if encoded_text[i].isalpha():
            offset = ord('a') if encoded_text[i].islower() else ord('A')
            value = (ord(encoded_text[i]) - offset - key_as_int[i % key_length]) % 26
            result += chr(value + offset)
        else:
            result += encoded_text[i]
    return result

plaintext = "hhtc{l66430331412m9k40wzbky05j2069c22}"
remote_key = "CWTBIU545"

decoded_message = decode_vigenere(plaintext, remote_key)
print(f"Decoded Message: {decoded_message}")
```
```bash
[siph:...Y/Programming/ch1/challenge]$ python3 vigenere.p
Decoded Message: flab{r66430331412e9o40oftdq05q[REDACTED]}
```
*written by [Siraphat-ohm](https://github.com/Siraphat-ohm)*
