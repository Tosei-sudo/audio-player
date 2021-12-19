<template>
    <div class="ap-wrapper">
        <div class="ap-ml">
            <vue-loading v-if="loading" type="bars" color="navy" :size="{ width: '100px', height: '100px' }"></vue-loading>
            <audioList v-if="!loading" :list="list" @play="play" ref="audios"></audioList>
        </div>
        <div class="ap-title">{{currentTitle}}</div>
        <div class="ap-ctrl">
            <audiocontroller
                @play="c_play"
                @pause="c_pause"
                @next="next"
                @back="back"
                ref="controller"
            ></audiocontroller>
        </div>
        <div>
            <audiotimebar
                :max="max"
                :marks="marks"
                ref="audiotimebars"
                @change="change"
            ></audiotimebar>
        </div>
        <div>
            <timestamp :list="stamps" @click="markclick" ref="timestamp"></timestamp>
        </div>
    </div>
</template>

<script>
import audioList from "./components/music-list.vue";
import audiocontroller from "./components/music-controller.vue";
import audiotimebar from "./components/music-timebar.vue";
import timestamp from "./components/timestamp.vue";
import { VueLoading } from 'vue-loading-template'

export default {
    components: { audioList, audiocontroller, audiotimebar,VueLoading ,timestamp},
    props: {
        list: {
            type: Array,
            required: true,
        },
        stamps: {
            type: Array,
            required: false,
            default: [],
        },
        loading:{
            type: Boolean,
            required: false,
            default: false,
        },
    },
    data() {
        return {
            audio: null,
            currentpath: "",
            currentTitle: "Select a music...",
            max: 0,
        };
    },
    methods: {
        markclick(second) {
            if(this.audio){
            this.audio.currentTime = Math.floor(second);

            }
        },
        c_play() {
            let p = this.currentpath;
            let t = this.currentTitle;
            if (this.currentpath == "") {
                p = this.list[0].path;
                t = this.list[0].Title;
            }
            this.$refs.audios.play(p, 1,t);
        },
        c_pause() {
            this.pause();
        },
        change(v) {
            this.audio.currentTime = v;
        },
        play(path, mode = 0 , title) {
            this.$refs.controller.stop();
            this.$refs.audiotimebars.stop();
            if (this.audio) {
                this.audio.pause();
            }
            if (mode == 0 || !this.audio || this.currentpath != path) {
                this.audio = new Audio(path);
                this.audio.oncanplay = () => {
                    this.max = this.audio.duration;
                };
                this.audio.ontimeupdate = () => {
                    this.$refs.timestamp.changeTime(this.audio.currentTime);
                    this.$refs.audiotimebars.timechange(this.audio.currentTime);
                };
                this.audio.onended = () => {
                    this.next();
                };
                this.currentpath = path;
                this.currentTitle = title;
            }
            this.$refs.controller.start();
            this.audio.play();
        },
        pause() {
            this.$refs.controller.stop();
            this.audio.pause();
        },
        next() {
            let index = this.list.findIndex(
                (item) => item.path == this.currentpath
            );
            if (index == this.list.length - 1) {
                index = 0;
            } else {
                index++;
            }
            let t = this.list[index].Title;
            let p = this.list[index].path;
            this.$refs.audios.play(p, 0,t);
        },
        back() {
            let index = this.list.findIndex(
                (item) => item.path == this.currentpath
            );
            if (index == 0) {
                index = this.list.length - 1;
            } else {
                index--;
            }
            let t = this.list[index].Title;
            let p = this.list[index].path;
            this.$refs.audios.play(p, 0,t);
        },
    },
    computed: {
        marks() {
            let marks = [];
            if (this.stamps) {
                for(let time of this.stamps){
                    marks.push(time[0]);
                }
            }else{
                return null;
            }
            return marks;
        },
    },
};
</script>

<style lang="scss">
.ap-wrapper {
    border: 3px solid gray;
    background-color: #999;
    .ap-ml {
        overflow-y: auto;
    }
    @media screen and (min-width: 1024px) {
        .ap-ml {
            height: 400px;
        }
        font-size: 18px;
    }
    @media screen and (max-width: 1023px) and (min-width: 768px) {
        .ap-ml {
            height: 300px;
        }
        font-size: 14px;
    }
    @media screen and (max-width: 767px) {
        .ap-ml {
            height: 250px;
        }
        font-size: 10px;
    }
    .ap-title {
        font-size: 1.8em;
        text-align: left;
        margin-top: 10px;
        margin-left: 10px;
    }
}
</style>
