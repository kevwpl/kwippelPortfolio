<script>
    import "../app.css";
</script>

<main class="flex items-center justify-center min-h-screen room-background">
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
</style>