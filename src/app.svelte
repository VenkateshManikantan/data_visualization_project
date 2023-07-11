<!-- App.svelte -->
<script>
  import { Router, Route } from 'svelte-routing';
  import Overview from './components/Overview.svelte';
  import GenesOverview from './components/GenesOverview.svelte';
  import InteractionsOverview from './components/InteractionsOverview.svelte';

  const name = 'Venkatesh Manikantan';
  const university = 'KU Leuven';
  const studentNumber = 'r0825919';
</script>

<main>
  <Router>
    <Route path="/" component={Overview} />
    <Route path="/genes" component={GenesOverview} />
    <Route path="/interactions" component={InteractionsOverview} />
  </Router>
</main>

<style>
  main {
    padding: 20px;
  }
</style>