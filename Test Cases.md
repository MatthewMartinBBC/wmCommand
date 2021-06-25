---


---

<h1 id="test-case">Test Case</h1>
<p>Allows easy repeatable semi-automatic testing of BNCS applications<br>
Consists of one or more “sections”<br>
Each section consists of one or more of the following</p>
<ul>
<li>a script</li>
<li>a list of required revertives to receive (requirements)</li>
<li>a list of revertives that must not be received (exclusions)</li>
<li>a list of revertives that can optionally be received (optionals)</li>
<li>a list of requirements that require manual acknowledgement (manuals)</li>
</ul>
<p>When a test case is run it proceeds through each section</p>
<p>Each section registers for any revertives defined in the lists of requirements, exclusions and optionals</p>
<p>The section’s script is then run</p>
<p>As revertives are received the requirements etc are marked as being met or not</p>
<p>At the end of a specified time period the section is said to have passed if</p>
<ul>
<li>all requirement revertives are recieved in the correct order</li>
<li>no exclusion revertives were received.</li>
</ul>
<p>The test case then moves on to the next section unless the previous one failed and was marked as stop the test if it fails.</p>
<p>A test case must start with one of the four following commands (each defining a particular section type)</p>
<ul>
<li>SECTION</li>
<li>LAUNCH</li>
<li>PRECONDITION</li>
<li>EXTEND</li>
</ul>
<p>All subsequent lines (until the next of these commands) are counted as being part of that section and treated accordingly.</p>
<h2 id="section">SECTION</h2>
<p>This is the default test case section. It is where you define a script to run and the revertives that must be received in order for the section to pass.</p>
<p>This has the following format</p>
<pre><code>SECTION [ name of section ] args
</code></pre>
<p>both the name of section and args are optional</p>
<p>If no name is given then one is assigned to the section based on its position in the test case</p>
<p><strong>args</strong> has the format</p>
<p>variableName1=value1 variableName2=value2 … variableNameN=valueN</p>
<p>where</p>
<p>valueX is either a number, a string of the format ‘some string’ or a global variable</p>
<h3 id="section-commands">SECTION Commands</h3>
<p>As well as all the allowable scripting commands a SECTION may have the following special test case commands. On parsing the section lines starting with the command below are dealt with first, then all other lines are assumed to be part of the script that should be run.</p>
<h4 id="timeout">TIMEOUT</h4>
<p>Sets the amount of time in milliseconds the section should wait for before checking if it has passed or not. The timer does not start until the section’s script has finished.</p>
<p>If this command is excluded then the default time of 1000ms is used</p>
<p>The format is</p>
<pre><code>TIMEOUT x
</code></pre>
<p>Where <strong>x</strong> is the number of milliseconds to wait</p>
<h4 id="exit">EXIT</h4>
<p>Sets whether the test case should stop running if this section fails.</p>
<p>If the command is excluded then the test will keep on running even if the section fails</p>
<p>The format is</p>
<pre><code>EXIT
</code></pre>
<h4 id="require">REQUIRE</h4>
<p>Defines a revertive that must be received in order for the section to pass.</p>
<p>The format is</p>
<pre><code>REQUIRE order device range value1 value2 … valueN comments
</code></pre>
<h5 id="order">order</h5>
<p>Defines the order that the revertive must be received.</p>
<p>More than one requirement may have the same order number (in which case no distinction is met)</p>
<p>If a requirement is met out of order (i.e. before a requirement of a lower order is met) then the section fails on “revertive order”</p>
<h5 id="device">device</h5>
<p>Defines the BNCS device that the revertive should come from.</p>
<p>This may either be</p>
<ul>
<li>a number between 1 and 999</li>
<li>a global number variable that has a value between 1 and 999</li>
<li>a number variable passed in to the section via args that has a value between 1 and 999</li>
</ul>
<p><strong>Note</strong> – if a variable is used then the value is calculated at the start of the section before the section’s script is run.</p>
<h5 id="range">range</h5>
<p>Defines a range of slots that the revertive may come from</p>
<p>This may either be</p>
<ul>
<li>a number between 1 and 4096</li>
<li>a comma separated string where each entry is either
<ul>
<li>a number between 1 and 4096</li>
<li>two numbers between 1 and 4096 separated by a <strong>–</strong></li>
</ul>
</li>
<li>a global number variable that has a value between 1 and 4096</li>
<li>a number variable passed in to the section via args that has a value between 1 and 4096</li>
<li>a global string variable that has a value in the same format as option 2.</li>
<li>a string variable passed in to the section via args that has a value value in the same format as option 2</li>
</ul>
<pre><code>e.g

1-4,7-10

