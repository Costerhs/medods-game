<template>
    <div class="app">
        <h2>кол-во {{ this.sequence.length }}</h2>
        <h2>количество кликов{{ this.clickNumber }}</h2>
        <div class="cubes">
            <button 
                v-for="btn of btns"
                v-bind:class="{[`cub${btn}Active`]: activeBtn === btn,[`cub${btn}`]: true}" 
                class="cub"
                :disabled="isDisabled()"
                @click="activateBtn(btn)"
                :key="btn">
            </button>
        </div>
        <button class="startBtn" @click="start">
            START
        </button>
        <div class="levels">
            <div v-for="el of Object.entries(levels)" :key="el[0]">
                <label :for='el[0]'>{{ el[0] }}</label>
                <input :checked="activeLevel == el[1]" @change="toggleChecked(el[1])" type="checkbox" :id="el[0]"/>
            </div>
        </div>
    </div>
</template>   

<script>

export default {
    components: {
    },
    data() {
        return {
            levels:{
                easy:1.5,
                normal:1,
                hard:0.4
            },
            activeLevel:1.5,
            sequence: [],
            activeBtn:null,
            btns: [1,2,3,4],
            statusBtn: true,
            clickNumber:0,
           
        }
    },
    methods: {
        wait(ms) {
            return new Promise(resolve => setTimeout(resolve, ms*500));
        },
        async waitAndChangeLight(ms){
            await this.wait(ms); 
            this.activeBtn = null;
            await this.wait(ms); 
        },

        async start() {  
            this.sequence = [...this.sequence,this.getRandomNumber(1,4)];
 
            for(let el of this.sequence){
                let sound = new Audio(`/sounds/${el}.mp3`);          
                sound.play()
                this.activeBtn = el;
                await this.waitAndChangeLight(this.activeLevel)
            }   
            this.statusBtn = false   
        },

        async activateBtn(number) {
            // console.log('work');
            
            this.activeBtn = number;
            let sound = new Audio(`/sounds/${number}.mp3`);  
            sound.play();

            //если неправильно ответил      
            if(!this.checkCorrectColorPressed(number)) {
                this.checkContinueOrNextRound()
            }
        },

        isDisabled() {
            return this.statusBtn
        },

        getRandomNumber(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },
        toggleChecked(id) {
            this.activeLevel = id;
            this.reset()
        },
        reset() {
            this.clickNumber = 0;
            this.sequence = [];
            this.statusBtn = true
        },
        checkCorrectColorPressed(number) {
            if (number !== this.sequence[this.clickNumber]) {
               this.reset()
               return true
            }
        },
        async checkContinueOrNextRound() {
            if(this.clickNumber + 1 == this.sequence.length) {
                this.clickNumber = 0;
                this.statusBtn = true
                await this.waitAndChangeLight(1) 
                this.start()
            } else {
                this.clickNumber += 1
                await this.waitAndChangeLight(this.activeLevel);  
            }
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
    /*  */
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