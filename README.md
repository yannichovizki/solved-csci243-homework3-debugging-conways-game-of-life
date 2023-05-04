Download Link: https://assignmentchef.com/product/solved-csci243-homework3-debugging-conways-game-of-life
<br>



This assignment is a practice/tutorial on:

Using the git version control system to track stages of code repair and improvement;

Debugging with gdb;

Studying existing code not written by you;

Fixing C language syntax;

Refactoring code to have better structure; and

Redesigning code to produce different, <em>cursor-controlled output</em>.

<h2>gdb Resources</h2>

This assignment leads you through the use of gdb and its commands step by step, and you should be able to finish this by <em>reading and doing</em>.

For those who may be interested in learning more, there are a number of online tutorials covering use of gdb to debug programs; some are lightweight, and some are comprehensive. See the <a href="http://www.cs.rit.edu/~csci243/resources.html">course resources pa</a><a href="http://www.cs.rit.edu/~csci243/resources.html">g</a><a href="http://www.cs.rit.edu/~csci243/resources.html">e</a> for links to several of them. You can also get help on using gdb commands while running gdb itself, through its built-in help command. For example, the following shows the help command issued at the gdb prompt:

(gdb) help

That will display a long menu of command information. For help with an individual gdb command, just add the command name to the help command; for instance,

(gdb) help where prints a description of how the where command works.

<h2>Style, Documentation and Version Control</h2>

The code we supply to you has very poor style and documentation, and no version control. As you work on this assignment, you will revise the style and add documentation while simultaneously using a version control system to record your evolution of the code.

Your solution should follow the course’s style conventions for layout of functions, placement of braces{}, and code blocks.

At the end, your solution must have proper function documentation detailing the purpose of the function, its input parameters, and its output.

The documentation should have structure that allows documentation generation using a tool such as

<a href="http://www.doxygen.org/">Doxy</a><a href="http://www.doxygen.org/">g</a><a href="http://www.doxygen.org/">en</a>. That means use of /// … or /** … */ style comments for text that you want published. (There are a number of online tutorials that cover <a href="https://justcheckingonall.wordpress.com/2008/07/20/simple-doxygen-guide/">basic</a> <a href="https://justcheckingonall.wordpress.com/2008/07/29/simple-doxygen-templates/">use of Doxy</a><a href="https://justcheckingonall.wordpress.com/2008/07/29/simple-doxygen-templates/">g</a><a href="https://justcheckingonall.wordpress.com/2008/07/29/simple-doxygen-templates/">en</a>, as well as an <a href="https://www.stack.nl/~dimitri/doxygen/manual/">online manual</a> covering full details of the program.)

For version control, you will use the git version control system (VCS). Instructions for using this from the command line are included in the steps you will follow.

<h1>START HERE: Supplied Code (git, then get)</h1>

We supply a naïve, buggy implementation that fails to compile cleanly without errors or warnings; it also does not run properly. You will clean up the problems with the code using gcc flags and the gdb debugger.

Navigate to the <em>parent directory</em> for your assignment location, and use a command to create a <em>git repository </em>named for this assignment; you could call the folder hw3 as shown or something like game-of-life. The command will <em>create a new directory</em> that is set up for git to manage.

git init hw3

Then cd hw3 and issue this get command to get the supplied materials:

get csci243 hw3

The get command creates the directories (folders) act1, act2, and act3 and places several files into them. You should work in these sub-directories for each separate activity described below.

Here is what get fetches:

The file act1/bad-life_c is a faulty version of code for <a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">Conwa</a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">y</a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">‘s Game of Life</a>. This file is the starting point for all the code in this assignment. (The file is named act1/bad-life_c just so you don’t compile it by accident!)

The act1/Makefile file will compile and link the code for debugging. Do not change this Makefile.

The act2/header.mak file will be used by gmakemake to create a Makefile for activity 2.

The act3/header.mak file will be used by gmakemake to create a Makefile for activity 3.

We also supply a module that controls the cursor position so that it can overwrite output on arbitrary lines of a standard terminal window. The display module source act3/display.c, and its act3/display.h header file have functions to pre-position the cursor before outputting a character.

This provides cursor control capability.

<h1>Instructions: Follow These Steps <em>Carefully</em></h1>

The assignment leads you through the use of gdb showing the major commands that will help you learn how to apply a debugger to solve coding problems you will encounter later in the course.

You will start with the supplied bad-life_c, a buggy version of Conway’s game of life which crashes.

