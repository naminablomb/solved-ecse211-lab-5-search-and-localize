Download Link: https://assignmentchef.com/product/solved-ecse211-lab-5-search-and-localize
<br>
<ol>

 <li>Implement a robot with ultrasonic and color sensors for determining the location and color of a ring without physical contact.</li>

 <li>Starting in a known corner, localizing to the grid, and performing a search along a specific path for a colored ring of known dimensions</li>

</ol>

<h2>Design requirements</h2>

The following design requirements must be met:

<ul>

 <li>The system must satisfy the design requirements from Lab 3 with respect to localization:

  <ul>

   <li>You must follow the (<strong>X</strong>​ ,​ ​<strong>Y</strong>,​ <strong>θ</strong>​ )​ convention specific to this lab (refer to <strong>Figure 2</strong>​ ).​ o You are free to use any localization method (with or without light localization).</li>

   <li>You are <strong>not</strong>​ required to provide a way of selecting rising or falling edge.​           o You do <strong>not</strong>​ have to wait for user input after completing ultrasonic or light localization.​                 The system must follow the same navigation requirements as in Lab 4:</li>

   <li>The robot must navigate through a series of specified points using the minimal distance.</li>

   <li>A list of waypoints must be provided in one location in the code.</li>

   <li>The robot must use the grid system found on the play floor, where the origin is the starting corner of a tile.</li>

   <li>Waypoints will be given with respect to the tile grid system. For example, (1, 0) will be located 30.48 cm to the right of the origin (0, 0).</li>

   <li>When turning to a waypoint, the robot must use the minimal angle needed to turn to it.</li>

  </ul></li>

</ul>

12th​

<ul>

 <li>Your robot must be able to localize in the starting corner, navigate through the given waypoints, detect the color of every ring along its path, and signify when it has reached the end of its course with <strong>3</strong>​</li>

 <li>Your robot must complete its demonstration procedure in a maximum of 5​ minutes.​</li>

</ul>

<h2>Color Calibration</h2>

The color assignments of the rings are 1: Green​ , 2: ​ Blue​ , 3: ​ Yellow​ , 5: ​ Orange​        , as defined ​                                                                                 <a href="http://www.lejos.org/ev3/docs/constant-values.html#lejos.robotics.Color">her</a>​    <a href="http://www.lejos.org/ev3/docs/constant-values.html#lejos.robotics.Color">e</a>.​

Part of the preparation for this experiment is calibrating your color sensor against the sample rings in the lab. This is an important step in the design process because it will allow for greater color detection accuracy than using constant values or the given leJOS methodology. Calibrating and validating sensor behaviour before testing the robot as a whole will make the entire process easier! Each color corresponds to a distribution of Red​ ​, Green​ ,​ and​ Blue​ values.​

For each ring, you need to take several measurements of the R​ G​ B​ values returned by the sensor. These measurement values will be used to build a statistical model for each ring’s color.

<strong>Quick Tip: This color calibration should be done and tested in both TR0110 and TR0090, since they have different lightings and could potentially yield different results! The demos will be conducted in TR0090 (the small lab), so testing color calibration in that room especially would be ideal.  </strong>

A particularly simple approach is to model each R​ ​G B​ channel as a Gaussian distribution, i.e. empirically compute the mean and standard deviation for the Red​ ​, Green​ ,​ and​ Blue​ channels​ for each ring from the measurement samples. Therefore, each ring can be represented by its mean RG​ B​ values. A simple means of classifying each ring is to compute the Euclidean distance, <em>d</em>, between the R​ G​ B​ values of a measured sample, <em>s<sub>R </sub></em>, <em>s<sub>G </sub></em>, and <em>s<sub>B </sub></em>, and a target ring’s mean R​ G​ B​ values, μ<em><sub>R </sub></em>, μ<em><sub>G</sub></em> and μ<em><sub>B </sub></em>, and. A ring is then deemed to be the match when the minimum Euclidean distance value is met.

√ 2 2 2 <em>d </em>= (<em>s<sub>R </sub></em>− μ<em><sub>R</sub></em>) + (<em>s<sub>G </sub></em>− μ<em><sub>G</sub></em>) + (<em>s<sub>B </sub></em>− μ<em><sub>B</sub></em>)

However, in a real-world scenario, this approach may not work accurately. Given the Gaussian distribution for each channel, an easy way to distinguish ring colors from each other is to use their standard deviations, i.e. σ<em><sub>R </sub></em>, σ<em><sub>G </sub></em>, and σ<em><sub>B </sub></em>, precomputed from the measurement samples. For example, if a sample distance is greater than 2 or 3 standard deviations from its mean, that ring is probably not the target. This method uses a probabilistic measure by introducing a level of uncertainty when dealing with the color sensor samples.




