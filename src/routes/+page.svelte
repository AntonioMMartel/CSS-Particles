<svelte:window bind:innerHeight={innerHeight} bind:innerWidth={innerWidth}/>
<div class="main">
    <canvas 
        bind:this={canvas}>
    </canvas>
</div>

<script lang="ts">

    import { onMount } from 'svelte';

    let canvas: any;
    let innerHeight: any;
    let innerWidth: any;

    onMount(() => {
        // Canvas //
        const ctx = canvas.getContext('2d');
        canvas.height = innerHeight
        canvas.width = innerWidth

        let particleArray = []

        // Mouse Handler //
        const mouse = {
            x: 0,
            y: 0,
            radius: 150
        }

        window.addEventListener('mousemove', function(event){
            mouse.x = event.x;
            mouse.y = event.y
            //console.log(mouse.x, mouse.y)
        });

        ctx.fillStyle = "red"
        ctx.font = "50px Cassandra"
        ctx.fillText("Texto crema risa", 0 , 200)

    })

</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.main {
    background: black;
    overflow: hidden;
}


canvas {
    position: absolute;
    top: 0;
    left: 0;
}



</style>