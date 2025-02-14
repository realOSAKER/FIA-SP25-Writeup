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
```
<div style="overflow:scroll; width: 300px; height: 150px;">
  ffuf -u https://notadmin.fia.io.vn/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc all
</div>
```
