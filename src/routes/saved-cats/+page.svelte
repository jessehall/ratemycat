<script>
    import { onMount } from 'svelte';
    import { fade, scale } from 'svelte/transition';
    import { flip } from 'svelte/animate';

    let savedCats = [];
    let isLoading = true;

    async function deleteCat(favouriteId) {
        try {
            const response = await fetch(`https://api.thecatapi.com/v1/favourites/${favouriteId}`, {
                method: 'DELETE',
                headers: {
                    'x-api-key': 'live_yPLrD2WynMD6OS50beH5EvUAuPvRUcto29HBOHTGb2IpJveIHQ23PkHcnq0eLhsd'
                }
            });

            if (response.ok) {
                // Remove the cat from the local array
                savedCats = savedCats.filter(cat => cat.id !== favouriteId);
            } else {
                throw new Error('Failed to delete cat');
            }
        } catch (error) {
            console.error('Error deleting cat:', error);
            alert('Failed to delete cat. Please try again.');
        }
    }

    onMount(async () => {
        try {
            const response = await fetch('https://api.thecatapi.com/v1/favourites', {
                headers: {
                    'x-api-key': 'live_yPLrD2WynMD6OS50beH5EvUAuPvRUcto29HBOHTGb2IpJveIHQ23PkHcnq0eLhsd'
                }
            });

            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            const data = await response.json();
            savedCats = data;
        } catch (error) {
            console.error('Error fetching saved cats:', error);
            savedCats = [];
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
            {#each savedCats as cat (cat.id)}
                <div 
                    class="cat-card"
                    animate:flip={{ duration: 300 }}
                    out:scale={{ duration: 200, start: 0.95 }}
                    in:fade={{ duration: 200 }}
                >
                    <div class="relative">
                        <img 
                            src={cat.image.url} 
                            alt="Saved cat" 
                            class="w-full h-64 object-cover rounded-lg shadow-lg"
                        />
                        <button
                            class="delete-button"
                            on:click={() => deleteCat(cat.id)}
                            title="Remove from saved"
                        >
                            ×
                        </button>
                    </div>
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

    .delete-button {
        position: absolute;
        top: 0.5rem;
        right: 0.5rem;
        background: rgba(255, 255, 255, 0.9);
        color: #FF4444;
        width: 2rem;
        height: 2rem;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 1.5rem;
        font-weight: bold;
        cursor: pointer;
        transition: all 0.2s;
        border: none;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .delete-button:hover {
        background: #FF4444;
        color: white;
        transform: scale(1.1);
    }

    .cat-card {
        transition: transform 0.2s;
        position: relative;
    }

    .cat-card:hover {
        transform: scale(1.05);
    }
</style> 