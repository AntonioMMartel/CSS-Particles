<svelte:window bind:innerHeight={innerHeight} bind:innerWidth={innerWidth}/>
<div class="main">
    <canvas 
        bind:this={canvas}>
    </canvas>
</div>

<script lang="ts">

    import { onMount } from 'svelte';
	import { init } from 'svelte/internal';

    let canvas: any;
    let innerHeight: any;
    let innerWidth: any;

    onMount(() => {
        // Canvas //
        const ctx = canvas.getContext('2d');
        canvas.height = innerHeight
        canvas.width = innerWidth

        let particleArray: any[] = []

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

        // 100x100px area to copy
        const data = ctx.getImageData(0,0,100,100)

        // Particles

        class Particle {
            x: number;
            y: number;
            baseX: number;
            baseY: number;
            size: number = 3; // in pixels
            density: number = (Math.random() * 30) + 1 // Makes particle faster or slower. Like weight
            constructor(x: number, y: number){
                this.x = x
                this.y = y
                this.baseX = this.x
                this.baseY = this.y
            }

            draw() {
                ctx.fillStyle = "white"
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2) // Draw a whole circle
                ctx.closePath();
                ctx.fill()
            }
 
        }

        function init() {
            particleArray = []
            particleArray.push(new Particle(80, 50))
            particleArray.push(new Particle(80, 50))
            particleArray.push(new Particle(20, 50))
            particleArray.push(new Particle(4, 50))


        }
        init();
        console.log(particleArray)


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