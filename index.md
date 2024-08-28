---
layout: base
title: Student Home 
description: Home Page
hide: true
---

<div style="text-align: center; margin-top: 50px;">
    <h2 style="font-family: 'Arial', sans-serif; color: #2c3e50; font-size: 28px; margin-bottom: 20px;">Welcome to My Project Hub</h2>
    <p style="font-family: 'Arial', sans-serif; color: #7f8c8d; font-size: 18px; max-width: 600px; margin: 0 auto 40px;">
        This is where I keep all my miscellaneous projects to improve my JavaScript ideation and development skills.
    </p>
</div>

<div style="display: flex; justify-content: center; align-items: center; flex-direction: column; min-height: 70vh;">
    <div style="text-align: center;">
        <input type="text" id="smilesInput" placeholder="Enter SMILES string" 
            style="width: 300px; padding: 10px; font-size: 16px; border: 2px solid #bdc3c7; border-radius: 5px;">
        <button onclick="visualizeMolecule()" 
            style="padding: 10px 20px; margin-left: 10px; font-size: 16px; color: white; background-color: #3498db; border: none; border-radius: 5px; cursor: pointer;">
            Visualize
        </button>
        <div id="viewer" 
            style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; background-color: white; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
        </div>
        <div id="loadingIndicator" 
            style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; display: none; justify-content: center; align-items: center; background-color: #ecf0f1;">
            Loading 3Dmol... <div class="spinner" style="margin-left: 10px;"></div> 
        </div>
        <div id="errorFallback" 
            style="width: 600px; height: 400px; border: 1px solid #e74c3c; margin-top: 20px; display: none; color: #e74c3c; padding: 20px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);">
            Error loading 3Dmol. Please check your internet connection or try again later.
        </div>
    </div>
</div>

<script src="https://3dmol.org/build/3Dmol.js"></script> 
<script>
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
                viewer.zoomTo();
                viewer.render();
                document.getElementById('loadingIndicator').style.display = 'none';
                document.getElementById('viewer').style.display = 'block';  // Ensure viewer is displayed
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
</script>

<style>
    body {
        font-family: 'Arial', sans-serif;
        background-color: #f7f7f7;
        color: #333;
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

    button:hover {
        background-color: #2980b9;
    }

    #viewer, #loadingIndicator, #errorFallback {
        transition: box-shadow 0.3s ease;
    }

    #viewer:hover, #loadingIndicator:hover, #errorFallback:hover {
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
    }
</style>