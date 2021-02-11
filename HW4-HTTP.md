---
title: "Homework 4: HTTP Network Traffic Analysis
---

*50 points possible*

## Objectives

* Expose the nuts and bolts of HyperText Transport Protocol (HTTP).
* Gain experience with tools for troubleshooting network protocols.
* Learn problem-solving skills for web-based systems.
* Look under the hood of web services.

## Resources

* Computer with either Google Chrome or Microsoft Edge web browser. (You can use FireFox but the process of analyzing traffic is a little different).
* [RFC 2616](https://www.rfc-editor.org/info/rfc2616) The IETF standard that defines HTTP 1.1
* A [tutorial](http://www.steves-internet-guide.com/http-basics/) on how HTTP works.
* [Blank Answer Sheet](HW4-HTTP-Answersheet.txt){: download="HW4-HTTP-Answersheet.txt"} text file into which you will insert your answers.

## How to Complete and Submit the Homework
*Please follow these instructions*

1. Download the [answersheet](HW4-HTTP-Answersheet.txt){: download="HW4-HTTP-Answersheet.txt"} and load it into a text editor such as VS Code.
2. Perform each of the following steps. Write or paste your answers into the answer sheet.
3. Submit your answer sheet to LearningSuite for grading.

## Steps: Do the Following

**1)** Get into your web browser's developer tools, select the Network tab, and start recording.

* Open your browser (Edge or Chrome)
* Open developer tools (upper-right menu ➜ More Tools ➜ Developer Tools, or F12, or right-click ➜ Inspect)
* Select the Network tab. (It may be hidden behind the »)
* Make sure the filter is set to All
* Recording should start automatically. See that the dot at the left of the button bar is red (and not black).
* It should look like this:

![Image of the network tab area](images/developer-tools-network.png)

**2)** Browse to [https://itc.byu.edu](https://itc.byu.edu) and select the request for the page itself. Look in the `general` section.

* The page may be in your cache if you've visited the site recently. To be sure to view the full request, click the clear button next to the record button (to clear your capture) and then refresh the page.
* If you cleared the recording first, the page request (for the HTML) should be the first one in the list. Click on that one to view the detail.

<div class="question">What URL was requested?</div>
<div class="question">Which request method (HTTP verb) was used?</div>
<div class="question">What is the IP address of the server?</div>
<div class="question">Which port on the server was accessed?</div>
<div>
<div class="question">Is the port the one you would expect it to be? Why?</div>

**3)**