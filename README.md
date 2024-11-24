NOTE: The lines of code corospond to the pasted files in this .md file, not the original.

questions.js code explanation

CODE

```javascript
// creating an array of objects
// Each object consists of the following members: question number, questions, options, and answers
let questions = [
  {
    numb: 1,
    question: "What does HTML stand for?",
    answer: "Hyper Text Markup Language",
    options: [
      "Hyper Text Preprocessor",
      "Hyper Text Markup Language",
      "Hyper Text Multiple Language",
      "Hyper Tool Multi Language",
    ],
  },
  {
    numb: 2,
    question: "What does CSS stand for?",
    answer: "Cascading Style Sheet",
    options: [
      "Common Style Sheet",
      "Colorful Style Sheet",
      "Computer Style Sheet",
      "Cascading Style Sheet",
    ],
  },
  {
    numb: 3,
    question: "What does PHP stand for?",
    answer: "Hypertext Preprocessor",
    options: [
      "Hypertext Preprocessor",
      "Hypertext Programming",
      "Hypertext Preprogramming",
      "Hometext Preprocessor",
    ],
  },
  {
    numb: 4,
    question: "What does SQL stand for?",
    answer: "Structured Query Language",
    options: [
      "Stylish Question Language",
      "Stylesheet Query Language",
      "Statement Question Language",
      "Structured Query Language",
    ],
  },
  {
    numb: 5,
    question: "What does XML stand for?",
    answer: "eXtensible Markup Language",
    options: [
      "eXtensible Markup Language",
      "eXecutable Multiple Language",
      "eXTra Multi-Program Language",
      "eXamine Multiple Language",
    ],
  },
  // Duplicate the object declaration and initialization above for more questions
];

```

This file consists of a single array of elements and there properties which is the data for each of the questions on the quiz. The the properties of the elements contain the text that will be displayed on screen for each of he questions.





quizApp.js 

CODE

