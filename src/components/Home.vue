<script>
import { defineComponent } from 'vue'

class Particle {
        constructor(canvas, ctx) {
            this.canvas = canvas
            this.ctx = ctx
            this.x = Math.random() * this.canvas.width
            this.y = Math.random() * this.canvas.height
            this.size = 0.5
            this.speedX = Math.random() * 3 - 1.5
            this.speedY = Math.random() * 3 - 1.5
            this.hue = Math.random() * 360
        }

        update() {
            // console.log(this.color)
            this.x += this.speedX
            this.y += this.speedY

            if (this.x - this.size < 0 || this.x + this.size > this.canvas.width) {
                this.speedX = -this.speedX;
            }

            if (this.y - this.size < 0 || this.y + this.size > this.canvas.height) {
                this.speedY = -this.speedY;
            }
            this.hue += 2
            // if (this.size > 0.2) this.size -= 0.1
        }

        draw() {
            this.ctx.fillStyle = `hsl(${this.hue}, 100%, 50%)`
            this.ctx.beginPath()
            this.ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2)
            this.ctx.fill()
        }
}

    export default defineComponent({
        name: 'Home',
        data() {
            return {
                canvas: null,
                ctx: null,
                particlesArray: [],
                numberOfParticles: 1000,
            }
        },
        methods: {
            init() {
                //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
                for (let i = 0; i < this.numberOfParticles; i++) {
                    this.particlesArray.push(new Particle(this.canvas, this.ctx))
                }
            },
            animate() {
                this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
                for (let i = 0; i < this.particlesArray.length; i++) {
                    this.particlesArray[i].update()
                    this.particlesArray[i].draw()
                }
                requestAnimationFrame(this.animate)
            },
        },
        mounted() {
            this.canvas = this.$refs.canvas
            this.ctx = this.canvas.getContext('2d')
            this.canvas.width = window.innerWidth
            this.canvas.height = window.innerHeight

            this.init()
            this.animate()
        },
    })
</script>

<template>
    <canvas ref="canvas"></canvas>
    <div class="container w-screen h-screen bg-gray-900 flex items-center justify-center text-green-700 overflow-hidden">
      <h1>Hello I'm Mikael</h1>
    </div>
</template>

<style>
    @import url('https://fonts.googleapis.com/css2?family=DotGothic16&display=swap');

    .container {
        font-family: 'DotGothic16', sans-serif;
        font-size: 42px;
        letter-spacing: 1px;
        font-weight: 700;
    }

    canvas {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
</style>