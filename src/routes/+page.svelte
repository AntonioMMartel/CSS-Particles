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
            defaultSize: number = 3;
            density: number = (Math.random() * 30) + 1 // Makes particle faster or slower. Like weight
            constructor(x: number, y: number){
                this.x = x
                this.y = y
                this.baseX = this.x
                this.baseY = this.y
            }

            draw() {
                ctx.fillStyle = "red"
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2) // Draw a whole circle
                ctx.closePath();
                ctx.fill()
            }

            update() {
                let dx = mouse.x - this.x
                let dy = mouse.y - this.y
                let distance = Math.sqrt(dy * dy + dx * dx)

                let forceDirectionX = dx/distance; // % of distance in x axis
                let forceDirectionY = dy/distance; // % of distance in y axis

                let maxDistance = mouse.radius
                let force = (maxDistance - distance) / maxDistance // The closer to the mouse the stronge the force

                let directionX = forceDirectionX * force * this.density // Amount of force the particle suffers reduced by density
                let directionY = forceDirectionY * force * this.density

                if(distance < maxDistance) { // putting a maxDistance > mouse radius generates magnet effect then push effect around mouse
                    this.x -= directionX
                    this.y -= directionY
                } else {
                    this.size = this.defaultSize
                }


                /* Change size
                if(distance < 500) {
                    this.size = 50
                } else {
                    this.size = this.defaultSize
                } */


            }
 
        }

        let particleAmount:number = 200

        function init() {
            particleArray = []
            // Fill with particles at random positions
            for(let i = 0; i < particleAmount; i++){
                particleArray.push(new Particle(Math.random() * canvas.width, Math.random() * canvas.height)) 
            }

        }
        init();

        // Animation loop
        function animate() {
            ctx.clearRect(0,0, canvas.width, canvas.height) // Clear canvas each frame
            particleArray.forEach(particle => {
                particle.draw()
                particle.update()
            });
            requestAnimationFrame(animate) // Recursive call to animation loop

        }

        animate();


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