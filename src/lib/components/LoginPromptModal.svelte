<script>
    export let show = false;
</script>

{#if show}
<div class="modal-backdrop">
    <div class="modal">
        <h2>Want to save your progress?</h2>
        <p>Log in to save your score and compete with others!</p>
        <div class="buttons">
            <a href="/login" class="btn primary">Log In</a>
            <button class="btn secondary" on:click={() => show = false}>Continue Playing</button>
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
        margin-top: 1.5rem;
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
    }

    .secondary {
        background: #9e9e9e;
        color: white;
    }
</style> 