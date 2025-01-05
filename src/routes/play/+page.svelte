<script>
  import { onMount } from 'svelte';
  import LoginPromptModal from '$lib/components/LoginPromptModal.svelte';
  import InstructionsModal from '$lib/components/InstructionsModal.svelte';
  import SaveCatButton from '$lib/components/SaveCatButton.svelte';
  import UsersSidebar from '$lib/components/UsersSidebar.svelte';
  import { cubicOut } from 'svelte/easing';
  import { elasticOut } from 'svelte/easing';

  function smoothTransition(node, {
    duration = 1000,
    delay = 0
  }) {
    return {
      delay,
      duration,
      css: (t) => {
        const eased = cubicOut(t);
        return `
          opacity: ${t};
          transform: translateX(${(1 - eased) * 250}px);
        `;
      }
    };
  }

  /** @typedef {{id: string, url: string}} CatImage */

  /** @type {CatImage | null} */
  let currentCat = null;
  let score = 0;
  let attempts = 0;
  let clickCount = 0;
  let showLoginModal = false;
  let touchStart = 0;
  let touchEnd = 0;
  let isLoading = true;
  let showInstructions = true;
  
  async function fetchNewCat() {
    isLoading = true;
    try {
      const response = await fetch('https://api.thecatapi.com/v1/images/search', {
        headers: {
          'x-api-key': import.meta.env.VITE_CAT_API_KEY || ''
        }
      });
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const [data] = await response.json();
      currentCat = data;
    } catch (error) {
      console.error('Error fetching cat:', error);
      // Set a default cat image or show error state
      currentCat = null;
    } finally {
      isLoading = false;
    }
  }

  /**
   * @param {number} value
   */
  async function rateCat(value) {
    if (!currentCat) return;
    
    clickCount++;
    try {
      const response = await fetch('https://api.thecatapi.com/v1/votes', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': import.meta.env.VITE_CAT_API_KEY || ''
        },
        body: JSON.stringify({
          image_id: currentCat.id,
          sub_id: 'user-123',
          value: value
        })
      });

      if (!response.ok) {
        throw new Error(`Failed to submit vote: ${response.status}`);
      }

      score++;
      handleRating();
      fetchNewCat();
    } catch (error) {
      console.error('Error submitting vote:', error);
      // Show error message to user
      alert('Failed to rate cat. Please try again.');
    }
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
<UsersSidebar {clickCount} />

<div class="relative min-h-screen">
  <!-- Score counter -->
  <div class="absolute top-4 left-4 bg-white p-4 rounded shadow hidden md:block">
    <p class="text-lg font-semibold mb-1">Your cat rating score is: {score}</p>
    <a 
      href="/login" 
      class="text-sm text-blue-500 hover:text-blue-700 hover:underline"
    >
      Log in to save your score →
    </a>
  </div>

  <!-- Mobile Score Display -->
  <div class="flex justify-center mb-2 md:hidden">
    <div class="text-center">
      <p class="text-sm font-medium">Score: {score}</p>
      <a 
        href="/login" 
        class="text-xs text-blue-500 hover:text-blue-700 hover:underline"
      >
        Log in to save →
      </a>
    </div>
  </div>

  <!-- Rating buttons -->
  <div class="flex justify-center gap-4 pt-4 pb-2">
    <button
      class="bg-red-500 hover:bg-red-700 text-white font-bold py-3 px-6 
             rounded-lg shadow-lg transition-all duration-300 hover:scale-110 animate-on-click"
      on:click={(e) => {
        const button = e.currentTarget;
        button.classList.add('button-clicked');
        setTimeout(() => {
          rateCat(-1);
          handleRating();
          button.classList.remove('button-clicked');
        }, 300);
      }}
    >
      NOT
    </button>

    <button
      class="bg-green-500 hover:bg-green-700 text-white font-bold py-3 px-6 
             rounded-lg shadow-lg transition-all duration-300 hover:scale-110 animate-on-click"
      on:click={(e) => {
        const button = e.currentTarget;
        button.classList.add('button-clicked');
        setTimeout(() => {
          rateCat(1);
          handleRating();
          button.classList.remove('button-clicked');
        }, 300);
      }}
    >
      HOT
    </button>
  </div>

  <!-- Main content -->
  <div 
    class="container mx-auto px-4 py-8 flex flex-col items-center lg:w-[80%]"
    on:touchstart={handleTouchStart}
    on:touchmove={handleTouchMove}
    on:touchend={handleTouchEnd}
  >
    {#if currentCat && !isLoading}
      <div 
        class="w-full max-w-2xl h-[60vh] mb-4"
        in:smoothTransition={{ duration: 1000 }}
      >
        <img 
          src={currentCat.url} 
          alt="Cat to rate" 
          class="w-full h-full object-cover rounded-lg shadow-lg"
        />
      </div>
      <p class="text-gray-500 md:hidden mt-4">Swipe left for NOT, right for HOT</p>
      
      <!-- Add Save Cat Button -->
      <SaveCatButton 
        catId={currentCat.id} 
        imageUrl={currentCat.url}
      />
    {:else}
      <div class="fixed inset-0 flex flex-col items-center justify-center space-y-4">
        <div class="loader"></div>
        <p class="text-xl text-gray-600 animate-pulse">Getting the next hottie...</p>
      </div>
    {/if}
  </div>
</div>

<style>
  /* Disable pull-to-refresh on mobile */
  :global(body) {
    overscroll-behavior-y: contain;
  }

  @keyframes pulseOnRate {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.2);
    }
    100% {
      transform: scale(1);
    }
  }

  :global(.animate-on-click:active) {
    animation: pulseOnRate 1s ease-in-out;
    animation-delay: 300ms;
  }

  /* Add a new class for the button when it's clicked */
  :global(.button-clicked) {
    pointer-events: none; /* Prevent additional clicks during animation */
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