<script>
    export let item;
</script>

<li>
    <h2>{item.no}</h2>
    <h3>{item.title}</h3>
    <p>
        {item.content}
    </p>
</li>

<style>
    li {
        display: inline-block;
        width: 20%;
        min-width: 150px;
        max-width: 230px;
        padding: 80px 20px 40px;
        position: relative;
        vertical-align: top;
        margin: 5px;
        font-family: "helvetica", san-serif;
        min-height: 25vh;
        text-align: left;
        border-radius: 10%;
    }
    li h2 {
        font-size: 114px;
        margin: 0;
        position: absolute;
        opacity: 0.2;
        top: 50px;
        right: 10px;
        transition: all 0.3s ease-in-out;
    }
    li h3 {
        font-size: 20px;
        color: #3c6997;
        margin-bottom: 5px;
        padding: 10px;
        box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px,
            rgba(0, 0, 0, 0.3) 0px 30px 60px -30px,
            rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
    }
    li p {
        font-size: 16px;
        line-height: 18px;
        color: rgba(33, 82, 131, 0.801);
        margin-top: 5px;
    }
    li:hover h2 {
        top: 0px;
        opacity: 0.6;
    }

    li:before {
        content: "";
        position: absolute;
        top: -2px;
        left: -2px;
        right: -2px;
        bottom: -2px;
        z-index: -1;
        transform: skew(2deg, 2deg);
    }
</style>
