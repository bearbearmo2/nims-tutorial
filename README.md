# Nims Tutorial In Node.js

---

## What You Will Need

In this tutorial I will explain every component used, however, I will **not** explain **syntax** or go into depth about why I'm using each component. So what I expect you to need to be able to complete this tutorial is:

+ Basic Knowledge Of Node.js Or JavaScript
+ A Repl.it Account
+ A Node.js Repl

---

## What You Will Make

Throughout the tutorial I will be teaching you how to make a **Nims** game. In a Nims game you and another player (computer) take turns to remove 1, 2 or 3 from a total of 21, however, you can use any total you like (as long as it is **not a multiple of 4**).To win the game you have to take the last number (either 1, 2 or 3) from the total so that you finish with 0. Example:

...\
...\
...\
5 left,\
player takes 1,\
4 left,\
computer takes 1,\
3 left,\
player takes 3,\
player wins.\

---

## Istalling Packages

One of the great things about repl.it is that they make it easy for you to install packages in node.js. The one package we need to install is the **readline-sync** package; just go to the packages icon on the far left of your screen - bellow files and above settings - search for **readline-sync**. If you type the whole thing in it should be the very first package underneath the the search bar, click on it and then click the green cross to install it. And finaly go to the files icon - above the packages icon - you should notice a new file has been added - packages.json - select the one above that called **index.js**. Welldone you have successfully completed the first step.

---

## Introductions

The very first thing you need to do is introduce the player to the game, you can say whatever you like, but I prefere to state the rules: "pick a number from 1 to 3 to take from 21".\
To show this In the console, we must use ``` console.log() ``` like so:

```node.js
console.log("pick a number from 1 to 3 to take from 21")
```
Run the code now and you should see your text appear in the console to the right.

---

## Going Loopy

Obviosly we will want our code for the game to repeat itself each turn, for this we create a for loop. we use a for loop because it allows us to shorten our code significantly as we can do 3 things inside it: define a variable; condition for when the loop should stop; execution of code. What we need to put inside the for loop is:

+ define the total that we take from (21) - ```nimstot = 21 ,```
+ define the turn counter (starts on 0) - ```turn = 0 ;```
+ condition for when the foor loop should stop (nimstot < 0) - ```nimstot > 0 ;```
+ add onto the turn counter - ```turn++```

So our for loop should look like this:

```node.js
for (nimstot = 21, turn = 0 ; nimstot > 0 ; turn++) {
}
```
---

## Youngest Goes First

Because we are going to make the computer so overpowered, the player needs to have the upper hand from the start or the computer will win instantly. So, the player gets the first go.\

First of all, we need to make an **if statement** that checks whos go it is. for this we are going to use **modular arithmatic**, modular arithmatic basically returns the remainder of the devision of two numbers, for example, 21/4 = 5 remainder **1** 21 mod 4 = **1** . We will use modular arithmatic for the computers stratagy, but for now we'll use it to tell whos go it is. An even number mod 2 = 0, we'll use this bassis to check if the turn is even or odd. Even means players go, odd means computers go. **Mod** is represented as a **%** in node.js, so our if statement looks like this:

```node.js
 if (turn % 2 === 0) {
  }
```
---

## Trust Issues

I'm going to skip ahead and say that anyone playing this game is going to cheat. To safe gaurd against that we need to do several things. But first, it's time to ask the big questions in life. What do you pick 1, 2 or 3. To ask this in the console we need to do our first bit of coding exclusive to node.js: ```require('readline-sync').question("enter number:")``` I'm not going to go into depth about what this does, but it basically asks a question in the console and waits for you answer. To use the players answer we need to assign it  to a new variable ```let num```  however this is not enough as the answer we receive to the question will be a string (inside speach marks) to get around this we use ``` parseInt() ``` . So our if statement from before should now look like this:

```node.js
  if (turn % 2 == 0) {
    let num = parseInt(require('readline-sync').question("enter number:"))
  }
```
Getting back to the problem of cheating, we need to do one more line that checks if:

+ The number is not bellow 1 (num < 1)
+ The number is not above 3 (num > 3)
+ The number is a number (isNaN(num))

Instead of an if statement we are going to use a **conditional (ternary) operator** , this basically allows us to do an if statement and an else statement in one line. condition ? if true : else. That means our next line looks like this:

```node.js
    num < 1 || num > 3 || isNaN(num) ? turn-- : nimstot -= num;
```

What this does is if the answer to the question is not acceptable, it stops the computer having a go, but if the answer is acceptable it removes your number from the total.

Our entirety of code should now look like this:

```node.js
console.log('pick a number from 1 to 3 to take from 21')
for (nimstot = 21, turn = 0; nimstot > 0; turn++) {
  if (turn % 2 == 0) {
    let num = parseInt(require('readline-sync').question("enter number:"))
    num < 1 || num > 3 || isNaN(num) ? turn-- : nimstot -= num;
  }
}
```
---

## Creating A Monster

You may know that there is an easy way of winning every single game you play of nims (if the other person doesn't know it too).
I am not going to go into detail but the bassis for it is that at the end of every go you have, you need to leave it on a **multiple of 4**. This means we can use modular arithmatic and a conditional (ternary) operator to create one line of devastation:

```node.js
nimstot -= nimstot % 4 == 0 ? Math.ceil(Math.random() * 3) : nimstot % 4
```
Basically this monstrous line checks if the total left is a multiple of 4, if it is it takes a random number because there is no strategic way of winning, however if it is, it uses modular arithmatic to make the total a multiple of 4 - if this happens you lose.

We now need to put this line in an else statement so that it only does it when it's the computers turn:

```node.js
  else {nimstot -= nimstot % 4 == 0 ? Math.ceil(Math.random() * 3) : nimstot % 4}
```
---
## Save The Worst For Last

The last and most important part of this code is showing the total that is left and if the total is 0, who won.\
Luckily for you, I have compressed this into one line that is easy to understand:

```node.js
  console.log(nimstot <= 0 ? turn % 2 == 0 ? "player won!!!" : "computer won, :(" : nimstot + " left")
```
Because no matter the output we want to log it in the console, we can do ```console.log()``` around the entire thing, inside the console.log() we need to check if the total is smaller or equal to 0, if it is we then check whos turn it is, and return the appropriate output, however if the game has not yet reached it's conclusion, the output needs to be the total that is left.

Your final code should be 9 lines long and look like this:

```node.js
console.log('pick a number from 1 to 3 to take from 21')
for (nimstot = 21, turn = 0; nimstot > 0; turn++) {
  if (turn % 2 == 0) {
    let num = parseInt(require('readline-sync').question("enter number:"));
    num < 1 || num > 3 || isNaN(num) ? turn-- : nimstot -= num;
  }
  else {nimstot -= nimstot % 4 == 0 ? Math.ceil(Math.random() * 3) : nimstot % 4}
  console.log(nimstot <= 0 ? turn % 2 == 0 ? 'player won!!!' : 'computer won, :(' : nimstot + ' left');
}
```
You can now run your code and try to defeat the monster you have created.

---

# Thankyou

I would Like to thankyou very much for reaching the end of this tutorial. Please have a look at **How To Make A Puzzle Platformer** if you would like to make a more visual based game. Please upvote, I would appreciate it very much, Thankyou.

Created By: Bearbearmo.
