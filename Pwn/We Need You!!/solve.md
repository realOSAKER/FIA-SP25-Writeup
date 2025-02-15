# We Need You!!   

## Thông tin về challenge:  

**- Author:** u53rr007    
**- Tags:** insane    
**- Description:** FIA club opens for registration. Hurry!! Fill in the form to join  
**- Hint:** Think outside the box. 😶‍🌫️    
**- Link:**  nc chall.fia.io.vn 8386  

## Bước 1: Đoc code
- Mở file source code mà challenge đã cho:
```
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

//------------------------Setup config (just ignore)---------------------
void kill_on_timeout(int sig) {
    if (sig == SIGALRM) {
        printf("[!] Anti DoS Signal. Patch me out for testing.");
        _exit(0);
    }
}

void ignore_me_init_signal() {
    signal(SIGALRM, kill_on_timeout);
    alarm(180);
}
//------------------------Setup config (just ignore)---------------------

//----------------------------------main------------------------------------
int main(){
	char buf[16];
	int v1 = 0;
	printf("Are you interested in joining FIA club (Yes or No only)> ");
	scanf("%s", buf);
	if(v1 != 0){
		printf("Your v1 var = %d", v1);
		system("/bin/sh");
	}
	else{
		printf("Hmmm.... Maybe try harder!!");
		exit(0);
	}
	return 0;
}
```

- Chú ý đoạn
```
char buf[16];
int v1 = 0;
scanf("%s", buf);
if(v1 != 0){
    system("/bin/sh");
}

```
- buf chỉ có 16 bytes, nhưng scanf có thể đọc được vô hạn input.
- Đây là lỗ hổng có thể khai thác, nếu ta nhập input siêu lớn sẽ gây buffer overflow.

  ## Bước 2: Khai thác lỗ hổng
  - Khi nhập input siêu lớn, ví dụ: ballsballsballsballsballsballsballsballsballsballsballsballs
  - Ta thấy dòng "Your bash shell is ready"
  - Bằng một cách thần kì nào đó mà chương trình trả về bash shell của server.
  - Sau dó ls -a, ta thấy 1 .flag khá là khả nghi
  ```
  cat .flag
  4649417b305633725f463130575f31535f333453597d
  ```
  - Chương trình trả về 1 chuỗi hex, mang lên cyberchef.io, ta được flag:
  ## FIA{0V3r_F10W_1S_34SY}
  ![image](https://github.com/user-attachments/assets/ac89a9a7-8ef2-4248-b65f-247a408eca90)
