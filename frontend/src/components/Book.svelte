<script lang="ts">
    export let item;
    export let switchers;

    function openChapter(id) {
        for (const key in switchers) {
            switchers[key] = false;
        }
        switchers["ch" + id] = true;
    }
</script>

<section class="book" style="--h: {item.h}">
    <article class="back" />
    <article class="page6">
        <p>{item.content}</p>
        <button on:click={openChapter(item.id)}>Czytaj dalej ...</button>
    </article>
    <article class="page5">
        <img src="img/{item.bg}" alt="book picture" />
    </article>
    <article class="page4" />
    <article class="page3" />
    <article class="page2" />
    <article class="page1" />
    <article class="front">
        <article class="front-shadow" />
        <h1>{item.title}</h1>
        <h2>{item.subtitle}</h2>
    </article>
</section>

<style>
    .book {
        --s: 50%;
        --l: 33%;

        margin: 50px 120px;
        transform-style: preserve-3d;
        position: relative;
        height: 300px;
        cursor: pointer;
        backface-visibility: visible;
        color: rgb(32, 30, 30);
    }

    .front,
    .back,
    .page1,
    .page2,
    .page3,
    .page4,
    .page5,
    .page6 {
        transform-style: preserve-3d;
        position: absolute;
        width: 200px;
        height: 100%;
        top: 0;
        left: 0;
        transform-origin: left center;
        transition: transform 0.5s ease-in-out, box-shadow 0.35s ease-in-out;
    }

    .front,
    .back {
        background: hsl(var(--h), var(--s), var(--l));
    }

    .front,
    .page1,
    .page3,
    .page5 {
        border-bottom-right-radius: 0.5em;
        border-top-right-radius: 0.5em;
    }

    .back,
    .page2,
    .page4,
    .page6 {
        border-bottom-right-radius: 0.5em;
        border-top-right-radius: 0.5em;
    }

    .front {
        box-shadow: inset -2px -1px 0.3px hsl(var(--h), var(--s), var(--l)),
            inset -4px 0 1px rgb(85, 84, 84),
            inset -8px 0 1px rgb(207, 206, 206),
            inset -12px 0 1px rgb(226, 224, 224);
    }

    .front-shadow {
        position: absolute;
        height: 100%;
        border-left: 1.9px solid black;
        margin-left: 12px;
        filter: blur(1.3px);
    }

    .front h1 {
        color: hsl(var(--h), calc(var(--s) - 10%), calc(var(--l) - 10%));
        text-align: center;
        font-size: 200%;
    }

    .front h2 {
        color: hsl(var(--h), calc(var(--s) - 10%), calc(var(--l) - 15%));
        text-align: center;
        font-size: 100%;
    }

    .page1 {
        background: #efefef;
    }

    .page2 {
        background: #efefef;
    }

    .page3 {
        background: #f5f5f5;
    }

    .page4 {
        background: #f5f5f5;
    }

    .page5 {
        background: #fafafa;
        border-left: rgba(148, 126, 126, 0.356) 0.6px solid;
    }

    .page5 img {
        width: 90%;
        margin-top: 20%;
        margin-left: 4%;
        opacity: 0.75;
    }

    .page6 {
        background: #fdfdfd;
    }

    .page6 p {
        margin: 30px;
        font-size: 120%;
    }

    .page6 button {
        margin-left: 15%;
        cursor: zoom-in;
    }

    .book:hover .front {
        transform: rotateY(-160deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .page1 {
        transform: rotateY(-150deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .page2 {
        transform: rotateY(-30deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .page3 {
        transform: rotateY(-140deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .page4 {
        transform: rotateY(-40deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .page5 {
        transform: rotateY(-130deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .page6 {
        transform: rotateY(-50deg) scale(1.1);
        box-shadow: 0 1em 3em 0 rgba(0, 0, 0, 0.2);
    }

    .book:hover .back {
        transform: rotateY(-20deg) scale(1.1);
    }
</style>
