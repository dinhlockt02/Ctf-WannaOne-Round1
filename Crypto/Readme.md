# Baby AES
## Phân tích vấn đề
```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import os

def xor(a: bytes, b: bytes) -> bytes:
    assert len(a) == len(b)
    return bytes(i^j for i, j in zip(a, b))

key = os.urandom(16)
iv = os.urandom(16)
aes = AES.new(key, AES.MODE_ECB)
f = open('flag.png', 'rb')
pt = pad(f.read(), 16)
f.close()

ct = b''
for i in range(0, len(pt), 16):
    ct += xor(aes.encrypt(iv), pt[i:i+16])

with open('ciphertext', 'wb') as f:
    f.write(ct)
```
## Kiểu mã hóa
Đây là mã hóa AES và để giải mã ta cần tìm chìa khóa để có thể giải mã ngược lại
Ta chú ý dòng lệnh này thì nhận ra chúng được tạo ngẫu nhiên và không có cách nào tìm ra được biến key và iv.
```python
key = os.urandom(16)
iv = os.urandom(16)
```
Liệu có cách ta có thể tìm được chìa khóa không?
## Kiểu ảnh PNG
Kiểu ảnh này có đặc trưng là 16 bytes đầu tiên đều giống nhau vì thế ta có thể tự tạo một ảnh PNG khác và XOR với file ciphertext để giải mã.
```
89 50 4E 47 0D 0A 1A 0A 00 00 00 0D 49 48 44 52
```
Ở đây tôi sử dụng file Red.png có thể tự tạo thông qua đoạn mã tìm được trên Wikipedia
## Script giải
```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import os

def xor(a: bytes, b: bytes) -> bytes:
    assert len(a) == len(b)
    return bytes(i^j for i, j in zip(a, b))

f = open('ciphertext','rb')
g = open('red.png','rb')
pt1 = pad(f.read(), 16)
pt2 = pad(g.read(), 16)
ct = xor(pt1[0:16], pt2[0:16])

result = b''
for i in range(0, len(pt1), 16):
    result += xor(ct,pt1[i:i+16])
print(result)
with open('flag.png', 'wb') as h:
    h.write(result)
print("end")
```
## Kêt quả
Ta tìm được một file flag.png là flag cần tìm.
