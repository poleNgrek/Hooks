## Introduction
 
This "blog" is going to be the beginning of a small series about my intense and fast-paced learning experience with React using [this tutorial series](https://www.udemy.com/course/react-the-complete-guide-incl-redux/). The aim is to try to describe some key concepts as well as try to present some more advanced React topics in a simple and, hopefully, humorous way!

The problem begins with me, and probably others like me, who refuse to give up their comfortable back-end development corner and do the leap towards a full-stack mindset. For some years, I actively tried to avoid going into too much depth with frontend libraries and frameworks. Maybe it is some kind of trauma from the earlier days when I had to use JQuery or vanilla javascript to work with the frontend where there was always a mine waiting for you to step on it to make a mess of your DOM and CSS. Maybe it is also the notoriety for the steep learning curve that comes with some libraries (React *cough* *cough*) which makes one less inclined to invest 900-1000 hours of development and studying to be able to feel comfortable with said library. Whichever the case might be, one thing is certain! Libraries were created to make our lives easier, more productive and hopefully more fun (can frontend development be ever fun?!).

Having said all that, let start talking about the latest most important feature of React, Hooks! (Or Hööks as I enjoy reading it in my head).


## React Hööks

React hooks, introduced with version 16.8, unlocked the full potential of functional components by giving them the ability to manage state, handle side effects and some other neat stuff which were previously only doable through a class-based component. That's cool but what are these hooks really?


> Hooks are special functions that can be used only in functional-components (and custom hooks) and offer some specific extra functionality (like using a state!). You can spot them by their name since they all start with the word "use".

Of course this is not an official definition of hooks but it encapsulates their essence as best as possible!
So, how many hooks are there you may ask. Well, the short answer is 9 whereof 3 are considered basic (or just mega-useful). What about the long answer then? If you noticed in the definition of hooks that I gave earlier, I mentioned something about custom hooks which means that we can have as many hooks as we see fit! But I will talk more about them in part two of this blog.

So, since knowledge is of no value unless you put it into practice, we will explore hooks while working on a small project. Feel free to download the zip folder attached and run `npm install` and then `npm start` inside your project folder. [HööksSummary.zip](uploads/f8ad6ce8b0e66848ed533cb398ede9aa/HööksSummary.zip)


### A brief description of the project

This is a very simple app that eventually will offer to the user the ability to insert a new ingredient through a form, search for ingredients and of course it will render a list of ingredients. Progressively some extra functionalities will be added as we progress but the main idea will not change.

![Screenshot_2022-01-11_at_12.01.42](uploads/de0bdeab50e5e8fe2cb3789c2a6c0de5/Screenshot_2022-01-11_at_12.01.42.png)

LOREM IPSUM
