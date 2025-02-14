# Hay dao choi va thanh admin  

## Thông tin về challenge:  

**- Author:** Mèo Bếu  
**- Tags:**  
**- Description:** Một thử thách đơn giản, trở thành Admin của trang web và bạn có thể làm bất cứ điều gì bạn muốn.
Chúc mọi người vượt qua thử thách hehehe  
**- Hint:**  
**- Link:** https://notadmin.fia.io.vn/  

## Cách giải:  
### Trước khi giải, chúng ta cần:  
- Ffuf  
- Seclist  
### Bước 1: Sử dụng ffuf cùng với seclists để bruteforce directories của link challenge.  
Đầu tiên sử dụng common list có sẵn của seclists để tìm những directories phổ biến.  
```
  ffuf -u https://notadmin.fia.io.vn/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc all -o output.txt
```
![image](https://github.com/user-attachments/assets/cb36e35c-5684-4b5f-8aa3-272c458b7ede)  
Như ta có thể thấy, đa số status là 429, có nghĩa là quá nhiều request trong 1 giây. Có vẻ server đã limit lại để tránh automated request.  
Chúng ta sẽ dùng lại ffuf, nhưng làm chậm tốc độ của nó lại còn 10 request 1 giây để tránh bị phát hiện.
```
  ffuf -u https://notadmin.fia.io.vn/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -rate 10 -mc all -o output.txt
```
Sau khi "grep -v "404" result.txt", có 2 directoris chúng ta cần lưu ý, dó là admin và robots.txt.  
Tiếp theo cùng đến với bước 2.
### Bước 1 (cách khác): Rùa ![image](https://github.com/user-attachments/assets/286e50ef-7ef2-4bb8-bfc4-29ee68b2ab82) 
** Đây là cách lúc đầu mình dùng để giải, hên hên được luôn =))))  **
Đầu tiên test https://notadmin.fia.io.vn/robots.txt theo phản xạ  
Vì đề tên là trở thành admin, nên thử https://notadmin.fia.io.vn/admin luôn :)))