One thing you will need to do several times is to copy and paste the output from gdb into a text file for submission. An easy way to do this is to use two shell windows:

In the first window, you will be doing the work for the various activities; this is where the copied text originates.

In the second window, you will be creating the text files containing the pasted output from gdb.

<strong>Note:</strong> Be sure that you have used cd in <em>both</em> shell windows to change directory into the appropriate activity directory, or your text files may go somewhere you don’t expect to see them!

The easiest way to create a file containing copied text on a UNIX<u><sup>®</sup></u> or Linux<u><sup>®</sup></u> system is using the cat command. For instance, to create the debug1.txt file for the first activity, run the command cat &gt; debug1.txt

in the second shell window to begin creating the file. In the first shell window, use the mouse to highlight the text you want to copy, then paste it into the second shell window. This text will all be added to the debug1.txt file. Afterwards, press the Enter key, followed by a CONTROL-D key combination (which tells the system that you’re done entering text).

The following are the activities of this assignment:

Activity 1: Debugging uses gdb to investigate some fatal and not so fatal problems, correct them, and commit the corrections.

Activity 2: Fixing Warnings and Refactoring Code rewrites the code to improves its efficiency, correct poor practices, and correct faulty algorithms, while also adding documentation and better formatting.

Activity 3: Redesigning to use Cursor-controlled Output changes the code’s behavior to use an overlayed display of generations rather than printing a sequence of generations one after the other.

<h2>Activity 1: Debugging with gdb in 2 Phases</h2>

The debugging activity has two phases, cleverly named Phase I and Phase II. It is good practice to run the code without any changes to find out what it does first. The first phase runs gdb and then makes a minimal change just to stop the crashes, and The second runs gdb after making some changes and recompiling.

<strong>While the knowledgable among you may find the problems before debugging and want to repair the problems before using gdb, please do not do so; follow the steps given below.</strong>

<h3>Phase I</h3>

In Phase I, you will create two files, debug1.txt and debug2.txt, to document your gdb investigation of the code problems, fix the first problem found, and commit the changes.

Important note: <em>do not</em> use gmakemake to create a Makefile for this activity – just use the one retrieved by the get command.

<ol>

 <li>Change to the act1 directory that get</li>

 <li>Issue the command cp bad-life_c good-life.c to create your initial source file for the assignment. This is the subject program to debug.</li>

 <li>Issue the command make phase1 to compile and link creating the good-life</li>

 <li>Follow this step carefully. You will need to save (i.e. copy and paste into a file) and submit the outputs of this step.</li>

</ol>

Run the command:

gdb -q good-life and do these things at the (gdb) prompt:

Issue the command run and then enter 123 for the number of organisms at the prompt. The program will crash.

Issue the command where or backtrace to show the program call stack.

Issue the command frame N or up and down commands to move up/down the call stack in order to. go to the function frame in <em>user code</em> that failed. User code is code in the good-life.c file.

Issue the list command to show surrounding lines.

Issue the break to put in a breakpoint at the line where the crash stopped execution in user code.

Issue the command info break to list breakpoint information.

Notice that the breakpoint’s line number is incorrect. The command break put a break at line number 174, but the failing line is 176.

Put a new <em>breakpoint</em> at line 176.

To delete the incorrect break, delete breakpoint 1

Then check with info break that the breakpoint is in the correct place, and there is only one.

<ol start="5">

 <li>Copy and paste the lines of text beginning at the gdb command and continuing through the backtrace, frame/up, list, break, and info break commands into the file <strong>txt</strong>. You will submit this file at the end of activity 1.</li>

 <li>Follow this step carefully. You will need to save (i.e. copy and paste into another file) and submit the outputs of this step also.</li>

</ol>

run again; gdb should stop at the breakpoint.

Issue info break to show breakpoint information.

Issue the command display row (display can display only one value at a time.)

Issue the command display col

Issue the command whatis life to find out the variable’s type.

Issue display life[row][col]

Issue run again, and

Issue the next command when you reach the breakpoint.

<ol start="7">

 <li>Copy and paste the text from the last step (i.e. the last run command) through what happened after next) into the file <strong>txt</strong>.</li>

 <li>Search for the same error since it occurs elsewhere in the program. <strong>Append a short description of the cause and source of the problem</strong> to the end of the file <strong>txt</strong>. This description should identify the problem and the line number or numbers at which the problem occurs.</li>

 <li>Issue the quit command to exit from gdb.</li>

 <li>Edit the file and fix the problem you found. If you found other problems, do not change them yet; just document them. (You’ll have opportunity to fix those problems later in the lab.)</li>

 <li>Issue the command make phase1 to re-compile and link.</li>

 <li>Run good-life to test your changes. The program should no longer crash; it now runs forever, but the output will be incorrect. To <em>kill the program</em> you must enter the CONTROL-C key combination. (You may have to enter that a couple times to interrupt and finally terminate the program.)</li>

 <li>Issue the command</li>

