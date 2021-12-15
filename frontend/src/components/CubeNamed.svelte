<script lang="ts">
    import Image from "./Image.svelte";
    import Container from "./Container.svelte";

    export let cubeType;
    export let name;
</script>

<article>
    {#if cubeType == "image"}
        <Image />
    {:else if cubeType == "container"}
        <Container />
    {/if}
    {name}
</article>

<style>
    article {
        text-align: center;
    }
</style>
