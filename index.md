---
layout: base
title: Student Home
description: Home Page
image: /images/mario_animation.png
hide: true
---

<!-- Liquid:  statements -->

<!-- Include submenu from _includes to top of pages -->
{% include nav/home.html %}
<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %}

<h2>Cookie Clicker</h2>
<img id="cookie" onclick="cookiePress()" src="assets/ugly-cookie.png">
<h3 id="counter">Counter:</h3>
<h3 id="rate">Cookie Rate:</h3>
<h3 id="grandma-list">Grandma Army: </h3>
<div>
  <button onclick="grandmaPress()" id="grandma-btn">Grandma Power</button>
</div>

<h2>Other</h2>
<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules,
        define how HTML elements look
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<script>
  // counter_value = 0;
  if (localStorage.getItem("counter_value") === null) {
    localStorage.setItem("counter_value", 0);
    localStorage.setItem("cookie_rate", 1);
  }

  function cookiePress(){
    // counter_value = Number(counter_value) + 1
    counter_storage = localStorage.getItem("counter_value");
    cookie_rate = localStorage.getItem("cookie_rate");
    localStorage.setItem("counter_value", Number(counter_storage) + Number(cookie_rate));

    counter = document.getElementById("counter");
    counter.innerHTML = `Counter: ${Number(counter_storage) + Number(cookie_rate)}`

    var audio = new Audio('assets/crunch.mp3');
    audio.play();
  }

  function grandmaPress(){
    var grandma_increase = 5;
    var grandma_price = 10;
    counter_storage = localStorage.getItem("counter_value");
    cookie_rate = localStorage.getItem("cookie_rate");
    localStorage.setItem("cookie_rate", Number(cookie_rate) + grandma_increase);

    if (Number(counter_storage) > grandma_price) {
      rate = document.getElementById("rate");
      counter = document.getElementById("counter");

      rate.innerHTML = `Cookie Rate: ${Number(cookie_rate) + grandma_increase}`;

      localStorage.setItem("counter_value", Number(counter_storage) - grandma_price);
      counter.innerHTML = `Counter: ${Number(counter_storage) - grandma_price}`
      grandma_number = (Number(cookie_rate) + grandma_increase - 1)/5
      grandmaArmy_string = "👵".repeat(grandma_number);
      document.getElementById("grandma-list").innerHTML = `Grandma Army: ${grandmaArmy_string}`
    }
  }

  window.onload = function() {
    var counter_storage = localStorage.getItem("counter_value");
    var cookie_rate = localStorage.getItem("cookie_rate");
    document.getElementById("counter").innerHTML = `Counter: ${counter_storage}`;
    document.getElementById("rate").innerHTML = `Cookie Rate: ${cookie_rate}`;

    var grandma_increase = 5;
    grandma_number = (Number(cookie_rate) + grandma_increase - 1)/5
    grandmaArmy_string = "👵".repeat(grandma_number);
    document.getElementById("grandma-list").innerHTML = `Grandma Army: ${grandmaArmy_string}`
  };
</script>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startPuffing();
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.2 * scale})`;
    mario.startResting();
  });

</script>

My journey starts here
# This is Nikhil's student webpage hosted on github 🐱🦍🦍🦍🦧🦧🦧🦧

[san diego](https://www.sandiego.org/-/media/images/sdta-site/articles/about-sd/1233x860/sdta-articles-11917-1230x860-0000s-0000-about-sd.jpg?bc=white&h=500&w=700&c=1)