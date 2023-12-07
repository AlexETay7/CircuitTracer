****************
* Project CircuitTracer
* Class project
* 12/4/2023
* Alex Taylor
**************** 

### ***OVERVIEW:***

 CircuitTracer at heart is a simple, configurable pathfinding program. A brute force search
 algorithm is used on a 2D array (which is a CircuitBoard) to find the best possible
 path (which is the shortest path) from a starting point to an ending point on the board. 
 The user is given an option on whether to configure the search as a stack-based search or a 
 queue-based search.


### ***ANALYSIS:***
 **i.** 
 The choice of storage configuration affects the sequence in which paths are
 explored significantly. When using a Stack for the search the most recently added
 "search state" (which in this case is our trace state) is evaluated first. This is
 called a "Depth First" search. When using a Stack, initially each (if there are multiple)
 start state is added to storage. Whichever starting state was added last will be
 evaluated first. This results in the search going down one particular path completely
 before it develops any of the other ones. This will result in a solution being generated
 quicker than a Queue-based search, but we cannot guarantee that the first solution 
 the search found is necessarily the best one. We do not know until we finish the 
 search if we have found the best solution.

 When using a Queue, however, the oldest "search state" in storage is evaluated first. This 
 is called a "Breadth First" search. When using a Queue, initially each (again, if there are multiple)
 start state is added to storage. Whichever starting state was added first will be evaluated
 first. This results in the search moving in a step-by-step (or fanning out) sequence where it evaluates
 the next move for one path, then evaluates the next move for another, and so on. This happens because
 our Queue is first in first out. Hence, the "oldest" state in the call stack is always up next
 to be evaluated. This results in the same ending search states that we eventually get to in 
 a Stack-based search, however, we get them in a different sequence. In contrast to the Stack-based
 search, in a Queue-based search we are guaranteed that once we find a solution, it is the best
 solution.

 **ii.**
 The total number of search states remains the same regardless of whether a stack or queue is used. 
 However, the order or sequence in which these states are explored varies greatly as described above.

 **iii.**
 Yes, a stack-based search (or "depth-first search") in theory is almost always going to find a solution
 before a queue-based search. However, this solution is not necessarily the BEST. So yes you
 will get a solution quicker, but you have no idea whatsoever if it is the best solution.

 **iv.**
 Yes, using a queue-based search guarantees that the first solution found is the best one. This is 
 because a queue-based search (which is a "breadth-first search") evaluates each step of each state
 level by level. In doing this, you know once you find a solution that it is the best one.

 **v.**
 Memory is significantly affected by the choice of underlying structure. When using a stack-based
 search we are almost guaranteed to find a solution faster, and in turn, there are fewer things on the
 call-stack so less memory is used(but we are not guaranteed the best solution). 
 In a queue-based search, we use more memory because we evaluate each state at the same level before 
 evaluating the next state. In doing this, our call stack becomes as large as however many possible 
 states there were in our search. 

 **vi.**
 The Big-Oh runtime order for the search algorithm implemented in this project is O(n). However, it
 does not reflect the maximum size of the Storage object (stateStore) because stateStore is only ever 
 as big as however many next possible moves there are. It does reflect the number of board positions
 (n positions) because the search algorithm is going to run n times, which is however many board 
 positions there are open. It will not reflect the number of paths explored because our while loop
 in the code does not depend on the number of paths, it depends on the number of possible search states.
 'n' is the number of possible moves (or possible board positions) that the current board being evaluated
 has.
 

### ***INCLUDED FILES:***

 * CircuitBoard.java - represents a 2D circuit board as read from an input file
 * CircuitTracer.java - implements a brute force search algorithm on an instance of the CircuitBoard class to find the best path
 * CircuitTracerTester.java - a unit test class for CircuitBoard.java and CircuitTracer.java
 * Storage.java - a container for storing elements of type T in one of several possible underlying data structures
 * TraceState.java - represents a search state along with a potential path through the CircuitBoard
 * OccupiedPositionException.java - an imported exception
 * InvalidFileFormatException.java - an imported exception
 * README - A brief overview of the program, how to run and compile it, related concepts, and testing information.


