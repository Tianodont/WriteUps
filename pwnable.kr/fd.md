# fd 
В папке, куда мы подключились, лежат три файла, привлекающие взгляд - fd, fd.c, flag. flag закрыт для чтения, fd - исполняемый файл, fd.c - скорее всего его исходник. 
![image](https://github.com/user-attachments/assets/78eedff3-0570-41b8-bb8a-696200cc25f5)  
Откроем и тщательно изучим fd.c:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
        if(argc<2){
                printf("pass argv[1] a number\n");
                return 0;
        }
        int fd = atoi( argv[1] ) - 0x1234;
        int len = 0;
        len = read(fd, buf, 32);
        if(!strcmp("LETMEWIN\n", buf)){
                printf("good job :)\n");
                setregid(getegid(), getegid());
                system("/bin/cat flag");
                exit(0);
        }
        printf("learn about Linux file IO\n");
        return 0;

}
``` 
Здесь видно, что из введенного аргумента мы получаем файловый дескриптор (`atoi( argv[1] ) - 0x1234`). После мы читаем в буффер максимум 
32 символа из соответствующего потока ввода/вывода. Если в буффере оказывается "LETMEWIN\n", то мы получаем флаг. Чтобы получить файловый дескриптор = 0 (поток стандартного ввода),
надо ввести десятичный эквивалент 0x1234 - 4660.  
![image](https://github.com/user-attachments/assets/23912fe1-5ce1-4d8a-a784-042e4fded331)  
Получаем флаг 
