# Forensics / Challenge 1
**NCSA CTF Bootcamp 2024**
**Forensics**
**Challenge 1**

## Step 1 : 
ดาวโหลดไฟล์ metadata.jpg จากนั้นทำการรันคำสั่ง file
```
┌──(kali㉿kali)-[~/Downloads/foresnsics]
└─$ file metadata.jpg      
metadata.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 120x120, segment length 16, comment: "Password is (4 * 4) + (123457890 * 4)", baseline, precision 8, 1024x615, components 3
```
จากการใช้คำสั่ง fileจะสั่งเกตุได้ว่าไฟล์ที่ได้เป็นไฟล์ JPEG Image หลังจากนั้นเราจะลองเช็คข้อมูลไฟล์ในขั้นตอนถัดไป
## Step 2 :
เราจะใช้คำสั่ง exiftool เพื่อดูข้อมูลต่างๆของรูปภาพนี้
```
──(kali㉿kali)-[~/Downloads/foresnsics]
└─$ exiftool metadata.jpg
ExifTool Version Number         : 12.76
File Name                       : metadata.jpg
Directory                       : .
File Size                       : 122 kB
File Modification Date/Time     : 2024:09:08 12:30:04+07:00
File Access Date/Time           : 2024:09:10 17:37:09+07:00
File Inode Change Date/Time     : 2024:09:10 17:36:39+07:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 120
Y Resolution                    : 120
Comment                         : Password is (4 * 4) + (123457890 * 4)
Image Width                     : 1024
Image Height                    : 615
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1024x615
Megapixels                      : 0.630
``` 
จากข้อมูลที่ได้ เราจะสั่งเกตุเห็นข้อมูล "Comment : Password is (4 * 4) + (123457890 * 4)" ซึ่งเมื่อเราทำการคำนวนออกมาจะได้เป็น '493831576'
## Step 3 :
ขั้นตอนนี้ เราจะทำการเข้ารหัส 493831576 ด้วย md5 โดยใช้โปรแกรม CyberChef 
<img src="https://raw.githubusercontent.com/KUBits-802/NCSA-CTF-Boot-Camp-2024/main/Forencics/ch1/image/Screenshot%20from%202024-09-10%2017-54-05.png">

**FLAG :** flag{865f514870de557cd636bbdff2ebb32b}  