Would mean the revertive could come from any of the slots 1,2,3,4,7,8,9 or 10
</code></pre>
<h5 id="value1-value2-…-valuen">value1 value2 … valueN</h5>
<p>Defines a list of possible values that the revertive must contain for the requirement to be met. If the requirement contains no values then any revertive from the given device in the given range will meet the requirement.</p>
<p>Each value may be</p>
<ul>
<li>A string surrounded by <code>‘</code> (whether the value is actually a string or a number) <code>e.g. '12' or 'testing'</code></li>
<li>A global variable</li>
<li>A variable passed in to the section via args</li>
<li>A lambda function (a script surrounded by {})</li>
</ul>
<p>If the value is a lamba function then the following two variables are passed in to the lambda function</p>
<ul>
<li><code>$data</code> the revertive value as a string</li>
<li><code>%value</code> the revertive value as a number (or 0 if it is not a number)</li>
<li><code>%index</code> the revertive slot/destination/gpi number</li>
</ul>
<p>The lambda function must return</p>
<ul>
<li>a <strong>1</strong> to say the requirement was met</li>
<li>a <strong>0</strong> to say the requirement was not met</li>
</ul>
<h5 id="comments">comments</h5>
<p>Comments must start with two dashes <code>--</code>. Anything after is treated as the comment. It is sensible to put a comment for each requirement stating what the requirement is.</p>
<h4 id="exclude">EXCLUDE</h4>
<p>Defines a revertive that must not be received in order for the section to pass.</p>
<p>The format is</p>
<pre><code>EXCLUDE device range value1 value2 … valueN comments
</code></pre>
<p>See REQUIRE for information on the command format</p>
<h4 id="optional">OPTIONAL</h4>
<p>Defines a revertive that we want to log being received or not, but that doesn’t affect whether the section passes.</p>
<p>The format is</p>
<pre><code>OPTIONAL device range comments
</code></pre>
<p>See REQUIRE for information on the command format</p>
<h4 id="manual">MANUAL</h4>
<p>Defines a test that requires user interaction. When a SECTION is run containing a manual requirement the section timeout is ignored. Once the script has been run a dialog box containing the text of the manual requirement appears and the test is paused until the user presses the “Pass” or “Fail” button on the dialog box. One dialog box will appear for each manual requirement.</p>
<p>The format is</p>
<pre><code>MANUAL text
</code></pre>
<h2 id="launch">LAUNCH</h2>
<p>This section tells the test runner what applications need to be running in order to run the test. It is usually one of the first sections in the test case but can actually appear anywhere.</p>
<p>The format is</p>
<pre><code>LAUNCH [name of section]
</code></pre>
<p>name of section is optional.</p>
<p>Every line in the launch section is assumed to be an application (and command line arguments) to check if running and if not launch.</p>
<p>The format of the lines should be</p>
<pre><code>PathToApplication args {timeout}
</code></pre>
<p>PathToApplication can either be the full path to the application or a relative path. If relative then it is assumed relative to <strong>CC_SYSTEM/windows/drivers</strong> on a v4.x machine and <strong>bncs_root/modules</strong> on a v3 machine. If there are spaces in the path then the whole path must be enclosed within ‘</p>
<p><strong>args</strong> is optional and is the command line arguments to launch the application with</p>
<p><strong>timeout</strong> is the number of seconds to wait once the application has been launched. If not included then the wait time defaults to 5 seconds.</p>
<pre><code>e.g

C:/developer/automatics/autototest.exe 33 {10}

wmCommand will

- Check if autotest.exe is running with command line argument 33.
- If so then it will move on to the next application to run (or next section).
- If not then it will start autotest.exe with command line argument 33
- Check if autotest.exe is running with command line argument 33.
- If so then it will wait 10 seconds and then move on to the next application to run (or next section). If not it will wait a second and repeat the check. After 10 checks it will fail the section
</code></pre>
<p>If PathToApplication  points to the wmSimulator application then wmCommand will first check if wmSimulator is running. If it is then it will use wmSimulator’s remote API to ask it to load the dataset defined in args (or the default dataset if args is empty). If wmSimulator is not running then it will start it like any other application</p>
<h2 id="precondition">PRECONDITION</h2>
<p>This section allows you to list anything that the user needs to do before running the test.</p>
<p>Anything that is written in this section will appear in a dialog box that the user must acknowledge before the test runner moves on to the next section.</p>
<pre><code>e.g.

PRECONDITION

Make sure dev_300.db0 is updated to the latest version
</code></pre>
<h2 id="extend">EXTEND</h2>
<p>The Extend section allows you to rerun a previously defined section, possibly extending it by adding to the script or requirements.</p>
<p>The format is</p>
<pre><code>EXTEND [name of previously defined section] args
</code></pre>
<p>The name is not optional. If no section is found with the same name then this section is ignored.</p>
<p><strong>args</strong> is optional and has the same format as in the SECTION command.</p>
<p>The EXTEND section can have all the same commands as a default SECTION</p>
<p>When the test runner comes across an EXTEND section it finds the previously defined section that the EXTEND section refers to and creates a new section based on it. It then appends any extra scripting or requirements defined in the EXTEND section and runs the EXTEND section just like a default section.</p>
<pre><code>e.g.

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
</code></pre>

