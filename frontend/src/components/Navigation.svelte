<script lang="ts">
    export let switchers;
    export let tabs;

    function openMenu() {
        for (const key in switchers) {
            switchers[key] = false;
        }
        switchers["menu"] = true;
    }

    function switchTab(id) {
        tabs.forEach((item) => {
            if (item.id == id) return;
            item.active = false;
        });
    }
</script>

<button on:click={openMenu}>Powrót do listy rozdziałów ...</button>

<div class="command">w tym podrozdziale jesteś ...</div>
<br />
<div class="bar">
    {#each tabs as tab}
        <input
            on:click={switchTab(tab.id)}
            bind:value={tab.active}
            class="bar-input"
            type="radio"
            name="input"
            id="input_{tab.id}"
            checked
        />
        <div class="bar-view">
            <label class="bar-button" for="input_{tab.id}" />
        </div>
    {/each}
</div>

<style>
    .bar {
        display: flex;
        flex-direction: row-reverse;
        margin: auto auto 0;
        width: 300px;
        max-width: calc(100% - 30px);
    }
    .bar-input {
        opacity: 0;
        pointer-events: none;
        position: absolute;
        top: -9999px;
        left: -9999px;
    }
    .bar-input:checked ~ .bar-view:nth-child(10):after {
        transition-delay: -0.05s;
    }
    .bar-input:checked ~ .bar-view:nth-child(10) .bar-button:before {
        transition-delay: 0.01s;
    }
    .bar-input:checked ~ .bar-view:nth-child(8):after {
        transition-delay: 0.05s;
    }
    .bar-input:checked ~ .bar-view:nth-child(8) .bar-button:before {
        transition-delay: 0.11s;
    }
    .bar-input:checked ~ .bar-view:nth-child(6):after {
        transition-delay: 0.15s;
    }
    .bar-input:checked ~ .bar-view:nth-child(6) .bar-button:before {
        transition-delay: 0.21s;
    }
    .bar-input:checked ~ .bar-view:nth-child(4):after {
        transition-delay: 0.25s;
    }
    .bar-input:checked ~ .bar-view:nth-child(4) .bar-button:before {
        transition-delay: 0.31s;
    }
    .bar-input:checked ~ .bar-view:nth-child(2):after {
        transition-delay: 0.35s;
    }
    .bar-input:checked ~ .bar-view:nth-child(2) .bar-button:before {
        transition-delay: 0.41s;
    }
    .bar-input:checked ~ .bar-view:after {
        transform: scaleX(1);
    }
    .bar-input:checked ~ .bar-view .bar-button:before {
        transform: none;
        opacity: 1;
    }
    .bar-input:checked + .bar-view .bar-button:after {
        opacity: 1;
        -webkit-animation: bouncing 0.6s cubic-bezier(0, 0, 0.74, 1.04) infinite;
        animation: bouncing 0.6s cubic-bezier(0, 0, 0.74, 1.04) infinite;
    }
    .bar-view {
        display: flex;
        flex-grow: 1;
        position: relative;
    }
    .bar-view:after {
        height: 2px;
        top: calc(50% - 1px);
        transition: transform 0.06s cubic-bezier(0, 0.72, 0.58, 1);
        transform: scaleX(0);
        background: #ffb732;
        transform-origin: left;
        z-index: 1;
    }
    .bar-view:not(:last-child):before,
    .bar-view:not(:last-child):after {
        content: "";
        width: calc(100% - 24px);
        position: absolute;
        right: calc(50% + 12px);
    }
    .bar-view:not(:last-child):before {
        height: 6px;
        top: calc(50% - 3px);
        background: #3c6997;
    }
    .bar-view:not(:last-child):nth-child(2):after {
        transition-delay: 0s;
    }
    .bar-view:not(:last-child):nth-child(2) .bar-button:before {
        transition-delay: 0.06s;
    }
    .bar-view:not(:last-child):nth-child(4):after {
        transition-delay: 0.1s;
    }
    .bar-view:not(:last-child):nth-child(4) .bar-button:before {
        transition-delay: 0.16s;
    }
    .bar-view:not(:last-child):nth-child(6):after {
        transition-delay: 0.2s;
    }
    .bar-view:not(:last-child):nth-child(6) .bar-button:before {
        transition-delay: 0.26s;
    }
    .bar-view:not(:last-child):nth-child(8):after {
        transition-delay: 0.3s;
    }
    .bar-view:not(:last-child):nth-child(8) .bar-button:before {
        transition-delay: 0.36s;
    }
    .bar-view:not(:last-child):nth-child(10):after {
        transition-delay: 0.4s;
    }
    .bar-view:not(:last-child):nth-child(10) .bar-button:before {
        transition-delay: 0.46s;
    }
    .bar-button {
        display: block;
        width: 30px;
        height: 30px;
        margin: auto;
        border-radius: 50%;
        border: 3px solid #3c6997;
        position: relative;
        cursor: pointer;
        box-shadow: inset 2px 2px 4px rgba(0, 0, 0, 0.3),
            2px 2px 8px rgba(0, 0, 0, 0.1);
    }
    .bar-button:hover:after {
        transform: none;
        opacity: 1;
    }
    .bar-button:before,
    .bar-button:after {
        content: "";
        position: absolute;
        pointer-events: none;
    }
    .bar-button:before {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background: radial-gradient(circle at center, #ffdd4a, #fe9000);
        transform: scale(0.3);
        opacity: 0;
        transition-property: transform, opacity;
        transition-duration: 0.2s;
        transition-timing-function: cubic-bezier(0, 0.72, 0.58, 1);
    }
    .bar-button:after {
        border: 8px solid transparent;
        border-top-color: #3c6997;
        bottom: calc(100% + 10px);
        left: calc(50% - 8px);
        transform: translateY(-10px);
        opacity: 0;
        transition-property: transform, opacity;
        transition-duration: 0.2s;
        transition-timing-function: cubic-bezier(0, 0, 0, 1.97);
    }

    .command {
        text-align: center;
        font-family: "Ubuntu Mono", monospace;
        letter-spacing: 1px;
        margin: 30px auto auto;
        padding-bottom: 8px;
        -webkit-animation: fade 2s ease infinite;
        animation: fade 2s ease infinite;
    }

    html,
    body {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        background: linear-gradient(
            to bottom right,
            rgba(255, 221, 74, 0.1),
            #fff
        );
    }

    * {
        box-sizing: border-box;
    }
    *:before,
    *:after {
        box-sizing: inherit;
    }

    @-webkit-keyframes bouncing {
        0%,
        100% {
            transform: translateY(0);
        }
        50% {
            transform: translateY(5px);
        }
    }

    @keyframes bouncing {
        0%,
        100% {
            transform: translateY(0);
        }
        50% {
            transform: translateY(5px);
        }
    }
    @-webkit-keyframes fade {
        50% {
            opacity: 0.3;
        }
    }
    @keyframes fade {
        50% {
            opacity: 0.3;
        }
    }
</style>
