<script>
    import { onMount } from 'svelte';

    let savedCats = [];
    let isLoading = true;

    onMount(async () => {
        try {
            const response = await fetch('https://api.thecatapi.com/v1/favourites', {
                headers: {
                    'x-api-key': import.meta.env.VITE_CAT_API_KEY
                }
            });
            const data = await response.json();
            savedCats = data;
        } catch (error) {
            console.error('Error fetching saved cats:', error);
        } finally {
            isLoading = false;
        }
    });
</script>

<div class="container mx-auto px-4 py-8">
    <h1 class="text-3xl font-bold mb-8 text-center">Your Saved Cats</h1>

    {#if isLoading}
        <div class="flex justify-center">
            <div class="loader"></div>
        </div>
    {:else if savedCats.length === 0}
        <div class="text-center">
            <p class="text-xl text-gray-600">No saved cats yet!</p>
            <a 
                href="/play" 
                class="inline-block mt-4 bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-600"
            >
                Start Rating Cats
            </a>
        </div>
    {:else}
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {#each savedCats as cat}
                <div class="cat-card">
                    <img 
                        src={cat.image.url} 
                        alt="Saved cat" 
                        class="w-full h-64 object-cover rounded-lg shadow-lg"
                    />
                </div>
            {/each}
        </div>
    {/if}
</div>

<style>
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
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }

    .cat-card {
        transition: transform 0.2s;
    }

    .cat-card:hover {
        transform: scale(1.05);
    }
</style> 