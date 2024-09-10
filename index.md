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
    <div id="randomImage" onclick="sayRandomText();" style="position: absolute; display: none; cursor: pointer; z-index: 9999; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); transition: transform 0.3s ease;">
        <!-- Change the src to your own PNG file -->
        <img src="{{ site.baseurl }}/images/logo.png" alt="Random Icon" style="width: 50px; height: 50px;" />
    </div>
</div>

<script src="https://3dmol.org/build/3Dmol.js"></script> 

<script>
// Function to show and hide the image randomly
function showRandomImage() {
    const image = document.getElementById('randomImage');
    const imageWidth = image.offsetWidth;
    const imageHeight = image.offsetHeight;
    const randomX = Math.random() * (window.innerWidth - imageWidth);
    const randomY = Math.random() * (window.innerHeight - imageHeight);
    
    // Set random position ensuring the image is fully visible
    image.style.left = `${Math.max(0, randomX)}px`;
    image.style.top = `${Math.max(0, randomY)}px`;
    
    // Show the image
    image.style.display = 'block';
}

// Function for text-to-speech on image click
function sayRandomText() {
    const messages = ["Code, code, code!"];
    const randomMessage = messages[Math.floor(Math.random() * messages.length)];
    const synth = window.speechSynthesis;
    const utterThis = new SpeechSynthesisUtterance(randomMessage);
    synth.speak(utterThis);

    // Move the image to a new random position
    showRandomImage();
}

// Show the image when the page loads
window.onload = () => {
    showRandomImage();
};

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
            console.log("#### opening");
            viewer.clear();
            viewer.addModel(data, 'sdf'); // 'sdf' is the correct format for this kind of data
            viewer.setStyle({}, { stick: {} });
            viewer.zoomTo(); // Automatically centers and fits the molecule
            viewer.render();
            document.getElementById('loadingIndicator').style.display = 'none';
            document.getElementById('viewer').style.display = 'block';  // Ensure viewer is displayed
            document.getElementById('undefined').style.position = '';  // Ensure viewer is displayed
        })
        .catch(error => {
            console.error('Error fetching or processing molecule data:', error);
            document.getElementById('loadingIndicator').style.display = 'none';
            document.getElementById('errorFallback').style.display = 'block';
        });
}

function initializeViewer() {
    const element = document.getElementById('viewer');
    viewer = $3Dmol.createViewer(element, { backgroundColor: 'white' });
}

function resetViewer() {
    if (viewer) {
        viewer.clear();
        viewer.render();
    }
    document.getElementById('smilesInput').value = ''; // Clear the input field
}
</script>

<style>
    body {
        font-family: 'Arial', sans-serif;
        background-color: #1a1a1a;
        color: #fff;
        background-image: linear-gradient(135deg, #1a1a1a 25%, #333 100%);
        background-size: cover;
        background-attachment: fixed;
        background-repeat: no-repeat;
        background-position: center;
        min-height: 100vh;
        margin: 0;
        padding: 0;
    }

    nav {
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    h2 {
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    }

    p {
        text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
    }

    input, button {
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    button:hover {
        background-color: #2980b9;
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    }

    #viewer {
        width: 600px;
        height: 400px;
        border: 1px solid #ccc;
        background-color: black;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        display: flex;
        justify-content: center;
        align-items: center;
    }

    #loadingIndicator, #errorFallback {
        transition: box-shadow 0.3s ease;
    }

    #loadingIndicator:hover, #errorFallback:hover {
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
    }

    .spinner {
        border: 4px solid #f3f3f3;
        border-top: 4px solid #3498db;
        border-radius: 50%;
        width: 20px;
        height: 20px;
        animation: spin 2s linear infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }

    input:focus, button:focus {
        outline: none;
        box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
    }

    /* Random Image */
    #randomImage {
        z-index: 9999; /* Ensure it is in front of everything */
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        transition: transform 0.3s ease;
    }

    #randomImage:hover {
        transform: scale(1.1);
    }

    /* Art Gallery Styles */
    .gallery {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 20px;
        margin-top: 50px;
    }

    .gallery-item {
        width: 200px;
        height: 200px;
        background-color: #fff;
        border: 2px solid #ccc;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        transition: transform 0.3s ease, box-shadow 0.3s ease;
        overflow: hidden;
        position: relative;
    }

    .gallery-item img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .gallery-item:hover {
        transform: scale(1.05);
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    }

    .gallery-item .caption {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        background-color: rgba(0, 0, 0, 0.7);
        color: #fff;
        text-align: center;
        padding: 10px;
        box-sizing: border-box;
        font-size: 14px;
        opacity: 0;
        transition: opacity 0.3s ease;
    }

    .gallery-item:hover .caption {
        opacity: 1;
    }
</style>

<!-- Art Gallery Section -->
<div class="gallery">
    <div class="gallery-item">
        <img src="{{ site.baseurl }}/images/art1.jpg" alt="Art 1">
        <div class="caption">Art 1</div>
    </div>
    <div class="gallery-item">
        <img src="{{ site.baseurl }}/images/art2.jpg" alt="Art 2">
        <div class="caption">Art 2</div>
    </div>
    <div class="gallery-item">
        <img src="{{ site.baseurl }}/images/art3.jpg" alt="Art 3">
        <div class="caption">Art 3</div>
    </div>
    <div class="gallery-item">
        <img src="{{ site.baseurl }}/images/art4.jpg" alt="Art 4">
        <div class="caption">Art 4</div>
    </div>
</div>