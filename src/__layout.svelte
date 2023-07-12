<script>
    import { Router } from '@sveltejs/kit';
  </script>
  
  <header>
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/genes">Genes Overview</a></li>
        <li><a href="/interactions">Interactions Overview</a></li>
      </ul>
    </nav>
  </header>
  
  <div class="page-container">
    <Router {component} {props} />
  </div>
  
  <footer>
    <!-- Add your footer content here -->
  </footer>
  