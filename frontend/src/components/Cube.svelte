<script lang="ts">
    export let cube;

    let faces = ["top", "bottom", "left", "right", "front", "back"];
</script>

<article
    class="container"
    style="--size: {cube.size}; --translate: {cube.translate}; --translate2: -{cube.translate}; --speed: {cube.speed}; --rotx: {cube
        .rotate[0]}; --roty: {cube.rotate[1]}; --rotz: {cube
        .rotate[2]}; margin: {cube.margin};"
>
    <div class="cube">
        {#each faces as face}
            <div class="face {face}" style="background: {cube.color};" />
        {/each}
    </div>
</article>

<style>
    @keyframes turn {
        from {
            transform: rotate3d(0, 0, 0, 0);
        }
        to {
            transform: rotate3d(var(--rotx), var(--roty), var(--rotz), 360deg);
        }
    }

    .container {
        width: var(--size);
        height: var(--size);
        perspective: 500px;
    }

    .cube {
        position: relative;
        width: var(--size);
        height: var(--size);
        transform-style: preserve-3d;
        animation: turn var(--speed) linear infinite;
    }

    .face {
        width: var(--size);
        height: var(--size);
        border: 0.7px solid black;
        position: absolute;
        opacity: 0.5;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: Arial, sans-serif;
        font-size: 1.5rem;
        transition: transform 900ms;
    }

    .front {
        transform: translateZ(var(--translate));
    }

    .back {
        transform: translateZ(var(--translate2)) rotateY(180deg);
    }

    .left {
        transform: translateX(var(--translate2)) rotateY(-90deg);
    }

    .right {
        transform: translateX(var(--translate)) rotateY(90deg);
    }

    .top {
        transform: translateY(var(--translate2)) rotateX(90deg);
    }

    .bottom {
        transform: translateY(var(--translate)) rotateX(-90deg);
    }

    @media (prefers-reduced-motion: reduce) {
        .cube {
            animation: none;
            transform: rotate3d(1, 1, 0, 45deg);
        }
    }
</style>
