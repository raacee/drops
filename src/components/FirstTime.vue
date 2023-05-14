<template>
    <div class="fullscreen">
        <div style="margin-bottom:50px;">
            <h2>Welcome to drops 💧 ! </h2>
            <div style="display: flex; justify-content: center">
                <span> Please enter your information : </span>
            </div>
        </div>
        <div style="display: flex; justify-content: center; flex-direction: column; margin:20px;">
            <div>
                <p>Name :</p>
                <input type="text" v-model="userName">
            </div>
            <div>
                <p>Weight :</p>
                <input type="text" v-model="userWeight">
            </div>
            <div>
                <p>Height:</p>
                <input type="text" v-model="userHeight">
            </div>
            <div>
                <p>Physical activity (hours of physical activity per week) </p>
                <input type="range" max="35" min="0" step="1"  v-model="activitySliderValue" id="slider">
                <p :style="'transform:translateX('+(this.activitySliderValue / 35 * 100 )+'%);'">{{ activitySliderValue }} hours</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "FirstTime",
    data(){
        return{
            userName:"",
            userWeight:"",
            userHeight:"",
            activitySliderValue:0
        }
    },
}
</script>

<style scoped>


input{
    width: 100%;
    padding:0 0 30px;
    border:none;
    border-radius: 30px;
    background: white;
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