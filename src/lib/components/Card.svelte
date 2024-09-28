<script>
    import { onMount, onDestroy } from "svelte";
    import { cubicOut } from 'svelte/easing';
    import { tweened } from 'svelte/motion';
    import bcFront from '$lib/img/Front.png';
    import bcBack from '$lib/img/Back.png';

    export let angle = 0;
    let isDragging = false;
    let startX = 0;
    let velocity = 0;
    let animationFrameId;
    let autoSpinTimeoutId;
    const DRAG_INTENSITY = 0.43;
    const AUTO_SPIN_INTERVAL = 3000;
    const AUTO_SPIN_MAX_SPEED = 0.3;
    const AUTO_SPIN_ACCELERATION = 0.0005;
    const MOUSE_PROXIMITY_THRESHOLD = 300;
    const scale = tweened(1, { duration: 150, easing: cubicOut });

    let cardContainer;

    function handleMouseDown(event) {
        isDragging = true;
        startX = event.clientX;
        cancelAnimationFrame(animationFrameId);
        clearTimeout(autoSpinTimeoutId);
    }

    function handleMouseUp() {
        isDragging = false;
        applyFriction();
        setAutoSpinTimeout();
    }

    function handleMouseMove(event) {
        if (!cardContainer) return;

        const cardRect = cardContainer.getBoundingClientRect();
        const cardCenterX = cardRect.left + cardRect.width / 2;
        const cardCenterY = cardRect.top + cardRect.height / 2;
        const distance = Math.sqrt(Math.pow(event.clientX - cardCenterX, 2) + Math.pow(event.clientY - cardCenterY, 2));

        if (distance < MOUSE_PROXIMITY_THRESHOLD) {
            scale.set(1.1);
        } else {
            scale.set(1);
        }

        if (isDragging) {
            const deltaX = event.clientX - startX;
            velocity = deltaX * DRAG_INTENSITY;
            angle += velocity;
            startX = event.clientX;
        }
    }

    function applyFriction() {
        velocity *= 0.95;
        angle += velocity;

        if (Math.abs(velocity) > 0.01) {
            animationFrameId = requestAnimationFrame(applyFriction);
        }
    }

    function setAutoSpinTimeout() {
        autoSpinTimeoutId = setTimeout(startAutoSpin, AUTO_SPIN_INTERVAL);
    }

    function startAutoSpin() {
        let currentSpinSpeed = 0;

        function autoSpin() {
            if (currentSpinSpeed < AUTO_SPIN_MAX_SPEED) {
                currentSpinSpeed += AUTO_SPIN_ACCELERATION;
            }
            angle += currentSpinSpeed;
            animationFrameId = requestAnimationFrame(autoSpin);
        }

        autoSpin();
    }

    onMount(() => {
        if (cardContainer) {
            cardContainer.addEventListener('mousedown', handleMouseDown);
            window.addEventListener('mousemove', handleMouseMove);
            window.addEventListener('mouseup', handleMouseUp);
            setAutoSpinTimeout();
        }
    });

    onDestroy(() => {
        if (cardContainer) {
            cardContainer.removeEventListener('mousedown', handleMouseDown);
            window.removeEventListener('mousemove', handleMouseMove);
            window.removeEventListener('mouseup', handleMouseUp);
            clearTimeout(autoSpinTimeoutId);
            cancelAnimationFrame(animationFrameId);
        }
    });
</script>

<div bind:this={cardContainer} class="card-container">
    <div class="card" style="transform: rotateY({angle}deg) scale({$scale})">
        <div class="front">
            <img src={bcFront} alt="Business Card Front">
        </div>
        <div class="back">
            <img src={bcBack} alt="Business Card Back">
        </div>
    </div>
</div>

<style>
    .card-container {
        width: 350px;
        height: 200px;
        perspective: 1000px;
    }

    .card {
        width: 100%;
        height: 100%;
        position: relative;
        transform-style: preserve-3d;
        transition: transform 0.1s linear;
    }

    .front, .back {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        backface-visibility: hidden;
        border-radius: 8px;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        background-color: white;
    }

    .front img, .back img {
        width: 100%;
        height: 100%;
        object-fit: contain;
        border-radius: 8px;
        user-select: none;
        pointer-events: none;
    }

    .back {
        transform: rotateY(180deg);
    }
</style>