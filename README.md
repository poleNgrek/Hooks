## Introduction
 
This "blog" is going to be the beginning of a small series about my intense and fast-paced learning experience with React using [this tutorial series](https://www.udemy.com/course/react-the-complete-guide-incl-redux/). The aim is to try to describe some key concepts as well as try to present some more advanced React topics in a simple and, hopefully, humorous way!

The problem begins with me, and probably others like me, who refuse to give up their comfortable back-end development corner and do the leap towards a full-stack mindset. For some years, I actively tried to avoid going into too much depth with frontend libraries and frameworks. Maybe it is some kind of trauma from the earlier days when I had to use JQuery or vanilla javascript to work with the frontend where there was always a mine waiting for you to step on it to make a mess of your DOM and CSS. Maybe it is also the notoriety for the steep learning curve that comes with some libraries (React *cough* *cough*) which makes one less inclined to invest 900-1000 hours of development and studying to be able to feel comfortable with said library. Whichever the case might be, one thing is certain! Libraries were created to make our lives easier, more productive and hopefully more fun (can frontend development be ever fun?!).

Having said all that, let start talking about the latest most important feature of React, Hooks! (Or Hööks as I enjoy reading it in my head).


## React Hööks

React hooks, introduced with version 16.8, unlocked the full potential of functional components by giving them the ability to manage state, handle side effects and some other neat stuff which were previously only doable through a class-based component. That's cool but what are these hooks really?


> Hooks are special functions that can be used only in functional-components (and custom hooks) and offer some specific extra functionality (like using a state!). You can spot them by their name since they all start with the word "use".

Of course this is not an official definition of hooks but it encapsulates their essence as best as possible!
So, how many hooks are there you may ask. Well, the short answer is 9 whereof 3 are considered basic (or just mega-useful). What about the long answer then? If you noticed in the definition of hooks that I gave earlier, I mentioned something about custom hooks which means that we can have as many hooks as we see fit! But I will talk more about them in part two of this blog.

So, since knowledge is of no value unless you put it into practice, we will explore hooks while working on a small project. Feel free to download the zip folder attached and run `npm install` and then `npm start` inside your project folder. [HööksSummary.zip](https://github.com/poleNgrek/Hooks/files/7863823/HooksSummary.zip)

### A brief description of the project

This is a very simple app that eventually will offer to the user the ability to insert a new ingredient through a form, search for ingredients and finally it will render a list of ingredients. Progressively some extra functionalities will be added as we progress but the main idea will not change.

<img width="872" alt="app" src="https://user-images.githubusercontent.com/6706501/149357191-0d12b3f6-e269-4892-87c9-8d7b34091ec0.png">

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

![hooks_2](https://user-images.githubusercontent.com/6706501/149251792-877ba7d6-d295-4b27-bbfa-897b0eac21a3.jpg)


## useEffect

Now it is time to discover another very important hook, the useEffect! Like before with useState we need to import it from React.
```javascript
import { useState, useEffect } from 'react';
```
This hook, as its name suggests, handles *side effects * in our component and it gets "activated" once the page loads for the first time. Some examples of what is considered side effect, are among others, fetching requests, using timer functions like setTimeout(), and in general tasks that have nothing to do with the rendering of the component. That's also the reason why it gets called **AFTER** the component has been rendered. It might be useful to think of useEffect as a tool which keeps rendering and side-effect logic independent from each other.

The useEffect hook accepts two arguments
1. A callback function that handles the side effect logic
2. An optional array of external dependencies
```javascript
useEffect(() => {
    // put your code here
}, dependencies)
```
While it is somewhat straight-forward to understand when and how to use this hook, there are some finer details and dangers that we will discuss as we continue working with our app.

### Setting up firebase

We are gonna need a "dummy backend" in order to continue with the next step of this tutorial. We can use Google's firebase which is free and easy to set up. To create a new Firebase project use [this link](https://console.firebase.google.com) which leads to the firebase console.
From there click on the 'Create a project' button and go through the simple creation steps where you will be asked to just provide a project name and the option to enable the Google Analytics for this project (which is not needed for this project). Once you are through the creation process you will be greeted with the following screen.

![firebase1](https://user-images.githubusercontent.com/6706501/149356323-5a449529-3723-4714-947a-8f9d2155e40e.jpg)

Then, on the left sidebar under the Build option, click on the "RealTime Database" option as the red (barely visible arrow :sweat_smile:) points. Then click on the "Create Database" option, select a server closer to your location and choose the "Start in test mode" option under the security rules.

After the setup is completed, you should be greeted with the following screen.

![firebase2](https://user-images.githubusercontent.com/6706501/149356379-2d0631d9-a60f-43b5-8800-99677e602971.jpg)

Here we have access to the URL needed to access our database which will be created dynamically! The final step is to access the Rules tab and set the read and write values to true. 

As we will be adding ingredients to the database, we will get a better understanding of how the "tables" are created. But for now we are ready to go back to coding! 

### Sending http requests

To avoid using other libraries, such as [Axios](https://axios-http.com/docs/intro), we will use the [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) which is provided by our beloved javascript and built-into modern browsers. 

Having said that, we can finally test whether our our form can successfully save a new ingredient in our database. To do that we will update our addIngredientHandler in the Ingredients component like this.

```javascript
  const addIngredientHandler = (ingredient) => {
    fetch(
      "https://react-http-dd947-default-rtdb.europe-west1.firebasedatabase.app/ingredients.json",
      {
        method: "POST",
        body: JSON.stringify(ingredient),
        headers: { "Content-Type": "application/json" },
      }
    )
      .then((response) => {
        return response.json();
      })
      .then((responseData) => {
        setUserIngredients((prevIngredients) => [
          ...prevIngredients,
          { id: responseData.name, ...ingredient },
        ]);
      });
  };
```
Since this tutorial is focused on React Hooks, I will not go into how fetch and promises work in javascript but it is worthwhile mentioning two small details in our code. The first one is that we have added the "ingredients.json" at the end of the URL we use inside the fetch function. This is just a convention that Google uses to be able to identify which table to access. In this case, we letting firebase know that we want to access, or create, a new table called ingredients and that's all we care to know for now!

The second detail is that we have changed the way we assign an id to the ingredient! Firebase returns a response which contains an automatically generated id which we can access by using the field "name".

If you have followed the tutorial so far, you can now add a new ingredient and check our database which should look like this now.

### useEffect and loading data

Before we demonstrate how useEffect works and why it is so valuable, let us make a small (but potentially extremely costly) experiment. We will use the fetch function, just under the useState declaration, to send a request to the database and get our list of ingredients. Then we will use the setUserIngredients function to update our state with the data we got from the request!

```javascript
fetch(
    "https://react-http-dd947-default-rtdb.europe-west1.firebasedatabase.app/ingredients.json"
  )
    .then((response) => response.json())
    .then((responseData) => {
      const loadedIngredients = [];
      for (const key in responseData) {
        loadedIngredients.push({
          id: key,
          title: responseData[key].title,
          amount: responseData[key].amount,
        });
      }
      setUserIngredients(loadedIngredients);
    });
```
If you actually save your file and let this code run, you will end up in an infinite-loop sending countless requests to the server!:clown: You can see this happening in the network tab of your browser. But why is this happening?!

Well the answer is simple. Every time we update the state of a component (by using the setUserIngredients), the component re-renders and every time our component re-renders it sends a request to the server!

I can already hear you thinking that we should finally use our useEffect hook! So let's just use the syntax that we learned before and call our fetch function. Just like before under our useState declaration inside the Ingredients component, we can add this piece of code!

```javascript
    useEffect(() => {
    fetch(
      "https://react-http-dd947-default-rtdb.europe-west1.firebasedatabase.app/ingredients.json"
    )
      .then((response) => response.json())
      .then((responseData) => {
        const loadedIngredients = [];
        for (const key in responseData) {
          loadedIngredients.push({
            id: key,
            title: responseData[key].title,
            amount: responseData[key].amount,
          });
        }
        setUserIngredients(loadedIngredients);
      });
  });
```

BOOM!! :bomb: :fire: We are once again stuck in an endless loop of requests and some network administrator is probably very annoyed at this point! But why did that happen?

This time the answer is little bit longer. We mentioned earlier that useEffect accepts an optional second argument which is an array of external dependencies. By external dependencies we mean functions or variables declared outside useEffect. In our case we don't use anything external except from the setUserIngredients. However, React guarantees that anything return from useState will never change and therefore we can omit adding it in the dependency array. So what is left to be done is just to add an empty array! With an empty array, the useEffect will run only once after the component renders. 

So useEffect can run endlessly if no array is provided, only once if an empty array is provided or sometimes depending on whether the provided external dependencies are updated or not.

```javascript
 useEffect(() => {
   fetch(
     "https://react-http-dd947-default-rtdb.europe-west1.firebasedatabase.app/ingredients.json"
   )
     .then((response) => response.json())
     .then((responseData) => {
       const loadedIngredients = [];
       for (const key in responseData) {
         loadedIngredients.push({
           id: key,
           title: responseData[key].title,
           amount: responseData[key].amount,
         });
       }
       setUserIngredients(loadedIngredients);
     });
 }, []);
```

If you run the above code now (it won't crush your browser, I promise) you can see in the network tab that only one request was sent to our server and that the ingredient list is updated correctly.

## useCallback
