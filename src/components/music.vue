<template>
    <li @click="play" v-bind:class="classval">
        <span class="ap-ml-title">{{ Title }}</span>
        <span class="ap-ml-day">{{ day }}</span>
    </li>
</template>

<script>
export default {
    props: {
        Title: {
            type: String,
            required: true,
        },
        path: {
            type: String,
            required: true,
        },
        day: {
            type: String,
            required: true,
        },
    },
    data() {
        return {
            playing: false,
        };
    },
    methods: {
        play() {
            this.$emit("play", this.path, 0, this.Title);
            this.playing = true;
        },
        stop() {
            this.playing = false;
        },
        start() {
            this.playing = true;
        },
    },
    computed: {
        classval() {
            return {
                "ap-ml-music": true,
                "ap-ml-playing": this.playing,
            };
        },
    },
};
</script>

<style lang="scss">
.ap-ml-music {
    border: 1.5px solid #aaaaaa;
    padding: 3px;
    display: flex;
    cursor: pointer;
    justify-content: space-between;
    background-color: #c0c0c0;

    &:hover {
        background-color: #ddd;
    }

    .ap-ml-title {
        font-size: 1.3em;
        display: inline-block;
    }
    .ap-ml-day {
        font-size: 0.8em;
        padding-top: 5px;
        color: #fff;
        display: inline-block;
    }
    &.ap-ml-playing {
        border-left: blue solid 3px;
        background-color: #bad3ff;
    }
}
</style>