```javascript
//selecting all required elements from html document
const start_btn = document.querySelector(".start_btn button");
const info_box = document.querySelector(".info_box");
const exit_btn = info_box.querySelector(".buttons .quit");
const continue_btn = info_box.querySelector(".buttons .restart");
const quiz_box = document.querySelector(".quiz_box");
const result_box = document.querySelector(".result_box");
const restart_quiz = result_box.querySelector(".buttons .restart");
const quit_quiz = result_box.querySelector(".buttons .quit");
const option_list = document.querySelector(".option_list");
const time_line = document.querySelector("header .time_line");
const timeText = document.querySelector(".timer .time_left_txt");
const timeCount = document.querySelector(".timer .timer_sec");
const next_btn = document.querySelector("footer .next_btn");
const bottom_ques_counter = document.querySelector("footer .total_que");

// if startQuiz button clicked
start_btn.addEventListener("click", (e) => {
  info_box.classList.add("activeInfo"); //show info box
});

// if exitQuiz button clicked
exit_btn.addEventListener("click", (e) => {
  info_box.classList.remove("activeInfo"); //hide info box
});

// if continueQuiz button clicked
continue_btn.addEventListener("click", (e) => {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.add("activeQuiz"); //show quiz box
  showQuetions(0); //calling showQestions function
  queCounter(1); //passing 1 parameter to queCounter
  startTimer(15); //calling startTimer function
  startTimerLine(0); //calling startTimerLine function
});

// Variables to control quiz operations
let timeValue = 15;
let que_count = 0;
let que_numb = 1;
let userScore = 0;
let counter;
let counterLine;
let widthValue = 0;

// if restartQuiz button clicked
restart_quiz.addEventListener("click", (e) => {
  quiz_box.classList.add("activeQuiz"); //show quiz box
  result_box.classList.remove("activeResult"); //hide result box
  timeValue = 15;
  que_count = 0;
  que_numb = 1;
  userScore = 0;
  widthValue = 0;
  showQuetions(que_count); //calling showQestions function
  queCounter(que_numb); //passing que_numb value to queCounter
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  startTimer(timeValue); //calling startTimer function
  startTimerLine(widthValue); //calling startTimerLine function
  timeText.textContent = "Time Left"; //change the text of timeText to Time Left
  next_btn.classList.remove("show"); //hide the next button
});

// if quitQuiz button clicked
quit_quiz.addEventListener("click", (e) => {
  window.location.reload(); //reload the current window
});

// if Next Question button is clicked
next_btn.addEventListener("click", (e) => {
  //check if it does not exceed max questions
  if (que_count < questions.length - 1) {
    que_count++; //increment the que_count value
    que_numb++; //increment the que_numb value
    showQuetions(que_count); //calling showQestions function
    queCounter(que_numb); //passing que_numb value to queCounter
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    startTimer(timeValue); //calling startTimer function
    startTimerLine(widthValue); //calling startTimerLine function
    timeText.textContent = "Time Left"; //change the timeText to Time Left
    next_btn.classList.remove("show"); //hide the next button
  } else {
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    showResult(); //calling showResult function
  }
});

// getting questions and options from array
// If you have lesser or more number of options you need to change this code
function showQuetions(index) {
  const que_text = document.querySelector(".que_text");

  //creating a new span and div tag for question and option and passing the value using array index
  // self exercise: re-write the following using backtick syntax for string in JS instead of concatenation operator
  let que_tag =
    "<span>" +
    questions[index].numb +
    ". " +
    questions[index].question +
    "</span>";
  let option_tag =
    '<div class="option"><span>' +
    questions[index].options[0] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[1] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[2] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[3] +
    "</span></div>";
  que_text.innerHTML = que_tag; //adding new (child) span tag inside que_tag
  option_list.innerHTML = option_tag; //adding new (child) div tag inside option_tag

  const option = option_list.querySelectorAll(".option");

  // set onclick attribute to all available options
  for (i = 0; i < option.length; i++) {
    option[i].setAttribute("onclick", "optionSelected(this)");
  }
}
// create new div tags for right or wrong tick icons
let tickIconTag = '<div class="icon tick"><i class="fas fa-check"></i></div>';
let crossIconTag = '<div class="icon cross"><i class="fas fa-times"></i></div>';

//if user clicked on option
function optionSelected(answer) {
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  let userAns = answer.textContent; //getting user selected option
  let correcAns = questions[que_count].answer; //getting correct answer from array
  const allOptions = option_list.children.length; //getting all option items

  //if user selected option is equal to array's correct answer
  if (userAns == correcAns) {
    userScore += 1; //update total score value increment by 1
    answer.classList.add("correct"); //add green color to correct selected option
    answer.insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to correct selected option
    console.log("Correct Answer");
    console.log("Your correct answers = " + userScore);
  } else {
    answer.classList.add("incorrect"); //add red color to correct selected option
    answer.insertAdjacentHTML("beforeend", crossIconTag); //add cross icon to correct selected option
    console.log("Wrong Answer");

    for (i = 0; i < allOptions; i++) {
      if (option_list.children[i].textContent == correcAns) {
        //if there is an option which is matched to an array answer
        option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
        option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
        console.log("Auto selected correct answer.");
      }
    }
  }
  for (i = 0; i < allOptions; i++) {
    option_list.children[i].classList.add("disabled"); //once user select an option, disable all options
  }
  next_btn.classList.add("show"); //show the next button if user selected any option
}

// display for result box based on user performance
function showResult() {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.remove("activeQuiz"); //hide quiz box
  result_box.classList.add("activeResult"); //show result box
  const scoreText = result_box.querySelector(".score_text");
  if (userScore > 3) {
    // if user scored more than 3
    //create a new span tag and pass the user score number and total question number
    let scoreTag =
      "<span>and congrats! , You got <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag; //add new span tag inside score_Text
  } else if (userScore > 1) {
    // if user scored more than 1
    let scoreTag =
      "<span>and nice , You got <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag;
  } else {
    // if user scored less than 1
    let scoreTag =
      "<span>and sorry , You got only <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag;
  }
}

// control the timer and actions associated to it
function startTimer(time) {
  counter = setInterval(timer, 1000);
  function timer() {
    timeCount.textContent = time; //change the value of timeCount with time value
    time--; //decrement the time value
    if (time < 9) {
      //if timer is less than 9
      let addZero = timeCount.textContent;
      timeCount.textContent = "0" + addZero; //add a 0 before time value
    }
    if (time < 0) {
      //if timer is less than 0
      clearInterval(counter); //clear counter
      timeText.textContent = "Time Off"; //change the time text to time off
      const allOptions = option_list.children.length; //get all option items
      let correcAns = questions[que_count].answer; //get correct answer from array
      for (i = 0; i < allOptions; i++) {
        if (option_list.children[i].textContent == correcAns) {
          //if there is an option which is matched to an array answer
          option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
          option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
          console.log("Time Off: Auto selected correct answer.");
        }
      }
      for (i = 0; i < allOptions; i++) {
        option_list.children[i].classList.add("disabled"); //once user select an option then disabled all options
      }
      next_btn.classList.add("show"); //show the next button if user selected any option
    }
  }
}

// Shows a progress bar mirroring timer value left
function startTimerLine(time) {
  counterLine = setInterval(timer, 29);
  function timer() {
    time += 1; //upgrading time value with 1
    time_line.style.width = time + "px"; //increasing width of time_line with px by time value
    if (time > 549) {
      //if time value is greater than 549
      clearInterval(counterLine); //clear counterLine
    }
  }
}

function queCounter(index) {
  //creating a new span tag and passing the question number and total question
  let totalQueCounTag =
    "<span><p>" +
    index +
    "</p> of <p>" +
    questions.length +
    "</p> Questions</span>";
  bottom_ques_counter.innerHTML = totalQueCounTag; //adding new span tag inside bottom_ques_counter
}
```

