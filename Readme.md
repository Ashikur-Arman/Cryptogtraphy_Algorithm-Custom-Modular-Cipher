
![Image](https://github.com/user-attachments/assets/e3359f8a-6dc1-4340-a450-fab1a58a5fea)
# Introduction

Custom Modular Cipher is a symmetric encryption algorithm that uses modular arithmetic to transform each character of the plaintext using a linear equation. It is inspired by the classical Affine Cipher but with more flexible parameter choices. This cipher is simple yet effective for demonstrating number theory and modular inverse concepts.

This cipher uses two numeric keys to obfuscate characters and ensures reversibility using modular inverses.


## Table of Contents
- [ Encryption Algorithm](#encryption-algorithm)
- [ Decryption Algorithm](#decryption-algorithm)
- [ Test Case Example](#text-case-example)
- [ Flowchart](#flowcharts)
- [ Implementation](#c-source-code)

#
#

## 🛡️ Encryption Algorithm

Formula:

For each character ch in the plaintext:

```bash
  E(x) = (a * x + b) mod m
```


#### Where:

- x = ASCII of ch
- a, b = secret keys
- m = modulus (typically 256 for ASCII)
- a must be coprime with m to allow decryption

#### Steps:

- Convert each character to its ASCII value.
- Apply the encryption formula.
- Convert back to characters to form the ciphertext.
## 🔓 Decryption Algorithm

To decrypt, we need the modular multiplicative inverse of 'a' modulo 'm', denoted as 'a_inv', such that:

```bash
  a * a_inv ≡ 1 (mod m)
```

#### Decryption Formula:

```bash
  D(y) = a_inv * (y - b) mod m
```


#
#
#
## Test Case Example

Let’s encrypt the message: "HSTUCSE"

Using:

- a = 5
- b = 8
- m = 256

#
#
### 🛡️ Encryption Table:

| Char | ASCII(x) | E(x) = (5x + 8) mod 256 | Encrypted ASCII | Encrypted Char |
| ---- | -------- | ----------------------- | --------------- | -------------- |
| H    | 72       | (5×72 + 8) % 256 = 112  | 112             | `p`            |
| S    | 83       | (5×83 + 8) % 256 = 179  | 179             | `³`            |
| T    | 84       | (5×84 + 8) % 256 = 184  | 184             | `¸`            |
| U    | 85       | (5×85 + 8) % 256 = 189  | 189             | `½`            |
| C    | 67       | (5×67 + 8) % 256 = 87   | 87              | `W`            |
| S    | 83       | (same as before) = 179  | 179             | `³`            |
| E    | 69       | (5×69 + 8) % 256 = 97   | 97              | `a`            |

🛡️ Encrypted Text: p³¸½W³a

#
#
#

### 🔓 Decryption Table:

Now apply:

```bash
  x = a_inv × (y - b) mod 256
```
Use:

𝐷(𝑦) = 205 × (𝑦−8) mod 256

| Cipher | ASCII(y) | y - b | ×205 % 256 | Decrypted ASCII | Original Char |
| ------ | -------- | ----- | ---------- | --------------- | ------------- |
| p      | 112      | 104   | 72         | 72              | H             |
| ³      | 179      | 171   | 83         | 83              | S             |
| ¸      | 184      | 176   | 84         | 84              | T             |
| ½      | 189      | 181   | 85         | 85              | U             |
| W      | 87       | 79    | 67         | 67              | C             |
| ³      | 179      | 171   | 83         | 83              | S             |
| a      | 97       | 89    | 69         | 69              | E             |

🔓 Decrypted Text: HSTUCSE

#
#


## Flowchart
![Image](https://github.com/user-attachments/assets/86084fb9-5c3a-4db6-b993-cb6cc3827f9d)

#
#
## 🖥️ Implementation

```bash 
#include <iostream>
#include <string>

using namespace std;

// Function to find modular inverse of 'a' under modulo 'm'
int modInverse(int a, int m) {
    for (int x = 1; x < m; x++) {
        if ((a * x) % m == 1)
            return x;
    }
    return -1; // Inverse doesn't exist
}

// Encryption Function
string encrypt(string text, int a, int b, int m = 256) {
    string encrypted = "";
    for (char ch : text) {
        int x = (int)ch;
        int enc = (a * x + b) % m;
        encrypted += (char)enc;
    }
    return encrypted;
}

// Decryption Function
string decrypt(string cipher, int a, int b, int m = 256) {
    int a_inv = modInverse(a, m);
    if (a_inv == -1) {
        throw runtime_error("Modular inverse does not exist for a = " + to_string(a));
    }

    string decrypted = "";
    for (char ch : cipher) {
        int y = (int)ch;
        int dec = (a_inv * (y - b + m)) % m; // Add m to avoid negative values
        decrypted += (char)dec;
    }
    return decrypted;
}

// Example usage
int main() {
    string text = "HSTUCSE";
    int a = 5, b = 8;

    string cipher = encrypt(text, a, b);
    cout << "Encrypted: ";
    for (char c : cipher) {
        cout << c;
    }
    cout << endl;

    string decrypted = decrypt(cipher, a, b);
    cout << "Decrypted: " << decrypted << endl;

    return 0;
}

```

## Sample Output
```bash
Encrypted: p³¸½W³a
Decrypted: HSTUCSE
```

Note: Encrypted characters like ³¸½ are extended ASCII and may show differently in some C++ compilers or terminals (especially on Windows CMD). But the logic works correctly, and we’ll always get back "HSTUCSE" as decrypted output.


## Future Improvements of the Custom Modular Cipher:

- Dynamic key generation based on passphrase or timestamp
- Block-based encryption instead of character-wise
- Rotating (a, b) key pairs for each character
- Use of stronger mathematical functions (e.g., non-linear equations)
- Extend to support full Unicode (UTF-8) characters
- Add integrity checks using hash functions (e.g., SHA-256)
- Combine with other ciphers for hybrid encryption
- Optimize modular inverse using Extended Euclidean Algorithm
- Improve performance using lookup tables

