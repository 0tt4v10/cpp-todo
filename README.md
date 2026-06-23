mach ein readme für diesen code #include <iostream>
#include <string>
#include <fstream>
#include <vector>
void loadTasks(std::vector<std::string>& todos) {
    std::ifstream datei("todos2.txt");
    std::string zeile;
    while (getline(datei, zeile)) {
        todos.push_back(zeile);
    }     
}
void saveTasks(std::vector<std::string>& todos) {
    std::ofstream datei("todos2.txt");
    for (int i = 0; i < todos.size(); i++) {
        datei << todos[i] << "\n";
    }
}
void showMenu() {
    std::cout << "\n";
    std::cout << "|===================|\n";
    std::cout << "|     TO-DO APP     |\n";
    std::cout << "|===================|\n";
    std::cout << "|   1. Add          |\n";
    std::cout << "|   2. Show         |\n";
    std::cout << "|   3. Delete       |\n";
    std::cout << "|   4. Close        |\n";
    std::cout << "|===================|\n";
    std::cout << "Chose: ";
}
void showTasks(std::vector<std::string>& todos) {
    std::cout << "\n";
    std::cout << "|===================|\n";
    std::cout << "|       Tasks       |\n";
    std::cout << "|===================|\n";
    if (todos.empty()) {
        std::cout << "|     No Tasks      |\n";
    } else {
        for (int i = 0; i < todos.size(); i++) {
            std::string line = "   " + std::to_string(i+1) + ". " + todos[i];
            int spaces = 19 - line.size();
            std::cout << "|" << line;
            for (int j = 0; j < spaces; j++) std::cout << " ";
            std::cout << "|\n";
        }
    }
    std::cout << "|===================|\n";
}
void addTasks(std::vector<std::string>& todos) {
    std::string input;
    std::cout << "New Task: ";
    std::getline(std::cin, input);
    todos.push_back(input);
    std::cout << "✓ Added!\n";
}
void deleteTasks(std::vector<std::string>& todos) {
    if (todos.empty()) {
        std::cout << "No tasks to delete!\n";
        return;
    }
    showTasks(todos);
    int num = 0;
    std::cout << "Which Number to delete? ";
    std::cin >> num;
    std::cin.ignore();
    if (num < 1 || num > todos.size()) {
        std::cout << "Invalid Number!\n";
        return;
    }
    todos.erase(todos.begin() + num -1);
    std::cout << "[OK] Deleted!\n";
}
int main() {
    std::vector<std::string> todos;
    loadTasks(todos);
    int vote = 0;
    while (vote != 4) {
        showMenu();
        std::cin >> vote;
        std::cin.ignore();
        switch(vote) {
            case 1: addTasks(todos); break;
            case 2: showTasks(todos); break;
            case 3: deleteTasks(todos); break;
            case 4: std::cout << "Bye!\n";
            default: std::cout << "Invalid Input!\n";
            saveTasks(todos);
        }
    }
    std::cin.ignore();
    std::cin.get();
    return 0;
}