<script lang="ts">
    import Cube from "./Cube.svelte";

    let cube = {
        size: "42px",
        translate: "21px",
        speed: "3s",
        color: "purple",
        rotate: [1, 1, 0],
        margin: "20px",
    };
</script>

<Cube {cube} />

<style>
</style>