</ol>

git add good-life.c

to add the file to the commit set.

<ol start="14">

 <li>Issue the command</li>

</ol>

git commit

to put the repaired code in the repository. You will have to enter a comment for the commit; usually, these comments are short phrases describing the changes made.

The default editor for git is vim on Linux. If you wanted to switch to nano in your bash environment however, you could do this to set the editor:

EDITOR=nano <strong>Phase II</strong>

In Phase II, you investigate the new problem, and document your second investigation in debug3.txt.

<ol>

 <li>Issue the command make phase2 to compile and link again. There should be no warnings from this step.</li>

 <li>Follow this step carefully. You will need to save (i.e. copy and paste into another file) and submit the outputs of this step.</li>

</ol>

Re-run the gdb command, and do the following things:

break 176

run and enter 123 for the number of organisms.

display row display col display life[row][col] continue through 17 breakpoints. You can continue N to do N cycles through a breakpoint.

continue through 4 more breakpoints and observe the program output interspersed with your debugging commands and display output.

<ol start="3">

 <li>Copy-paste the lines from the setting of the first breakpoint to where gdb stopped after continue 4 into the file <strong>txt</strong>.</li>

 <li>Identify the source of the problem and search the code for the same error since it occurs in <em>several places</em>. <strong>Append a short description of the cause and sources of this problem</strong> to the end of the file <strong>txt</strong>. This description should identify the problem and list the line numbers at which the problem occurs.</li>

 <li>Quit gdb, correct those errors you found, re-make phase2 and re-test using gdb as appropriate.</li>

 <li>Issue the command git add good-life.c to add these changes to your commit set.</li>

 <li>Issue the command git commit to save the repaired code to the repository.</li>

 <li>You need to save the version log for your submission because you have finished this activity. Issue the command git log &gt; revisions.txt</li>

</ol>

to save the log to a file called revisions.txt.

You will know you have finished phase II when you can make phase2 and run the program, which now runs forever until you kill it with the CONTROL-C combination. (The code still has algorithmic and other problems, but it does run without crashing.)

<h3>Activity 1 Submission</h3>

Submit your results for this activity with the try command:

try grd-243 hw3-1 debug1.txt debug2.txt debug3.txt revisions.txt

(You submit the revision information but not the code; it’s not ready yet!)

<h2>Activity 2: Fixing Warnings and Refactoring Code</h2>

<h3>Fixing warnings to clean up the code</h3>

First clean up the code based on the compiler’s detailed warning messages.

<ol>

 <li>Change to the act2 directory created by get, and copy the good-life.c file from the previous activity into act2. This code should compile (with warnings) and run without crashing.</li>

 <li>Issue the command gmakemake &gt; Makefile to create a Makefile for use by make for this activity.</li>

 <li>Issue the command make to compile and link. You will see quite a few warning messages that it is time to fix.</li>

 <li>Fix the warnings about “statement with no effect”.</li>

 <li>Also fix the warnings about “comparison with string literal”.</li>

 <li>Lastly fix the “unused…” warnings in this activity.</li>

 <li>Now modify the while loop to make the loop end after 100 generations.</li>

 <li>Issue the command make to re-compile and link. Find and fix any errors and warnings, editing and making until the program no longer produces any compile and link warnings.</li>

 <li>As the code now compiles and links cleanly with no warnings, it is time to issue the commands git</li>

</ol>

add good-life.c and git commit to save the cleaned-up code to the repository.

At this point of activity 2, the program should run and produce outputs of ‘*’ and ‘ ‘(space) characters, but it works incorrectly due to coding design errors.

<ol start="10">

 <li>Run the program like this to save output to a file for study:</li>

</ol>

echo 321 | ./good-life &gt; output.txt

<ol start="11">

 <li>Open txt and study several of its cases. Observe that the program does not implement Conway’s rules correctly.</li>

</ol>

One way to investigate the flaws in the algorithm is to look at sequential generations where there are several neighboring organisms. For example, the rules state that too many neighbors cause an organism to die, but the program does not correctly make them die.

Now it is time to redesign the code.

