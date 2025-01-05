<script>
    export let show = false;
</script>

{#if show}
<div class="modal-backdrop">
    <div class="modal">
        <h2>How to Rate Cats</h2>
        <div class="instructions">
            <div class="instruction">
                <span>👈 Swipe LEFT for NOT</span>
            </div>
            <div class="instruction">
                <span>Swipe RIGHT for HOT 👉</span>
            </div>
        </div>
        <button class="btn primary" on:click={() => show = false}>Got it!</button>
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

    .instructions {
        margin: 2rem 0;
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    .instruction {
        font-size: 1.2rem;
        padding: 1rem;
    }

    .btn {
        padding: 0.5rem 1rem;
        border-radius: 4px;
        text-decoration: none;
        cursor: pointer;
        border: none;
    }

    .primary {
        background: #4CAF50;
        color: white;
        width: 100%;
        padding: 0.75rem;
    }
</style> 