<template>
  <component :is=this.view />
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
            view: 'FirstTime'
        }
    },
    beforeMount() {
        const profileData = localStorage.getItem('profile')
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