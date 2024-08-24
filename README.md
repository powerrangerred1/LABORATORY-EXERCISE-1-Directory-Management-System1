#include <iostream>
#include <filesystem>
#include <vector>
#include <regex>

namespace fs = std::filesystem;

void displayMainMenu();
void handleMainMenuSelection(int choice);
void listFilesMenu();
void listAllFiles();
void listFilesByExtension();
void listFilesByPattern();
void createDirectory();
void changeDirectory();
void displayDirectoryMenu();
void handleDirectorySelection(int choice);
void exitProgram();

int main() {
    int choice;
    do {
        displayMainMenu();
        std::cin >> choice;
        handleMainMenuSelection(choice);
    } while (choice != 4); // 4 is the exit option
    return 0;
}
