<script>
</script>

<div class="cloud"><span style="z-index: 10000;">DockerHub</span></div>

<style>
    .cloud {
        width: 350px;
        height: 120px;
        text-align: center;
        background: #f2f9fe;
        background: linear-gradient(top, #f2f9fe 5%, #d6f0fd 100%);
        background: -webkit-linear-gradient(top, #f2f9fe 5%, #d6f0fd 100%);
        background: -moz-linear-gradient(top, #f2f9fe 5%, #d6f0fd 100%);
        background: -ms-linear-gradient(top, #f2f9fe 5%, #d6f0fd 100%);
        background: -o-linear-gradient(top, #f2f9fe 5%, #d6f0fd 100%);

        border-radius: 100px;
        -webkit-border-radius: 100px;
        -moz-border-radius: 100px;

        position: relative;
        margin: 120px auto 20px;
    }

    .cloud:after,
    .cloud:before {
        content: "";
        position: absolute;
        background: #f2f9fe;
        z-index: 1;
    }

    .cloud:after {
        width: 100px;
        height: 100px;
        top: -50px;
        left: 50px;

        border-radius: 100px;
        -webkit-border-radius: 100px;
        -moz-border-radius: 100px;
    }

    .cloud:before {
        width: 180px;
        height: 180px;
        top: -90px;
        right: 50px;
        border-radius: 200px;
        -webkit-border-radius: 200px;
        -moz-border-radius: 200px;
    }
</style>
