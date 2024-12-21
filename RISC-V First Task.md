**THE FIRST TASK :**
---

Our task can be divided into two parts. 
- The first one being the installation of Leafpad and writing the code for the sum of numbers.
- The second one is dumping this code in the RISC-V compiler in two different options.

**FIRST PART:**
---
First we shall begin with installing 'leafpad' in our system. Leafpad is an editor which we will use to write the code. To do this we can use the following code:
```
sudo apt install leafpad
```
![Screenshot 2024-12-21 100451](https://github.com/user-attachments/assets/f93a1786-185f-4288-a4a1-af5c5be2b1c8)


To execute 'leafpad' and create a file named as 'Puspa.c' (in my case) on it, we run the following code:
```
leafpad Puspa.c &
```
![Screenshot 2024-12-21 100556](https://github.com/user-attachments/assets/ca9f60d9-67fa-46f6-a737-0e25b0131c67)


After that an interface opens where we need to write the following code which gives us the sum of the numbers from 1 to a particular chosen number n (in our case we are taking n = 14).
```
#include <stdio.h>
int main(){
      int i , sum = 0 , n = 14;
      for (i =1 ; i <= n ; ++i){
      sum += i;
      }
      printf("Sum of the numbers from 1 to %d is %d\n :" n , sum);
      return 0;
}
```
This code will give us the sum of the numbers from 1 to the chosen number n (n=14; in my case). 

![Screenshot 2024-12-21 102719 1](https://github.com/user-attachments/assets/0f9a65da-8498-4f9e-871c-add72f31332e)


And to execute this code we need to again open the terminal and write the following code:
```
gcc Puspa.c
```
Then the following one:
```
./a.out
```
This will execute our code and the result can be seen itself in the terminal.

![Screenshot 2024-12-21 104239](https://github.com/user-attachments/assets/36b0591d-2154-46f3-b7d3-fa6115d28919)


*Hence the first part of our task is accomplished.*

**SECOND PART:**
---
In the second part we shall compile the code 'Puspa.c' which we had created in the first part in the RISC-V compiler.  
The following code can be launched to see the code in the terminal
```
cat Puspa.c
```
![Screenshot 2024-12-21 104402](https://github.com/user-attachments/assets/c34767a0-688a-4cba-aa9a-5969c25956cb)


Once the code is seen in the terminal, we want to dump it in the RISC-V compiler. To dump it we need to run the following code:
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o Puspa.o Puspa.c
```
![Screenshot 2024-12-21 104743](https://github.com/user-attachments/assets/228442a3-44e6-489f-8a43-2f4d46639084)


The option we used here is the '-O1' and the 'lp' is the 'long-integer point'. This creates the 

![Screenshot 2024-12-21 104854](https://github.com/user-attachments/assets/4dd8f138-d0d5-42f2-9fab-efca9d6271f8)


Now we open a new tab in the terminal itself and write the following code to dump the code (the one we wrote in first part).
```
riscv64-unknown-elf-objdump -d Puspa.o
```
![Screenshot 2024-12-21 105029](https://github.com/user-attachments/assets/aa53b4f3-a8ea-4131-a13a-8317f329e08b)


Where '-d' stands for disassemble. We get the following output which is the bunch of assembly language code.

![Screenshot 2024-12-21 105045](https://github.com/user-attachments/assets/8c85e53a-579c-4342-bbac-f19407224531)


To get the disassemble station, we need to write the following code:
```
riscv64-unknown-elf-objdump -d Puspa.o | less
```
![Screenshot 2024-12-21 105123](https://github.com/user-attachments/assets/78f3687a-74bd-4de1-b304-26958c4baa45)


Now we need to go to the main section, for this we write the following code:
```
/main
```

![Screenshot 2024-12-21 105332](https://github.com/user-attachments/assets/5528a89d-03d7-4e9b-8db8-c6f4b97eff0d)


Then press 'n' to go to main. The 

![Screenshot 2024-12-21 111301](https://github.com/user-attachments/assets/ba6e02cc-861e-4190-80a6-e028d8b89b1d)
![Screenshot 2024-12-21 112528](https://github.com/user-attachments/assets/4d11b760-bf40-4454-8222-bca57293af0f)


In my case the address of the main section is 10184 (which is in Hexadecimal) and got 11 instructions which we found out by subtracting 101B0 and 10184 and dividing it by 4.  

Now, let's run the same command again but we will change the option to '-Ofast'. We run the following command in the terminal:
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o Puspa.o Puspa.c
```
![Screenshot 2024-12-21 112625](https://github.com/user-attachments/assets/4c13328b-5a88-4e7e-ac78-de8e9ab44782)

And now again we run the same command which is as follows:
```
riscv64-unknown-elf-objdump -d Puspa.o | less
```
Then we write the following code to go to the main section:
```
/main
```
![Screenshot 2024-12-21 112657](https://github.com/user-attachments/assets/4189c106-21b4-43ac-9d33-0bbe9b1bc671)
![Screenshot 2024-12-21 112748](https://github.com/user-attachments/assets/b6a432fe-8e83-4658-9e77-6020297d72f6)

In my case the address of the main section is 100B0 (which is in Hexadecimal) and got 11 instructions which we found out by subtracting 100B0 and 100DC and dividing it by 4.   
**With this we not only wrote a code for the sum of numbers from 1 to n (n = 14), we also compiled it in the RISC-V compiler with two different option that is one is using '-O1' and '-Ofast'.**
