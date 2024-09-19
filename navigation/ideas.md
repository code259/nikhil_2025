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
    background-color: black;
    border-radius: 50%;
    position: absolute;
  }
  .content {
    z-index: 999;
  }
</style>
<style>
  #score {
    position: absolute;
    top: 10px;
    left: 10px;
    font-size: 24px;
    color: white;
    z-index: 1;
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
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<script>
        let scene, camera, renderer, ball, plane, cubes = [], obstacles = [], score = 0;
        let ballSpeed = 0.2;
        let direction = { x: 0, y: 0 };
        const cubeCount = 5;
        const obstacleCount = 3;

        // Initialize the game
        function init() {
            // Scene, Camera, Renderer setup
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 5, 10);
            
            renderer = new THREE.WebGLRenderer();
            renderer.setSize(window.innerWidth, window.innerHeight);
            document.body.appendChild(renderer.domElement);

            // Add light
            const light = new THREE.DirectionalLight(0xffffff, 1);
            light.position.set(10, 10, 10).normalize();
            scene.add(light);

            // Create the plane (ground)
            const planeGeometry = new THREE.PlaneGeometry(100, 100);
            const planeMaterial = new THREE.MeshStandardMaterial({ color: 0x228b22 });
            plane = new THREE.Mesh(planeGeometry, planeMaterial);
            plane.rotation.x = -Math.PI / 2;
            scene.add(plane);

            // Create the player ball
            const ballGeometry = new THREE.SphereGeometry(1, 32, 32);
            const ballMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 });
            ball = new THREE.Mesh(ballGeometry, ballMaterial);
            ball.position.set(0, 1, 0);
            scene.add(ball);

            // Generate random cubes
            for (let i = 0; i < cubeCount; i++) {
                createCube();
            }

            // Generate random obstacles
            for (let i = 0; i < obstacleCount; i++) {
                createObstacle();
            }

            // Listen for key events
            document.addEventListener('keydown', onKeyDown);
            document.addEventListener('keyup', onKeyUp);

            // Start game loop
            animate();
        }

        // Handle keydown events
        function onKeyDown(event) {
            switch (event.key) {
                case 'W':
                    direction.y = -ballSpeed; // Move forward
                    break;
                case 'S':
                    direction.y = ballSpeed; // Move backward
                    break;
                case 'ArrowA':
                    direction.x = -ballSpeed; // Move left
                    break;
                case 'ArrowD':
                    direction.x = ballSpeed; // Move right
                    break;
            }
        }

        // Handle keyup events (stop moving when key is released)
        function onKeyUp(event) {
            switch (event.key) {
                case 'ArrowUp':
                case 'ArrowDown':
                    direction.y = 0; // Stop vertical movement
                    break;
                case 'ArrowLeft':
                case 'ArrowRight':
                    direction.x = 0; // Stop horizontal movement
                    break;
            }
        }

        // Create a collectable cube at a random position
        function createCube() {
            const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
            const cubeMaterial = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
            const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
            cube.position.set(randomInRange(-20, 20), 0.5, randomInRange(-20, 20));
            scene.add(cube);
            cubes.push(cube);
        }

        // Create a moving obstacle
        function createObstacle() {
            const obstacleGeometry = new THREE.BoxGeometry(2, 2, 2);
            const obstacleMaterial = new THREE.MeshStandardMaterial({ color: 0x0000ff });
            const obstacle = new THREE.Mesh(obstacleGeometry, obstacleMaterial);
            obstacle.position.set(randomInRange(-20, 20), 1, randomInRange(-20, 20));
            scene.add(obstacle);
            obstacles.push(obstacle);
        }

        // Helper function to get a random value in a range
        function randomInRange(min, max) {
            return Math.random() * (max - min) + min;
        }

        // Animation loop
        function animate() {
            requestAnimationFrame(animate);

            // Move the ball
            ball.position.x += direction.x;
            ball.position.z += direction.y;

            // Check for collision with cubes
            cubes.forEach((cube, index) => {
                if (ball.position.distanceTo(cube.position) < 1.5) {
                    scene.remove(cube);
                    cubes.splice(index, 1);
                    score += 10;
                    document.getElementById('score').innerText = `Score: ${score}`;
                    createCube(); // Replace collected cube
                }
            });

            // Move obstacles and check for collision
            obstacles.forEach(obstacle => {
                obstacle.position.x += Math.sin(Date.now() * 0.001) * 0.05; // Sway motion
                obstacle.position.z += Math.cos(Date.now() * 0.001) * 0.05;

                if (ball.position.distanceTo(obstacle.position) < 1.5) {
                    alert("Game Over! Final Score: " + score);
                    location.reload(); // Restart game
                }
            });

            renderer.render(scene, camera);
        }

        // Resize the canvas on window resize
        window.addEventListener('resize', function() {
            renderer.setSize(window.innerWidth, window.innerHeight);
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
        });

        // Initialize the game
        init();
</script>

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
    y: '50px',
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
