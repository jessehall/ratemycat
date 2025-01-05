<script>
  import { onMount } from 'svelte';
  import LoginPromptModal from '$lib/components/LoginPromptModal.svelte';
  import InstructionsModal from '$lib/components/InstructionsModal.svelte';

  /** @typedef {{id: string, url: string}} CatImage */

  /** @type {CatImage | null} */
  let currentCat = null;
  let score = 0;
  let attempts = 0;
  let showLoginModal = false;
  let touchStart = 0;
  let touchEnd = 0;
  let isLoading = true;
  let showInstructions = true;
  
  async function fetchNewCat() {
    isLoading = true;
    const response = await fetch('https://api.thecatapi.com/v1/images/search', {
      headers: {
        'x-api-key': import.meta.env.VITE_CAT_API_KEY
      }
    });
    const [data] = await response.json();
    currentCat = data;
    isLoading = false;
  }

  /**
   * @param {number} value
   */
  async function rateCat(value) {
    if (!currentCat) return;
    
    await fetch('https://api.thecatapi.com/v1/votes', {
      method: 'POST',
      headers: {
        'content-type': 'application/json',
        'x-api-key': import.meta.env.VITE_CAT_API_KEY
      },
      body: JSON.stringify({
        image_id: currentCat.id,
        value: value
      })
    });

    score++;
    fetchNewCat();
  }

  /**
   * @param {TouchEvent} e
   */
  function handleTouchStart(e) {
    touchStart = e.touches[0].clientX;
  }

  /**
   * @param {TouchEvent} e
   */
  function handleTouchMove(e) {
    touchEnd = e.touches[0].clientX;
  }

  function handleTouchEnd() {
    const swipeDistance = touchStart - touchEnd;
    const minSwipeDistance = 50;

    if (Math.abs(swipeDistance) > minSwipeDistance) {
      if (swipeDistance > 0) {
        // Swiped left
        rateCat(-1);
      } else {
        // Swiped right
        rateCat(1);
      }
    }
  }

  function handleRating() {
    attempts++;
    
    // Save to cookies
    document.cookie = `score=${score};path=/;max-age=86400`; // expires in 24 hours
    document.cookie = `attempts=${attempts};path=/;max-age=86400`;

    // Show login modal after 5 attempts
    if (attempts === 5) {
      showLoginModal = true;
    }
  }

  onMount(() => {
    fetchNewCat();

    // Load score from cookie if it exists
    const savedScore = document.cookie.split('; ')
      .find(row => row.startsWith('score='))
      ?.split('=')[1];
    if (savedScore) {
      score = parseInt(savedScore);
    }

    const savedAttempts = document.cookie.split('; ')
      .find(row => row.startsWith('attempts='))
      ?.split('=')[1];
    if (savedAttempts) {
      attempts = parseInt(savedAttempts);
    }
  });
</script>

<LoginPromptModal bind:show={showLoginModal} />
<InstructionsModal bind:show={showInstructions} />

<div class="relative min-h-screen">
  <!-- Score counter -->
  <div class="absolute top-4 right-4 bg-white p-2 rounded shadow">
    <p class="text-lg font-semibold">{score} cat ratings</p>
  </div>

  <!-- Main content -->
  <div 
    class="container mx-auto px-4 py-8 flex flex-col items-center"
    on:touchstart={handleTouchStart}
    on:touchmove={handleTouchMove}
    on:touchend={handleTouchEnd}
  >
    {#if currentCat && !isLoading}
      <div class="w-full max-w-2xl aspect-square mb-4">
        <img 
          src={currentCat.url} 
          alt="Cat to rate" 
          class="w-full h-full object-cover rounded-lg shadow-lg"
        />
      </div>
      <p class="text-gray-500 md:hidden mt-4">Swipe left for NOT, right for HOT</p>
    {:else}
      <div class="flex flex-col items-center space-y-4">
        <div class="loader"></div>
        <p class="text-xl text-gray-600 animate-pulse">Getting the next hottie...</p>
      </div>
    {/if}
  </div>

  <!-- Rating buttons -->
  <button
    class="fixed left-4 top-1/2 transform -translate-y-1/2 bg-red-500 hover:bg-red-700 
           text-white font-bold py-4 px-8 rounded-lg shadow-lg transition-all duration-300 
           hover:scale-110"
    on:click={() => {
      rateCat(-1);
      handleRating();
    }}
  >
    NOT
  </button>

  <button
    class="fixed right-4 top-1/2 transform -translate-y-1/2 bg-green-500 hover:bg-green-700 
           text-white font-bold py-4 px-8 rounded-lg shadow-lg transition-all duration-300 
           hover:scale-110"
    on:click={() => {
      rateCat(1);
      handleRating();
    }}
  >
    HOT
  </button>
</div>

<style>
  /* Disable pull-to-refresh on mobile */
  :global(body) {
    overscroll-behavior-y: contain;
  }

  .loader {
    width: 48px;
    height: 48px;
    border: 5px solid #FFF;
    border-bottom-color: #FF3D00;
    border-radius: 50%;
    display: inline-block;
    box-sizing: border-box;
    animation: rotation 1s linear infinite;
  }

  @keyframes rotation {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style> 