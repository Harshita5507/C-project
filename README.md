# 🗳️ E-Voting System in C

##  Project Overview

The **E-Voting System** is a console-based application developed in the C programming language that simulates a simple electronic voting process. The system allows an administrator to set up an election, voters to register and cast their votes, and users to view election results.

The project demonstrates the practical implementation of core C programming concepts such as structures, file handling, dynamic memory allocation, functions, pointers, and modular programming.

---

## 🎯 Objectives

* Create and manage an election.
* Register voters securely.
* Allow each voter to vote only once.
* Store voting data permanently using files.
* Display election results and winner information.

---

##   Features

### 👨‍💼 Admin Module

* Create a new election.
* Enter the number of candidates.
* Add candidate details:

  * Candidate ID
  * Candidate Name
  * Party Name
* Initialize vote counts.

### 🧑‍🤝‍🧑 Voter Module

* Register as a new voter.
* Store voter details permanently.
* Prevent duplicate voting.
* Cast a vote for a preferred candidate.

### 📊 Result Module

* Display all candidates and vote counts.
* Determine and display the winning candidate.
* Show election summary in a formatted table.

### 💾 File Management

* Candidate information stored in `candidate.txt`
* Voter information stored in `voters.txt`
* Data remains available even after program termination.

### 🎨 User Interface

* Colorful terminal output using ANSI escape sequences.
* Interactive menu-driven system.
* Typing animation effect for improved user experience.

---

## 🛠️ Technologies Used

* **Language:** C
* **Compiler:** GCC
* **Concepts Implemented:**

  * Structures
  * Functions
  * Arrays
  * Pointers
  * Dynamic Memory Allocation (`malloc`)
  * File Handling
  * Conditional Statements
  * Loops
  * Macros

---

## 📂 Project Structure

```text
E-Voting-System/
│
├── main.c
├── candidate.txt
├── voters.txt
└── README.md
```

---

## ⚙️ How to Run

### Step 1: Compile the Program

```bash
gcc main.c -o voting
```

### Step 2: Run the Program

```bash
./voting
```

For Windows:

```bash
gcc main.c -o voting.exe
voting.exe
```

---

## 📋 Workflow

1. Admin creates an election.
2. Candidates are stored in `candidate.txt`.
3. Voters register using a unique voter ID.
4. Voter details are stored in `voters.txt`.
5. Registered voters cast their votes.
6. The system prevents multiple voting attempts.
7. Results are generated and displayed.

---

## 🔒 Voting Rules

* Each voter must have a unique Voter ID.
* A voter can vote only once.
* Duplicate voting is not allowed.
* Votes are recorded permanently in files.

---

## 📸 Sample Menu

```text
1. ADMIN
2. REGISTER AS VOTER AND VOTE
3. VIEW RESULTS
4. EXIT
```

---

## 🚀 Future Enhancements

* Admin authentication using password protection.
* Candidate search functionality.
* Vote percentage calculation.
* Tie detection between candidates.
* Graphical User Interface (GUI).
* Database integration (MySQL/SQLite).
* Enhanced security and encryption.

---

## 🎓 Academic Information

**Project Title:** E-Voting System

**Submitted To:** Mr. Pankaj Badoni

**Submitted By:** Harshita Pandey

**Batch:** 9

---

## 📄 License

This project is developed for educational and academic purposes.
