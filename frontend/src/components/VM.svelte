<script lang="ts">
    import Cube from "./Cube.svelte";

    let cube = {
        size: "168px",
        translate: "84px",
        speed: "20s",
        color: "darkgrey",
        rotate: [0, 0.5, 1],
        margin: "50px",
    };
</script>

<Cube {cube} />

<style>
</style>
