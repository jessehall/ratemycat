<script>
    import { onMount } from 'svelte';
    import LoginPromptModal from '$lib/components/LoginPromptModal.svelte';
    
    let score = 0;
    let attempts = 0;
    let showLoginModal = false;

    onMount(() => {
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

    function updateScore(newScore) {
        score = newScore;
        attempts++;
        
        // Save to cookies
        document.cookie = `score=${score};path=/;max-age=86400`; // expires in 24 hours
        document.cookie = `attempts=${attempts};path=/;max-age=86400`;

        // Show login modal after 5 attempts
        if (attempts === 5) {
            showLoginModal = true;
        }
    }
</script>

<LoginPromptModal bind:show={showLoginModal} />

<!-- Rest of your game component code -->
