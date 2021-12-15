<script lang="ts">
    import Book from "./Book.svelte";
    export let items;
    export let switchers;
</script>

<section>
    <section class="books">
        {#each items as item}
            <Book {item} bind:switchers />
        {/each}
    </section>
</section>

<style>
    .books {
        margin: 0 200px 0 70px;
        display: flex;
        flex-flow: row wrap;
        justify-content: space-evenly;
    }
</style>
