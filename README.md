#include <iostream>
#include <fstream>
#include <string>

// Shift value for Caesar cipher encryption
const int SHIFT = 3;

// Function to encrypt a string using Caesar cipher
std::string encrypt(const std::string& str) {
    std::string encrypted = str;
    for (char& c : encrypted) {
        if (isalpha(c)) {  // Check if character is a letter
            char base = islower(c) ? 'a' : 'A';  // Determine base (a/A)
            c = base + (c - base + SHIFT) % 26;  // Shift character
        }
    }
    return encrypted;
}

// Function to decrypt a string using Caesar cipher
std::string decrypt(const std::string& str) {
    std::string decrypted = str;
    for (char& c : decrypted) {
        if (isalpha(c)) {  // Check if character is a letter
            char base = islower(c) ? 'a' : 'A';  // Determine base (a/A)
            c = base + (c - base - SHIFT + 26) % 26;  // Shift character back
        }
    }
    return decrypted;
}

// Function to save password information to a file
void savePassword(const std::string& username, const std::string& password) {
    std::ofstream file("pass.txt", std::ios::app);  // Open file in append mode
    if (file.is_open()) {
        file << username << " " << encrypt(password) << "\n";  // Write username and encrypted password
        file.close();  // Close the file
    } else {
        std::cerr << "Unable to open file for writing.\n";  // Error message if file can't be opened
    }
}

// Function to list all saved passwords
void listPasswords() {
    std::string adminPassword;
    std::cout << "Enter admin password: ";
    std::cin >> adminPassword;

    if (adminPassword != "123") {  // Check for correct admin password
        std::cout << "Incorrect admin password. Access denied.\n";
        return;
    }

    std::ifstream file("pass.txt");  // Open file in read mode
    if (file.is_open()) {
        std::string username, password;
        while (file >> username >> password) {  // Read username and encrypted password
            std::cout << "Username: " << username << ", Password: " << decrypt(password) << "\n";  // Decrypt and display
        }
        file.close();  // Close the file
    } else {
        std::cerr << "Unable to open file for reading.\n";  // Error message if file can't be opened
    }
}

int main() {
    int choice;
    std::cout << "Password Manager\n";
    std::cout << "1. Add a new password\n";
    std::cout << "2. List all passwords\n";
    std::cout << "3. Exit\n";
    
    do {
        std::cout << "Choose an option: ";
        std::cin >> choice;
        
        switch (choice) {
            case 1: {
                std::string username, password;
                std::cout << "Enter username: ";
                std::cin >> username;
                std::cout << "Enter password: ";
                std::cin >> password;
                savePassword(username, password);  // Save the new password
                break;
            }
            case 2:
                listPasswords();  // List all saved passwords
                break;
            case 3:
                std::cout << "Exiting...\n";
                break;
            default:
                std::cout << "Invalid option. Please try again.\n";
        }
    } while (choice != 3);

    return 0;
}
