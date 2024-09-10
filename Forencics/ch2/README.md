# Forensics / Challenge 2
**NCSA CTF Bootcamp 2024**
**Forensics**
**Challenge 2**
## Step 1 :
ทำการดาวโหลดไฟล์ [ctf_image.zip](https://github.com/KUBits-802/NCSA-CTF-Boot-Camp-2024/raw/main/Forencics/ch2/ctf_image.zip) จากนั้นทำการแตกไฟล์ที่ดาวโหลดไว้
```
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2]
└─$ unzip ctf_image.zip                      
Archive:  ctf_image.zip
  inflating: ctf_image.dd.gz  
```
จากนั้นก็นั้นการแตกไฟล์ "ctf_image.dd.gz" ด้วยคำสั่ง gzip -d <file_name> 
```
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2]
└─$ gzip -d ctf_image.dd.gz 
```
เราก็จะได้ไฟล์ "ctf_image.dd" ซึ่งเป็นไฟล์ disk image ซึ่งเราจะนำไปดึงข้อมูลในขั้นตอนถัดไป
## Step 2 :
ในขั้นตอนนี้ เราจะทำการดึงข้อมูลจากไฟล์ disk โดยใช้เครื่องมือที่มีชื่อว่า [<u>foremost</u>](https://www.kali.org/tools/foremost/) โดยค้นหาจากชนิดของข้อมูลที่เราต้องการโดยใช้ foremost -t <file_type> -i <file_name>
```
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2]
└─$ foremost -t doc,jpg,pdf,xls -i ctf_image.dd 
Processing: ctf_image.dd
|*|
                                                                               
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2]
└─$ ls
ctf_image.dd  ctf_image.zip  output

```
จะเห็นได้ว่ามี directory ชื่อว่า "output" เพิ่มขึ้นมา 
```
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2]
└─$ ls
ctf_image.dd  ctf_image.zip  output

┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2]
└─$ cd output 
``` 
เมื่อเราเข้าไปใน output เราก็จะพบกับ audit.txt ที่เป็นไฟล์ ASCII text และ /jpg ซึ่งเป็น directory 
```
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2/output]
└─$ ls -l
total 8
-rw-rw-r-- 1 kali kali  720 Sep 10 23:33 audit.txt
drwxrwxr-- 2 kali kali 4096 Sep 10 23:33 jpg           
```
เมื่อเรากดเข้าไปดูก็จะพบกับรูปภาพที่มีนามสกุลไฟเป็น .jpg
```
┌──(kali㉿kali)-[~/Downloads/foresnsics/forensics_2/output]
└─$ cd jpg   
                                                                                
┌──(kali㉿kali)-[~/…/foresnsics/forensics_2/output/jpg]
└─$ ls   
00016902.jpg
```
## Step 3 :
เนื่องจากในข้อนี้ มีการซ่อนข้อมูลโดยใช้วิธีการที่เรียกว่า [**Steganography**](https://mayaseven.com/th/%E0%B8%A7%E0%B8%B4%E0%B8%97%E0%B8%A2%E0%B8%B2%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%AD%E0%B8%B3%E0%B8%9E%E0%B8%A3%E0%B8%B2%E0%B8%87%E0%B8%82%E0%B9%89%E0%B8%AD%E0%B8%A1%E0%B8%B9%E0%B8%A5-steganography/)  เราจึงจำเป็นต้องใช้เครื่องมือที่ชื่อว่า [<u>steghide</u>](https://www.kali.org/tools/steghide/) เพื่อใช้ในการถอดรหัสจากรูปภาพนี้
และปัญหาอีกอย่างหนึ่งในข้อนี้ก็คือ เราไม่มีข้อมูลเกี่ยวกับ passphrase ที่ใช้ในการถอดรหัส เราจึงต้องใช้เครื่องมืออีกตัวหนึ่งคือ [<u>stegseek</u>](https://github.com/RickdeJager/stegseek) มาใช้ในการ brute forece ตัวของ passphrase ออกมา
```
┌──(kali㉿kali)-[~/…/foresnsics/forensics_2/output/jpg]
└─$ stegseek 00016902.jpg 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "popstar"
[i] Original filename: "flag.txt".
[i] Extracting to "00016902.jpg.out".

┌──(kali㉿kali)-[~/…/foresnsics/forensics_2/output/jpg]
└─$ ls
00016902.jpg  00016902.jpg.out

┌──(kali㉿kali)-[~/…/foresnsics/forensics_2/output/jpg]
└─$ file 00016902.jpg.out 
00016902.jpg.out: ASCII text

```
จะเห็นได้ว่า เราจะได้ไฟล์ที่มีชื่อว่า "00016902.jpg.out" ซึ่งเป็นไฟล์ประเภท ASCII text เราสามารถอ่านข้อความภายในไฟล์ได้โดยใช้คำสั่ง cat <file_name>
```
┌──(kali㉿kali)-[~/…/foresnsics/forensics_2/output/jpg]
└─$ cat 00016902.jpg.out 
flag is flag{79a352706fc69e70b68a457015ccaf0f}
```
**FLAG :** flag{79a352706fc69e70b68a457015ccaf0f}