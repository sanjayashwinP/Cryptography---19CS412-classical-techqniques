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

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30
// Function to convert the string to lowercase
void toLowerCase(char plain[], int ps)
{
for (int i = 0; i < ps; i++)
{
if (plain[i] >= 'A' && plain[i] <= 'Z')
plain[i] += 32;
}
}
// Function to remove all spaces in a string
int removeSpaces(char* plain, int ps)
{
int i, count = 0;
for (i = 0; i < ps; i++)
if (plain[i] != ' ')
plain[count++] = plain[i];
plain[count] = '\0';
return count;
}
// Function to generate the 5x5 key square
void generateKeyTable(char key[], int ks, char keyT[5][5])
{
int i, j, k;
int dicty[26] = {0};
// Marking characters that appear in the key (excluding 'j')
for (i = 0; i < ks; i++) {
if (key[i] != 'j') {
dicty[key[i] - 'a'] = 2;
}
}
dicty['j' - 'a'] = 1; // Mark 'j' as already used
i = 0, j = 0;
// Filling key table with characters from key
for (k = 0; k < ks; k++)
{
if (dicty[key[k] - 'a'] == 2)
{
dicty[key[k] - 'a'] -= 1;
keyT[i][j] = key[k];
j++;
if (j == 5){
i++;
j = 0;
}
}
}
// Filling the rest of the key table with remaining characters
for (k = 0; k < 26; k++) {
if (dicty[k] == 0 && (char)(k + 'a') != 'j') {
keyT[i][j] = (char)(k + 'a');
j++;
if (j == 5) {
i++;
j = 0;
}
}
}
}
// Function to search for the characters of a digraph in the key square
void search(char keyT[5][5], char a, char b, int arr[]) {
int i, j;
if (a == 'j')
a = 'i';
if (b == 'j')
b = 'i';
for (i = 0; i < 5; i++) {
for (j = 0; j < 5; j++) {
if (keyT[i][j] == a) {
arr[0] = i;
arr[1] = j;
} else if (keyT[i][j] == b) {
arr[2] = i;
arr[3] = j;
}
}
}
}
// Function to find the modulus with 5
int mod5(int a) {
return (a % 5 + 5) % 5; // Ensures positive modulus
}
// Function to make the plain text length even
int prepare(char str[], int ptrs) {
if (ptrs % 2 != 0) {
str[ptrs++] = 'z'; // Add padding 'z'
str[ptrs] = '\0';
}
return ptrs;
}
// Function for performing the encryption
void encrypt(char str[], char keyT[5][5], int ps) {
int i, a[4];
for (i = 0; i < ps; i += 2) {
search(keyT, str[i], str[i + 1], a);
if (a[0] == a[2]) { // Same row
str[i] = keyT[a[0]][mod5(a[1] + 1)];
str[i + 1] = keyT[a[2]][mod5(a[3] + 1)];
} else if (a[1] == a[3]) { // Same column
str[i] = keyT[mod5(a[0] + 1)][a[1]];
str[i + 1] = keyT[mod5(a[2] + 1)][a[3]];
} else { // Rectangle swap
str[i] = keyT[a[0]][a[3]];
str[i + 1] = keyT[a[2]][a[1]];
}
}
}
// Function for performing the decryption
void decrypt(char str[], char keyT[5][5], int ps) {
int i, a[4];
for (i = 0; i < ps; i += 2) {
search(keyT, str[i], str[i + 1], a);
if (a[0] == a[2]) { // Same row
str[i] = keyT[a[0]][mod5(a[1] - 1)];
str[i + 1] = keyT[a[2]][mod5(a[3] - 1)];
} else if (a[1] == a[3]) { // Same column
str[i] = keyT[mod5(a[0] - 1)][a[1]];
str[i + 1] = keyT[mod5(a[2] - 1)][a[3]];
} else { // Rectangle swap
str[i] = keyT[a[0]][a[3]];
str[i + 1] = keyT[a[2]][a[1]];
}
}
}
// Function to encrypt using Playfair Cipher
void encryptByPlayfairCipher(char str[], char key[]) {
int ps, ks;
char keyT[5][5];
// Key
ks = strlen(key);
ks = removeSpaces(key, ks);
toLowerCase(key, ks);
// Plaintext
ps = strlen(str);
toLowerCase(str, ps);
ps = removeSpaces(str, ps);
ps = prepare(str, ps);
generateKeyTable(key, ks, keyT);
encrypt(str, keyT, ps);
}
// Function to decrypt using Playfair Cipher
void decryptByPlayfairCipher(char str[], char key[]) {
int ps, ks;
char keyT[5][5];
// Key
ks = strlen(key);
ks = removeSpaces(key, ks);
toLowerCase(key, ks);
// Ciphertext
ps = strlen(str);
toLowerCase(str, ps);
ps = removeSpaces(str, ps);
generateKeyTable(key, ks, keyT);
decrypt(str, keyT, ps);
}
// Driver code
int main() {
char str[SIZE], key[SIZE];
printf("Simulating Playfair Cipher\n");
// Key to be used
strcpy(key, "Monopoly");
printf("Key text: %s\n", key);
// Plaintext to be encrypted
strcpy(str, "SANJAY ASHWIN");
printf("Plain text: %s\n", str);
// Encrypt using Playfair Cipher
encryptByPlayfairCipher(str, key);
printf("Cipher text: %s\n", str);
// Decrypt using Playfair Cipher
decryptByPlayfairCipher(str, key);
printf("Decrypted text: %s\n", str);
return 0;
}
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/d471c22c-55d1-4299-a8a9-705f30f70213)


## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Function to encode three characters
void encode(char a, char b, char c, char *ret) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;

    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];

    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

// Function to decode three characters
void decode(char a, char b, char c, char *ret) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;

    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];

    ret[0] = key[(x % 26 < 0) ? (26 + x % 26) : (x % 26)];
    ret[1] = key[(y % 26 < 0) ? (26 + y % 26) : (y % 26)];
    ret[2] = key[(z % 26 < 0) ? (26 + z % 26) : (z % 26)];
    ret[3] = '\0';
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    char temp[4];
    int n;

    strcpy(msg, "sanjayashwin");
    printf("Simulation of Hill Cipher\n");
    printf("Input message : %s\n", msg);

    // Convert to uppercase
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Padding if necessary
    n = strlen(msg) % 3;
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message : %s\n", msg);

    // Encoding
    for (int i = 0; i < strlen(msg); i += 3) {
        encode(msg[i], msg[i + 1], msg[i + 2], temp);
        strcat(enc, temp);
    }
    printf("Encoded message : %s\n", enc);

    // Decoding
    for (int i = 0; i < strlen(enc); i += 3) {
        decode(enc[i], enc[i + 1], enc[i + 2], temp);
        strcat(dec, temp);
    }
    printf("Decoded message : %s\n", dec);

    return 0;
}

```

## OUTPUT:

![image](https://github.com/user-attachments/assets/a6a8cdd8-ab44-412a-9bbe-da9d862e3045)

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
PROGRAM:
```
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <stdlib.h>
void encipher();
void decipher();
int main() {
int choice;
while (1) {
printf("\n1. Encrypt Text");
printf("\t2. Decrypt Text");
printf("\t3. Exit");
printf("\n\nEnter Your Choice: ");
scanf("%d", &choice);
if (choice == 3)
return 0; // Proper exit from main()
else if (choice == 1)
encipher();
else if (choice == 2)
decipher();
else
printf("Please Enter a Valid Option.\n");
}
}
void encipher() {
unsigned int i, j;
char input[50], key[10];
printf("\n\nEnter Plain Text: ");
scanf("%s", input);
printf("\nEnter Key Value: ");
scanf("%s", key);
printf("\nResultant Cipher Text: ");
for (i = 0, j = 0; i < strlen(input); i++, j++) {
if (j >= strlen(key)) {
j = 0; // Reset key index if it exceeds the key length
}
printf("%c", 65 + (((toupper(input[i]) - 65) + (toupper(key[j]) - 65)) % 26));
// Encryption formula
}
printf("\n"); // New line after output
}
void decipher() {
unsigned int i, j;
char input[50], key[10];
int value;
printf("\n\nEnter Cipher Text: ");
scanf("%s", input);
printf("\nEnter the Key Value: ");
scanf("%s", key);
printf("\nDecrypted Plain Text: ");
for (i = 0, j = 0; i < strlen(input); i++, j++) {
if (j >= strlen(key)) {
j = 0; // Reset key index if it exceeds the key length
}// Decryption formula
value = (toupper(input[i]) - 65) - (toupper(key[j]) - 65);
if (value < 0) {
value += 26; // Correct the negative wrap-around in alphabet
}
printf("%c", 65 + (value % 26));
}
printf("\n"); // New line after output
}
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/1d95cb8c-6ab8-476c-859f-8cf95cda97f4)

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

```
#include <stdio.h>
#include <string.h>
int main() {
int i, j, k, l;
char a[20], c[20], d[20];
printf("\n\t\tRAIL FENCE TECHNIQUE\n");
// Safely getting input string using fgets instead of gets
printf("\nEnter the input string: ");
fgets(a, sizeof(a), stdin);
// Removing the newline character if it exists
a[strcspn(a, "\n")] = '\0';
l = strlen(a); // Get the length of the input string
// Rail fence encryption: first collect even indices, then odd
for (i = 0, j = 0; i < l; i++) {
if (i % 2 == 0) {
c[j++] = a[i];
}
}
for (i = 0; i < l; i++) {
if (i % 2 == 1) {
c[j++] = a[i];
}
}
c[j] = '\0'; // Null-terminate the encrypted string
printf("\nCipher text after applying rail fence: %s\n", c);
// Rail fence decryption
if (l % 2 == 0) {
k = l / 2;
} else {
k = (l / 2) + 1;
}
// Reconstructing the original text
for (i = 0, j = 0; i < k; i++) {
d[j] = c[i];
j += 2;
}
for (i = k, j = 1; i < l; i++) {
d[j] = c[i];
j += 2;
}
d[l] = '\0'; // Null-terminate the decrypted string
printf("\nText after decryption: %s\n", d);
return 0; // Properly return from main
}
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/859f1706-6316-4b65-b848-475746ff77f5)

## RESULT:
The program is executed successfull