<strong>Background: What is Refactoring?</strong>

As you work on an implementation and get it running, you should consider how you can <em>refactor</em> the code to reduce the amount of repetition of similar code segments. This turns repeated code into proper functions that can be called and reused when appropriate.

What this means is that you find places where the same code is repeated; pull that code out and convert the code into a function which you then call at the places where the duplicate code was originally found. When you refactor code, you have to identify and define the parameters and types which the new function must have to be able to serve the needs where the new function will be called.

Refactoring also reorganizes code for greater execution efficiency, space consumption, or both.

<h3>Refactoring to fix the algorithm</h3>

Your refactoring will fix the algorithm and the matrix-passing syntax, and finish reformatting and documenting the code along the way.

<ol>

 <li>Implement Conway’s rules correctly.</li>

</ol>

Search online to learn the rules, and implement them properly. As you repair the implementation, refactor the code to reduce the amount of repetition of similar code. Turn repeated code into proper functions that can be called when appropriate.

Consider merging functions that have a lot of duplication. Notice that the three …Rule() functions are very similar and are called sequentially in the while loop. These operations are not sequential and should be combined into one function that handles the cases of birth, death and survival. You can also factor out the neighbor counting into a function to simplify the rule computation. (See the <em><u>Redesi</u></em><em>g<u>nin</u>g <u>the nei</u>g<u>hbors</u></em> section below.)

<ol start="2">

 <li>As you refactor, document the functions you revise and the ones you create.</li>

 <li>Reformat the code for readability. Use either <em>space</em> (‘ ‘) or <em>TAB</em> (‘t’) indentation characters consistently and do not mix them.</li>

 <li>Declare parameters properly for passing a matrix. The compiler needs to know the number of columns in the matrix passed into the function; this means that you must specify the size of the second dimension, because the compiler is processing the parameter list left-to-right. This example shows the proper way to declare a matrix parameter for a function in C:</li>

</ol>

void foo( int size, char matrix[][size] ) { … }

You will need to change your revised rule function to have proper parameter declarations, and update the calling code so that it calls the function properly.

As part of this refactoring, you should replace all the <em>magic numbers</em> with appropriate symbols so that it is easy to change the grid size by changing one symbol’s value. For example, 19 and 20 appear in <em>magically</em> in many places and should be replaced by a well-named symbol.

If the matrix were non-square, you would need a rowsize and a columnsize parameter to specify the matrix dimensions before the matrix parameter itself.

<ol start="5">

 <li>Remake (compile and relink using make) and be sure there are no errors or warnings.</li>

 <li>Add the file to the repository change list using git add good-life.c and issue git commit to save the code to the repository.</li>

 <li>Test the code to see that the game rule implementation is correct.</li>

</ol>

(You could temporarily change the size of the grid and the number of cycles the code runs to make testing easier. Be sure however, to change that back to the required size 20 and 100 cycles before you submit.)

Continue your refactoring <em>edit, recompile and retest cycle</em> until the code properly implements the algorithm.

<ol start="8">

 <li>After the refactoring is finished, tested, documented, and version controlled (i.e. committed), issue the command git log &gt;revisions.txt to save the version log. (This would overwrite any previous txt file you may have created in the current folder, but that’s ok.)</li>

 <li>It is now time to submit this activity. <strong>Details: Redesigning the neighbors</strong></li>

</ol>

The supplied code calculates the neighbors over and over, and it is incorrect at the boundaries of the life grid. What’s needed is a common way to count neighbors of an arbitrary grid location, <em>and wrap around at the horizontal and vertical margins</em>. That way the game of life can have its life forms flow around from bottom to top and left to right and vice versa.

Given a location with (row, col) indices, you calculate their neighboring indices using row +/- 1 and col +/- 1 to get the 8 neighbors’ locations. For example, if the grid location is (row 3, col 0), then the neighbors on the row above there are:

(row 2, col -1) (row 2, col 0) and (row 2, col 1)

Clearly the negative index will go out of bounds, and this needs to wrap around to:

(row 2, col 19) (row 2, col 0) and (row 2, col 1)

because the size of the grid is 20 X 20.

Whenever a neighbor’s row index is -1 or 20 (the width of the life grid), you know that the neighbor is on the top or bottom of the grid. In the case that the row is -1, then you can adjust the value to be 19, and when row is 20, you adjust it to 0 to get the row index wrapped around. The situation works similarly for the column index of the life grid.

You should factor this logic into a function that can compute the neighbors and count whether they have a * and are organisms or a (space) indicating no organism.

