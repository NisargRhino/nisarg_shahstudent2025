---
layout: base
title: Student Home 
description: Home Page
hide: true
---

# Molecule 3D Viewer

<div style="text-align:center;">
    <input type="text" id="smilesInput" placeholder="Enter SMILES string" style="width: 300px; padding: 10px;">
    <button onclick="visualizeMolecule()" style="padding: 10px 20px; margin-left: 10px;">Visualize</button>
    <div id="viewer" style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; background-color: white;"></div>
    <div id="loadingIndicator" style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; display: none; justify-content: center; align-items: center;">
        Loading 3Dmol... <div class="spinner"></div> 
    </div>
    <div id="errorFallback" style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; display: none; color: red;">
        Error loading 3Dmol. Please check your internet connection or try again later.
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
</style>

<link rel="preload" href="https://3dmol.org/build/3Dmol.js" as="script">