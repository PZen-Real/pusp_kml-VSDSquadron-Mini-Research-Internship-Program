###SECOND TASK###
----
As we know that when we type the following code, we get the required output.
```
gcc Puspa.o
```
And to execute it we write the following:
```
./a.out
```

![Screenshot 2024-12-27 115553](https://github.com/user-attachments/assets/c69b4148-1609-4ce0-aa37-d20b9a2a789c)


We get the same thing using 'spike'. All we need to do is run the following code:
```
spike pk Puspa.o
```
![Screenshot 2024-12-27 115636](https://github.com/user-attachments/assets/29dc7a85-7a01-4352-8047-1f5180b27609)


Now after we got the verification that the code is working we will now try to debug the dumped assembly. The following code is written to dump the assembly:
```
riscv64-unknown-elf-objdump -d Puspa.o
```
NOTE: This can be done using any of '-O1' or 'Ofast'

![Screenshot 2024-12-27 120020](https://github.com/user-attachments/assets/b0a7c9aa-0983-4003-b973-ff334a7bff08)


To open the Debugger write the following command:
```
spike -d pk Puspa.o
```
Now once the debugger is executed, we want the whole assembly to run automatically until '100B0' which is the memory address, after which we shall run it manually. The command to make the pc run own of own till some place is
```
until pc 0 100b0
```
![Screenshot 2024-12-27 121205](https://github.com/user-attachments/assets/40ac49fe-6546-4f84-a0d7-cfe5bb272d18)


To check the register content, we write the following command where 'a2' is the register
```
reg 0 a2
```
After which we get a 64 bit assembly and to run next instruction, we press enter.
![Screenshot 2024-12-27 121348](https://github.com/user-attachments/assets/dd2ae935-dbb7-4e90-8693-8c9c94d0204d)


Then again to run the next assembly, we write the following code.
```
reg 0 a2
```
Once 'sp' comes, which stands for spike pointer, we now need to perform 'addi', which adds an immediate value to a source register and stores the result in the destination register. In my case, `ADDI SP, SP, -16` subtracts `0x10` (hexadecimal 16) from the stack pointer (`SP`).

![Screenshot 2024-12-27 121745](https://github.com/user-attachments/assets/87b9d62c-0349-467d-bebc-91e672a87830)


This will subtract 16(in decimal which becomes 10 in hexadecimal)
![Screenshot 2024-12-27 130228](https://github.com/user-attachments/assets/c81ec53d-0af5-45e8-a93c-9c8243211324)

----
*EXAMPLE*
-----
```
#include <stdio.h>

int main() {
    float celsius, fahrenheit;
    celsius = 28;
    scanf("%f", &celsius);
    fahrenheit = (celsius * 9 / 5) + 32;
    printf("%.2f Celsius is equal to %.2f Fahrenheit\n", celsius, fahrenheit);

    return 0;
}
```

