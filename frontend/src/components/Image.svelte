<script lang="ts">
    import Cube from "./Cube.svelte";

    let cube = {
        size: "70px",
        translate: "35px",
        speed: "7s",
        color: "blue",
        rotate: [1, 0, 0.2],
        margin: "30px",
    };
</script>

<Cube {cube} />

<style>
</style>
