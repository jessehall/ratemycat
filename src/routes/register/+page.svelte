<script>
    let email = '';
    let password = '';
    let confirmPassword = '';

    async function handleSubmit(e) {
        e.preventDefault();
        if (password !== confirmPassword) {
            alert("Passwords don't match!");
            return;
        }
        // TODO: Implement registration logic
        console.log('Register attempt:', { email, password });
    }
</script>

<div class="auth-container">
    <h1>Register</h1>
    <form on:submit={handleSubmit}>
        <div class="form-group">
            <label for="email">Email</label>
            <input 
                type="email" 
                id="email" 
                bind:value={email} 
                required
            />
        </div>
        <div class="form-group">
            <label for="password">Password</label>
            <input 
                type="password" 
                id="password" 
                bind:value={password} 
                required
            />
        </div>
        <div class="form-group">
            <label for="confirm-password">Confirm Password</label>
            <input 
                type="password" 
                id="confirm-password" 
                bind:value={confirmPassword} 
                required
            />
        </div>
        <button type="submit">Register</button>
    </form>
    <p>Already have an account? <a href="/login">Login here</a></p>
</div>

<style>
    /* Same styles as login page */
    .auth-container {
        max-width: 400px;
        margin: 2rem auto;
        padding: 2rem;
        border: 1px solid #ddd;
        border-radius: 8px;
    }

    .form-group {
        margin-bottom: 1rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
    }

    input {
        width: 100%;
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 4px;
    }

    button {
        width: 100%;
        padding: 0.75rem;
        background: #4CAF50;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    button:hover {
        background: #45a049;
    }
</style> 