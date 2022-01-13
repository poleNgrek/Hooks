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
1. ***Hooks shall be used ONLY inside functional components or other custom hooks.*** 
2. ***Hooks shall be used ONLY in the root level of the component.*** Or simply put, never use a hook inside nested functions.


Now that we established the two most important rules (so far), we can simply import useState like this	:arrow_down:
```javascript
import { useState } from 'react'
```
useState can be initialized with a default state and that state can be of any value(object, number, string, boolean). Calling useState returns ALWAYS an array with exactly two elements:
1. The first element is our current state snapshot.
2. The second element is a function that allows us to update the state.
In our project we import useState inside the IngredientForm.js in order to manage the input state for the title and the amount.

```javascript
const [enteredTitle, setEnteredTitle] = useState('');
const [enteredAmount, setEnteredAmount] = useState('');
```

Once we have created the states we set accordingly our input element values and the onChange properties.

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

:exclamation:
Theoretically we could use only one state and set as a default value an object with a title and an amount field. However, this means that we would have to update both fields on each input since state updates using objects are not automatically merged. Of course for this small app, typing some extra lines of code wouldn't make a big difference, however in a more complex application that could be a big inconvenience. 

### Passing State Data Across Components

Now that we went through the basics of useState, it is time to make our app a bit more usefull. We need to find a way to save the ingredient created in the form and update the list of ingredients. Once we do that, it would be also a good idea to add a way to remove an item from the list!

To achieve our first goal, we could have a state in our Ingredients.js file which will be initialized as an empty array. This array will naturally keep our ingredients and will pass this array to our IngredientList component. Therefore, let's start by adding inside our Ingredients component the following piece of code.

```javascript
const [userIngredients, setUserIngredients] = useState([]);
```
The provided project file provides conveniently the IngredientList.js file so we can simply import it and pass our array of ingredients as props.

```javascript
return (
    <div className="App">
      <IngredientForm />

      <section>
        <Search />
        <IngredientList
          ingredients={userIngredients}
        />
      </section>
    </div>
  );
```
Now it would also a good idea to take a look inside the IngredientList component to see how it is structured. We can notice that it receives the props from its parent component which contains the array with the ingredients and then maps it into list items. We can also notice that props contain, or rather should contain, a function which hints us that should be also provided from the Ingredients component which should remove a list item. However, we will come back to it little later.

There is also another thing that stands out, which is that extra id field assigned on each list item. This id should probably be added when our form is submitted and a new ingredient is created. Therefore we will create a new function in the Ingredients component which will be passed down to the IngredientForm through props.

```javascript
const addIngredientHandler = (ingredient) => {
    setUserIngredients((prevIngredients) => [
      ...prevIngredients,
      { id: Math.random().toString(), ...ingredient },
    ]);
  };
```
In the above function notice that we are using an anonymous function to update our UserIngredients state. This way of updating the state ensures that React will use the latest copy of our ingredients array snapshot. We will discuss more about how React works behind the scenes in another blog post but for now, we are passing the previous state with the spread operator and we generate also a 'not-so-unique' id on the fly. Now we can finally pass the addIngredientHandler to our form component.

```javascript
<IngredientForm onAddIngredient={addIngredientHandler} />
```
and then use it inside our IngredientForm. A reminder though! Forms will try to reload the page therefore we create a small helper function which takes care of that problem and then calls our handler.
```javascript
const submitHandler = (event) => {
    event.preventDefault();
    props.onAddIngredient({ title: enteredTitle, amount: enteredAmount });
  };
/*****************************************/
<form onSubmit={submitHandler}>
```
Notice that we are passing as an argument to our handler an object with the same field names that the IngredientList component requires.
Now that we have access to the id field, we can create a new handler that removes an item from the rendered list! So we can go back to our Ingredients component and add the following.
```javascript
const removeIngredientHandler = (ingredientId) => {
    setUserIngredients((prevIngredients) =>
      prevIngredients.filter((ingredient) => ingredient.id !== ingredientId)
    );
  };
```
This function simply accepts an ingredient id and updates the state by using the filter function which creates a new copy of the array and uses an anonymous function to make the necessary comparison. Once we höök up this handler to our IngredientList component we are finally done with the basic logic of our app!
```javascript
<IngredientList
  ingredients={userIngredients}
  onRemoveItem={removeIngredientHandler}
/>
```
Now we can see in the updated diagram how our components are linked together, which props are passed down and also the states and the handlers declared in each of them.
![Diagram_after_UseState](https://user-images.githubusercontent.com/6706501/149245913-6df4677d-158a-4569-8851-793991aa2283.jpg)


## useEffect

## useCallback

