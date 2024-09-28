<!-- src/routes/+page.svelte -->
<script>
    import Card from '$lib/components/Card.svelte';
</script>

<main class="flex items-center justify-center min-h-screen room-background">
    <Card />
</main>

<style>
    .room-background {
        position: relative;
    }
</style>