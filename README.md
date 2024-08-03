#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Crop class definition
class Crop {
public:
    string type;
    int growthStage;
    int waterLevel;

    Crop(string t) : type(t), growthStage(0), waterLevel(0) {}

    void grow() {
        if (waterLevel > 0) {
            growthStage++;
            waterLevel--;
        }
    }

    void water() {
        waterLevel++;
    }

    bool isMature() {
        return growthStage >= 3;
    }

    string display() {
        if (type.empty()) return "[ ]";
        if (isMature()) return "[H]";
        return "[" + type.substr(0, 1) + to_string(growthStage) + "]";
    }
};

// Farm class definition
class Farm {
public:
    vector<vector<Crop>> grid;

    Farm(int rows, int cols) : grid(rows, vector<Crop>(cols, Crop(""))) {}

    void plantCrop(int row, int col, string type) {
        if (grid[row][col].type.empty()) {
            grid[row][col] = Crop(type);
            cout << "Planted " << type << " at (" << row << ", " << col << ").\n";
        } else {
            cout << "There is already a crop at (" << row << ", " << col << ").\n";
        }
    }

    void waterCrop(int row, int col) {
        if (!grid[row][col].type.empty()) {
            grid[row][col].water();
            cout << "Watered crop at (" << row << ", " << col << ").\n";
        } else {
            cout << "There is no crop at (" << row << ", " << col << ") to water.\n";
        }
    }

    void harvestCrop(int row, int col) {
        if (grid[row][col].isMature()) {
            cout << "Harvested " << grid[row][col].type << " from (" << row << ", " << col << ").\n";
            grid[row][col] = Crop("");
        } else {
            cout << "Crop at (" << row << ", " << col << ") is not ready for harvest.\n";
        }
    }

    void growCrops() {
        for (auto &row : grid) {
            for (auto &crop : row) {
                crop.grow();
            }
        }
    }

    void displayFarm() {
        for (const auto &row : grid) {
            for (const auto &crop : row) {
                cout << crop.display() << " ";
            }
            cout << endl;
        }
    }
};

// Main game loop
int main() {
    Farm farm(3, 3);
    int day = 1;

    while (true) {
        cout << "\nDay " << day << ":\n";
        farm.displayFarm();

        cout << "Choose an action:\n";
        cout << "1. Plant a crop\n";
        cout << "2. Water a crop\n";
        cout << "3. Harvest a crop\n";
        cout << "4. End day\n";
        cout << "5. Quit game\n";
        int choice;
        cin >> choice;

        if (choice == 1) {
            int row, col;
            string type;
            cout << "Enter row, column, and crop type: ";
            cin >> row >> col >> type;
            farm.plantCrop(row, col, type);
        } else if (choice == 2) {
            int row, col;
            cout << "Enter row and column to water: ";
            cin >> row >> col;
            farm.waterCrop(row, col);
        } else if (choice == 3) {
            int row, col;
            cout << "Enter row and column to harvest: ";
            cin >> row >> col;
            farm.harvestCrop(row, col);
        } else if (choice == 4) {
            farm.growCrops();
            day++;
        } else if (choice == 5) {
            break;
        } else {
            cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}
