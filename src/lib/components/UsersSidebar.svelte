<script>
    const mockUsers = [
        { name: 'Whiskers McGee', rating: 'Hot!', time: '2m ago', avatar: '🐱' },
        { name: 'Pawsome Paula', rating: 'Not', time: '5m ago', avatar: '😺' },
        { name: 'Meow Master', rating: 'Hot!', time: '7m ago', avatar: '😸' },
        { name: 'Purrington III', rating: 'Hot!', time: '12m ago', avatar: '😺' },
        { name: 'Feline Dion', rating: 'Not', time: '15m ago', avatar: '🐱' },
        { name: 'Claw-dia', rating: 'Hot!', time: '20m ago', avatar: '😸' }
    ];
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
        {#each mockUsers as user}
            <div class="activity-item">
                <span class="user-avatar">{user.avatar}</span>
                <div class="activity-details">
                    <span class="user-name">{user.name}</span>
                    <span class="rating" class:hot={user.rating === 'Hot!'}>
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
        height: 100vh;
        position: fixed;
        right: 0;
        top: 0;
        background: white;
        border-left: 1px solid #e5e7eb;
        padding: 1.5rem;
        overflow-y: auto;
        box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
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
        color: #ef4444;
    }

    .rating.hot {
        color: #22c55e;
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