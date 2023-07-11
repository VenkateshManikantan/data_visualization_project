<!-- genes.svelte -->
<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
  
    let genes = [];
  
    onMount(() => {
      fetch('https://vda-lab.github.io/assets/genes.json')
        .then(response => response.json())
        .then(data => {
          genes = data;
        });
    });
  
    function navigateToGeneDetails(geneId) {
      goto(`/gene/${geneId}`);
    }
  </script>
  
  <h1>Genes Overview</h1>
  
  <ul>
    {#each genes as gene}
      <li>
        <a href="#" on:click={() => navigateToGeneDetails(gene.gene_id)}>
          {gene.gene_name}
        </a>
      </li>
    {/each}
  </ul>