<script>
    import SaveSuccessModal from './SaveSuccessModal.svelte';
    export let catId = '';
    export let imageUrl = '';
    export let visible = true;
    let showSuccessModal = false;
    
    async function saveCat() {
        try {
            const response = await fetch('https://api.thecatapi.com/v1/favourites', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'x-api-key': import.meta.env.VITE_CAT_API_KEY || ''
                },
                body: JSON.stringify({
                    image_id: catId,
                    sub_id: 'user-1'
                })
            });

            const responseData = await response.json();
            console.log('Save response:', responseData);

            if (response.ok) {
                showSuccessModal = true;
            } else {
                console.error('Save response:', responseData);
                throw new Error(`Failed to save cat: ${JSON.stringify(responseData)}`);
            }
        } catch (error) {
            console.error('Error saving cat:', error);
            alert(`Failed to save cat: ${error.message}`);
        }
    }
</script>

<SaveSuccessModal 
    show={showSuccessModal} 
    onClose={() => showSuccessModal = false}
/>

<div class:invisible={!visible}>
    <button
        on:click={saveCat}
        class="save-button"
        title="Save this Purrrfect cat"
    >
        <span class="heart-icon">♥</span>
        Save this cat
    </button>
</div>

<style>
    .save-button {
        background: white;
        border: 2px solid #FF69B4;
        color: #FF69B4;
        padding: 0.5rem 1rem;
        border-radius: 9999px;
        font-weight: bold;
        display: flex;
        align-items: center;
        gap: 0.5rem;
        margin: 1rem auto;
        transition: all 0.2s;
    }

    .save-button:hover {
        background: #FF69B4;
        color: white;
        transform: scale(1.05);
    }

    .heart-icon {
        font-size: 1.2em;
    }
</style> 