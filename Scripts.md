# Scripts

## Syntax

A wmCommand script consists of one or more command separated by a new line or a  **;**

Individual elements of a command are separated by a space  
Commands are case-insensitive but variables are case sensitive.
```
%value=1  
START  
IW  2  'hello'  %value  
%value++  
LOOP  10
```
The above code

1.  Assigns the number 1 to variable %value
2.  Performs an infowrite to device 2 writing 'hello' to the slot which has the value of %value
3.  Increments %value by 1
4.  Repeats steps 2. and 3. 10 times

String literals should be enclosed in single quotes
```
echo 'this is a string literal'
```
## Comments

A comment in wmCommand is a line that is not executed as part of the script. It's main purpose is to be read by someone looking at the code  
Note in some situations (such as in Test Cases) comments are output to the log.  
All comments are preceded by  **--**. Everything on the line after  **--**  is considered to be part of the comment  
Comments may appear on a line by themselves or at the end of a command line
```
--this is a comment  
RC  200 12 30  --and so is this
```
## Variables

Variables are "containers" for storing information.  
wmCommand understands two types of variable:

-   **Number**  - any real number.
-   **String**  - any text.

### Number

Number variables can have either  local  or  global  scope

Local  
A variable with local scope is only valid in the script that is running.  
Local number variables start with a  **%**  followed by the name of the variable
```
%local=3.5
```
Global  
A variable with global scope is valid in all the scripts contained in the script file. If the variable is changed in one script then it will have value when referred to in another script.  
It is also possible to create/edit and delete global variable from the  global variables window.
*  Global number variables start with a  **%**  followed by the name of the variable and end with a **_**
```
%global_=1.232
```
#### Number Manipulation

wmCommand contains a full featured calculator to allow number manipulation. The general syntax is
`%variable=expression` 
**Note that there must be no spaces in the line**

There are the simple mathematical operators

-   **%value=expression+expression**
-   **%value=expression-expression**
-   **%value=expression*expression**
-   **%value=expression/expression**

There are also a number of operational shortcuts

-   **%value++**  - this is the same as **%value=%value+1**
-   **%value--**  - this is the same as  **%value=%value-1**
-   **%value+=expression**  - this is the same as  **%value=%value+expression**
-   **%value-=expression**  - this is the same as  **%value=%value-expression**
-   **%value/=expression**  - this is the same as  **%value=%value/expression**
-   **%value*=expression**  - this is the same as  **%value=%value*expression**

There are also the following advanced functions

-   **sin(expression)**  - returns the sine of the expression within brackets
-   **cos(expression)**  - returns the cosine of the expression within brackets
-   **tan(expression)**  - returns the tan of the expression within brackets
-   **sinh(expression)**  - returns the sinh of the expression within brackets
-   **cosh(expression)**  - returns the cosh of the expression within brackets
-   **tanh(expression)**  - returns the tanh of the expression within brackets
-   **abs(expression)**  - return the absolute (non-negative) value of the expression within the brackets
-   **acos(expression)**  - returns the acos of the expression within brackets
-   **asin(expression)**  - returns the asine of the expression within brackets
-   **atan(expression)**  - returns the atan of the expression within brackets
-   **sinh(expression)**  - returns the asinh of the expression within brackets
-   **acosh(expression)**  - returns the atanh of the expression within brackets
-   **tanh(expression)**  - returns the sine of the expression within brackets
-   **ceil(expression)**  - returns the smallest possible integer equal or greater than the expression
-   **floor(expression)**  - returns the largest possible integer equal or less than the expression
-   **ln(expression)**  - returns the natural log of the expression
-   **lg(expression)**  - returns the log base 10 of the expression
-   **exp(expression)**  - returns e to the power of the expression
-   **sqrt(expression)**  - returns the square root of the expression
-   **int(expression)**  - returns the nearest integer to the xpression
-   **variable^expression**  - returns the variable raised to the power expression
-   **variable,expression**  - returns the remainder left when expression is divided by the variable (modulo)

Note: an expression can be a number, variable or one of the functions above
e.g.
```
%value=int(sqrt(2))
```

### String

String variables have can have local and global scope. 

Local  
A variable with local scope is only valid in the script that is running.  
Local string variables start with a  **$**  followed by the name of the variable
```
$local='I am local'
```
Global  
A variable with global scope is valid in all the scripts contained in the script file. If the variable is changed in one script then it will have value when referred to in another script.  
It is also possible to create/edit and delete global variable from the  global variables window.
*  Global string variables start with a  **$**  followed by the name of the variable and end with a **_**
```
$global_='I am global'
```

#### String Manipulation

There are a number of powerful string manipulation functions. The general syntax is
```
$string=f1().f2()...f(n)
```
The functions are run from left to right.  
The following functions are available

-   **upper()**  - makes the string uppercase
-   **lower()**  - makes the string lowercase
-   **cap()**  - makes the first letter of each word in the string uppercase
-   **sub(nFirst,nCount)**  - return substring starting at  nFirst  of length  nCount  (or till the end)
-   **length()**  - returns the number of characters in the string (as a string)
-   **bf('char')**  - return the string up to the first occurance of 'char'
-   **af('char')**  - return the string after the first occurance of 'char'
-   **bl('char')**  - return the string up to the last occurance of 'char'
-   **al('char')**  - return the string after the last occurance of 'char'
-   **number()**  - return a string containing only digits that occur in the proceeding expression
-   **number(stop)**  - return a string containing only digits that occur in the proceeding expression and stops as soon as it finds a non-digit
-   **letternumber()**  - return a string containing only letters and digits that occur in the proceeding expression
-   **letternumber(stop)**  - return a string containing only letters and digits that occur in the proceeding expression and stops as soon as it finds a non-digit/non-letter
-   **letter()**  - return a string containing only letters that occur in the proceeding expression
-   **letter(stop)**  - return a string containing only letters that occur in the proceeding expression and stops as soon as it finds a non-letter
-   **notnumber()**  - does the opposite of number()
-   **notletternumber()**  - does the opposite of letternumber()
-   **notletter()**  - does the opposite of letter()
-   **replace('needle','with')**  - replaces all occurances of 'needle' with 'with'

