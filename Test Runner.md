---


---

<h1 id="test-runner">Test Runner</h1>
<p>The test runner window is where test cases are run from.<br>
You can launch it by</p>
<ul>
<li>clicking on “Run” next to the test case you want you run in the Test Cases window</li>
<li>selecting a test case in the Test Cases window and clicking on “Run” in the Script editor window</li>
</ul>
<p>The Test Runner window looks like this</p>
<p>The window is split in to two panes (3 in step mode)</p>
<h4 id="test-view">Test View</h4>
<p>On the LHS of the window<br>
Shows a graphical view of the sections that make up the test<br>
Each section consists of</p>
<ul>
<li>a toggle button allowing you to Include or Ignore this section when running the test</li>
<li>the title of the section</li>
<li>the timeout value for the section</li>
<li>whether the section failing aborts the test or not<br>
Underneath this is listed the requirements</li>
<li>Req: is the order of the requirement</li>
<li>Device: is the BNCS device number</li>
<li>Slot: is the range of slots</li>
<li>Values: lists the allowable values.
<ul>
<li>If empty then any is allowable</li>
<li>If {…} then a lambda function is being run</li>
</ul>
</li>
<li>Comments: shows any comments</li>
<li>Order: shows the order the revertive was received in when the test was run</li>
<li>Rev.: shows the value of the revertive received</li>
</ul>
<p>When a requirement is met the appropriate row will turn green.<br>
When an exclusion is met or a requirement met out of order the appropriate row will turn red.<br>
When an optional revertive is received the appropriate row will turn  blue.<br>
Once the section has finished the title row will turn green if the section passed and red if it failed.</p>
<h4 id="log-view">Log View</h4>
<p>On the RHS of the window.<br>
Shows a log of the test case as it is run.</p>
<h4 id="step-view">Step View</h4>
<p>When stepping through a test the Step View pane appears above the other two panes.<br>
It shows the currently running script with the next line to be run highlighted. Pressing the step button on the toolbar will run the highlighted line and move the highlight to the next line.</p>
<h4 id="toolbar">Toolbar</h4>
<p>The toolbar consists of two parts</p>
<h5 id="section-selection">Section Selection</h5>
<ol>
<li>Include all sections in the test case</li>
<li>Only include sections that have previously failed</li>
<li>Don’t include any sections</li>
<li>Invert the selection of which sections to include</li>
</ol>
<h5 id="control">Control</h5>
<ol>
<li>Run the test</li>
<li>Step through the test one script line at a time</li>
<li>Run through the rest of the current section and then pause</li>
<li>Abort the test</li>
<li>Export the logs</li>
</ol>

