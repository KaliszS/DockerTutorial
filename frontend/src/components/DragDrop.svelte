<script>
    import { draggable } from "../dragdrop.js";
    import { crossfade } from "svelte/transition";
    import { quintOut, elasticOut } from "svelte/easing";
    import { flip } from "svelte/animate";

    const shelf = [null, null, null, null];
    const cart = [
        { id: 5, name: "FROM python:slim" },
        { id: 4, name: "CMD python src/main.py" },
        { id: 3, name: "COPY /src ." },
        { id: 2, name: "RUN pip -r install /src/req.txt" },
    ];

    function putInShelf(item, index) {
        const oldItem = shelf[index];
        const oldShelfIndex = shelf.indexOf(item);
        if (cart.indexOf(item) !== -1) cart.splice(cart.indexOf(item), 1);
        if (oldShelfIndex !== -1) shelf[oldShelfIndex] = oldItem;
        else if (oldItem) cart.push(oldItem);
        shelf[index] = item;
        cart = cart;
    }

    function putInCart(item) {
        if (cart.indexOf(item) !== -1) cart.splice(cart.indexOf(item), 1);
        if (shelf.indexOf(item) !== -1) shelf[shelf.indexOf(item)] = null;
        cart.push(item);
        cart = cart;
    }

    const [send, receive] = crossfade({
        duration: (d) => 600,
        easing: elasticOut,
        fallback(node, params) {
            const style = getComputedStyle(node);
            const transform = style.transform === "none" ? "" : style.transform;

            return {
                duration: 600,
                easing: quintOut,
                css: (t) => `
            transform: ${transform} scale(${t});
            opacity: ${t}
          `,
            };
        },
    });
</script>

Obraz
<div class="shelf">
    {#each shelf as item, index}
        <div class="slot" on:dropped={(e) => putInShelf(e.detail, index)}>
            {#if item}
                {#each [item] as item (item.id)}
                    <div
                        class="item"
                        use:draggable={{
                            data: item,
                            targets: [".cart", ".slot", ".slot .item"],
                        }}
                        in:receive={item.id}
                        out:send={item.id}
                        on:dropped={(e) => putInShelf(e.detail, index)}
                    >
                        {item.name}
                    </div>
                {/each}
            {/if}
        </div>
    {/each}
</div>
Warstwy z Dockerfile
<div class="cart" on:dropped={(e) => putInCart(e.detail)}>
    {#each cart as item, index (item.id)}
        <div
            class="item"
            animate:flip
            use:draggable={{ data: item, targets: [".slot", ".slot .item"] }}
            in:receive={item.id}
            out:send={item.id}
        >
            {item.name}
        </div>
    {/each}
</div>

<style>
    .slot {
        position: relative;
        display: inline-block;
        background: #eee;
        box-shadow: 5px 5px 10px -10px black inset;
        width: 250px;
        height: 64px;
        margin: 3px;
        vertical-align: top;
    }
    .cart {
        position: relative;
        background: #eee;
        box-shadow: 5px 5px 10px -10px black inset;
        min-height: 64px;
    }
    .item {
        height: 56px;
        width: 240px;
        position: relative;
        display: inline-block;
        background: rgba(255, 255, 255, 0.5);
        margin: 4px;
        box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.2);
    }
    .slot .item {
        position: absolute;
    }
    :global(.dragged) {
        pointer-events: none;
        z-index: 100;
    }
    :global(.slot.droptarget, .cart.droptarget) {
        background: #ddd;
    }
</style>
