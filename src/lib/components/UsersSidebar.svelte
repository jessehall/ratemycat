<script>
    import { flip } from 'svelte/animate';
    import { fade } from 'svelte/transition';

    let mockUsers = [
        { id: 1, name: 'Whiskers McGee', avatar: '🐱' },
        { id: 2, name: 'Pawsome Paula', avatar: '😺' },
        { id: 3, name: 'Meow Master', avatar: '😸' },
        { id: 4, name: 'Purrington III', avatar: '😺' },
        { id: 5, name: 'Feline Dion', avatar: '🐱' },
        { id: 6, name: 'Claw-dia', avatar: '😸' }
    ];

    const ratings = ['PURRFECT!', 'HOTTER!', 'HOT!', 'MEH', 'NOT QUITE', 'NOT'];
    export let clickCount = 0;

    $: if (clickCount > 0) {
        mockUsers = [...mockUsers]
            .sort(() => Math.random() - 0.5)
            .map((user, index) => ({
                ...user,
                rating: ratings[index],
                time: '1s ago'
            }));
    }

    $: getRatingColor = (rating) => {
        switch(rating) {
            case 'PURRFECT!': return '#FF1493'; // Deep pink
            case 'HOTTER!': return '#FF4500';   // Orange red
            case 'HOT!': return '#FF6B6B';      // Light red
            case 'MEH': return '#FFA500';       // Orange
            case 'NOT QUITE': return '#808080';  // Gray
            case 'NOT': return '#ef4444';       // Red
            default: return '#374151';
        }
    };
</script>

<div class="sidebar">
    <div class="profile-section">
        <div class="avatar">😻</div>
        <div class="user-info">
            <h2 class="username">Cat Queen</h2>
            <p class="title">Professional Cat Rater</p>
        </div>
    </div>

    <div class="recent-activity">
        <h3>Recent Ratings</h3>
        {#each mockUsers as user (user.id)}
            <div 
                class="activity-item"
                animate:flip={{ duration: 500 }}
                in:fade={{ duration: 200 }}
            >
                <span class="user-avatar">{user.avatar}</span>
                <div class="activity-details">
                    <span class="user-name">{user.name}</span>
                    <span 
                        class="rating" 
                        style="color: {getRatingColor(user.rating)}"
                    >
                        {user.rating}
                    </span>
                    <span class="time">{user.time}</span>
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    .sidebar {
        width: 20%;
        height: calc(100vh - 4rem);
        position: fixed;
        right: 0;
        top: 4rem;
        background: white;
        border-left: 1px solid #e5e7eb;
        padding: 1.5rem;
        overflow-y: auto;
        box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
        z-index: 50;
    }

    .profile-section {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding-bottom: 1.5rem;
        border-bottom: 1px solid #e5e7eb;
        margin-bottom: 1.5rem;
    }

    .avatar {
        font-size: 3rem;
        margin-bottom: 0.5rem;
    }

    .user-info {
        text-align: center;
    }

    .username {
        font-size: 1.5rem;
        font-weight: bold;
        color: #FF69B4;
        margin: 0;
    }

    .title {
        color: #666;
        margin: 0;
        font-size: 0.9rem;
    }

    .recent-activity h3 {
        font-size: 1.1rem;
        color: #374151;
        margin-bottom: 1rem;
    }

    .activity-item {
        display: flex;
        align-items: center;
        padding: 0.75rem 0;
        border-bottom: 1px solid #f3f4f6;
    }

    .user-avatar {
        font-size: 1.5rem;
        margin-right: 0.75rem;
    }

    .activity-details {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 0.25rem;
    }

    .user-name {
        font-weight: 500;
        color: #374151;
        font-size: 0.9rem;
    }

    .rating {
        font-size: 0.8rem;
        font-weight: bold;
    }

    .time {
        font-size: 0.75rem;
        color: #9ca3af;
    }

    /* Media query for smaller screens */
    @media (max-width: 1024px) {
        .sidebar {
            display: none;
        }
    }
</style> 