##  DATE:
##  EX-1:Caeser Cipher
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


```
#include <stdio.h>
#include <stdlib.h>

// Function to perform Caesar Cipher encryption
void caesarEncrypt(char *text, int key) {
    for (int i = 0; text[i] != '\0'; i++) {
        char c = text[i];
        // Check if the character is an uppercase letter
        if (c >= 'A' && c <= 'Z') {
            text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
        }
        // Check if the character is a lowercase letter
        else if (c >= 'a' && c <= 'z') {
            text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
        }
        // Ignore non-alphabetic characters
    }
}

// Function to perform Caesar Cipher decryption
void caesarDecrypt(char *text, int key) {
    // Decryption is the same as encryption with a negative key
    caesarEncrypt(text, -key);
}

int main() {
    char message[100]; // Declare a character array to store the message
    int key;

    printf("Enter the message to encrypt: ");
    fgets(message, sizeof(message), stdin); // Read input from the user

    printf("Enter the Caesar Cipher key (an integer): ");
    scanf("%d", &key); // Read the key from the user

    // Encrypt the message using the Caesar Cipher
    caesarEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    caesarDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}
```



## OUTPUT:
Simulating Caesar Cipher

![Screenshot 2024-10-21 082900](https://github.com/user-attachments/assets/29f9e138-d620-45cf-995c-dacb595dc2c8)

```
Enter the message to encrypt: kavin
Enter the Caesar Cipher key (an integer): 13
Encrypted Message: xniva
Decrypted Message: kavin
```



## RESULT:
The program for caesar cipher method is executed successfully

---------------------------------
## DATE:
## EX-2: PlayFair Cipher
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
void toLowerCase(char plain[], int ps) {
    for (int i = 0; i < ps; i++) {
        if (plain[i] >= 'A' && plain[i] <= 'Z')
            plain[i] += 32;
    }
}

// Function to remove all spaces in a string
int removeSpaces(char* plain, int ps) {
    int i, count = 0;
    for (i = 0; i < ps; i++)
        if (plain[i] != ' ')
            plain[count++] = plain[i];
    plain[count] = '\0';
    return count;
}

// Function to generate the 5x5 key square
void generateKeyTable(char key[], int ks, char keyT[5][5]) {
    int i, j, k;
    int dicty[26] = {0};

    for (i = 0; i < ks; i++) {
        if (key[i] != 'j') {
            dicty[key[i] - 'a'] = 2;
        }
    }

    dicty['j' - 'a'] = 1;

    i = 0;
    j = 0;
    for (k = 0; k < ks; k++) {
        if (dicty[key[k] - 'a'] == 2) {
            dicty[key[k] - 'a'] -= 1;
            keyT[i][j] = key[k];
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }

    for (k = 0; k < 26; k++) {
        if (dicty[k] == 0) {
            keyT[i][j] = (char)(k + 'a');
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
}

// Function to search for the characters of a digraph
// in the key square and return their position
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
            }
            else if (keyT[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

// Function to find the modulus with 5
int mod5(int a) {
    return (a % 5);
}

// Function to make the plain text length to be even
int prepare(char str[], int ptrs) {
    if (ptrs % 2 != 0) {
        str[ptrs++] = 'z';
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
            str[i] = keyT[a[0]][mod5(a[1] - 1 + 5)];
            str[i + 1] = keyT[a[2]][mod5(a[3] - 1 + 5)];
        } else if (a[1] == a[3]) { // Same column
            str[i] = keyT[mod5(a[0] - 1 + 5)][a[1]];
            str[i + 1] = keyT[mod5(a[2] - 1 + 5)][a[3]];
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
    strcpy(key, "harin");
    printf("Key text: %s\n", key);

    // Plaintext to be encrypted
    strcpy(str, "saveetha");
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

Simulating Playfair Cipher

![image](https://github.com/user-attachments/assets/64520d37-8d37-4a08-8e89-02ba2ffbb408)



```
Simulating Playfair Cipher
Key text: kavin
Plain text: saveetha
Cipher text: qiidfsqc
Decrypted text: saveetha
```

## RESULT:
The program for playfair cipher method is executed successfully


---------------------------
## DATE:
## EX-3: Hill Cipher
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
```
#include <stdio.h>
#include <string.h>

// Key matrix and its inverse for the Hill Cipher
int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };

char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Function to encode a block of three characters
char* encode(char a, char b, char c) {
    static char ret[4];  // Static to persist outside the function
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

    return ret;
}

// Function to decode a block of three characters
char* decode(char a, char b, char c) {
    static char ret[4];  // Static to persist outside the function
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

    return ret;
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;

    strcpy(msg, "HARINI");
    printf("Simulation of Hill Cipher\n");
    printf("Input message: %s\n", msg);

    // Convert to uppercase
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Remove spaces (though the provided string has no spaces)
    n = strlen(msg) % 3;

    // Append padding text 'X' if needed
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message: %s\n", msg);

    // Encode the message in blocks of three characters
    for (int i = 0; i < strlen(msg); i += 3) {
        char a = msg[i];
        char b = msg[i + 1];
        char c = msg[i + 2];
        strcat(enc, encode(a, b, c));
    }

    printf("Encoded message: %s\n", enc);

    // Decode the encoded message in blocks of three characters
    for (int i = 0; i < strlen(enc); i += 3) {
        char a = enc[i];
        char b = enc[i + 1];
        char c = enc[i + 2];
        strcat(dec, decode(a, b, c));
    }

    printf("Decoded message: %s\n", dec);

    return 0;
}
```

## OUTPUT:
Simulating Hill Cipher
![Screenshot 2024-10-21 083438](https://github.com/user-attachments/assets/1f9fc5e9-6c81-4d8d-aa41-de716794802d)


```
Simulation of Hill Cipher
Input message: kavin
Padded message: KAVINX
Encoded message: AKFCXF
Decoded message: KAVINX
```
## RESULT:
The program for hill cipher method is executed successfully

-------------------------------------------------
## DATE:
## EX-4: Vigenere Cipher
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
```
#include <stdio.h>
#include <string.h>

// Function to perform Vigenère encryption
void vigenereEncrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + (key[i % keyLen] - 'A')) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + (key[i % keyLen] - 'A')) % 26) + 'a';
        }
    }
}

// Function to perform Vigenère decryption
void vigenereDecrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "HARINI"; // Replace with your desired key
    char message[] = "SAVEETHA ENGINEERING COLLEGE"; // Replace with your message
    printf("Simulating Vignere cipher\n");
    
    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}
```
## OUTPUT:


Simulating Vigenere Cipher
![Screenshot 2024-10-21 084516](https://github.com/user-attachments/assets/4a593d05-52e7-4e13-b945-ec7e74da2919)

```
Simulating Vignere cipher
Encrypted Message: CAQMRDHV RXGDVRORDVT CJTYOGZ
Decrypted Message: SAVEETHA ENGINEERING COLLEGE
```



## RESULT:
The program for vignere cipher method is executed successfully

-----------------------------------------------------------------------
# DATE:
## EX-5: Rail Fence Cipher
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
#include <stdlib.h>

void encryptRailFence(char str[], int rails, char encrypted[]) {
    int i, j, len, count, code[100][1000];
    len = strlen(str);

    // Initialize the code array to 0
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }

    count = 0;
    j = 0;
    while (j < len) {
        if (count % 2 == 0) { // Moving down the rails
            for (i = 0; i < rails && j < len; i++) {
                code[i][j] = (int)str[j];
                j++;
            }
        } else { // Moving up the rails
            for (i = rails - 2; i > 0 && j < len; i--) {
                code[i][j] = (int)str[j];
                j++;
            }
        }
        count++;
    }

    // Construct the encrypted message
    int pos = 0;
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] != 0) {
                encrypted[pos++] = code[i][j];
            }
        }
    }
    encrypted[pos] = '\0'; // Null-terminate the string
}

void decryptRailFence(char str[], int rails, char decrypted[]) {
    int i, j, len, count, code[100][1000], pos = 0;
    len = strlen(str);

    // Initialize the code array to 0
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }

    // Mark the positions where characters will go
    count = 0;
    j = 0;
    while (j < len) {
        if (count % 2 == 0) { // Moving down the rails
            for (i = 0; i < rails && j < len; i++) {
                code[i][j] = 1;
                j++;
            }
        } else { // Moving up the rails
            for (i = rails - 2; i > 0 && j < len; i--) {
                code[i][j] = 1;
                j++;
            }
        }
        count++;
    }

    // Fill the marked positions with the characters from the cipher text
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] == 1) {
                code[i][j] = str[pos++];
            }
        }
    }

    // Reconstruct the decrypted message by following the zigzag pattern
    pos = 0;
    count = 0;
    j = 0;
    while (j < len) {
        if (count % 2 == 0) { // Moving down the rails
            for (i = 0; i < rails && j < len; i++) {
                if (code[i][j] != 0) {
                    decrypted[pos++] = code[i][j];
                }
                j++;
            }
        } else { // Moving up the rails
            for (i = rails - 2; i > 0 && j < len; i--) {
                if (code[i][j] != 0) {
                    decrypted[pos++] = code[i][j];
                }
                j++;
            }
        }
        count++;
    }
    decrypted[pos] = '\0'; // Null-terminate the string
}

int main() {
    char str[1000], encrypted[1000], decrypted[1000];
    int rails;
    printf("Simulating Rail Fence Cipher\n");

    printf("Enter a Secret Message: ");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = 0; // Remove trailing newline character if exists

    printf("Enter number of rails: ");
    scanf("%d", &rails);

    // Encrypt the message
    encryptRailFence(str, rails, encrypted);
    printf("Encrypted Message: %s\n", encrypted);

    // Decrypt the message
    decryptRailFence(encrypted, rails, decrypted);
    printf("Decrypted Message: %s\n", decrypted);

    return 0;
}
```
## OUTPUT:

Simulating Rail Fence Cipher

![Screenshot 2024-10-21 083727](https://github.com/user-attachments/assets/10784ff6-4476-4daf-8fd8-454701e22f82)

```
Simulating Rail Fence Cipher
Enter a Secret Message: kavin
Enter number of rails: 14
Encrypted Message: kavin
Decrypted Message: kavin
```


## RESULT:
The program for rail fence cipher method is executed successfully
