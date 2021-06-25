# Test Case

Allows easy repeatable semi-automatic testing of BNCS applications
Consists of one or more "sections"
Each section consists of one or more of the following
-   a script
-   a list of required revertives to receive (requirements)
-   a list of revertives that must not be received (exclusions)
-   a list of revertives that can optionally be received (optionals)
-   a list of requirements that require manual acknowledgement (manuals)

When a test case is run it proceeds through each section

Each section registers for any revertives defined in the lists of requirements, exclusions and optionals

The section's script is then run

As revertives are received the requirements etc are marked as being met or not

At the end of a specified time period the section is said to have passed if

-   all requirement revertives are recieved in the correct order
-   no exclusion revertives were received.

The test case then moves on to the next section unless the previous one failed and was marked as stop the test if it fails.

A test case must start with one of the four following commands (each defining a particular section type)

-   SECTION
-   LAUNCH
-   PRECONDITION
-   EXTEND

All subsequent lines (until the next of these commands) are counted as being part of that section and treated accordingly.

## SECTION

This is the default test case section. It is where you define a script to run and the revertives that must be received in order for the section to pass.

This has the following format
```
SECTION [ name of section ] args
```
both the name of section and args are optional

If no name is given then one is assigned to the section based on its position in the test case

**args** has the format

variableName1=value1 variableName2=value2 .... variableNameN=valueN

where

valueX is either a number, a string of the format 'some string' or a global variable

### SECTION Commands

As well as all the allowable scripting commands a SECTION may have the following special test case commands. On parsing the section lines starting with the command below are dealt with first, then all other lines are assumed to be part of the script that should be run.

#### TIMEOUT

Sets the amount of time in milliseconds the section should wait for before checking if it has passed or not. The timer does not start until the section's script has finished.

If this command is excluded then the default time of 1000ms is used

The format is
```
TIMEOUT x
```
Where **x** is the number of milliseconds to wait

#### EXIT

Sets whether the test case should stop running if this section fails.

If the command is excluded then the test will keep on running even if the section fails

The format is
```
EXIT
```
#### REQUIRE

Defines a revertive that must be received in order for the section to pass.

The format is
```
REQUIRE order device range value1 value2 … valueN comments
```
##### order

Defines the order that the revertive must be received.

More than one requirement may have the same order number (in which case no distinction is met)

If a requirement is met out of order (i.e. before a requirement of a lower order is met) then the section fails on “revertive order”

##### device

Defines the BNCS device that the revertive should come from.

This may either be

-  a number between 1 and 999
-  a global number variable that has a value between 1 and 999
-  a number variable passed in to the section via args that has a value between 1 and 999

**Note** – if a variable is used then the value is calculated at the start of the section before the section’s script is run.

##### range

Defines a range of slots that the revertive may come from

This may either be

- a number between 1 and 4096
-  a comma separated string where each entry is either
    -   a number between 1 and 4096
    -   two numbers between 1 and 4096 separated by a **–**
-  a global number variable that has a value between 1 and 4096
-  a number variable passed in to the section via args that has a value between 1 and 4096
-  a global string variable that has a value in the same format as option 2.
-  a string variable passed in to the section via args that has a value value in the same format as option 2
```
e.g

1-4,7-10

Would mean the revertive could come from any of the slots 1,2,3,4,7,8,9 or 10
```
##### value1 value2 … valueN

Defines a list of possible values that the revertive must contain for the requirement to be met. If the requirement contains no values then any revertive from the given device in the given range will meet the requirement.

Each value may be

- A string surrounded by `‘` (whether the value is actually a string or a number) `e.g. '12' or 'testing'`
- A global variable
- A variable passed in to the section via args
- A lambda function (a script surrounded by {})

If the value is a lamba function then the following two variables are passed in to the lambda function

-   `$data` the revertive value as a string
-   `%value` the revertive value as a number (or 0 if it is not a number)
-   `%index` the revertive slot/destination/gpi number

The lambda function must return

-   a **1** to say the requirement was met
-   a **0** to say the requirement was not met

##### comments

Comments must start with two dashes `--`. Anything after is treated as the comment. It is sensible to put a comment for each requirement stating what the requirement is.

#### EXCLUDE

Defines a revertive that must not be received in order for the section to pass.

The format is
```
EXCLUDE device range value1 value2 … valueN comments
```
See REQUIRE for information on the command format

#### OPTIONAL

Defines a revertive that we want to log being received or not, but that doesn’t affect whether the section passes.

The format is
```
OPTIONAL device range comments
```
See REQUIRE for information on the command format

#### MANUAL

Defines a test that requires user interaction. When a SECTION is run containing a manual requirement the section timeout is ignored. Once the script has been run a dialog box containing the text of the manual requirement appears and the test is paused until the user presses the “Pass” or “Fail” button on the dialog box. One dialog box will appear for each manual requirement.

The format is
```
MANUAL text
```
## LAUNCH

This section tells the test runner what applications need to be running in order to run the test. It is usually one of the first sections in the test case but can actually appear anywhere.

The format is
```
LAUNCH [name of section]
```
name of section is optional.

Every line in the launch section is assumed to be an application (and command line arguments) to check if running and if not launch.

The format of the lines should be
```
PathToApplication args {timeout}
```
PathToApplication can either be the full path to the application or a relative path. If relative then it is assumed relative to **CC_SYSTEM/windows/drivers** on a v4.x machine and **bncs_root/modules** on a v3 machine. If there are spaces in the path then the whole path must be enclosed within ‘

**args** is optional and is the command line arguments to launch the application with

**timeout** is the number of seconds to wait once the application has been launched. If not included then the wait time defaults to 5 seconds.
```
e.g

C:/developer/automatics/autototest.exe 33 {10}

wmCommand will

- Check if autotest.exe is running with command line argument 33.
- If so then it will move on to the next application to run (or next section).
- If not then it will start autotest.exe with command line argument 33
- Check if autotest.exe is running with command line argument 33.
- If so then it will wait 10 seconds and then move on to the next application to run (or next section). If not it will wait a second and repeat the check. After 10 checks it will fail the section
```
If PathToApplication  points to the wmSimulator application then wmCommand will first check if wmSimulator is running. If it is then it will use wmSimulator’s remote API to ask it to load the dataset defined in args (or the default dataset if args is empty). If wmSimulator is not running then it will start it like any other application

## PRECONDITION

This section allows you to list anything that the user needs to do before running the test.

Anything that is written in this section will appear in a dialog box that the user must acknowledge before the test runner moves on to the next section.
```
e.g.

PRECONDITION

Make sure dev_300.db0 is updated to the latest version
```
## EXTEND

The Extend section allows you to rerun a previously defined section, possibly extending it by adding to the script or requirements.

The format is
```
EXTEND [name of previously defined section] args
```
The name is not optional. If no section is found with the same name then this section is ignored.

**args** is optional and has the same format as in the SECTION command.

The EXTEND section can have all the same commands as a default SECTION

When the test runner comes across an EXTEND section it finds the previously defined section that the EXTEND section refers to and creates a new section based on it. It then appends any extra scripting or requirements defined in the EXTEND section and runs the EXTEND section just like a default section.
```
e.g.

SECTION [setup]
    %index=1
    START
        IW 2 ‘0’ %index
        %index++
    LOOP 10

SECTION [test]
    …

EXTEND [setup]

SECTION [test2]
    ….
```
