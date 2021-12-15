<script lang="ts">
    export let code;
</script>

<section class="editor">
    <div class="browser-mockup">
        {@html code}
    </div>
</section>

<style>
    .browser-mockup {
        border-top: 2em solid rgba(230, 230, 230, 0.7);
        box-shadow: 0 0.1em 1em 0 rgba(0, 0, 0, 0.4);
        position: relative;
        border-radius: 3px 3px 0 0;
        color: white;
        background-color: #1e1e1e;
        padding: 5px;
    }

    .browser-mockup:before {
        display: block;
        position: absolute;
        content: "";
        top: -1.25em;
        left: 1em;
        width: 0.5em;
        height: 0.5em;
        border-radius: 50%;
        background-color: #f44;
        box-shadow: 0 0 0 2px #f44, 1.5em 0 0 2px #9b3, 3em 0 0 2px #fb5;
    }

    .browser-mockup > * {
        /* display: block; */
    }

    .browser-mockup {
        margin: 2em;
        flex: 1;
    }

    .docker {
        color: #008afa;
    }
</style>
