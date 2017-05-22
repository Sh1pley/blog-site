+++
categories = [JavaScript]
date = "2017-05-21T23:33:35-06:00"
tags = [ES6, this, not-this, arrow function]
title = "JavaScript01"

+++

# JavaScript30

So in my search and hunger for more JavaScript power I have scoured the internet for fun things and stumbled across Wes Bos and his [JavaScript30](https://javascript30.com/).

It's free, and pretty fun. He seems like a great teacher. Anyways, it does come with the completed work as well as the base html that you are supposed to add JavaScript to. *-no cheating*

---
## Drum Kit

Drum Kit:
![Site](/images/content/drumkit/site.jpg)

A html site that uses javascript to allow you to press keys listed on the site, hear sounds from files included in the directory and lastly use the include css to make the interaction with the listed keys light up an fade away.

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

Next step is to use plain ol' JavaScript, no JQuery no lodash. Just JavaScript. Also I have a goal of writing as much ES6 as possible.. and understanding the changes it brings *especially with **this***.

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

Finally because I like functions and making things readable we can change our drum-kit.js to something like this..

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

This will be continued for the css highlighting and transitions..
If you haven't already feel free to sign up for his JavaScript30 and follow along or complete on your own.

##### * no affiliation with Wes Bos, nor am I receiving pay or compensation. I just dig his tutorial!