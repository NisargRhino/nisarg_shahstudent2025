---
layout: base
title: Student Home 
description: Home Page
hide: true
---

<!-- Submenu -->
<nav style="background-color: #2c3e50; padding: 10px 0; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);">
    <ul style="list-style: none; margin: 0; padding: 0; display: flex; justify-content: center; gap: 20px;">
        <li><a href="http://127.0.0.1:4100/nisarg_shahstudent2025/2024/09/03/MiniGame.html" style="text-decoration: none; color: #ecf0f1; font-size: 18px;">MiniGame</a></li>
        <li><a href="http://127.0.0.1:4100/nisarg_shahstudent2025/2024/08/28/Hacks(Summary).html" style="text-decoration: none; color: #ecf0f1; font-size: 18px;">Sprints</a></li>
        <li><a href="http://127.0.0.1:4100/nisarg_shahstudent2025/2024/08/25/captures.html" style="text-decoration: none; color: #ecf0f1; font-size: 18px;">Captures</a></li>
        <li><a href="http://127.0.0.1:4100/nisarg_shahstudent2025/" style="text-decoration: none; color: #ecf0f1; font-size: 18px;">Home Page</a></li>
        <li><a href="http://127.0.0.1:4100/nisarg_shahstudent2025/" style="text-decoration: none; color: #ecf0f1; font-size: 18px;">Home Page</a></li>
    </ul>
</nav>

<div style="text-align: center; margin-top: 50px;">
    <h2 style="font-family: 'Arial', sans-serif; color: #fff; font-size: 28px; margin-bottom: 20px; text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);">Welcome to My Project Hub</h2>
    <p style="font-family: 'Arial', sans-serif; color: #ecf0f1; font-size: 18px; max-width: 600px; margin: 0 auto 40px; text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);">
        This is where I keep all my miscellaneous projects to improve my JavaScript ideation and development skills.
    </p>
</div>

<!-- Parent container with flexbox -->
<div style="display: flex; justify-content: center; align-items: center; flex-direction: column; min-height: 70vh; position: relative;">
    <div style="text-align: center; z-index: 2;">
        <input type="text" id="smilesInput" placeholder="Enter SMILES string" 
            style="width: 300px; padding: 10px; font-size: 16px; border: 2px solid #bdc3c7; border-radius: 5px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);">
        <button onclick="visualizeMolecule()" 
            style="padding: 10px 20px; margin-left: 10px; font-size: 16px; color: white; background-color: #3498db; border: none; border-radius: 5px; cursor: pointer; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);">
            Visualize
        </button>
        <button onclick="resetViewer()" 
            style="padding: 10px 20px; margin-left: 10px; font-size: 16px; color: white; background-color: #e74c3c; border: none; border-radius: 5px; cursor: pointer; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);">
            Reset
        </button>
        <!-- Viewer Box -->
        <div id="viewer" 
            style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; background-color: black; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); display: flex; justify-content: center; align-items: center;">
        </div>
        <div id="loadingIndicator" 
            style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; display: none; justify-content: center; align-items: center; background-color: #ecf0f1; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
            Loading 3Dmol... <div class="spinner" style="margin-left: 10px;"></div> 
        </div>
        <div id="errorFallback" 
            style="width: 600px; height: 400px; border: 1px solid #e74c3c; margin-top: 20px; display: none; color: #e74c3c; padding: 20px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
            Error loading 3Dmol. Please check your internet connection or try again later.
        </div>
    </div> 
    <!-- Random Image -->
    <div id="randomImage" onclick="sayRandomText();" style="position: absolute; cursor: pointer; z-index: 9999;">
        <img src="{{ site.baseurl }}/images/mort.png" alt="Random Icon" style="width: 50px; height: 50px;" />
    </div>
</div>

<script src="https://3dmol.org/build/3Dmol.js"></script> 

<script>
    // Variables to control image movement
    let posX = Math.random() * (window.innerWidth - 50);
    let posY = Math.random() * (window.innerHeight - 50);
    let velocityX = 1;
    let velocityY = 1;

    // Function to move the image
    function moveImage() {
        const image = document.getElementById('randomImage');
        const imageWidth = image.offsetWidth;
        const imageHeight = image.offsetHeight;

        // Update the position
        posX += velocityX;
        posY += velocityY;

        // Bounce off the walls
        if (posX <= 0 || posX + imageWidth >= window.innerWidth) {
            velocityX = -velocityX;
        }
        if (posY <= 0 || posY + imageHeight >= window.innerHeight) {
            velocityY = -velocityY;
        }

        // Apply the new position
        image.style.left = `${posX}px`;
        image.style.top = `${posY}px`;

        // Call the moveImage function repeatedly to keep the image moving
        requestAnimationFrame(moveImage);
    }

    // Start the image moving when the page loads
    window.onload = () => {
        moveImage();
    };

    // Function for text-to-speech on image click
    function sayRandomText() {
        const messages = ["Code, code, code!"];
        const randomMessage = messages[Math.floor(Math.random() * messages.length)];
        const synth = window.speechSynthesis;
        const utterThis = new SpeechSynthesisUtterance(randomMessage);
        synth.speak(utterThis);
    }

    // SMILES visualization logic
    let viewer;

    function visualizeMolecule() {
        const smiles = document.getElementById('smilesInput').value.trim();
        if (!smiles) {
            alert('Please enter a valid SMILES string.');
            return;
        }

        if (!viewer) {
            initializeViewer();
        }

        // Show loading indicator and hide other elements
        document.getElementById('loadingIndicator').style.display = 'flex';
        document.getElementById('viewer').style.display = 'none';
        document.getElementById('errorFallback').style.display = 'none';

        const url = `https://cactus.nci.nih.gov/chemical/structure/${encodeURIComponent(smiles)}/file?format=sdf`;

        fetch(url)
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok.');
                }
                return response.text();
            })
            .then(data => {
                viewer.clear();
                viewer.addModel(data, 'sdf'); // 'sdf' is the correct format for this kind of data
                viewer.setStyle({}, { stick: {} });
                viewer.zoomTo(); // Automatically centers and fits the molecule
                viewer.render();
                document.getElementById('loadingIndicator').style.display = 'none';
                document.getElementById('viewer').style.display = 'block';  // Ensure viewer is displayed
            })
            .catch(error => {
                console.error('Error fetching or processing molecule data:', error);
                document.getElementById('loadingIndicator').style.display = 'none';
                document.getElementById('errorFallback').style.display = 'flex';  // Show error fallback
            });
    }

    function initializeViewer() {
        viewer = $3Dmol.createViewer('viewer', {
            defaultcolors: $3Dmol.rasmolElementColors,
            backgroundColor: '#000'  // Set the background color to black
        });
    }

    function resetViewer() {
        document.getElementById('smilesInput').value = '';
        if (viewer) {
            viewer.clear();
        }
    }


</script>