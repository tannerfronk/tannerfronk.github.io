---
layout: post
title:  "Functions, Callbacks, and Promises"
date:   2021-02-22 5:57:00 -0700
categories: blog post
---
# Functions

Things can't happen in programming without functions. There are multiple ways at declaring functions.

This is the most basic way of declaring a function in JavaScript. You simply put function at the beginning, name it, then fill it in with what you want it to do:

    function myFunc(a, b){
        return a+b
    }
    
Another way to declare a function is inside of a variable:

    const myFunc = function(a, b){
        return a+b
    }

Then we have arrow functions which are close to the last example:

    const myFunc = (a, b) => {
        return a+b
    }

All of these examples work the same and they all act as a function, and allow parameters to be passed in the parenthesis. As you can see in each example, I am returning variable a plus variable b. Returning the result of a function allows the result to be useable for other functions or whatever you have going on. In the above example this code would work by taking in the values of a and b, and returning the sum of both.

# Callbacks

A callback function is a function that is called within another function. They are used when something outside of the function needs to be ran as a part of another functions process. Lets say you have a function that will display the result of our function above:

    function myCallback(sum){
        document.getElementById('result').innerHTML = sum
    }

This function takes in the sum as a parameter, and all we need to do is get the function it's parameter. So, when do we call it? Lets modify the example function from above to handle it:

    function myFunc(a, b){
        sum = a+b
        myCallback(sum)
    }

Once myFunc is called, it will take in it's parameters and add them together while assigning the value to the variable sum. It is then passed to the callback function as a parameter and that function is now called. Hypothetically, this would then display our result to whichever HTML element has the id of 'result'. While this example may not be as complex as other callback function uses, they all follow this basic principle of being called within another function. They are used in real world situations often, such as using fetch() with a .then() function attached. .then() takes it first argument as a callback function for it to work.

# Promises 

A promise in programming is used to wait for sufficient information before executing a task or function. A common use for promises is when retrieving data from an API. You have to wait for all the necessary data to come back, so how do you wait until you can run your function? By promising that once the data is received or the task is performed the next step can then be executed. Lets look at a common example:

    fetch(url)
    .then()

While this example isn't super fleshed out, it can still demonstrate what a promise does. The fetch() function grabs all the data back available at a url. It may take a while for all of the information to come back, and we can't work with it until it does. To handle this delay, we create a promise using .then(). This function will wait until the data comes back, and we can decide what to do with it once it does, that is all defined in the .then() function. This way, we can ensure that nothing starts happening until all of the data we are looking for is received. Due to JavaScripts asynchronous nature, everything will run at once regardless if the appropriate data is there or if the steps performed prior have finished. This creates a lot of issues, so we can control the order everything will run in using promises.

#### Async/Await

Normally you would need to define a promise then what happens after the promise is either resolved or rejected. Here is an example of what that looks like:

    const myPromise = new Promise((resolve, reject) =>{
        //code to handle what happens
    })

There is a lot going on with this function, but essentially you define what happens after the promise is fulfilled. Often, you would put a setTimeout() function inside the code above to wait a specific amount of time before performing an action. This is a lot to define each time you want to use a promise, so JavaScript has some great syntactical sugar to help. Instead of defining a new Promise, we can use the 'async' and 'await' syntax. Lets look at an example using our previous examples.

    let sum = myFunc(5,5)
    function myFunc(a,b){
        sum = a+b
        return sum
    }

    async function displayResult(sum){
        await sum
        document.getElementById('result').innerHTML = sum
    }

In this example, I am assigning the variable sum to myFunc and passing two integer parameters. Our displayResult function was defined using the async syntax, this means that it will run after the promise it is assigned is fulfilled. The variable sum has await used infront of it, so the promise is assigned to the variable and the function will wait until that promise has been fulfilled before it can continue on with the rest of the function. For this example, the HTML element with the id of 'result' will not be displayed with the sum until sum has been defined by the function that it is assigned to. It will all happen in this order: 1. myFunc will be called and ran, then return it's value. 2. When displayResult has recognized that the value has been returned in the previous function, it will then run the rest of its defined steps.