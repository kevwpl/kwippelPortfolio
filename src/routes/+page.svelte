<!-- src/routes/+page.svelte -->
<script>
    import Card from '$lib/components/Card.svelte';
</script>

<svelte:head>
    <title>Kevin Wippel</title>
</svelte:head>


<main class="flex items-center justify-center min-h-screen room-background">
    <Card />
</main>

<style>
    .room-background {
        position: relative;
    }
</style>