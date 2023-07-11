<!-- Overview.svelte -->
<script>
    export let name;
    export let university;
    export let studentNumber;
  </script>
  
  <h1>Overview</h1>
  
  <p>Name: {name}</p>
  <p>University: {university}</p>
  <p>Student Number: {studentNumber}</p>
  
  <a href="/genes">Genes Overview</a>
  <a href="/interactions">Interactions Overview</a>
  