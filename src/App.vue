<template>
    <div class="app">
        <div class="cubes">
            <button 
                v-for="btn of btns"
                v-bind:class="{[`cub${btn}Active`]: activeBtn === btn,[`cub${btn}`]: true}" 
                class="cub cub1"
                @click="activeateBtn(btn)"
                :key="btn">
                1
            </button>
            <!-- <button 
                v-bind:class="{cub1Active: activeBtn === 1}" 
                class="cub cub1"
                @click="activeateBtn(1)">
                1
            </button>
            <button
                v-bind:class="{cub2Active: activeBtn === 2}"
                class="cub cub2"
                @click="activeateBtn(2)">
                2
            </button>
            <button 
                v-bind:class="{cub3Active: activeBtn === 3}"
                class="cub cub3"
                @click="activeateBtn(3)">
                3
            </button>
            <button 
                v-bind:class="{cub4Active: activeBtn === 4}"
                class="cub cub4"
                @click="activeateBtn(4)">
                4
            </button> -->
        </div>
        <button class="startBtn" @click="start">
            START
        </button>
    </div>
</template>   

<script>
// import sound1 from './sounds/1.mp3'

export default {
    components: {
    },
    data() {
        return {
            sequence: [1,3,2,1,4],
            activeBtn:null,
            btns: [1,2,3,4]
        }
    },
    mounted() {
        
    },
    methods: {
        wait(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        },
        async start() {  
            // let index = 1;
            for(let el of this.sequence){
                let sound = new Audio(`/sounds/${el}.mp3`);          
                // sound1.play()
                sound.play()
                this.activeBtn = el;
                await this.wait(1000); 
                this.activeBtn = null;
                // index+=1;
            }      
        },
        async activeateBtn(number) {
            this.activeBtn = number;
            let sound = new Audio(`/sounds/${number}.mp3`);  
            sound.play()
            await this.wait(1000); 
            this.activeBtn = null
        }
    }
}
</script>

<style>
* {
    margin:0;
    padding:0;
    box-sizing: border-box;
}

.app {
    margin-left: 30px;
}

.cubes {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
    /* border: 1px solid black; */
    width: 400px;
    height: 400px;
    margin-top: 20px;
}
.cub {
    width: 200px;
    height: 200px;
    cursor: pointer;
    border: 0;

}
.cub1{
    background: #FF5643;
    border-radius: 20px 0 0 0 ;
}
.cub1:hover {
    border-left: 1px solid black;
    border-top: 1px solid black;
}
.cub1Active{
    background: red;
}

.cub2{
    background: dodgerblue;
    border-radius: 0 20px 0 0 ;
}
.cub2:hover {
    border-right: 1px solid black;
    border-top: 1px solid black;
}
.cub2Active{
    background: blue;
}

.cub3{
    background: #BEDE15;
    border-radius: 0 0 0 20px ;
}
.cub3:hover {
    border-left: 1px solid black;
    border-bottom: 1px solid black;
}
.cub3Active{
    background: green;
}

.cub4{
    background: #FEEF33;
    border-radius: 0 0 20px 0 ;
}
.cub4:hover {
    border-right: 1px solid black;
    border-bottom: 1px solid black;
}
.cub4Active{
    background: yellow;
}

.startBtn {
    background: rgb(118, 226, 118);
    color: white;
    width: 200px;
    height: 50px;
    margin-top: 20px;
    border: 0;
    border-radius: 20px;
    cursor: pointer;
}




</style>