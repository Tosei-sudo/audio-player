<template>
    <div id="controller_button" class="ap-ctrl-btns">
        <span @click="back"><i class="fas fa-step-backward fa-3x"></i></span>
        <span v-show="!playing" @click="play"
            ><i class="far fa-play-circle fa-3x"></i
        ></span>
        <span v-show="playing" @click="pause"
            ><i class="far fa-pause-circle fa-3x"></i
        ></span>
        <span @click="next"><i class="fas fa-step-forward fa-3x"></i></span>
    </div>
</template>

<script>
import { config, dom, library } from "@fortawesome/fontawesome-svg-core";
import {
    faStepBackward,
    faStepForward,
} from "@fortawesome/free-solid-svg-icons";
import {
    faPlayCircle,
    faPauseCircle,
} from "@fortawesome/free-regular-svg-icons";
library.add(faStepBackward, faStepForward, faPlayCircle, faPauseCircle);
dom.i2svg({ node: document.getElementById("controller_button") });

export default {
    data() {
        return {
            playing: false,
        };
    },
    mounted() {
        document.addEventListener("keydown", this.onKeyDown);
    },
    beforeDestroy() {
        document.removeEventListener("keydown", this.onKeyDown);
    },
    methods: {
        onKeyDown(e) {
            if (e.keyCode == 32) {
                if (this.playing) {
                    this.$emit("pause");
                } else {
                    this.$emit("play");
                }
            }
        },
        play() {
            this.$emit("play");
        },
        pause() {
            this.$emit("pause");
        },
        next() {
            this.$emit("next");
        },
        back() {
            this.$emit("back");
        },
        start() {
            this.playing = true;
        },
        stop() {
            this.playing = false;
        },
    },
    computed: {},
};
</script>

<style lang="scss">
.ap-ctrl-btns {
    margin-top: 10px;
    display: flex;
    justify-content: space-around;
    span {
        cursor: pointer;
    }
}
</style>