## Internal Commands

### Output Text

There are two commands that can be used to output text

`ECHO some text ` will output the text to the console window.

`LOG  some text` will output the text to the log window.

It is possible to make every command appear in the console window using the following `ECHO  on` and turn this off using `ECHO  off`

### Timing

There are two commands that can be used to alter the speed the script runs at

`SLEEP  x` will make the script pause for _x_ milliseconds before it continues to the next line

`INTERVAL  x` will make the script pause for _x_ milliseconds before it parses every subsequent line.

### Conditionals

A conditional statement is a comparison between two values. If the statement is true the statement returns a 1 if false then the statement returns a 0  
The following statements are available

-   **==**  - returns true if the two sides of the statement are the same
-   **<**  - returns true if the left side is less than the right side
-   **<=**  - returns true if the left side is less than or the same as the right side
-   **>**  - returns true if the left side is greater than the right side
-   **>=**  - returns true if the left side is greater than or the same as the right side
-   **!=**  - returns true if the two sides of the statement are the different

You can use the commands  `TEST`, `ELSE`  and  `TSET`  to run different blocks of code depending on the result of a conditional statement. The format is as follows
```
TEST  conditional  
code to run if conditional is true  
ELSE  
code to run if conditional is false  
TSET
```
### Loops

Loops are used to execute the same block of code again and again.  
In wmCommand a loop block always begins with
`START` and ends with one of the two commands 
- `LOOP  n`  will execute the loop block n times. Note n can be a number variable.
-  `LOOP_IF condition`  will execute the loop whilst the condition is true.

The following two examples will produce the same result, looping 10 times a printing out the numbers from 1 to 10
```
%value=1  
START  
ECHO  %value  
%value++  
LOOP  10  
```
  ```
%value=1  
START  
ECHO  %value  
%value++  
LOOP_IF  %value<=10
```
### File Commands

wmCommand has a number of commands to read and write text files

Reading a file  
To read a line from a file and store the contents in a string variable use the `READ`  command  
This has the format  _Filename LineToRead StringVariable_
```
READ  test.txt %line $var
```
Writing to a file  
To append a line to the end of a file use the `WRITE`  command  
This has the format  _Filename 'TextToWrite'_  
```
WRITE  test.txt  'some text to append'
```
Deleting a file  
To delete a file use the  `DELETE`  command  
This has the format  _Filename_  
```
DELETE  test.txt
```
Counting the number of lines in a file  
To count the number of lines in a file and store the result in a number variable use the  `COUNT`  command  
This has the format  _Filename NumberVariable_
```
COUNT  test.txt %NumberOfLines
```
_e.g. to read every line of a file in to a variable_
```
COUNT  file.txt %lineCount  
%line=0  
START  
READ  file.txt %line $var  
--do something with the $var  
%line++  
LOOP  %lineCount  
```
### Calling Other Scripts

It is possible to call other scripts within the same script file to run from within a script.  
You can do simply using the name of the other script `MyOtherScript` or by using the  `RUN`  command. `RUN  MyOtherScript` **Note that if you do not use the  RUN  command then the script you call must not have a name the same as any of the internal commands.** 
e.g. `RUN  echo` will run a script called  **echo**  whilst `echo` will print a blank line to the console

#### Passing variables in to scripts

In some circumstances you might wish to pass one or more variables in to a script that you are calling. In order to do this simply add the variables to the end of script call separated by a white space in the form number `Var1=number stringVar1='text'`.  
e.g.
```
RUN  MyOtherScript input='some text',value=3
```
in the script **$input** will equal 'some text' and **%value** will equal 3.

#### Returning values from a script

You can return a value from a script using the  `RETURN`   command. 
e.g.
```
RETURN  66
```
It is possible to assign the return value of a script directly to another variable using the following format `$var=RUN{MyOtherScript}`

### wmCommand manipulation commands

There are a few commands which are there to manipulate wmCommand

-   `CONNECT` - attempts to connect to CSI
-   `CLL` - clears the log window
-  `CLC` - clears the console
- `CLS` - clears the log and console
-  `SCROLL  on/Off`- enables/disables automatic scrolling on the log window
-  `ZERO`  - zeros the revertive counter
-   `HELP`  - lists all the possible commands to the log window
-   `HELP command`  - outputs the help for the command to the log window

### Lambda scripts

A lambda script is an unnamed script that can be embedded in a normal script. When wmCommand finds a lambda script it pauses the main script, runs the lambda script and replaces it with the return value of the script, it then continues with the main script.  
A lambda script is enclosed in  `{}`  and can span multiple lines  
It is possible to pass variables in to the lambda script by enclosing them in  ()
```
%value=4  
$result={  
TEST  %value<10  
RETURN  'less than 10'  
ELSE  
RETURN  '10 or more'  
TSET  
}(%value)
echo value is $result
```
You can replace most terms of a command with a lambda script. They are most useful though with BNCS registrations and snapshots. 
