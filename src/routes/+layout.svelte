<script>
  import "../app.css";
</script>

<div class="min-h-screen bg-gray-100">
  <nav class="bg-white shadow-lg">
    <div class="max-w-6xl mx-auto px-4">
      <div class="flex justify-between">
        <div class="flex space-x-7">
          <div class="flex items-center py-4">
            <span class="font-semibold text-gray-500 text-lg">Cat Rater</span>
          </div>
          <div class="flex items-center space-x-1">
            <a href="/" class="py-4 px-2 text-gray-500 hover:text-gray-900">Home</a>
            <a href="/play" class="py-4 px-2 text-gray-500 hover:text-gray-900">Play</a>
            <a href="/about" class="py-4 px-2 text-gray-500 hover:text-gray-900">About</a>
          </div>
        </div>
      </div>
    </div>
  </nav>
  
  <slot />
</div> 