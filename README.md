![image](https://github.com/user-attachments/assets/367b6b0a-e114-468a-b7e2-e9164119a805)# AI-Powered-Directory-Management-System
#include <iostream>
#include <cstdlib>   // For rand() in prediction
#include <ctime>     // For seeding random function
#include <iomanip>   // For formatting output

using namespace std;

// Structure to represent a process
struct Process {
    int pid;            // Process ID
    int arrivalTime;
    int burstTime;
    int waitingTime;
    int turnAroundTime;
};

// Function to perform FCFS scheduling
void fcfsScheduling(Process processes[], int n) {
    int completionTime[n];
    
    processes[0].waitingTime = 0; 
    completionTime[0] = processes[0].arrivalTime + processes[0].burstTime;
    processes[0].turnAroundTime = processes[0].burstTime;

    for (int i = 1; i < n; i++) {
        // Waiting time = Completion of previous process - Arrival of current process
        processes[i].waitingTime = max(0, completionTime[i - 1] - processes[i].arrivalTime);
        completionTime[i] = processes[i].arrivalTime + processes[i].waitingTime + processes[i].burstTime;
        processes[i].turnAroundTime = processes[i].waitingTime + processes[i].burstTime;
    }
}

// Function to display process details
void displayProcesses(Process processes[], int n) {
    cout << "\nProcess\tArrival\tBurst\tWaiting\tTurnaround\n";
    for (int i = 0; i < n; i++) {
        cout << processes[i].pid << "\t"
             << processes[i].arrivalTime << "\t"
             << processes[i].burstTime << "\t"
             << processes[i].waitingTime << "\t"
             << processes[i].turnAroundTime << "\n";
    }
}

// Function to display Gantt chart
void displayGanttChart(Process processes[], int n) {
    cout << "\nGantt Chart:\n|";
    for (int i = 0; i < n; i++) {
        cout << " P" << processes[i].pid << " |";
    }
    cout << "\n0";
    int time = 0;

    for (int i = 0; i < n; i++) {
        time += processes[i].burstTime;
        cout << "   " << time;
    }
    cout << endl;
}

// ML-like prediction (random process prediction)
int predictSchedule(int n) {
    srand(time(0));
    return (rand() % n) + 1;   // Randomly predict next process ID
}

// Main function
int main() {
    int n;

    cout << "Enter the number of processes: ";
    cin >> n;

    Process processes[n];

    // Taking input for arrival time and burst time
    for (int i = 0; i < n; i++) {
        cout << "Enter Arrival Time and Burst Time for Process " << i + 1 << ": ";
        processes[i].pid = i + 1;
        cin >> processes[i].arrivalTime >> processes[i].burstTime;
    }

    // Perform FCFS Scheduling
    fcfsScheduling(processes, n);

    // Display results
    displayProcesses(processes, n);
    displayGanttChart(processes, n);

    // Predict next process using random selection (ML-like prediction)
    int predictedProcess = predictSchedule(n);
    cout << "\n[ML Prediction] Next likely process to execute: P" << predictedProcess << "\n";

    // Literature Review (as comments)
    /*
    Literature Review:
    1. First Come First Serve (FCFS) – Simple but inefficient in power management.
    2. Round Robin (RR) – Ensures fairness but leads to frequent context switching, increasing power consumption.
    3. Shortest Job Next (SJN) – Optimizes execution time but does not consider energy consumption.
    */

    return 0;
}
