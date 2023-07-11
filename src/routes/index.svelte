<!-- index.svelte -->
<script>
    export let name;
    export let university;
    export let studentNumber;
  </script>
  
  <h1>Welcome to the Overview Page</h1>
  <p>Name: {name}</p>
  <p>University: {university}</p>
  <p>Student Number: {studentNumber}</p>
  
  <p>Click on the links below to explore:</p>
  <a href="/genes">Genes Overview</a>
  <a href="/interactions">Interactions Overview</a>