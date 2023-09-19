---
toc: true
comments: false
layout: post
title: JS Calculator
description: This is Javascript calculator
type: hacks
courses: { compsci: {week: 4} }
---

<style>
.calculator-output {
  /* Calculator output 
    Top bar shows the results of the calculator;
    Result to take up the entirety of the first row;
    Span defines 4 columns and 1 row
  */
  grid-column: span 4;
  grid-row: span 1;

  border-radius: 10px;
  padding: 0.25em;
  font-size: 20px;
  border: 5px solid black;

  display: flex;
  align-items: center;
}
</style>

<!-- Add a container for the animation -->
<div id="animation">
  <div class="calculator-container">
    <!-- Result -->
    <div class="calculator-output" id="output">0</div>
    <!-- Row 1 -->
    <div class="calculator-number">1</div>
    <div class="calculator-number">2</div>
    <div class="calculator-number">3</div>
    <div class="calculator-operation">+</div>
    <!-- Row 2 -->
    <div class="calculator-number">4</div>
    <div class="calculator-number">5</div>
    <div class="calculator-number">6</div>
    <div class="calculator-operation">-</div>
    <!-- Row 3 -->
    <div class="calculator-number">7</div>
    <div class="calculator-number">8</div>
    <div class="calculator-number">9</div>
    <div class="calculator-operation">*</div>
    <!-- Row 4 -->
    <div class="calculator-clear">A/C</div>
    <div class="calculator-number">0</div>
    <div class="calculator-number">.</div>
    <div class="calculator-equals">=</div>
  </div>
</div>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
// Initialize important variables to manage calculations
var firstNumber = null;
var operator = null;
var nextReady = true;
// Build objects containing key elements
const output = document.getElementById("output");
const numbers = document.querySelectorAll(".calculator-number");
const operations = document.querySelectorAll(".calculator-operation");
const clear = document.querySelectorAll(".calculator-clear");
const equals = document.querySelectorAll(".calculator-equals");

// Number buttons listener
numbers.forEach(button => {
  button.addEventListener("click", function() {
    number(button.textContent);
  });
});

// Number action
function number(value) {
  // Function to input numbers into the calculator
  if (value != ".") {
    if (nextReady == true) {
      output.innerHTML = value;
      if (value != "0") {
        nextReady = false;
      }
    } else {
      output.innerHTML = output.innerHTML + value;
    }
  } else {
    if (output.innerHTML.indexOf(".") == -1) {
      output.innerHTML = output.innerHTML + value;
      nextReady = false;
    }
  }
}

// Operation buttons listener
operations.forEach(button => {
  button.addEventListener("click", function() {
    operation(button.textContent);
  });
});

// Operator action
function operation(choice) {
  // Function to input operations into the calculator
  if (firstNumber == null) {
    firstNumber = parseInt(output.innerHTML);
    nextReady = true;
    operator = choice;
    return;
  }
  firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
  operator = choice;
  output.innerHTML = firstNumber.toString();
  nextReady = true;
}

// Calculator
function calculate(first, second) {
  // Function to calculate the result of the equation
  let result = 0;
  switch (operator) {
    case "+":
      result = first + second;
      break;
    case "-":
      result = first - second;
      break;
    case "*":
      result = first * second;
      break;
    case "/":
      result = first / second;
      break;
    default:
      break;
  }
  return result;
}

// Equals button listener
equals.forEach(button => {
  button.addEventListener("click", function() {
    equal();
  });
});

// Equal action
function equal() {
  // Function used when the equals button is clicked; calculates equation and displays it
  firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
  output.innerHTML = firstNumber.toString();
  nextReady = true;
}

// Clear button listener
clear.forEach(button => {
  button.addEventListener("click", function() {
    clearCalc();
  });
});

// A/C action
function clearCalc() {
  // Clears calculator
  firstNumber = null;
  output.innerHTML = "0";
  nextReady = true;
}
</script>

<!-- Vanta animations just for fun, load JS onto the page -->
<script src="/teacher/assets/js/three.r119.min.js"></script>
<script src="/teacher/assets/js/vanta.halo.min.js"></script>
<script src="/teacher/assets/js/vanta.birds.min.js"></script>
<script src="/teacher/assets/js/vanta.net.min.js"></script>
<script src="/teacher/assets/js/vanta.rings.min.js"></script>

<script>
// Setup Vanta scripts as functions
var vantaInstances = {
  halo: VANTA.HALO,
  birds: VANTA.BIRDS,
  net: VANTA.NET,
  rings: VANTA.RINGS
};

// Obtain a random Vanta function
var vantaInstance = vantaInstances[Object.keys(vantaInstances)[Math.floor(Math.random() * Object.keys(vantaInstances).length)]];

// Run the animation
vantaInstance({
  el: "#animation",
  mouseControls: true,
  touchControls: true,
  gyroControls: false
});
</script>