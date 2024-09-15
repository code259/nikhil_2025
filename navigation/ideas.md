---
layout: page
title: Ideas
permalink: /ideas/
---

<div id="particles-js"></div>
<h2 class="floating-text">My future plans</h2>
<br>
<div class="content">
  <ul>
    <li><p>Improve website design</p></li>
    <li><p>Add javascript animations (i.e the ball animation)</p></li>
    <li><p>Add many more interactive elements (try press on the background it moves)</p></li>
    <li><p>Voice synthesis (try saying "sound" to mic when you reload the page)</p></li>
    <li><p>Many Others...</p></li>
  </ul>
  <div class="ball"></div>
</div>

<style>
  .floating-text {
    font-size: 3em;
    font-weight: bold;
    margin-top: 50px;
    opacity: 0;
  }
  .ball {
    width: 50px;
    height: 50px;
    background-color: white;
    border-radius: 50%;
    position: absolute;
  }
  .content {
    z-index: 2;
  }
</style>

<style>
  #particles-js {
      position: absolute;
      width: 100%;
      height: 100vh;
      top: 0;
      left: 0;
      z-index: 0;
  }
</style>


<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.11.5/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
<script>
  anime({
    targets: '.floating-text',
    translateY: [-50, 0],
    opacity: [0, 1],
    easing: 'easeOutQuad',
    duration: 2000,
    loop: false
  });
  gsap.to('.ball', {
    y: '200px',
    duration: 1,
    ease: 'bounce',
    repeat: -1,
    yoyo: true
  });
</script>

<script>
        particlesJS("particles-js", {
            "particles": {
                "number": {
                    "value": 100,
                    "density": {
                        "enable": true,
                        "value_area": 800
                    }
                },
                "color": {
                    "value": "#ffffff"  /* Set particle color */
                },
                "opacity": {
                    "value": 0.75,  /* Set particle opacity */
                    "random": false,
                    "anim": {
                        "enable": false
                    }
                },
                "size": {
                    "value": 3,
                    "random": true
                },
                "line_linked": {
                    "enable": true,
                    "distance": 150,
                    "color": "#ffffff",
                    "opacity": 0.2,  /* Set line opacity */
                    "width": 1
                },
                "move": {
                    "enable": true,
                    "speed": 2,
                    "direction": "none",
                    "random": false,
                    "straight": false,
                    "out_mode": "out",
                    "bounce": false
                }
            },
            "interactivity": {
                "detect_on": "canvas",
                "events": {
                    "onhover": {
                        "enable": true,
                        "mode": "repulse"
                    },
                    "onclick": {
                        "enable": true,
                        "mode": "push"
                    },
                    "resize": true
                }
            },
            "retina_detect": true
        });
</script>

<script>
  const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
  recognition.onresult = (event) => {
    const speechResult = event.results[0][0].transcript.toLowerCase();
    console.log(speechResult)
    if (speechResult.includes("sound")) {
      var audio = new Audio('../assets/duck.mp3');
      audio.play();
    }
  };
  recognition.start();
</script>
