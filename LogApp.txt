#include "LogApp.h"
#include "User.h"
#include "Admin.h"
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

LogApp::LogApp()
{
}

// Function to check if a username already exists in the file
bool LogApp::check_username(string username)
{
    ifstream file("users.txt");
    string line;
    while (getline(file, line))
    {
        if (line.substr(0, username.length()) == username)
        {
            file.close();
            return true;
        }
    }
    file.close();
    return false;
}

// Function to register a new user
void LogApp::register_user()
{
    string username, password;
    cout << "\tEnter a username: ";
    cin >> username;
    if (check_username(username))
    {
        cout << "************ That username already exists. Please choose a different one. ************" << endl << endl;
        return;
    }
    cout << "\tEnter a password: ";
    cin >> password;
    cout << endl;
    ofstream file("users.txt", ios::app);
    file << username << " " << password << endl;
    file.close();
    cout << "*************** User registered successfully. ***************" << endl << endl;
}

// Function to log in an existing user
bool LogApp::login_user()
{
    string username, password;
    cout << "\tEnter your username: ";
    cin >> username;
    cout << "\tEnter your password: ";
    cin >> password;
    cout << endl;
    ifstream file("users.txt");
    string line;
    while (getline(file, line))
    {
        if (line.substr(0, username.length()) == username)
        {
            string stored_password = line.substr(username.length() + 1);
            if (stored_password == password)
            {
                file.close();
                cout << "*************** Logged in successfully. ***************" << endl << endl;
                return true;
            }
            else
            {
                file.close();
                cout << "*************** Incorrect password. Please try again. ***************" << endl << endl;
                return false;
            }
        }
    }
    file.close();
    cout << "*************** That username does not exist. Please register a new account. ***************" << endl << endl;
    return false;
}

//Admin login function
bool LogApp::admin_login()
{
    string adminname, password;
    cout << "\tEnter your admin name: ";
    cin >> adminname;
    cout << "\tEnter your password: ";
    cin >> password;
    cout << endl;

    if (adminname == "admin" && password == "admin")
    {
        cout << "*************** Logged in successfully. ***************" << endl << endl;
        return true;
    }
    else
    {
        cout << "*************** Invalid admin name and password ***************" << endl << endl;
        return false;
    }
}


// Main Login Function
void LogApp::Login()
{
    int choice=1;
    while (choice)
    {
        cout << "\n******************** Welcome! ********************" << endl << endl;
        cout << "1. Register" << endl;
        cout << "2. Login" << endl;
        cout << "3. Admin Login" << endl;
        cout << "4. Exit" << endl << endl;
        cout << "\tEnter your choice: ";
        cin >> choice;
        cout << endl;
        switch (choice)
        {
        case 1:
            register_user();
            break;
        case 2:
            if (login_user())
            {
                User user1;
                user1.processUserMenu();
            }
            break;
        case 3:
            if (admin_login())
            {
                Admin admin1;
                admin1.processAdminMenu();
            } 
                break;
        case 4:
            choice = 0;
            break;
        default:
            cout << "*************** Invalid choice. Please try again. ***************" << endl << endl;
        }
    }
}

