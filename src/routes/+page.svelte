<script>
    import { onMount } from 'svelte';
    import LoginPromptModal from '$lib/components/LoginPromptModal.svelte';
</script>

<div class="container mx-auto px-4 py-8">
    <h1 class="text-4xl font-bold text-center mb-4">Is my CAT HOT OR NOT??</h1>
    <p class="text-xl text-center mb-8">Rate cats and earn points!</p>
    <div class="text-center">
        <a 
            href="/play" 
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded inline-block"
        >
            Start Rating Cats!
        </a>
    </div>
</div>

<style>
    .container {
        min-height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }
</style>
