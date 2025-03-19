# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
PROGRAM:
CaearCipher.
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
int main() {
char plain[100], cipher[100];
int key, i, length;
printf("Enter the plain text: ");
scanf("%s", plain);
printf("Enter the key value: ");
scanf("%d", &key);
printf("\nPLAIN TEXT: %s", plain);
printf("\nENCRYPTED TEXT: ");
length = strlen(plain);
for (i = 0; i < length; i++) {
cipher[i] = plain[i] + key;
// Handling uppercase letters
if (isupper(plain[i]) && cipher[i] > 'Z') {
cipher[i] = cipher[i] - 26;
}
// Handling lowercase letters
if (islower(plain[i]) && cipher[i] > 'z') {
cipher[i] = cipher[i] - 26;
}
printf("%c", cipher[i]);
}
cipher[length] = '\0'; // Null-terminate the cipher text string
printf("\nDECRYPTED TEXT: ");
for (i = 0; i < length; i++) {
plain[i] = cipher[i] - key;
// Handling uppercase letters
if (isupper(cipher[i]) && plain[i] < 'A') {
plain[i] = plain[i] + 26;
}
// Handling lowercase letters
if (islower(cipher[i]) && plain[i] < 'a') {
plain[i] = plain[i] + 26;
}
printf("%c", plain[i]);
}
plain[length] = '\0'; // Null-terminate the plain text string
return 0;
}
```

## OUTPUT:
OUTPUT:
Simulating Caesar Cipher


![image](https://github.com/user-attachments/assets/60e1d92f-7012-433a-9dc7-e763e1a98fdd)


## RESULT:
The program is executed successfull
