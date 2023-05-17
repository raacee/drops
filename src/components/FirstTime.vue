<template>
    <div class="fullscreen">
        <div style="display:flex; justify-content: center; flex-direction: column; align-items: center; min-width: 25%">
            <div style="margin-bottom:10px;">
                <h2>Welcome to drops 💧 ! </h2>
                <div style="display: flex; justify-content: center">
                    <span> Please enter your information : </span>
                </div>
            </div>
            <div style="display: flex; justify-content: center; flex-direction: column; margin:20px;">
                <div class="input-section">
                    <p style="margin:0;">Gender :</p>
                    <div style="display: flex; justify-content: center; align-items: center">
                        <div>
                            <input type="radio" id="one" value="Male" v-model="this.profileData.userGender" style=""/>
                            <label for="one" style="color:white;">Male</label>
                        </div>
                        <div>
                            <input type="radio" id="two" value="Female" v-model="this.profileData.userGender" />
                            <label for="two" style="color:white;">Female</label>
                        </div>
                    </div>
                </div>
                <div class="input-section">
                    <span>Name :</span>
                    <input type="text" v-model="profileData.userName" class="text-input" style="width:45%">
                </div>
                <div class="input-section">
                    <span>Age :</span>
                    <input type="text" v-model="profileData.userAge" @input="inputNumberAbsAge" class="text-input">
                </div>
                <div class="input-section">
                    <span>Weight (in kg) :</span>
                    <input type="text" v-model="profileData.userWeight" @input="inputNumberAbsWeight" class="text-input">
                </div>
                <div class="input-section">
                    <span>Height (in cm) :</span>
                    <input type="text" max="200" min="100" v-model="profileData.userHeight" @input="inputNumberAbsHeight" class="text-input">
                </div>
                <div class="input-section">
                    <p>Physical activity (hours of physical activity per week) </p>
                    <input type="range" max="35" min="0" step="1" v-model="this.profileData.userPhysicalActivity" id="slider">
                    <p style="margin-left:10px;">
                        {{this.profileData.userPhysicalActivity}} hours</p>
                </div>
            </div>
            <div>
                <button @click.prevent=emitCreateAccount>
                    Create profile
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "FirstTime",
    emits:['createAccount'],
    data(){
        return{
            profileData : {
                userGender:"",
                userName: "",
                userWeight: 70,
                userHeight: 100,
                userPhysicalActivity : 0,
                userAge:0
            }
        }
    },
    methods:{
        inputNumberAbsWeight(event) {
            const input = event.target
            this.profileData.userWeight = this.profileData.userWeight.replace(/^0+|[^\d.]/g, '');
            input.value = this.profileData.userWeight
        },
        inputNumberAbsHeight(event) {
            const input = event.target
            this.profileData.userHeight = this.profileData.userHeight.replace(/^0+|[^\d.]/g, '');
            input.value = this.profileData.userWeight
        },
        inputNumberAbsAge(event){
            const input = event.target
            this.profileData.userAge = this.profileData.userAge.replace(/^0+|[^\d.]/g, '');
            input.value = this.profileData.userAge
        },
        emitCreateAccount(){
            console.log(this.profileData)
            if(!this.profileData.userGender || this.profileData.userGender === ""){
                window.alert("Please choose a gender")
                return;
            }
            if(!this.profileData.userAge || this.profileData.userAge === 0){
                window.alert("Please enter your age")
                return;
            }
            if(!this.profileData.userName || this.profileData.userName === ""){
                window.alert("Please enter your name")
                return;
            }
            this.profileData.userHeight = parseInt(this.profileData.userHeight)
            this.profileData.userWeight = parseInt(this.profileData.userWeight)
            this.profileData.userPhysicalActivity = parseInt(this.profileData.userPhysicalActivity)
            this.profileData.userAge = parseInt(this.profileData.userAge)
            this.$emit('createAccount', this.profileData)
        }
    }
}
</script>

<style scoped>
button{
    border-radius: 30px;
    border:none;
    padding:20px;
    background: deepskyblue;
    color:white;
    cursor: pointer;
}

button:hover{
    border-radius: 30px;
    border:none;
    padding:20px;
    background: dodgerblue;
    color:white;
    cursor: pointer;
}

button:active{
    border-radius: 30px;
    border:none;
    padding:20px;
    background: royalblue;
    color:white;
    cursor: pointer;
}

.text-input{
    text-indent: 15px;
    direction: ltr;
    color:white;
    font-size: 20px;
    width: 25%;
    padding-top:5px;
    padding-bottom: 5px;
    border:none;
    border-radius: 30px;
    background: dodgerblue;
}

#slider{
    width: 100%;
    margin:5px;
}

.input-section{
    margin-bottom: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.fullscreen{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    overflow:auto;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}
span, p, h2{
    color:white;
}
</style>