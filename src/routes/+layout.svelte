<script>
  import "../app.css";
  import Nav from '$lib/components/Nav.svelte';
</script>

<div class="min-h-screen bg-gray-100">
  <Nav />
  
  <slot />
</div> 