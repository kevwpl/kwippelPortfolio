<script>
    import "../app.css";
    import { onMount } from 'svelte';

    let darkMode = false;

    onMount(() => {
        // Check if the user previously selected dark mode
        darkMode = localStorage.getItem('darkMode') === 'true';
    });

    function toggleDarkMode() {
        darkMode = !darkMode;
        localStorage.setItem('darkMode', darkMode);
    }
</script>

<main class:dark-theme={darkMode} class="flex items-center justify-center min-h-screen room-background">
    <div class="top-container">
        <button type="button" class="dark-mode-toggle nav-link" on:click={toggleDarkMode}>
            {#if darkMode}Light{/if}
            {#if !darkMode}Dark{/if}
        </button>
    </div>

    <slot />
    <div class="links-container">
        <a href="/" class="nav-link">Home</a>
        <a href="/imprint" class="nav-link">Imprint</a>
    </div>
</main>

<style>
    main {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        background: radial-gradient(circle at center, #ffffff 0%, #f2f2f2 50%, #eaeaea 100%),
        radial-gradient(circle at 50% 25%, rgba(255, 255, 255, 0.8), transparent 50%);
        background-blend-mode: lighten;
        box-shadow: inset 0 50px 100px rgba(255, 255, 255, 0.5),
        inset 0 -50px 100px rgba(255, 255, 255, 0.5);
    }

    .room-background {
        position: relative;
    }

    .links-container {
        position: absolute;
        bottom: 10px;
        display: flex;
        gap: 10px;
    }

    .nav-link {
        font-size: 0.8rem;
        color: #333;
        text-decoration: none;
        transition: color 0.3s, filter 0.3s;
    }

    .nav-link:hover {
        color: #555;
    }

    .links-container:hover .nav-link {
        filter: blur(2px);
    }

    .links-container:hover .nav-link:hover {
        filter: none;
    }

    .top-container {
        position: absolute;
        top: 10px;
        right: 10px;
    }

    .dark-mode-toggle {
        background: none;
        border: none;
        font-size: 0.8rem; /* matches the nav-link font size */
        cursor: pointer;
        text-decoration: none;
        color: #333;
    }

    .dark-theme .dark-mode-toggle {
        color: #ddd;
    }

    .dark-theme {
        background: radial-gradient(circle at center, #111 0%, #1b1b1b 50%, #2e2e2e 100%),
        radial-gradient(circle at 50% 25%, rgba(0, 0, 0, 0.8), transparent 50%);
        background-blend-mode: lighten;
        box-shadow: inset 0 50px 100px rgba(0, 0, 0, 0.5),
        inset 0 -50px 100px rgba(0, 0, 0, 0.5);
    }

    .dark-theme .nav-link:hover {
        color: #fff;
    }

    .dark-theme p, .dark-theme h1, .dark-theme h2, .dark-theme h3, .dark-theme h4, .dark-theme h5, .dark-theme h6, .dark-theme a {
        color: #ddd;
    }

    .dark-theme p:hover, .dark-theme h1:hover, .dark-theme h2:hover, .dark-theme h3:hover, .dark-theme h4:hover, .dark-theme h5:hover, .dark-theme h6:hover, .dark-theme a:hover {
        color: #fff;
    }
</style>