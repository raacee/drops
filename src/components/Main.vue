<template>
  <FirstTime v-if="this.view === 'FirstTime'" @createAccount="accountCreated" />
  <App v-else :profileData="this.profileData"/>
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
            this.profileData = profileData
            localStorage.setItem('profile_data',JSON.stringify(profileData))
            this.view = 'App'
        }
    },
    beforeMount() {
        const profileData = localStorage.getItem('profile_data')
        if(profileData){
            this.profileData = JSON.parse(profileData)
        }
        else{
            this.view = 'FirstTime'
        }
    }
}
</script>

<style scoped>

</style>