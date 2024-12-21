Our task can be divided into two parts. 
- The first one being the installation of Leafpad and writing the code for the sum of numbers.
- The second one is dumping this code in the RISC-V compiler in two different options.

**FIRST PART:**
---
First we shall begin with installing 'leafpad' in our system. Leafpad is an editor which we will use to write the code. To do this we can use the following code:
```
sudo apt install leafpad
```
![[Screenshot 2024-12-21 100451.png]]

To execute 'leafpad' and create a file named as 'Puspa.c' (in my case) on it, we run the following code:
```
leafpad Puspa.c &
```
![[Screenshot 2024-12-21 100556.png]]

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

![[Screenshot 2024-12-21 102719 1.png]]

And to execute this code we need to again open the terminal and write the following code:
```
gcc Puspa.c
```
Then the following one:
```
./a.out
```
This will execute our code and the result can be seen itself in the terminal.

![[Screenshot 2024-12-21 104239.png]]

*Hence the first part of our task is accomplished.*

**SECOND PART:**
---
In the second part we shall compile the code 'Puspa.c' which we had created in the first part in the RISC-V compiler.  
The following code can be launched to see the code in the terminal
```
cat Puspa.c
```
![[Screenshot 2024-12-21 104402.png]]

Once the code is seen in the terminal, we want to dump it in the RISC-V compiler. To dump it we need to run the following code:
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o Puspa.o Puspa.c
```
![[Screenshot 2024-12-21 104743.png]]

The option we used here is the '-O1' and the 'lp' is the 'long-integer point'. This creates the 

![[Screenshot 2024-12-21 104854.png]]

Now we open a new tab in the terminal itself and write the following code to dump the code (the one we wrote in first part).
```
riscv64-unknown-elf-objdump -d Puspa.o
```
![[Screenshot 2024-12-21 105029.png]]

Where '-d' stands for disassemble. We get the following output which is the bunch of assembly language code.

![[Screenshot 2024-12-21 105045.png]]

To get the disassemble station, we need to write the following code:
```
riscv64-unknown-elf-objdump -d Puspa.o | less
```
![[Screenshot 2024-12-21 105123.png]]

Now we need to go to the main section, for this we write the following code:
```
/main
```

![[Screenshot 2024-12-21 105332.png]]

Then press 'n' to go to main. The 

![[Screenshot 2024-12-21 111301.png]]
![[Screenshot 2024-12-21 112528.png]]

In my case the address of the main section is 10184 (which is in Hexadecimal) and got 11 instructions which we found out by subtracting 101B0 and 10184 and dividing it by 4.  

Now, let's run the same command again but we will change the option to '-Ofast'. We run the following command in the terminal:
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o Puspa.o Puspa.c
```
![[Screenshot 2024-12-21 112625.png]]

And now again we run the same command which is as follows:
```
riscv64-unknown-elf-objdump -d Puspa.o | less
```
Then we write the following code to go to the main section:
```
/main
```
![[Screenshot 2024-12-21 112657.png]]
![[Screenshot 2024-12-21 112748.png]]

In my case the address of the main section is 100B0 (which is in Hexadecimal) and got 11 instructions which we found out by subtracting 100B0 and 100DC and dividing it by 4.   
With this we not only wrote a code for the sum of numbers from 1 to n (n = 14), we also compiled it in the RISC-V compiler with two different option that is one is using '-O1' and '-Ofast'.    