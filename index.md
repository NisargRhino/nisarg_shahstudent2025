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
    <div id="viewer" style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; background-color: white; display: none;"></div>
    <div id="loadingIndicator" style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; display: flex; justify-content: center; align-items: center;">
        Loading 3Dmol... <div class="spinner"></div> 
    </div>
    <div id="errorFallback" style="width: 600px; height: 400px; border: 1px solid #ccc; margin-top: 20px; display: none; color: red;">
        Error loading 3Dmol. Please check your internet connection or try again later.
    </div>
</div>

<script src="https://3dmol.org/build/3Dmol.js"></script> 
<script>
    let viewer;
    let is3DmolLoaded = false;

    function visualizeMolecule() {
        const smiles = document.getElementById('smilesInput').value.trim();
        if (!smiles) {
            alert('Please enter a valid SMILES string.');
            return;
        }

        if (!is3DmolLoaded) {
            load3Dmol().then(() => {
                visualizeMolecule();
            }).catch(error => {
                console.error('Error loading 3Dmol:', error);
                document.getElementById('loadingIndicator').style.display = 'none';
                document.getElementById('errorFallback').style.display = 'block';
            });
            return;
        }

        if (viewer) {
            viewer.clear();

            const url = `https://3dmol.org/3Dmol/convert?smiles=${encodeURIComponent(smiles)}&format=mol`;

            $3Dmol.download(url, viewer, {}, function(m, error) {
                if (error) {
                    console.error('Error fetching or processing molecule data:', error);
                    alert('Error fetching or processing molecule data. Please check your SMILES string and try again. Details in console.');
                } else if (m) {
                    viewer.addModel(m, 'mol');
                    viewer.setStyle({}, { stick: {} });
                    viewer.zoomTo();
                    viewer.render();

                    document.getElementById('loadingIndicator').style.display = 'none';
                    document.getElementById('viewer').style.display = 'block';
                } else {
                    alert('Unexpected error occurred. Please check the console for details.');
                }
            });
        } else {
            alert('3Dmol viewer is not yet ready. Please try again in a few moments.');
        }
    }

    function load3Dmol() {
        return new Promise((resolve, reject) => {
            // Check if 3Dmol is already loaded (might be preloaded)
            if (typeof $3Dmol !== 'undefined') {
                is3DmolLoaded = true;
                const element = document.getElementById('viewer');
                viewer = $3Dmol.createViewer(element, { backgroundColor: 'white' });
                resolve();
                return; 
            }

            const script = document.createElement('script');
            script.src = 'https://3dmol.org/build/3Dmol.js';
            script.onload = () => {
                is3DmolLoaded = true;
                const element = document.getElementById('viewer');
                viewer = $3Dmol.createViewer(element, { backgroundColor: 'white' });
                resolve();
            };
            script.onerror = (error) => {
                reject(error); 
            };
            document.head.appendChild(script);
        });
    }
</script>

<style>
/* ... (spinner styles remain the same) */
</style>

<link rel="preload" href="https://3dmol.org/build/3Dmol.js" as="script">