<h3>Activity 2 Submission</h3>

Submit your results for this activity with this try command:

try grd-243 hw3-2 good-life.c revisions.txt [readme.txt]

The program must run without crashing and print 100 generations of the game of life. The initial generation counts as one generation, numbered as generation 0.

The readme.txt is an optional file you can submit to provide additional comments.

<h2>Activity 3: Redesigning to use Cursor-controlled Output</h2>

Change to the act3 directory that get created, and copy the good-life.c file from your previous activity directory to the act3 directory.

Instead of printing each generation after the previous one, you now must use the supplied display.c code to control the cursor’s position on the terminal and overwrite the next generation on top of the previous generation.

<ol>

 <li>Use cursor functions to make each generation overwrite previous ones.</li>

</ol>

Change the code so that it uses the supplied display.c code to position the cursor so that it <em>overwrites the output in the same place on the screen for each generation of the game</em>. The display of each generation:

Draws the game board starting at the upper left of the terminal window.

Prints the count of the generation displayed on the line under the last line of the game board.

(This is already done in the supplied code, but you might want to revise it.)

<ol start="2">

 <li>Revise the code to make the generating loop into an infinite loop that uses cursor control functions (see h) to overwrite each generation on top of the previous generation.</li>

</ol>

The only way to terminate a program with an infinite loop will be by an interrupt (CONTROL-C) or a kill command (see the man kill page for information).

<ol start="3">

 <li>Introduce a delay between displaying each generation.</li>

</ol>

Because the program is now running with an infinite loop, the program’s loop needs to have a <em>delay </em>between the display of each displayed generation of the game. The usleep function can provide this delay; here is a suitable delay:

usleep( 81000 );

The system header file unistd.h declares this function; however, to get that declaration, you must add

#define _BSD_SOURCE

<em>at the beginning of your source file </em>before any of your other #include statements.

<ol start="4">

 <li>Find and fix any remaining warnings.</li>

 <li>Add proper, publishable function documentation to the code. You should use either the /// … or the /** … */</li>

 <li>Validate your implementation of Conway’s rules in final testing.</li>

 <li>Add the file to the repository change list using git add good-life.c and issue git commit to save the code to the repository.</li>

 <li>After testing and documenting are finished, issue the command git log &gt; revisions.txt to save the version log.</li>

</ol>

<h3>Notes on Cursor Control and display.c for Activity 3</h3>

The display code provides functions to clear the terminal window, position the cursor at a specified row and column, and put a character to the terminal. The module addresses the terminal as a matrix of characters. While the column values are 0-based indices, the row values are 1-based. That means index of the first row of the screen is ‘1’, not ‘0’, but the index of the first column of the screen is ‘0’.

The set_cur_pos function will position the cursor to the location specified by the arguments. The put function will output a single character at the location of the cursor, <em>and move the cursor one position to the right as a side effect</em>.

Note that the program must <strong>clear the terminal window screen only once, after it receives the number of organisms</strong>. The supplied module source display.c file provides that functionality. It is a design error to clear the terminal within the loop because that defeats the purpose of the cursor-controlled output.

With cursor control in place, here is a sample display of the expected output of one generation: <a href="https://www.cs.rit.edu/~csci243/protected/homeworks/03/sample-life.txt">sample-life.txt</a>

.

<h3>Extra Credit Options for Activity 3</h3>

The following are optional enhancements to the solution. Each is worth 3%; do all successfully and earn 9% extra credit. (It is possible to get a score greater than 100% as a result.)

<em>You must supply the </em><em>readme.txt file to document your extra features</em> if you wish to get full credit for the extra work.

<ol>

 <li>Override the size of the grid if the user provides a size as a command line argument. For example, entering this command: good-life 27 would change the game board size from the default <em>20 by 20</em> to <em>27 by 27</em> Note: this is an <strong>override</strong>; the program must also run using the default value with no supplied command line arguments.</li>

 <li><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">Write code to produce one or more </a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life"><em>oscillators</em></a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">, </a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life"><em>gliders</em></a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life"> or </a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life"><em>spaceships</em></a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life"> as discussed in </a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">Conwa</a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">y</a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">‘s Game of Life</a><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">.</a></li>

 <li>Add code to produce a random birth of a cell.</li>

</ol>

<h3>Activity 3 Submission</h3>

Submit your results for this activity with this try command:

try grd-243 hw3-3 good-life.c revisions.txt [readme.txt]

Remember to supply the readme.txt if you did the extra credit.