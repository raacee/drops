<template>
    <div class="main-container">
        <div class="control">
            <p style="font-size:25px;">
                Welcome back {{ userName }} <br><br> Today you have drank {{ totalVolume }} liters of water
            </p>
            <p v-if="this.totalVolume < this.volumeObjective">
                Your objective is {{ volumeObjective }} liters per day
            </p>
            <p v-else >
                Good Job ! You have reached your objective for today.
            </p>
            <p v-if="this.totalVolume < this.volumeObjective">
                You need {{ Math.round((volumeObjective - totalVolume)*100)/100 }} to complete
            </p>
        </div>
        <div style="color:white">
            <p style="font-size: 40px;">
                Add more water
            </p>
        </div>
        <div style="margin-bottom:30px;">
            <input class="liters-input" @input="inputNumberAbs" v-model="volumeInputText">
            <span style="color:white; margin-left:10px; font-size: 20px"> mL </span>
        </div>
        <div>
            <button class="add-water-button" @click.prevent="addDrink">
                +
            </button>
        </div>
    </div>

    <div style="z-index:-9999">
        <div :style="'position:fixed; bottom:'+this.wavePosition+'px;'" ref="wave" class="">
            <Wave :volumePercentage="this.volumePercentage"/>
        </div>
        <div ref="water" class="bottom-fill"
             :style="'position:fixed; bottom:0; height:'+this.waterHeight+'px; width:100%; background:rgb('+this.bottomColor+')'">
        </div>
    </div>


</template>

<script>
import Wave from "./Wave.vue";

export default {
    name:'App',
    props:['profileData'],
    components: {Wave},
    data() {
        return {
            userName:"",
            volumeInputText:0,
            totalVolume: 0,
            volumeObjective:0,
            lastDate:new Date()
        }
    },
    computed:{
        volumeInputNumber(){
            if(this.volumeInputText[0]==='.'){
                this.volumeInputText = '0' + this.volumeInputText
            }
            return parseFloat(this.volumeInputText)
        },
        volumePercentage(){
            return this.totalVolume/this.volumeObjective
        },
        waterHeight(){
            return this.volumePercentage * 850
        },
        wavePosition(){
            return this.waterHeight-235
        },
        bottomColor(){
            const redInitial = 204
            const redFinal = 79

            const greenInitial = 150
            const greenFinal = 195

            const blueInitial = 0
            const blueFinal = 247

            return [
                redInitial - (redInitial - redFinal) * this.volumePercentage,
                greenInitial - (greenInitial - greenFinal) * this.volumePercentage,
                blueInitial - (blueInitial - blueFinal) * this.volumePercentage
            ].join(',')
        }
    },
    methods:{
        inputNumberAbs() {
            const input = document.getElementsByTagName("input")[0];
            this.volumeInputText = this.volumeInputText.replace(/^0+|[^\d.]/g, '');
            input.value = this.volumeInputText
        },
        addDrink(){
            if(this.volumeObjective < this.totalVolume + this.volumeInputNumber / 1000){
                this.totalVolume = this.volumeObjective
                localStorage.setItem('totalVolume', this.volumeObjective)
                return;
            }

            this.totalVolume = Math.round((this.totalVolume + (this.volumeInputNumber / 1000))*100)/100

            localStorage.setItem('totalVolume', this.totalVolume)
            this.lastDate = new Date().toISOString()
            localStorage.setItem('lastDate',this.lastDate.toString())
        }
    },
    beforeMount() {
        const genderDict = {
            'Male' : function(weight, height, physicalActivity, age){
                return 1.8 + physicalActivity/7*0.1 + 50/age * 0.05 + 70/weight + 175/height
            },
            'Female' : function(weight, height, physicalActivity, age){
                return 1.5 + physicalActivity/7*0.1 + 50/age * 0.05 + 70/weight + 175/height
            }
        }
        this.userName = this.profileData.userName;
        const estimateObjective = genderDict[this.profileData.userGender]
        const {userWeight, userHeight, userPhysicalActivity, userAge} = this.profileData
        this.volumeObjective = Math.round(estimateObjective(userWeight,userHeight,userPhysicalActivity, userAge)*100)/100

        const date = new Date(localStorage.getItem('lastDate'))
        const today = new Date()
        if(today.getDay() !== date.getDay() || date.getMonth() !== today.getMonth()){
            localStorage.setItem('totalVolume','0')
        }
    }
}

</script>

<style>
.main-container{
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 50px;
    flex-direction: column;
    z-index: 1;
}
.liters-input{
    text-align: center;
    padding:10px;
    border:none;
    border-radius: 15px;
    font-size:25px;
    color:white;
    background: dodgerblue;
    height:70px;
    width:200px;
}

.control{
    color:white;
    text-align: center;
    padding:20px;
    border: 2px solid rgba(255, 255, 255, 0.5);
    border-radius: 25px;
    margin: 10px;
}

.add-water-button {
    height: 100px;
    width: 100px;
    border-radius: 999999px;
    border: none;
    font-size: 100px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    background: rgb(14, 180, 255);
    cursor: pointer;
}

.add-water-button:hover {
    height: 100px;
    width: 100px;
    border-radius: 999999px;
    border: none;
    font-size: 100px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    background: rgb(11, 135, 191);
    cursor: pointer;
}

.add-water-button:active {
    height: 100px;
    width: 100px;
    border-radius: 999999px;
    border: none;
    font-size: 100px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    background: rgb(7, 90, 128);
    cursor: pointer;
}

</style>
