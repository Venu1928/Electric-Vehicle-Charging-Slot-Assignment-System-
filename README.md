# EV Charging Slot Assignment System

## Overview
The **Electric Vehicle (EV) Charging Slot Assignment System** is a console-based C++ application designed to simulate and manage the allocation of charging slots and waiting queues at an EV charging station. It utilizes foundational data structures to handle fixed resources and manage user flow efficiently.

---

## Features
* **Fixed Slot Management:** Tracks the availability status (Available/Occupied) of charging slots using arrays.
* **Waiting Line Management:** Manages incoming vehicles using a First-In, First-Out (FIFO) queue when all charging slots are fully occupied.
* **Ordered Display:** Implements a basic sorting algorithm to present slot information sequentially by slot number.
* **Dynamic Assignment & Release:** Automatically assigns waiting vehicles from the queue to newly released slots.

---

## Data Structures Used
* **Array:** Stores fixed charging slot identifiers, occupancy flags, and assigned vehicle IDs.
* **Queue (`std::queue`):** Maintains the FIFO waiting line for vehicles arriving when capacity is full.
* **Sorting Algorithm:** Arranges slot numbers in ascending order for clean console outputs.

---

## System Menu Options
1. **View slots:** Displays current slot statuses (Available or Occupied by a specific vehicle) and the number of vehicles currently waiting in the queue.
2. **Assign vehicle:** Takes a vehicle ID and assigns it to the first available slot, or adds it to the waiting queue if all slots are occupied.
3. **Release slot:** Frees an occupied slot and automatically assigns it to the next vehicle in the waiting queue (if any).
4. **Exit:** Terminates the program execution.

---

## Source Code

```cpp
#include <iostream>
#include <queue>
using namespace std;
 
const int SIZE = 5;
 
int slotNumber[SIZE] = {1, 2, 3, 4, 5};
int occupied[SIZE] = {0, 0, 0, 0, 0};
string vehicleID[SIZE];
queue<string> waiting;
 
void sortSlots()
{
    int i, j, temp;
    for (i = 0; i < SIZE - 1; i++)
    {
        for (j = 0; j < SIZE - i - 1; j++)
        {
            if (slotNumber[j] > slotNumber[j + 1])
            {
                temp = slotNumber[j];
                slotNumber[j] = slotNumber[j + 1];
                slotNumber[j + 1] = temp;
            }
        }
    }
}
 
void displaySlots()
{
    int i;
    sortSlots();
 
    cout << "\n--- Charging Slot Status ---\n";
    for (i = 0; i < SIZE; i++)
    {
        int index = slotNumber[i] - 1;
        cout << "Slot " << slotNumber[i] << ": ";
        if (occupied[index] == 1)
            cout << "Occupied by " << vehicleID[index];
        else
            cout << "Available";
        cout << endl;
    }
    cout << "Vehicles waiting: " << waiting.size() << endl;
}
 
void assignVehicle()
{
    string id;
    int i, found = -1;
 
    cout << "Enter vehicle ID: ";
    cin >> id;
 
    for (i = 0; i < SIZE; i++)
    {
        if (occupied[i] == 0)
        {
            found = i;
            break;
        }
    }
 
    if (found == -1)
    {
        waiting.push(id);
        cout << "All slots are occupied. Vehicle added to waiting queue.\n";
    }
    else
    {
        occupied[found] = 1;
        vehicleID[found] = id;
        cout << "Vehicle " << id << " assigned to Slot " << found + 1 << ".\n";
    }
}
 
void releaseSlot()
{
    int number, index;
    cout << "Enter slot number to release (1-5): ";
    cin >> number;
 
    if (number < 1 || number > SIZE)
    {
        cout << "Invalid slot number.\n";
        return;
    }
 
    index = number - 1;
 
    if (occupied[index] == 0)
    {
        cout << "That slot is already available.\n";
        return;
    }
 
    cout << "Slot " << number << " released from vehicle "
         << vehicleID[index] << ".\n";
    occupied[index] = 0;
    vehicleID[index] = "";
 
    if (!waiting.empty())
    {
        vehicleID[index] = waiting.front();
        waiting.pop();
        occupied[index] = 1;
        cout << "Next waiting vehicle " << vehicleID[index]
             << " assigned to Slot " << number << ".\n";
    }
}
 
int main()
{
    int choice;
 
    do
    {
        cout << "\n=== EV Charging Slot Assignment System ===\n";
        cout << "1. View slots\n";
        cout << "2. Assign vehicle\n";
        cout << "3. Release slot\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;
 
        switch (choice)
        {
            case 1:
                displaySlots();
                break;
            case 2:
                assignVehicle();
                break;
            case 3:
                releaseSlot();
                break;
            case 4:
                cout << "Exiting system.\n";
                break;
            default:
                cout << "Invalid choice. Try again.\n";
        }
    } while (choice != 4);
 
    return 0;
}
```

---

## How to Compile and Run
1. Ensure you have a C++ compiler installed (e.g., `g++`).
2. Save the source code to a file named `main.cpp`.
3. Compile the source code using your terminal or command prompt:
   ```bash
   g++ -o ev_system main.cpp
   ```
4. Run the executable:
   ```bash
   ./ev_system