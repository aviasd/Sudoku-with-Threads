# Sudoku-with-Threads
Sudoku with Threads in C, second year second semester's second Operating Systems project

### Sudoku Puzzle
This type of puzzle has a 9-row and 9-column matrix when some entries have digits (between 1 and 9) and some of the entries are empty.  Digits must be entered in the empty entries so that the following conditions are met:
1) Each of the rows will display all digits between 1 and 9
2) Each of the columns will display all digits between 1 and 9
3) Each of the nine *3x3* "sub-matrices" will display all digits between 1 and 9

### Checker Interface
The input file name (only one input file) is given to the program as a command line argument.  
If there is no command line argument, then the program reads single matrix from the standard input.  
The program reads a matrix from the file, check if it represents a valid solution to the Sudoku problem and prints a suitable message for standard output. If the solution is valid it prints "valid" and if not, it prints "not valid".

### Sudoku Checker with Threads
#### First Version:
The main thread creates 9 + 9 + 9 = 27 threads. The work of testing the solution is shared among those threads.  
Nine threads (one per line) is responsible for checking that all lines are valid (contains all digits between 1 and 9).  
Nine additional threads is responsible for checking that all columns are are valid (contains all digits between 1 and 9).  
And 9 additional threads is responsible for checking that all the sub-matrices are valid (contains all digits between 1 and 9).  
The input matrix is read from the input by the main thread and stored in a global data structure so that all the threads can access it.  
Only the main thread writes this data structure, and it will do this before creating the other threads. This is why we do not need to use synchronization measures like mutex, in accessing the data structure.  
The main thread notifies each thread what is the task it needs to do (for example: "check validity of row number 3"), by passing an appropriate argument.  
Each of the threads notifies the main thread of its test results by writing to the appropriate entry (different entry for each thread), in a global results array (the array will have 27 entries).  
If a threads finds out the part it's testing is valid, it writes 1 to the appropriate entry. If not, it writes 0.  
The main thread wait for all the threads to complete the test, and then scan the results, and come to a final conclusion as to whether the solution is valid or not.

#### Second Version: Using *mutex* and *condition variable*:

