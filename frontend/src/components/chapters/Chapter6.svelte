<script lang="ts">
    import Navigation from "../Navigation.svelte";
    import Image from "../Image.svelte";
    import Container from "../Container.svelte";
    export let switchers;

    let tabs = [
        { id: 1, active: false },
        { id: 0, active: true },
    ];
</script>

<Navigation bind:switchers bind:tabs />
<section class="subbody">
    <h1>Rozdział 6</h1>
    <h2>- Sieci kontenerów -</h2>

    {#if tabs[1].active}
        <article class="content-area">
            <h2>6.1</h2>
        </article>
        <br />
    {:else if tabs[0].active}
        <article class="content-area">
            <h2>6.2</h2>
        </article>
        <br />
    {/if}
</section>

<style>
</style>
