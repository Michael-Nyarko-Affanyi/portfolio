<script>
import { defineComponent } from 'vue'
import HomeContent from './HomeContent.vue'

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
        components: {
            HomeContent
        },
        data() {
            return {
                canvas: null,
                ctx: null,
                particlesArray: [],
                numberOfParticles: 1000,
                greeting: ["Hello I'm Mikael👋🏻", "I'm a full-stack developer", "Hop on and let me take you on a tour in my world"],
                doneIntro: localStorage.getItem('doneIntro') || false,
            }
        },
        methods: {
            wait(ms) { 
                return new Promise(resolve => setTimeout(resolve, ms))
            },
            greet() {
                const greetingText = this.$refs.greeting
                let i = 0
                let curWordIndex = 0
                const type = async () => {
                    while(curWordIndex < this.greeting.length && !this.doneIntro) {
                        for(let i = 0; i < this.greeting[curWordIndex].length; i++) {
                            greetingText.innerText = this.greeting[curWordIndex].substring(0, i + 1)
                            await this.wait(100)
                        }
    
                        await this.wait(2500)
    
                        for(let i = this.greeting[curWordIndex].length; i > 0; i--) {
                            greetingText.innerText = this.greeting[curWordIndex].substring(0, i - 1)
                            await this.wait(100)
                        }

                        if(curWordIndex === this.greeting.length - 1) {
                            this.doneIntro = true
                            localStorage.setItem('doneIntro', true)
                        }
                        curWordIndex++
                    }
                }
                type()
            },
            init() {
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
            this.greet()
        },
        // watch: {
        //     doneIntro(newVal) {
        //         if(newVal) {
        //             console.log('doneIntro')
        //             setTimeout(() => {
        //                 this.doneIntro = false
        //             }, 1000)
        //         }
        //     }
        // }
    })
</script>

<template>
    <div 
    class="w-screen h-screen bg-gray-900"
    >
    <div v-if="!doneIntro" class="container w-full h-full flex items-center justify-center text-green-700">
        <canvas ref="canvas"></canvas>
        <h1 ref="greeting" class=""></h1>
         <span id="blinker">|</span>
    </div>
    <transition name="fade">
        <HomeContent v-if="doneIntro" />
    </transition>
    </div>
</template>

<style>
    @import url('https://fonts.googleapis.com/css2?family=DotGothic16&display=swap');

    .fade-enter-active, .fade-leave-active {
        transition: opacity 0.5s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

    /* .text-green-700 {
        color: #00ff00;
    }

    .text-green-600 {
        color: #00cc00;
    } */

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

    #blinker {
        animation: blink 1s linear infinite;
    }

    @keyframes blink {
        0% {
            opacity: 1;
        }
        50% {
            opacity: 0;
        }
        100% {
            opacity: 1;
        }
    }

</style>