<strong>Figure 1. Normalized Gaussian distribution: 68% within ± 1σ, 95.4% within ± 2σ. </strong>




In computing your mean statistics, i.e., R​ ​mi, G​ ​mi, B​ ​mi, R​ ​s, G​ s​, and B​ ​s, variations will creep in due to

​            ​           ​         ​          ​                     ​

changes in ambient lighting.  One way of mitigating this is to normalize the mean in a particular

channel by the average over all 3 channels, shown for R​s, G​s, and B​s below,

​          ​                      ​




and similarly for R​ ​mi, G​ ​mi, and B​ mi​ .

​            ​                       ​

<h3>Demonstration</h3>

The design must satisfy the requirements by completing the demonstration outlined below.

<h4>Design presentation</h4>

Before demoing the design, your group will be asked some questions for approximately 5 minutes. You will present your design (hardware and software) and answer questions to test your understanding of the lab concepts. Grades will apply to the entire group, although TAs reserve the right to grade individually if they deem it necessary.




You must present your workflow, an overview of the hardware design, and an overview of the software functionality. Visualizing software with graphics such as flowcharts is valuable.

<h4>Color Classification</h4>

The TA will check whether your robot can correctly classify the Blue, Green, Yellow and Orange rings. When any object is brought close to your sensor(s), the LCD must display “<em>Object</em>​

<em>Detected</em>​” on the first line. The next line should read “<em>Blue</em>​ ​”, “​<em>Green</em> ​”, “<em>Yellow</em>​ ​” and “<em>Orange</em>​ ​” for a target ring. Nothing should be displayed on the LCD if no object is detected. You can decide the distance between the tested ring and the sensor. This color classification should not take more than <strong>10</strong>​<strong> seconds per trial</strong> for <strong>5</strong>​<strong> successive trials</strong> using the rings in the lab. False positives and negatives are considered incorrect. Hence, the following points are awarded:

<ul>

 <li><strong>1 point</strong> for each time the robot correctly classifies a ring (i.e. “​ <em>Object Detected</em>​ ​” and <em>color</em>​                                                       ​).</li>

</ul>

<h4>Field Test</h4>

The field test will be set up on an 8×8 grid as shown below in <strong>Figure 2</strong>​ . There will be at least 2​       and at most 5 rings in the navigation path.

<strong> </strong>

<strong> </strong>

<strong>Figure 2:  Search and Localize demo setup (with Map 4 and 4 rings) </strong>

<strong> </strong>

For a successful field test, the following steps must be followed:

<ul>

 <li>Enter the given path as decided by the TA:

  <ul>

   <li>As in Lab 4, we ask that you encode the given maps (in the FAQ section below) so you can easily select the correct map to use at demo time.</li>

  </ul></li>

 <li>Place your robot in the starting corner (0, 0).</li>

 <li>Start your robot upon a button push. At this point, the program should already be fully loaded and all the sensors initialized.</li>

 <li>Your robot must proceed to localize and navigate through the waypoints.</li>

 <li>While navigating, the robot must then detect rings and their colors:

  <ul>

   <li>If a ring is detected, the robot must stop and display the words “Object Detected” on the top line of the LCD.</li>

   <li>Once the color of the ring has been determined, the robot must display the color on the second line of the LCD display, BEEP TWICE, and then pause for about 10 seconds (to allow a TA to remove the ring from the path) before continuing its navigation.</li>

   <li>Once the ring and color have been detected, the ring will be removed from the robot’s path so as not to disrupt navigation.</li>

  </ul></li>

 <li>Once the robot reaches its final waypoint, it should BEEP THREE TIMES and stop.

  <ul>

   <li>For the ease of the demo, the robot should display these two parameters once it has stopped:</li>

  </ul></li>

</ul>

&#x25aa; <u>On the first line: </u>The number of rings detected throughout the demo<u>​ </u>

&#x25aa; <u>On the second line:</u> The color of the rings detected, in order from first to last.​       &#x25aa; For example, for Figure 2 it should display:

<em>Number detected: 4 </em>

<em>Colors detected: Blue, Yellow, Orange, Green </em>

<ul>

 <li>All these steps must be completed within 5​ minutes.​</li>

</ul>

Hence, the following points are awarded for the field test:

<ul>

 <li>are​ awarded for localization and navigation to the point (1, 1) before navigation begins.</li>

 <li>are awarded for correctly identifying all rings along the path and not misidentifying their colors. Partial marks will be awarded as deemed by the number of rings detected and by the TA.</li>

 <li>are​ awarded for navigating through all the waypoints and for finishing within approximately half a tile of the last point.</li>

</ul>

We recommend referring to the <strong>FAQ</strong>​            section for details on some specific limitations.​

<h2>Report Requirements</h2>

The following sections must be included in your report. Answer all questions in the report and copy them into your report. For more information, refer to the submission instructions. Always provide justifications and explanations for all your answers.