### ***COMPILING AND RUNNING:***

 From the directory containing all source files, compile the
 driver class (and all dependencies) with the command:
 $ javac *.java

 Run the compiled CircuitTracerTester class with the command:
 $ java CircuitTracerTester.java

 Console output will give the results of the tester after the program finishes.

 To run a single file, compile CircuitTracer.java (and all dependencies) 
 with the command:
 $ javac *.java

 Run the compiled CircuitTracer class with the command:
 $ java CircuitTracer.java -(s or q) -(c or g) your_file
 
 Please note that the above command takes 3 command line arguments. The first
 being whether to use a stack or queue, the second being whether to run in the console
 or through a GUI (graphical user interface), and the third is the file name.

 For example, testing a file called "file1.dat" with a stack, through the console,
 would look like this:
 $ java CircuitTracer.java -s -c file1.dat

 Using a queue, through the GUI, with "file1.dat" looks like this:
 $ java CircuitTracer.java -q -g file1.dat


### ***PROGRAM DESIGN AND IMPORTANT CONCEPTS:***

 CiricuitTracer is a pathfinding program that relies on a brute-force
 search algorithm to determine the best (most efficient) path 
 through a virtual CircuitBoard object. The program is configurable
 by the user to run either a stack-based search or a queue-based search.
 This is done via user input through the terminal.

 The CircuitBoard.java constructor serves as an exception handler by
 parsing the file (which is a virtual CircuitBoard) given by the user
 to make sure it meets the proper format. If any formatting errors are detected
 in the user-given file, the proper exception is thrown and caught, and a message is printed to
 the console. No user error should cause the program to crash, only exit cleanly.

 Once the file has been parsed and declared a valid file by the CircuitBoard 
 constructor, a CircuitTrace is performed on the file. This is done by first
 validating the command line arguments provided by the user. The user should
 provide 3 and only 3 parameters. Those are whether to use a stack or queue,
 to run through the console or the GUI and lastly the file name. 

 To continue the CircuitTrace, a configurable storage object is initialized to 
 store a new traceState object for each position adjacent to the initial position.
 An ArrayList of TraceStates called "bestPaths" is initialized to keep track
 of the best paths for the particular CircuitBoard it is working on. 
 
 Lastly, once the storage object contains all the initial starting states(trace states),
 the brute force search algorithm is applied. The memory usage of this search depends on
 the type of storage used (stack or queue) as described in the analysis section above.
 The search starts by evaluating the states in the starting state storage object
 (stateStore) and if any of them is a solution, add it to the bestPaths list. If the 
 solution is better than those currently in the best paths list, the list is cleared and the
 best solution is added. The algorithm continues to generate new starting states in the 
 storage until all possible paths have been explored. The search algorithm essentially 
 persists until the storage object of the starting states is empty. One thing that could
 be improved is the two 4 if-statement sequences could be reduced to 2 for loops with
 2 if statements inside.
 

### ***TESTING:***

 I tested my program in two ways. I used the test class my professor gave me,
 and I ran and compiled CircuitTracer.java in a separate terminal with test
 files. Through a combination of these two methods, I was able to ensure that
 my program was giving the right output. The program can handle bad input 
 without crashing, and is idiot-proof (I hope). I tested it often with many
 different kinds of bad input and it handled everything as it was supposed to.
 As far as I know, there are no issues or bugs in the program.


### ***DISCUSSION:***
 
 The main issue I encountered during development was that I was not 
 setting the starting point and ending point in the CircuitBoard constructor.
 This caused my program to throw NullPointerExceptions whenever I tried
 to load a test file to be parsed. Once I realized that this was the issue
 the fix was simple enough. Going back to my CircuitBoard constructor I simply
 set the starting point to the point in my nested for loop where I came across
 a '1' in the file and did the same for the ending point when I came across
 a '2' in the file.
 
 The challenging part of the project for me was the exception-handling
 part at the beginning. Exception handling is not my strongest suit at the 
 moment and I am sure there are much more efficient ways to check for exceptions
 in my program than what I did, but it is what it is. What finally clicked for me
 at the end of the project was how to use the '.x' and '.y' fields for a Point object
 to get my x and y value for the "isOpen(x, y)" method.
 
----------------------------------------------------------------------------
