<!-- GenesOverview.svelte -->
<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    
    let genes = []; // Initialize an empty array to store gene data
    let selectedGene = null; // Store the selected gene ID
    
    onMount(() => {
      // Fetch gene data from the provided URL
      fetch('https://vda-lab.github.io/assets/genes.json')
        .then(response => response.json())
        .then(data => {
          genes = data; // Assign the fetched gene data to the `genes` array
          drawChromosome(); // Call the function to draw the chromosome visual
        });
    });
    
    function drawChromosome() {
      const svgWidth = 600;
      const svgHeight = 600;
      const circleRadius = 250;
      const geneOpacity = 0.5;
      const tickLength = 10;
      const tickInterval = 200000;
      
      const svg = document.getElementById('chromosome-svg');
      const geneContainer = document.getElementById('gene-container');
      const tickContainer = document.getElementById('tick-container');
      
      // Clear any existing content
      geneContainer.innerHTML = '';
      tickContainer.innerHTML = '';
      
      // Draw the circle
      const circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
      circle.setAttribute('cx', svgWidth / 2);
      circle.setAttribute('cy', svgHeight / 2);
      circle.setAttribute('r', circleRadius);
      circle.setAttribute('fill', 'transparent');
      circle.setAttribute('stroke', 'black');
      svg.appendChild(circle);
      
      // Draw genes
      genes.forEach(gene => {
        const geneLine = document.createElementNS('http://www.w3.org/2000/svg', 'line');
        const genePosition = calculateGenePosition(gene.gene_start);
        
        geneLine.setAttribute('x1', svgWidth / 2);
        geneLine.setAttribute('y1', svgHeight / 2);
        geneLine.setAttribute('x2', genePosition.x);
        geneLine.setAttribute('y2', genePosition.y);
        geneLine.setAttribute('stroke', 'black');
        geneLine.setAttribute('stroke-opacity', geneOpacity);
        
        geneLine.addEventListener('mouseover', () => {
          geneLine.setAttribute('stroke-opacity', 1);
          geneLine.setAttribute('stroke', 'red');
          showGeneName(gene.gene_name, genePosition.x, genePosition.y);
        });
        
        geneLine.addEventListener('mouseout', () => {
          geneLine.setAttribute('stroke-opacity', geneOpacity);
          geneLine.setAttribute('stroke', 'black');
          hideGeneName();
        });
        
        geneLine.addEventListener('click', () => {
          selectedGene = gene.gene_id;
          goto(`/gene/${selectedGene}`);
        });
        
        geneContainer.appendChild(geneLine);
      });
      
      // Draw ticks
      const tickCount = Math.floor(circleRadius / tickInterval);
      
      for (let i = 0; i < tickCount; i++) {
        const angle = (360 / tickCount) * i;
        const tick = document.createElementNS('http://www.w3.org/2000/svg', 'line');
        
        const startX = (svgWidth / 2) + (circleRadius * Math.cos(degreesToRadians(angle)));
        const startY = (svgHeight / 2) + (circleRadius * Math.sin(degreesToRadians(angle)));
        const endX = (svgWidth / 2) + ((circleRadius + tickLength) * Math.cos(degreesToRadians(angle)));
        const endY = (svgHeight / 2) + ((circleRadius + tickLength) * Math.sin(degreesToRadians(angle)));
        
        tick.setAttribute('x1', startX);
        tick.setAttribute('y1', startY);
        tick.setAttribute('x2', endX);
        tick.setAttribute('y2', endY);
        tick.setAttribute('stroke', 'black');
        tick.setAttribute('stroke-opacity', geneOpacity);
        
        tickContainer.appendChild(tick);
      }
    }
    
    function calculateGenePosition(geneStart) {
      const circleRadius = 250;
      const geneRadius = circleRadius - 10; // Adjust the gene position to be slightly inside the circle
      
      const angle = (geneStart / 4639675) * 360; // Assuming the maximum position is 4639675 (E.coli genome size)
      const radians = degreesToRadians(angle);
      
      const x = (600 / 2) + (geneRadius * Math.cos(radians));
      const y = (600 / 2) + (geneRadius * Math.sin(radians));
      
      return { x, y };
    }
    
    function degreesToRadians(degrees) {
      return (degrees * Math.PI) / 180;
    }
    
    function showGeneName(geneName, x, y) {
      const geneNameDiv = document.createElement('div');
      geneNameDiv.className = 'gene-name';
      geneNameDiv.textContent = geneName;
      geneNameDiv.style.left = `${x}px`;
      geneNameDiv.style.top = `${y}px`;
      document.body.appendChild(geneNameDiv);
    }
    
    function hideGeneName() {
      const geneNameDiv = document.querySelector('.gene-name');
      if (geneNameDiv) {
        document.body.removeChild(geneNameDiv);
      }
    }
  </script>
  
  <style>
    .gene-name {
      position: absolute;
      background-color: white;
      padding: 5px;
      border-radius: 5px;
      font-size: 14px;
      font-weight: bold;
    }
  </style>
  
  <h2>Genes Overview</h2>
  <p>Your name, university, and student number</p>
  
  <svg id="chromosome-svg" width="600" height="600">
    <g id="gene-container"></g>
    <g id="tick-container"></g>
  </svg>