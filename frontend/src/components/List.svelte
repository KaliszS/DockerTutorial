<script>
    import ListItem from "./ListItem.svelte";
    export let items;
</script>

<ul class="tilesWrap">
    {#each items as item}
        <ListItem {item} />
    {/each}
</ul>

<style>
    .tilesWrap {
        padding: 0;
        margin: 20px auto;
        list-style: none;
        text-align: center;
    }
</style>
