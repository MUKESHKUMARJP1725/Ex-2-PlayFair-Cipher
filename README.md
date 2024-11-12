# Ex-2 PlayFair Cipher

### DATE:

Playfair Cipher using with different ket values.

## AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

## DESIGN STEPS:

Step 1: Design of PlayFair Cipher algorithnm.

Step 2: Implementation using C or pyhton code.

Step 3: Testing algorithm with different key values.

## ALGORITHM DESCRIPTION: 

The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre. The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:

  1. If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Somevariants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
    
  2. If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping around to the left side of the row if a letter in the original pair was on the right side of the row).
  3. If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around to the top side of the column if a letter in the original pair was on the bottom side of the column).

  4. If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that lies on the same row as the first letter of the plaintext pair. To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).

## PROGRAM:
```
#include <iostream>
#include <cstring>
#include <cctype>

#define SIZE 30

using namespace std;

// Function to convert the string to lowercase
void toLowerCase(char str[]) {
    for (int i = 0; str[i]; i++) {
        str[i] = tolower(str[i]);
    }
}

// Function to remove all spaces in a string
int removeSpaces(char* str) {
    int count = 0;
    for (int i = 0; str[i]; i++) {
        if (str[i] != ' ') {
            str[count++] = str[i];
        }
    }
    str[count] = '\0'; // Null-terminate the string
    return count;
}

// Function to generate the 5x5 key square
void generateKeyTable(char key[], char keyTable[5][5]) {
    bool charSet[26] = {false}; // To track characters used in the key
    int index = 0;

    // Insert key characters into keyTable
    for (int i = 0; key[i]; i++) {
        if (key[i] == 'j') key[i] = 'i'; // Treat 'j' as 'i'
        if (!charSet[key[i] - 'a']) { // Check if character is already used
            charSet[key[i] - 'a'] = true;
            keyTable[index / 5][index % 5] = key[i];
            index++;
        }
    }

    // Fill the rest of the table with remaining characters
    for (char ch = 'a'; ch <= 'z'; ch++) {
        if (ch == 'j') continue; // Skip 'j'
        if (!charSet[ch - 'a']) {
            keyTable[index / 5][index % 5] = ch;
            index++;
        }
    }
}

// Function to search for the characters of a digraph in the key square and return their position
void search(char keyTable[5][5], char a, char b, int arr[]) {
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyTable[i][j] == a) {
                arr[0] = i; // Row of first character
                arr[1] = j; // Column of first character
            }
            if (keyTable[i][j] == b) {
                arr[2] = i; // Row of second character
                arr[3] = j; // Column of second character
            }
        }
    }
}

// Function to prepare the plaintext for encryption
int prepare(char str[]) {
    int len = strlen(str);
    if (len % 2 != 0) { // If the length is odd, append 'x'
        str[len++] = 'x';
    }
    str[len] = '\0'; // Null-terminate the string
    return len;
}

// Function for performing the encryption
void encrypt(char str[], char keyTable[5][5]) {
    int len = strlen(str);
    for (int i = 0; i < len; i += 2) {
        int arr[4];
        search(keyTable, str[i], str[i + 1], arr);
        if (arr[0] == arr[2]) { // Same row
            str[i] = keyTable[arr[0]][(arr[1] + 1) % 5];
            str[i + 1] = keyTable[arr[2]][(arr[3] + 1) % 5];
        } else if (arr[1] == arr[3]) { // Same column
            str[i] = keyTable[(arr[0] + 1) % 5][arr[1]];
            str[i + 1] = keyTable[(arr[2] + 1) % 5][arr[3]];
        } else { // Rectangle swap
            str[i] = keyTable[arr[0]][arr[3]];
            str[i + 1] = keyTable[arr[2]][arr[1]];
        }
    }
}

// Function to encrypt using Playfair Cipher
void encryptByPlayfairCipher(char str[], char key[]) {
    char keyTable[5][5];
    toLowerCase(key);
    int ks = removeSpaces(key); // Remove spaces and get length
    generateKeyTable(key, keyTable);
    int ps = removeSpaces(str); // Prepare plaintext
    ps = prepare(str); // Make length even
    encrypt(str, keyTable); // Encrypt
}

int main() {
    char str[SIZE], key[SIZE];

    // Key to be encrypted
    strcpy(key, "Monarchy");
    cout << "Key text: " << key << endl;

    // Plaintext to be encrypted
    strcpy(str, "instruments");
    cout << "Plain text: " << str << endl;

    // Encrypt using Playfair Cipher
    encryptByPlayfairCipher(str, key);
    cout << "Cipher text: " << str << endl;

    return 0;
}
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/0a99c975-4b53-4503-aeed-60bd913cf600)

## RESULT:

The program is executed successfully.
