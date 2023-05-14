<template>
    <div style="display: flex; align-items: center; justify-content: center;">
        <div class="ocean">

            <svg class="wave" viewBox="0 0 12960 1120" :style="'fill:rgb('+this.color+')'">

                <path
                        d="M9720,320C8100,320,8100,0,6480,0S4860,320,3240,320,1620,0,0,0V1120H12960V0C11340,0,11340,320,9720,320Z">

                    <animate
                            dur="5s"
                            repeatCount="indefinite"
                            attributeName="d"
                            values="
                        M9720,320C8100,320,8100,0,6480,0S4860,320,3240,320,1620,0,0,0V1120H12960V0C11340,0,11340,320,9720,320Z;
                        M9720,0C8100,0,8100,319,6480,319S4860,0,3240,0,1620,320,0,320v800H12960V320C11340,320,11340,0,9720,0Z;
                        M9720,320C8100,320,8100,0,6480,0S4860,320,3240,320,1620,0,0,0V1120H12960V0C11340,0,11340,320,9720,320Z"
                    />

                </path>

            </svg>

        </div>
    </div>
</template>

<script>

export default {
    name: "Wave",
    props: ['volumePercentage'],
    computed: {
        color() {
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
    }

}
</script>

<style scoped>

.ocean {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    width: 1920px;
    height: 400px;
    overflow: hidden;
}

.wave {
    width: 3840px;
    animation-name: swell;
    animation-duration: 2s;
    animation-fill-mode: forwards;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
}

@keyframes swell {
    0% {
        transform: translateX(-50%);
    }
    100% {
        transform: translateX(0%);
    }
}

</style>