<strong>Important Note about Grading:  </strong>

The grading rubric of this report has been altered. Section 1 (Design Evaluation) will now count for 10 of the 50 marks, and Section 5 (Further Improvements) will count for 5 out of the 50 marks.

<h3>Section 1: Design Evaluation</h3>

<strong>For Lab 5, we are asking that this section be longer than the half a page required for previous labs. Now that you will be working in larger groups, more thought should be put into the design process, workflow, and teamwork of this lab. This section now has a limit of about 2 pages. </strong>

<strong>Workflow </strong>

<ul>

 <li>How did you distribute tasks amongst team members? How did this distribution of work aid in the design process?</li>

 <li>What was the timeline of your work? Did any aspect of the lab take you longer than expected?</li>

 <li>Did your design process or workflow change drastically with more group members? How did you deal with this change in environment?</li>

 <li>Include any flowcharts or graphics to help describe your workflow.</li>

</ul>

<strong>Hardware Design </strong>

<ul>

 <li>Give a general overview of the hardware design. Include graphics or any images taken of your robot here.</li>

 <li>How did you validate this hardware design prior to building it? If you did not, how could you include this step in the future?</li>

</ul>

<strong>Software Design </strong>

<ul>

 <li>Give a general overview of the software design. Include graphics and diagrams here.</li>

 <li>How did you validate your software design prior to running it on the robot? If you did not, how could you include this step in the future?</li>

</ul>

<strong> </strong>

<h3>Section 2: Test Data</h3>

This section describes what data must be collected to evaluate your design requirements. Collect the data using the methodology described below and present it in your report.

<strong>Model Acquisition </strong><em>(4</em>​ <em>x10 independent trials) </em>

<ul>

 <li>Collect a minimum of 10 R​ G​ B​ color values for each ring color (i.e. Blue​ ,​ Green​ ,​ Yellow​ ,​ and Orange).​</li>

 <li>The ring-to-sensor distance should be within the working range of the sensor (which you need to determine). You should ensure to sample different points on the surface.</li>

</ul>

<strong>Color and Position Identification </strong><em>(4</em>​ <em> independent runs)</em>

<ul>

 <li>Set the robot to follow the navigation path from Map 4 (Figure 2).</li>

 <li>Place all 4 rings in the search region as shown in Figure 2.</li>

 <li>Run the field test 4 times:

  <ul>

   <li>In each run, record (X​F, Y<sub>​ </sub>​F)<sub>​ </sub>, the final position of the robot.</li>

   <li>In each run, record the sample R​ G​ B​ values (<em>s<sub>R </sub></em>, <em>s<sub>G </sub></em>, <em>s<sub>B </sub></em>) for all the identified rings.</li>

  </ul></li>

</ul>

These are used to evaluate your classifier’s performance.

<ul>

 <li>In each run, record how many of the rings were detected and correctly color-identified.</li>

 <li><u>Hint</u>:​ you can write your measurements to the LCD or remote console program.</li>

</ul>

<h3>Section 3: Test Analysis</h3>

<strong>Color Calibration </strong>

<ul>

 <li>Compute the R​ G​ B​ mean and standard deviation values for each of the 4 ring colors.​</li>

 <li>For each color, plot the corresponding R​ G​ B​ normalized Gaussian distributions and draw vertical lines at μ, ±1σ from μ, and ±2σ from μ.</li>

 <li>Ensure that your plots are labelled and captioned in sufficient detail.</li>

</ul>

<strong>Color and Position Identification </strong>

<ul>

 <li>For each field test run, compute the Euclidean distance, <em>d</em>, of all recorded R​ G​ B​ This​ should result in 4 distance values for each recorded sample. Then, rank order the distances for each ring index in ascending order. For each entry in the rank order table, list the classification that your algorithm returns (i.e. was a Red ring ​ detected as ​     Red​       ?).​</li>

 <li>For each run, compute the Euclidean error distance between (X​F, Y​F) and the expected</li>

</ul>

​           ​

destination waypoint (X​D, Y​D).

​          ​

<h3>Section 4: Observations and Conclusions</h3>

<ul>

 <li>Are rank-ordering Euclidean distances a sufficient means of identifying ring colors? Explain in detail why or why not.</li>

 <li>Is the standard deviation a useful metric for detecting false positives? In other words, if the ring color determined using the Euclidean distance metric, <em>d</em>, is incorrect, can this false positive be detected by using μ±1σ or μ±2σ values instead?</li>

 <li>Under what conditions does the color sensor work best for correctly distinguishing colors?</li>

</ul>

<h3>Section 5: Further Improvements</h3>

<ul>

 <li>Depending on how you implemented your color classifier, can your results be improved by using one or more of the noise filtering methods discussed in class? ● How could you improve the accuracy of your target detection?</li>

</ul>