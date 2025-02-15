# Baby Penguin   

## Thông tin về challenge:  

**- Author:** Kahang   
**- Tags:**  Misc, Easy
**- Description:** Take your first steps in the Linux terminal! Use basic commands to explore and solve simple tasks like a curious baby penguin. (Download the file and run it on your own Linux machine to start)  
**- Link:**  

## Chạy file challenge được cho sẵn (Lưu ý nhớ cấp quyền execute cho file)
```
chmod +x BabyPenguin 
```
## Level 1: Checking if the directory 'FIA' exists in your home directory...
![image](https://github.com/user-attachments/assets/b316c235-a6e9-4d2f-9c60-30fc21bbbb73)  

- Level 1 kiểm tra xem có directory FIA ở home hay chưa.
```
cd ~
sudo mkdir FIA
```
- Chạy lại file, đã xong level 1.
  ![image](https://github.com/user-attachments/assets/527a60df-57f5-45ad-b1d3-2c8c44047ec2)

## Level 2: Checking if the file 'shiba.txt' exists in the directory...
![image](https://github.com/user-attachments/assets/2a92035a-9b56-43e1-8081-0d1257348aa2)  

- Level 2 kiểm tra file shiba.txt có tồn tại hay không.
  ```
  sudo touch FIA/shiba.txt
  ```
- Chạy lại file, đã xong level 2.
  ![image](https://github.com/user-attachments/assets/223b7225-e32c-4e1d-923f-cc12f818d778)

## Level 3: Checking if the file contains the text 'woof woof'...
![image](https://github.com/user-attachments/assets/ff0c48bb-38cb-4cba-b4be-4747dd0a7900)

- Level 3 kiểm tra file có chứa "woof woof" hay không. (Nhớ cấp quyền write cho shiba.txt)
  ```
  sudo chmod 777 FIA/shiba.txt
  sudo chmod 777 FIA
  nano FIA/shiba.txt
  ```
- Sau khi ghi woof woof vào file, Ctr + X -> Y -> Enter để lưu.
- Chạy lại file, đã xong level 3.
  ![image](https://github.com/user-attachments/assets/5eb16ca5-8c62-4294-aa9a-4cf01bc1c387)

## Level 4: Checking if the file has permissions set to 766...
![image](https://github.com/user-attachments/assets/8d5e71d0-6a66-47d8-9645-13b88ca78b3d)  

- Level 4 kiểm tra xem file có quyền 766 hay chưa.
  ```
  sudo chmod 766 FIA/shiba.txt
  ```
- Chạy lại file, đã xong level 4.
  ![image](https://github.com/user-attachments/assets/03c57b29-9d61-49e9-a389-8a7a2dc7e524)  
  
## Level 5: Checking if the file is owned by 'root'...
![image](https://github.com/user-attachments/assets/18ad0f09-7637-4b67-8e2e-c4722756cec9)  

- Level 5 kiểm tra xem file đã sở hữu bởi rôt hay chưa.
```
sudo chown root FIA/shiba.txt
```
- Chạy lại file, ta được flag:

![image](https://github.com/user-attachments/assets/24476a5d-f796-472a-afe5-939a8ef1ae17)  

## Flag: FIA{E45y_l1nux_c0mm4nd_f0r_b3g1nn3r} 

