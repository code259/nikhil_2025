---
layout: page
title: About
permalink: /about/
---

<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
<h2 class="subtitle">About Me</h2>
<p class="para">My name is Nikhil Maturi, and I am a student at Del Norte High School in San Diego, California. I am currently apart of AP Computer Science Principles. I enjoy playing sports, spending time with friends and family, and watching movies.</p>
<h2 class="subtitle">Do you share my interests?</h2>
<p class="para">Click on the basketball!</p>
<div class="interests">
    <img id="ball" class="image" onclick="imageInteraction()" src="https://cdn.pixabay.com/photo/2022/04/09/15/10/basketball-7121617_640.jpg" alt="ball">
    <img class="image" src="https://ecampusontario.pressbooks.pub/app/uploads/sites/2109/2021/11/programming-gb0e197598_1920.jpg" alt="programming">
</div>

<h2 class="subtitle">Do we like the same foods?</h2>

<div id="carouselExample" class="carousel slide">
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="https://cdn77-s3.lazycatkitchen.com/wp-content/uploads/2021/10/roasted-tomato-sauce-portion-800x1200.jpg" class="d-block w-100 img" alt="...">
    </div>
    <div class="carousel-item">
      <img src="https://images.seattletimes.com/wp-content/uploads/2024/04/04082024_OpEd-Potatoes_124536.jpg?d=2040x1488" class="d-block w-100 img" alt="...">
    </div>
    <div class="carousel-item">
      <img src="https://upload.wikimedia.org/wikipedia/commons/9/91/Pizza-3007395.jpg" class="d-block w-100 img" alt="...">
    </div>
  </div>
  <button class="carousel-control-prev" type="button" data-bs-target="#carouselExample" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Previous</span>
  </button>
  <button class="carousel-control-next" type="button" data-bs-target="#carouselExample" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Next</span>
  </button>
</div>

<h2>Story</h2>
<p class="para">
1. Born in San Diego 🚑<br>
2. Went to stone ranch elementary school until third grade 3️⃣<br>
3. Moved to India for family from 4th to 6th grade ⚅<br>
4. Moves back to the states after Covid 🦠<br>
5. Went to oak valley middle school for 7th and 8 grade 🕗<br>
6. Now in Del Norte High School 🏫 <br>
7. Future and beyond: interests in Artificial Intelligence, Robotics, Bioinformatics, etc.
</p>

<p id="timer" class="para"></p>

<h2>Origins</h2>
<div class="grid-container" id="grid_container">
    <!-- content will be added here by JavaScript -->
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>

<script>
    // 1. Make a connection to the HTML container defined in the HTML div
    var container = document.getElementById("grid_container"); // This container connects to the HTML div

    // 2. Define a JavaScript object for our http source and our data rows for the Living in the World grid
    var http_source = "https://upload.wikimedia.org/wikipedia/commons/";
    var living_in_the_world = [
        {"flag": "0/01/Flag_of_California.svg", "greeting": "Hey", "description": "California - 12 years"},
        {"flag": "4/41/Flag_of_India.svg", "greeting": "Hi", "description": "India - 3 years"},
    ]; 
    // 3a. Consider how to update style count for size of container
    // The grid-template-columns has been defined as dynamic with auto-fill and minmax

    // 3b. Build grid items inside of our container for each row of data
    for (const location of living_in_the_world) {
        // Create a "div" with "class grid-item" for each row
        var gridItem = document.createElement("div");
        gridItem.className = "grid-item";  // This class name connects the gridItem to the CSS style elements
        // Add "img" HTML tag for the flag
        var img = document.createElement("img");
        img.src = http_source + location.flag; // concatenate the source and flag
        img.alt = location.flag + " Flag"; // add alt text for accessibility

        // Add "p" HTML tag for the description
        var description = document.createElement("p");
        description.textContent = location.description; // extract the description

        // Add "p" HTML tag for the greeting
        var greeting = document.createElement("p");
        greeting.textContent = location.greeting;  // extract the greeting

        // Append img and p HTML tags to the grid item DIV
        gridItem.appendChild(img);
        gridItem.appendChild(description);
        gridItem.appendChild(greeting);

        // Append the grid item DIV to the container DIV
        container.appendChild(gridItem);
    }
</script>

<script>
  function lifeTimer(){
    var timer = document.getElementById("timer")
    const now = new Date();
    const then = new Date("2009-01-20");
    const diff = now.getTime() - then.getTime();
    time = Math.floor(diff / 1000);
    timer.innerHTML = `Number of seconds ⏰ I've been alive: ${time}`;
  }
  intervalId = setInterval(lifeTimer, 1000);
</script>

<script>
    function imageInteraction(){
        document.getElementById("ball").style.filter="invert(100%)";
        var audio = new Audio('../assets/ball.mp3');
        audio.play();
    }
</script>

<style>
    body {
        cursor: url('../assets/smiley.png'), auto;
        font-family: Arial, Helvetica, sans-serif;
    }
    .title {
        font-family: Arial, Helvetica, sans-serif;
    }
    .interests {
        display: flex;
        width: 500px;
    }
    .image {
        padding-right: 20px;
    }
    .subtitle {
        padding-bottom: 20px;
        padding-top: 20px;
    }
    .img {
      height: 1000px;
    }
    .para {
      font-size: 20px;
      color: white;
    }
    .grid-container {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); /* Dynamic columns */
      gap: 10px;
    }
    .grid-item {
      text-align: center;
    }
    .grid-item img {
      width: 100%;
      height: 100px; /* Fixed height for uniformity */
      object-fit: contain; /* Ensure the image fits within the fixed height */
    }
    .grid-item p {
      margin: 5px 0; /* Add some margin for spacing */
    }
</style>