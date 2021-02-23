---
layout: post
title:  "Reactive Programming"
date:   2021-02-22 5:57:00 -0700
categories: blog post
---
#### Reactive Programming
What is reactive programming? With all the interactivity pushed into web development in Today's world, a user needs real time feedback. React handles this by creating a shadow DOM, and when it notices a change has been made, it reruns all the code checking for what has changed. This can be cumbersome on large applications, and is unncessarily slow when running code that isn't relevant just to change one tiny thing. Svelte changes this by only checking what has been changed and updating the state accordingly. This is done by compiling everything once it is built, then essentially watching individual elements for change. This removes the need to rerun all the code, and only what is needed.

#### Topological Rending
To go along with reactive programming, Topological Rendering looks at what is needed before certain elements can be presented on a screen. Using a spreadsheet as an example, if we are calculating cell C as the sum of A plus B, C cannot be rendered until after A and B are rendered. This technique ensures that required information is rendered before anything else.

#### Accessibility 
Svelte has accessibility built right in. When coding a website, it is so easy to forget to add what is absolutely required for some people to use the internet. Svelte has built in accessibility that checks elements to ensure certain accessible attributes are present. For example, if you create an image that has no alt attribute, it will throw a warning in the console letting you know it is needed. Of course, you can still ignore it, but at that point it is on you for refusing to implement accessbility. When using Svelte, you can't declare ignorance.