Code Explanation

### Lines 83-96
    This code selects the various html elements for use in dynamic display


### Lines 99-116
    These lines are event listeners with the following procceses

        -startQuiz button click
            when clicked this will show the "activeinfo" element in the info_box html element
        
        -exitQuiz button click
            when clicked this will remove the "activeinfo" element in the info_box html element

        -continueQuiz button click
            when clicked this will remove the "activeinfo" element and add the "activequiz" element
            it them calls the following functions 

                showQuestion

                queCounter

                startTimer

                startTimerLine

### lines 119-125
    the lines of code are declareing the initial variables related to the quiz timer and quiz questions

### Lines 128-144 
    This code beguins with an event listener for the restartQuiz button
    when clicked this button resets all the variables and removes the activeRelsults elements form the results_box html element and add the "active quiz element to the quiz_box html element
    It then calls various functions that are used in reseting the quiz 
    it them sets the text for the timetext html element to time left and uses another function to hide the next button 

### Lines 146-149
    This code is an event handler for the quit_quiz button, when click it will reset the app to its main page

### Lines 152-170
    This code is an event handler for the next_btn button, It beguins by checking 

     ```javascript
     if (que_count < questions.length - 1) 
     ```

     which is determining if the question that your on is not the last question or it is the last question and passes that to an if else statement to run functions accordingly
     
     NOT LAST QUESTION

    it begins by running

     ```javascript
        que_count++; //increment the que_count value
        que_numb++; //increment the que_numb value
     ```

     which adds one to both the question count for internal procces and to the question number html element on screen for eac respective question

     It them calls various functions resposnible for reseting the the timer and updating the question and counter numbers and updating the width of the of the timer bar seen on screen.

     IS LAST QUESTION
     If the function determines its on the last question it will clear the question counter will call the showResults function that adds the html elemts that shows the score. 


### Lines 174 and 175
    This code is responsible for setting a fuction that sets the current question on the screen in the que_text html element

