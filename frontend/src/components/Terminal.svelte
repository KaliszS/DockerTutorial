<script>
    export let command;
    export let output;
</script>

<section id="container">
    <div id="terminal">
        <!-- Terminal Bar -->
        <section id="terminal__bar">
            <div id="bar__buttons">
                <button class="bar__button" id="bar__button--exit"
                    >&#10005;</button
                >
                <button class="bar__button">&#9472;</button>
                <button class="bar__button">&#9723;</button>
            </div>
            <p id="bar__user">root@ubuntu: ~</p>
        </section>
        <!-- Terminal Body -->
        <section id="terminal__body">
            <div id="terminal__prompt">
                <span id="terminal__prompt--user">root@ubuntu:</span>
                <span id="terminal__prompt--location">~</span>
                <span id="terminal__prompt--bling">$</span>
                <span id="terminal__prompt--cursor" />
                <input spellcheck="false" bind:value={command} />
            </div>
            <div class="return">
                {#if output}
                    {#if output[command]}
                        {@html output[command]}
                    {/if}
                {/if}
            </div>
        </section>
    </div>
</section>

<style>
    @import url("https://fonts.googleapis.com/css?family=Ubuntu");
    @import url("https://fonts.googleapis.com/css?family=Ubuntu+Mono");

    input,
    input:focus,
    select:focus {
        -webkit-appearance: none;
        outline: none;
        width: 80%;
        border: 0;
        font: inherit;
        color: #fff;
        padding: inherit;
        background-color: inherit;
    }

    .return {
        color: #fff;
        margin-top: 20px;
        margin-left: 10px;
    }

    #container {
        display: flex;
        justify-content: center;
        align-items: center;
        background: linear-gradient(45deg, #57003f 0%, #f57453 100%);
        font-family: Ubuntu;
        width: 70vw;
        margin: auto;
    }

    #terminal {
        width: 70vw;
        height: 65vh;
        box-shadow: 2px 4px 10px rgba(0, 0, 0, 0.5);
    }

    #terminal__bar {
        display: flex;
        width: 100%;
        height: 30px;
        align-items: center;
        padding: 0 8px;
        box-sizing: border-box;
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
        background: linear-gradient(#504b45 0%, #3c3b37 100%);
    }

    #bar__buttons {
        display: flex;
        align-items: center;
    }

    .bar__button {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 0;
        margin-right: 5px;
        font-size: 8px;
        height: 12px;
        width: 12px;
        box-sizing: border-box;
        border: none;
        border-radius: 100%;
        background: linear-gradient(#7d7871 0%, #595953 100%);
        text-shadow: 0px 1px 0px rgba(255, 255, 255, 0.2);
        box-shadow: 0px 0px 1px 0px #41403a, 0px 1px 1px 0px #474642;
    }
    .bar__button:hover {
        cursor: pointer;
    }
    .bar__button:focus {
        outline: none;
    }
    #bar__button--exit {
        background: linear-gradient(#f37458 0%, #de4c12 100%);
        background-clip: padding-box;
    }

    #bar__user {
        color: #d5d0ce;
        margin-left: 6px;
        font-size: 14px;
        line-height: 15px;
    }

    #terminal__body {
        background: rgba(56, 4, 40, 0.9);
        font-family: "Ubuntu Mono";
        height: calc(100% - 30px);
        padding-top: 2px;
        margin-top: -1px;
    }

    #terminal__prompt {
        display: flex;
    }
    #terminal__prompt--user {
        color: #7eda28;
    }
    #terminal__prompt--location {
        color: #4878c0;
    }
    #terminal__prompt--bling {
        color: #dddddd;
    }
    #terminal__prompt--cursor {
        display: block;
        height: 17px;
        width: 8px;
        margin-left: 9px;
        animation: blink 1200ms linear infinite;
    }

    @keyframes blink {
        0% {
            background: #ffffff;
        }
        49% {
            background: #ffffff;
        }
        60% {
            background: transparent;
        }
        99% {
            background: transparent;
        }
        100% {
            background: #ffffff;
        }
    }

    @media (max-width: 600px) {
        #terminal {
            max-height: 90%;
            width: 90%;
        }
    }
</style>
