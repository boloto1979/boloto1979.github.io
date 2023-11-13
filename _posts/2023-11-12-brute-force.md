---
layout: post
title: Brute Force Attack
description: "A tiresome form of passwords."
---

A brute force attack is a technique used by hackers to gain access to systems, accounts, or protected information by systematically attempting all possible combinations of passwords, encryption keys, or other authentication information. This approach is known for its "brute force" method of trying every possible combination until the correct one is found. I'll explain in detail how this type of attack works, including the technical aspects involving brute force software.

### How a Brute Force Attack Works:

1. **Information Gathering**: The first step in conducting a brute force attack is to gather information about the target. This may include the username or email address for the account, if it's a authentication system, or any other relevant information that can be used to guess the password or key.

2. **Generating Combinations**: The next step is to generate all possible combinations of passwords or keys. This can vary in complexity depending on the length and complexity of the password. For example, if a password is 8 alphanumeric characters, there will be many combinations to test.

3. **Automation**: To efficiently execute a brute force attack, hackers use specialized software. This software automates the login attempt process, testing each password combination in rapid succession. It typically uses lists of popular passwords, word dictionaries, or password generation rules to optimize the attempts.

4. **Testing Combinations**: The brute force software attempts each password or key combination until the correct password is found or all combinations have been tested. This process can be time-consuming depending on password complexity and attack speed.

5. **Evasion Measures**: To avoid detection or blocking, attackers often incorporate evasion techniques such as delays between attempts, using proxies or VPNs, and changing the user agent to appear as if the attack is coming from different sources.

6. **Detection and Mitigation**: Security systems can detect and mitigate brute force attacks by monitoring repeated login attempts and blocking access after a specific number of failed tries. Additionally, stronger passwords and two-factor authentication (2FA) are effective measures to reduce vulnerability to brute force attacks.

### Brute Force Software:

There are several software tools available that enable automated brute force attacks. Some popular examples include:

- **Hydra**: Hydra is a widely-used brute force tool that supports multiple protocols such as SSH, FTP, RDP, HTTP, and more.

- **John the Ripper**: This is another widely used tool for brute-forcing passwords. It is capable of handling various hash algorithms.

- **Medusa**: Medusa is similar to Hydra, supporting various protocols and services.

- **Aircrack-ng**: This software is specifically designed for brute force attacks on Wi-Fi networks with the goal of discovering wireless network passwords.

- **Hashcat**: Hashcat is a specialized tool for password cracking, particularly for passwords stored as hashes.

### Software example

The code you provided is an example of a brute-force algorithm implemented in C++ to crack a simple text password. Let's break down the code step by step:

**Including Libraries**
```c
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
```
In this section, you include the necessary libraries for your program. `iostream` is used for standard input and output, `string` for string manipulation, `vector` to store the characters used in brute force, and `algorithm` for operations like generating permutations.

**`generate_characters` Function**
```c
std::vector<char> generate_characters() {
    // Creates a vector of characters containing lowercase letters, uppercase letters, and digits.
}
```
This function generates a vector of characters that contains lowercase letters (a-z), uppercase letters (A-Z), and digits (0-9). It is used as the character set that the program will use for brute forcing.

**`brute_force` Function**
```c
std::string brute_force(const std::vector<char>& chars, const std::string& password, int len_password) {
    // Performs a brute force search to find the provided password.
}
```
This function performs a brute force search to find the provided password. It generates combinations of characters and checks if they match the given password.

**`brute_force_recursive` Function**
```c
void brute_force_recursive(const std::vector<char>& chars, const std::string& password, int len_password, const std::string& comb_previous = "") {
    // Performs a recursive brute force search to find the provided password.
}
```
This function is a recursive version of the brute force search and essentially does the same thing as the `brute_force` function. However, it uses recursion to generate all possible combinations of characters.

**`main` Function**
```c
int main() {
    std::vector<char> chars = generate_characters();
    std::string password = "cat"; // Password to be cracked
    // Calls the `brute_force` and `brute_force_recursive` functions with different parameters.
}
```
The `main` function is the entry point of the program. It initializes the character vector, sets the password you want to crack, and calls the `brute_force` and `brute_force_recursive` functions with different parameters to find the password.

### Full code:
```c
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

std::vector<char> generate_characters() {
    std::vector<char> chars;
    for (int i = 97; i <= 122; i++) {
        chars.push_back(static_cast<char>(i));
    }
    for (int i = 65; i <= 90; i++) {
        chars.push_back(static_cast<char>(i));
    }
    for (int i = 48; i <= 57; i++) {
        chars.push_back(static_cast<char>(i));
    }
    return chars;
}

std::string brute_force(const std::vector<char>& chars, const std
    ::string& password, int len_password) {
        int attempt = 0;
        do {
            std::string matches;
            for (int i = 0; i < len_password; i++) {
                matches.push_back(chars[attempt % chars.size()]);
                attempt /= chars.size();
            }
            attempt++;
            if (attempt % 500000 == 0) {
                std::cout << attempt << " --> " << matches << std::endl; 
            }
            if (password == matches){
                return "password found is \"" + matches + "\", after " + std::to_string(attempt) + 
                " attempt. "; 
            }
        } while (attempt > 0);

        return "password not found";
    }

void brute_force_recursive(const std::vector<char>& chars,const std::string& password, int len_password, 
    const std::string& comb_previous = "") {
    static int attempt = 0;
    for ( char letter : chars ) {
        std::string matches = comb_previous + letter;
        attempt++;
        if (attempt % 500000 == 0) {
            std::cout << attempt << " --> " << matches << std::endl;
        }
        if (password == matches) {
            std::cout << "password found is \" " << matches << "\", after " << attempt << "attempt." << std::endl;
            return;
        }
        if (len_password != 1) {
            brute_force_recursive(chars, password, len_password - 1,
            matches);
        }
    }
}

int main() {
    std::vector<char> chars = generate_characters();
    std::string password = "cat"; //hash to be cracked
    std::cout << brute_force(chars, password, 4) << std::endl;
    std::cout << "***************************************" << std::endl;
    brute_force_recursive(chars, password, password.length());
    std::cout << "***************************************" << std::endl;
    brute_force_recursive(chars, "end", 4);
    std::cout << "***************************************" << std::endl;
    brute_force_recursive(chars, "end", 5);
    return 0;
}
```

In summary, the code generates all possible combinations of characters and checks if any of them match the provided password. It's important to note that brute force is an inefficient method for cracking passwords and should not be used for malicious purposes. It's useful only for educational purposes or for testing the security of passwords in your own system. Additionally, it's crucial to use strong passwords and proper security methods to protect sensitive information.