<script>
    export let show = false;
    export let onClose = () => {};
</script>

{#if show}
<div class="modal-backdrop">
    <div class="modal">
        <h2 class="text-2xl font-bold mb-4">Cat Saved! 😻</h2>
        <p class="mb-6">This purrfect kitty has been added to your collection</p>
        <div class="buttons">
            <a 
                href="/saved-cats" 
                class="btn primary"
            >
                View Saved Cats
            </a>
            <button 
                class="btn secondary"
                on:click={onClose}
            >
                Keep Playing
            </button>
        </div>
    </div>
</div>
{/if}

<style>
    .modal-backdrop {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: grid;
        place-items: center;
        z-index: 1000;
    }

    .modal {
        background: white;
        padding: 2rem;
        border-radius: 8px;
        max-width: 400px;
        text-align: center;
    }

    .buttons {
        display: flex;
        gap: 1rem;
        justify-content: center;
    }

    .btn {
        padding: 0.75rem 1.5rem;
        border-radius: 4px;
        font-weight: bold;
        cursor: pointer;
        transition: all 0.2s;
    }

    .btn:hover {
        transform: scale(1.05);
    }

    .primary {
        background: #FF69B4;
        color: white;
        text-decoration: none;
    }

    .secondary {
        background: #4CAF50;
        color: white;
        border: none;
    }
</style> 