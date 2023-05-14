<template>
  <component :is=this.view @createAccount="accountCreated"/>
</template>


<script>
import FirstTime from "./FirstTime.vue";
import App from "./App.vue"

export default {
    name: "Main",
    components:{App,FirstTime},
    data(){
        return {
            profileData:{},
            view: 'App'
        }
    },
    methods:{
        accountCreated(profileData){
            profileData.userHeight = parseInt(profileData.userHeight)
            profileData.userWeight = parseInt(profileData.userWeight)
            profileData.physicalActivity = parseInt(profileData.physicalActivity)
            localStorage.setItem('profile_data',JSON.stringify(profileData))
            this.view = 'App'
        }
    },
    beforeMount() {
        const profileData = localStorage.getItem('profile_data')
        if(profileData){
            this.profileData = profileData
        }
        else{
            this.view = 'FirstTime'
        }
    }
}
</script>

<style scoped>

</style>