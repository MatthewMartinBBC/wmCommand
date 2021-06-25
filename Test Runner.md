
# Test Runner
The test runner window is where test cases are run from.
You can launch it by 
- clicking on "Run" next to the test case you want you run in the Test Cases window
- selecting a test case in the Test Cases window and clicking on "Run" in the Script editor window

The Test Runner window looks like this

The window is split in to two panes (3 in step mode)
#### Test View
On the LHS of the window
Shows a graphical view of the sections that make up the test
Each section consists of
- a toggle button allowing you to Include or Ignore this section when running the test
- the title of the section
- the timeout value for the section
- whether the section failing aborts the test or not
Underneath this is listed the requirements
- Req: is the order of the requirement
- Device: is the BNCS device number
- Slot: is the range of slots
- Values: lists the allowable values. 
  - If empty then any is allowable
  - If {...} then a lambda function is being run
- Comments: shows any comments
- Order: shows the order the revertive was received in when the test was run
- Rev.: shows the value of the revertive received

When a requirement is met the appropriate row will turn green.
When an exclusion is met or a requirement met out of order the appropriate row will turn red.
When an optional revertive is received the appropriate row will turn  blue.
Once the section has finished the title row will turn green if the section passed and red if it failed.

#### Log View
On the RHS of the window.
Shows a log of the test case as it is run.

#### Step View
When stepping through a test the Step View pane appears above the other two panes.
It shows the currently running script with the next line to be run highlighted. Pressing the step button on the toolbar will run the highlighted line and move the highlight to the next line.
 
#### Toolbar
The toolbar consists of two parts
##### Section Selection
1. Include all sections in the test case
2. Only include sections that have previously failed
3. Don't include any sections 
4. Invert the selection of which sections to include
##### Control
1. Run the test
2. Step through the test one script line at a time
3. Run through the rest of the current section and then pause
4. Abort the test
5. Export the logs

