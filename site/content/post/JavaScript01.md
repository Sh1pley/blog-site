+++
date = "2017-05-22T00:21:26-06:00"
draft = false
title = "JavaScript01"
description = "Let's do some JavaScript"
+++

# JavaScript30
##### Updated June 3rd, 2017

So in my search and hunger for more JavaScript power I have scoured the internet for fun things and stumbled across Wes Bos and his [JavaScript30](https://javascript30.com/).

It's free, and pretty fun. He seems like a great teacher. Anyways, it does come with the completed work as well as the base html that you are supposed to add JavaScript to. *-no cheating*

---
## Drum Kit

Drum Kit:
![Site](/images/content/drumkit/site.jpg)

A html site that uses javascript to allow you to press keys listed on the site, hear sounds from files included in the directory and lastly use the included css to make the interaction with the listed keys light up and fade away.

---

### Lets get started

##### *Starter files include html base and css files. In order to make it cool you do JavaScript.. fun!

we start with this
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>JS Drum Kit</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="keys">
    <div data-key="65" class="key">
      <kbd>A</kbd>
      <span class="sound">clap</span>
    </div>
    <div data-key="83" class="key">
      <kbd>S</kbd>
      <span class="sound">hihat</span>
    </div>
    <div data-key="68" class="key">
      <kbd>D</kbd>
      <span class="sound">kick</span>
    </div>
    <div data-key="70" class="key">
      <kbd>F</kbd>
      <span class="sound">openhat</span>
    </div>
    <div data-key="71" class="key">
      <kbd>G</kbd>
      <span class="sound">boom</span>
    </div>
    <div data-key="72" class="key">
      <kbd>H</kbd>
      <span class="sound">ride</span>
    </div>
    <div data-key="74" class="key">
      <kbd>J</kbd>
      <span class="sound">snare</span>
    </div>
    <div data-key="75" class="key">
      <kbd>K</kbd>
      <span class="sound">tom</span>
    </div>
    <div data-key="76" class="key">
      <kbd>L</kbd>
      <span class="sound">tink</span>
    </div>
  </div>

  <audio data-key="65" src="sounds/clap.wav"></audio>
  <audio data-key="83" src="sounds/hihat.wav"></audio>
  <audio data-key="68" src="sounds/kick.wav"></audio>
  <audio data-key="70" src="sounds/openhat.wav"></audio>
  <audio data-key="71" src="sounds/boom.wav"></audio>
  <audio data-key="72" src="sounds/ride.wav"></audio>
  <audio data-key="74" src="sounds/snare.wav"></audio>
  <audio data-key="75" src="sounds/tom.wav"></audio>
  <audio data-key="76" src="sounds/tink.wav"></audio>

  <script>
  
  </script> 
</body>
</html>
```

Given my love for breaking things up I went ahead and broke the project into a separate file lets call it drum-kit.js.

``` html
<script type='text/javascript'src='drum-kit.js'></script>
```
We can include it like so.

Next step is to use plain ol' JavaScript, no JQuery no lodash. Just JavaScript. Also I have a goal of writing as much ES6 as possible.. and understanding the changes it brings *especially with 'this'*.

``` javascript
window.addEventListener('keydown', (event) => {
  const audio = document.querySelector(`audio[data-key="${event.keyCode}"]`);
  console.log(audio)
})
```

Open the Dev Tools, press some keys, see some stuff show up. *Magic*.
Let's explain some of it. addEventListener() is the vanilla way of JQuery's .on() methods. It simply listens for that event and then triggers the callback event. In our case we just pass it all into a function with the '=>' or [Arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions). Also we are making use of ES6's [Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). If you are coming from Ruby land, this is similar to string interpolation, using backticks **`** you can write a string out and using **${}** you can toss a JavaScript variable in and it will place its value inside the String. Neat! 
So far we have our window listening for us to press a button, let's make it a little more clear.

``` javascript
  const audio = document.querySelector(`audio[data-key="${event.keyCode}"]`);

  if (!audio) return;
  audio.currentTime = 0;
  audio.play();
```

This will check our pressed keys against our existing html, the return there will just end the function if there is not an audio found. If following along, go ahead and check functionality with and without [currentTime](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext/currentTime) set to 0. Throwing this in before the .play() function saves us from a headache of not being able to play the same sound back to back. If for some reason the sound is still playing when we try and press it again, the browser makes us wait. So resetting that time allows us to interrupt the sound if we should press the same key repeatedly. And finally [.play](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play) Lets us play a media file. Nice! However JavaScript also goes ahead and wraps that into a Promise for us. It will automagically play it if found, or give us a decent error if not.. which allows us to handle that and give a nice output to our user. *We will save that for another lesson.*

Adding everything into a giant function is ok.. I mean it will work. But given that JavaScript is fairly flexible, it does allow you to use many Object Oriented Programming ( OOP ) principles to help make your code maintainable, and readable.

Going down the Single Responsibility route we can change some of what we have to this:

``` javascript
window.addEventListener('keydown', (event) => {
  playAudio();
})

playAudio = () => {
  const audio = document.querySelector(`audio[data-key="${event.keyCode}"]`);

  if (!audio) return;
  audio.currentTime = 0;
  highlightKey();
  audio.play();
}
```

Looks a little better. Gives it a nice name like playAudio to help us remember what in the hell it was for. Also would allow us to further break up this file if needed and have just one file for listeners, one for functions to fire off on events being called.. etc.

---
---
**Updated June 3, 2017**

*Recap*
We covered using query selectors to grab a pressed key event and have it fire off a .play() method to trigger sounds. And put that in its on module / file allowing us to push javascript into kind of a Single Responsiblity (SRP) fashion. 

This doesn't quite satisfy all the requirements from the spec asked for in the JS30 lesson. So let's continue..

Above you may notice a function I have called highlightKey(), part of the spec for this mini project was to have the keys light up and fade away using a transition timer set in the css file. 

For the highlights I used something like this:

``` javascript
highlightKey = () => {
  const key = document.querySelector(`.key[data-key="${event.keyCode}"]`);
  if (key) key.classList.add('playing');
}
```

Which saves the keypress to a variable called key, and passes it through an if statement so that only the keys we want ( or ones that have a data-key attribute on them ) are used. Then simply calls JavaScript's .classList() which is similar to jQuery's .class(). And then simply adds a css class of 'playing' to that key item ( the square visual representation on the actual webpage ). We do this so that we can utilitize the included css 'playing' class that was given to us in the supplied html.

Great, so we have the something that should look like this when a key is pressed that matches the ones listed.
![highlight](/images/content/drumkit/highlight.jpg)

So thats getting the selected box highlighted! Given it was just using JavaScript to connect the two building blocks given to us, it is pretty cool to see how JavaScript can manipulate the DOM and connect pieces dynamically.

Onto fading this hightlight effect out. This one can be done simply by just building a new function inside of an event listener like so:

``` javascript
window.addEventListener('keyup', (event) => {
  const key = document.querySelector(`.key[data-key="${event.keyCode}"]`);
  if (key) key.classList.remove('playing');
})
```

Now to be fair that works fine, just has a event listener attached to the window. It listens for keyup events, checks to see that the keyup was attached to a key we want, and simply removes the playing class from the css.

This totally takes the highlight effect off... but doesn't really play with the building blocks we were given. And is, to be fair, kind of a cop-out or cheesy way to handle this. To customize the fade effect to be smoother than just the highlight vanishing, you would have to dig into the JavaScript here and add a timer of sorts, or some kind of fade out. Really if you were building this as a developer and didn't want to have to keep diving into your JavaScript, you would want to build this so that the effect could be adjusted through the css. That way a designer could mess around with it, adjust it, whatever they thought of and it would technically work.

Let's dive into that path..
And in doing so we can bring up this awesome example of *'this'* using both
old function syntax
``` javascript 
function() {

}
```
and
new arrow function syntax
``` javascript
() => {

}
```
---

ES6 brings a lot of niceties to JavaScript, makes it more pretty ( for lack of a better word ). Among these is the arrow function, you have seen me use this throughout this post. I prefer it, it looks cleaner and saves you from having function this, function that all over the damn place.

However there is one catch to this, the arrow function changes the game on how JavaScript's *'this'* is handled. Like a lot.

Examples!
Since we are on the fading part, we can clear this topic up and solve our lazy code from above in the same set of examples. Nice!

So to use the transition type, or to see what they have given us to work with. We can look into our CSS:

``` css
.key {
  border: .4rem solid black;
  border-radius: .5rem;
  margin: 1rem;
  font-size: 1.5rem;
  padding: 1rem .5rem;
  transition: all .07s ease; /* <--- Here we have the Transition set on our key class */
  width: 10rem;
  text-align: center;
  color: white;
  background: rgba(23,23,23,0.8);
  text-shadow: 0 0 .5rem black;
}
```
Using this transition settings fires off an event in the window.. We can use that. This can be done kind of like this:

``` javascript
transition = () => {
  const keys = Array.from(document.querySelectorAll('.key'))
  keys.forEach( (key) => {
    key.addEventListener('transitionend', fadeKey)
  })
}
```

Note we are still using the arrow function syntax. And what this does is makes an array out of all of our wanted keys using the [Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from). 
Then we want to start listening to each of these keys, so we use a [forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach). This iterates through our array of keys. *For each* key ( see what I did there ( ͡° ͜ʖ ͡°) ) we then want to attach a listener. The one we want is the end of the transition, so transitionend.. naturally. And here is where we add a callback function. We can call it fadeKey.

So then we can check that it all works by doing something like this.

``` javascript
fadeKey = () => { // <- arrow function still
  console.log(this)
}
```

Cool, it console logs something..
![window](/images/content/drumkit/thiswindow.jpg)
But its the window, and not the this we are used to? This is the important difference in the arrow function. While pretty, it adds a decent game changer to what *'this'* means. With an arrow function, *'this'* is a globally scoped item, and its the window. Which is great for some things, and in our case not great for some things. I mean we could dive into the window object and manipulate it to grab the specific html item we want.. but why. Right?

So we try something different..
``` javascript
function fadeKey(){
  console.log(this)
}
```
![this](/images/content/drumkit/this.jpg)
Thats the *'this'* we want! Full disclosure.. your console results will look a lot crazier than the image I have posted.. But we get to clean it up atleast. Tutorials right?!

So progress check, we have transition listener that is listening too an array of all our keys and it is listening for an end of the transition. This has a callback to send it over to fadeKey which should unhighlight our html items. For bonus, we got to cover this and this, with old function syntax and new arrow function syntax.. awesome.

Lets clean up our fadeKey selection
``` javascript
function fadeKey(){
  if (event.propertyName !== 'transform') return;

  this.classList.remove('playing');
}
```

So here is what we got. We send our event through an if, filter? something like that, and tell it to return or break out of all events except 'transform'. All this is really doing is selecting one of like 10 different events going on every time we press a key. And gives us one *'this'* to manipulate. Neat.

So for all that work we have the window listener looking like this at the end: 
``` javascript
window.addEventListener('keydown', (event) => {
  playAudio();
  transition();
})
```

and our whole JavaScript functionality is called and passed around each other by 2 functions. Woo!

Your whole .js file would look something like this:
``` javascript
window.addEventListener('keydown', (event) => {
  playAudio();
  transition();
})

transition = () => {
  const keys = Array.from(document.querySelectorAll('.key'))
  keys.forEach( (key) => {
    key.addEventListener('transitionend', fadeKey)
  })
}

playAudio = () => {
  const audio = document.querySelector(`audio[data-key="${event.keyCode}"]`);

  if (!audio) return;
  audio.currentTime = 0;
  highlightKey();
  audio.play();
}

highlightKey = () => {
  const key = document.querySelector(`.key[data-key="${event.keyCode}"]`);
  if (key) key.classList.add('playing');
}

function fadeKey(){
  if (event.propertyName !== 'transform') return;

  this.classList.remove('playing');
}
```

And that's all I have for this mini project.. which it turns out is much quicker to code than it is to actually explain. Feedback is awesome, if I screwed anything up in explaining I would love to talk about it and learn. 

I hope you found something useful in this blog.

##### * no affiliation with Wes Bos, nor am I receiving pay or compensation. I just dig his tutorial!