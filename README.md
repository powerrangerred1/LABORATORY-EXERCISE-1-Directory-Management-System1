#include <iostream>
#include <string>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include <dirent.h>

using namespace std;

void list_files(const string& path) {
    DIR* dir;
    struct dirent* entry;

    if ((dir = opendir(path.c_str())) == NULL) {
        cout << "No files found or directory does not exist.\n";
        return;
    }

    while ((entry = readdir(dir)) != NULL) {
        if (entry->d_type != DT_DIR) {
            cout << entry->d_name << endl;
        }
    }
    closedir(dir);
}

void list_files_by_extension() {
    string extension;
    cout << "Enter file extension (e.g., .txt): ";
    cin >> extension;
    cin.ignore();

    string path = ".";
    DIR* dir;
    struct dirent* entry;

    if ((dir = opendir(path.c_str())) == NULL) {
        cout << "No files found or directory does not exist.\n";
        return;
    }

    while ((entry = readdir(dir)) != NULL) {
        string filename = entry->d_name;
        if (filename.find(extension) != string::npos && entry->d_type != DT_DIR) {
            cout << filename << endl;
        }
    }
    closedir(dir);
}

void list_files_by_name() {
    string name;
    cout << "Enter file name (use * as wildcard, e.g., *.txt): ";
    cin >> name;
    cin.ignore();

    cout << "Wildcard search not supported in this platform version.\n";
}

void create_directory() {
    string dir_name;
    cout << "Enter directory name: ";
    getline(cin, dir_name);

    if (mkdir(dir_name.c_str(), 0777) == 0) {
        cout << "Directory created.\n";
    } else {
        cout << "Failed to create directory or directory already exists.\n";
    }
}

void change_directory() {
    cout << "1. Go up one level\n2. Go to root directory\n3. Go to a specific directory\n";
    int choice;
    cout << "Enter your choice: ";
    cin >> choice;
    cin.ignore();  

    switch (choice) {
        case 1: {
            if (chdir("..") == 0) {
                cout << "Moved up one level.\n";
            } else {
                cout << "Error moving up.\n";
            }
            break;
        }
        case 2:
            if (chdir("/") == 0) {
                cout << "Moved to root directory.\n";
            } else {
                cout << "Error changing to root directory.\n";
            }
            break;
        case 3: {
            string new_dir;
            cout << "Enter path: ";
            getline(cin, new_dir);
            if (chdir(new_dir.c_str()) == 0) {
                cout << "Changed to directory: " << new_dir << endl;
            } else {
                cout << "Directory does not exist.\n";
            }
            break;
        }
        default:
            cout << "Invalid choice.\n";
            break;
    }
}

void showFileListMenu() {
    int fileChoice;

    do {
        cout << "\n";
        cout << " LIST FILE DETAILS\n"; 
        cout << "------------------\n";
        cout << "1. List All Files\n";
        cout << "2. List by Extension\n";
        cout << "3. List by Name\n"; 
        cout << "Enter the Number: ";
        cin >> fileChoice;
        cin.ignore();  

        switch (fileChoice) {
            case 1:
                list_files(".");
                break;
            case 2:
                list_files_by_extension();
                break;
            case 3:
                list_files_by_name();
                break;
            default:
                cout << "Invalid choice.\n";
                break;
        }
    } while (fileChoice < 1 || fileChoice > 3);  
}

