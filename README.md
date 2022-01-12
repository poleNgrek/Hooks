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

This is a very simple app that eventually will offer to the user the ability to insert a new ingredient through a form, search for ingredients and finally it will render a list of ingredients. Progressively some extra functionalities will be added as we progress but the main idea will not change.

![Screenshot_2022-01-11_at_12.01.42](uploads/de0bdeab50e5e8fe2cb3789c2a6c0de5/Screenshot_2022-01-11_at_12.01.42.png)

The initial structure of the components used can also be seen in the diagram below. The solid lines indicate that a component is being used by another component while the dotted lines indicate the usage of UI wrapper components which they usually have no functionality. It is also worth mentioning that there are some components in the project that are not shown yet in the diagram such as the IngredientsList component, simply because we are not using them yet. The diagram will be updated as we go through the tutorial.

![hooks](https://user-images.githubusercontent.com/6706501/149150964-db3bf91e-314b-468b-8623-b5ea9c64769e.jpg)

## useState

We should probably start with the most important hook, according to me, the useState hook which allows us to manage states!
But before we do, let's establish two very important rules (which also apply for the other hooks as well!).
1. Hooks shall be used ONLY inside functional components or other custom hooks. 
2. Hooks shall be used ONLY in the root level of the component. Or in simpler words, never use a hook inside nested functions.


Now that we established the two most important rules (so far), we can simply import useState like this	:arrow_down:
```javascript
import { useState } from 'react'
```
In our project we import useState inside the IngredientForm.js in order to manage the input state for the title and the amount.

```javascript
const [enteredTitle, setEnteredTitle] = useState('');
const [enteredAmount, setEnteredAmount] = useState('');
```

Once we have created the states then we will set accordingly our input element values and the onChange properties.

```javascript
 <input
   type="text"
   id="title"
   value={enteredTitle}
   onChange={(event) => {
     setEnteredTitle(event.target.value);
   }}
/>
/******************************************************/
<input
  type="number"
  id="amount"
  value={enteredAmount}
  onChange={(event) => {
    setEnteredAmount(event.target.value);
  }}
/>
``` 
### Some notes about useState
useState can be initialized with a default state and that state can be of any value(object, number, string, boolean). useState also returns something and to be more precise it returns ALWAYS an array with exactly two elements:
1. The first element is our current state snapshot.
2. The second element is a function that allows us to update the state.

Theoretically we could use only one state and set as a default value an object with a title and an amount field. However, this means that we would have to update both fields on each input since state updates using objects are not automatically merged. Of course for this small app, typing some extra lines of code wouldn't make a big difference, however in a more complex application, that could be a big inconvenience. 
