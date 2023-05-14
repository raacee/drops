<template>
    <div class="main-container">
        <div class="control">
            <p style="font-size:25px;">
                Welcome back {{ userName }} <br><br> Today you have drank {{ totalVolume }} liters of water
            </p>
            <p>
                Your objective is {{ volumeObjective }} liters per day
            </p>
        </div>
        <div style="color:white">
            <p style="font-size: 40px;">
                Add more water
            </p>
        </div>
        <div style="margin-bottom:30px;">
            <input class="liters-input" @input="inputNumberAbs" v-model="volumeInputText">
        </div>
        <div>
            <button class="add-water-button" style="">
                +
            </button>
        </div>
    </div>

    <div style="z-index:-9999">
        <div :style="'position:fixed; bottom:'+(this.bottomOffset-235)+'px;'">
            <Wave :volumePercentage="this.volumePercentage"/>
        </div>
        <div class="bottom-fill"
             :style="'position:fixed; bottom:0; height:'+this.bottomOffset+'px; width:100%; background:rgb('+this.bottomColor+')'">
        </div>
    </div>


</template>

<script>
import Wave from "./Wave.vue";

export default {
    name:'App',
    props:['profileData'],
    components: {Wave},
    computed:{
        volumeInputNumber(){
            return parseInt(this.volumeInputText)
        },
        volumePercentage(){
            return this.totalVolume/this.volumeObjective
        },
        bottomOffset(){
            return this.volumePercentage * 850
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
    data() {
        return {
            userName:"",
            volumeInputText:0,
            totalVolume: 0,
            volumeObjective:0,
        }
    },
    methods:{
        inputNumberAbs() {
            const input = document.getElementsByTagName("input")[0];
            this.volumeInputText = this.volumeInputText.replace(/^0+|[^\d.]/g, '');
            input.value = this.volumeInputText
        }
    },
    beforeMount() {
        const genderDict = {
            'Male' : function(weight, height, physicalActivity, age){
                return 3 + physicalActivity*0.5 + 0.25*age
            },
            'Female' : function(weight, height, physicalActivity, age){
                return 2 + physicalActivity*0.5 + 0.25*age
            }
        }
        const estimateObjective = genderDict[this.profileData.userGender]
        const {userWeight, userHeight, userPhysicalActivity, userAge} = this.profileData
        this.volumeObjective = estimateObjective(userWeight,userHeight,userPhysicalActivity, userAge)
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
    top: 100px;
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