### Lines 179-199
    This code is responsible or dynmically creating the quiz questions html element for each of the questions, it creates specific variables for the que_tag and option tag.

    it then runs the code
    ```javascript
        que_text.innerHTML = que_tag;
        option_list.innerHTML = option_tag; 
    ```
    which sets the html element to the most current version of que_tag and option_tag

### Line 201-210
    This code creates a variable option which is them set to all the options of the quiz questions to later be called

    It then creates a loop the length of the number of questions to set a clickable event for each of the questions it then passes the values of what is clicked to the optionSelected function.

    Lastly it creates the html elements that represent the right and wrong signifyers on screen when a answer is clicked

### Lines 213-245
    The function optionSelected evaluates what the user picked and does the folllowing tasks
        Stops running the timers

        gets the users awnsers and the correct awnser

        counts the total number of options in the option_List

    It then uses an if statement to process what happens if you slect the right or wrong awnser

        RIGHT AWNSER
            It first updates the users score by one, it then updates the color of the htlm element holding the question to green and adds the corrospoding tick icon
            It then sends send text to the console telling you you got it correct and your current score

        WRONG ANSWER
            It updates the the color of the html element holding the question to red and add the corrosponding tick icon and sends text to the console telling you are wrong

    It then runs a for loop which iterates through each of the options in th option list to see if the awnser you selected is one of the correct awsners

    If it find that the option selected matches the correct answer on the array it then adds the green color the the html element that contains the question and adds the corrosponding tick icon 

    It then runs another loop to disable each of the question after an awser is picked making sure the user cannot change their answer

### Lines 248-282
    This code is responsible for the showResults functions
        it beguins with removing the activeQuiz and activeInfo classes from their corrosponding html elements 
        it them adds the activeResults element to the results_box html element to show the user to score

        it then runs an if statement to dynmaicall change the scoreTag response depending on the users score
        the responsed is slighty changed depending on wheter the user got a score better or worse than one or three

### Lines 285-315
    This code is responsible for the startTimer function
        it first sets a counter to run the timer function to run every 1000 miliseconds

    the timer function first sets the time values from the inital variable declared and them decrease the it by 1 every time the function is called

    It then runs an if statement that detemines when the counter is 9 or less and adds a 0 before them for proper formatting

    it then runs an if statement to determine if the time is below zero, if the timer is below zero it will stop the timer and changes the timeText elemts to show that it has run out of time 

    It then gathers the entire list of all the options and the correct answer for that question, it will then find the correct answer for that question and automatically highlight it green and update the tick icon to show the correct answer,

    it finally disables the other quiz elements to prevent futher interaction

### Lines 318-328
    This code first starts with the passing of the time variable to the startTimerLine function
    It then sets the function counterLine to rune every 29 miliseconds for a seamless visual transistion

    The timer function is responsible for dynmically updating the width of the line on screen with the miliseconding corosponding to pixels on screen

    When the timer reaches 549 miliseconds it will then clear the timer line to show that the user has run out of time

### Lines 330-339
    the queCounter function is responsible for creating a dynamic counter to show what question the user is on and the total number of questions.







#### Problem Statment
    The program is a dynamic quiz application designed to present multiple-choice questions to users. It manages the entire quiz process, including displaying questions, tracking user answers, managing timers, and providing real-time feedback

#### Functional Features
    Dynamic question and answer display
    Answer validation with feedback 
    Countdown timer with a synchronized progress bar
    Automatic correct answer display on timeout
    Real-time score tracking and updates
    Personalized result summary and feedback
    Interactive "Next" button for navigation
    Dynamic question counter display
    Responsive UI with relevant section toggling

#### Directory Structure
    index.html serves as an entry point to the app containg the main html elements

    .gitignore is a file used to to specify sensitive files that are untracked, this it is empty

    the js folder contains the two javascript files that a responsible for the dynmaic funtion of the app

    the css folder contain the style sheet for each of the html elements














     

