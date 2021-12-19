<template>
    <ul class="ap-ml-ul">
        <music
            v-for="m in musics"
            :key="m.key"
            :path="m.path"
            :Title="m.Title"
            :day="dayText(m.day)"
            @play="play"
            ref="music"
        ></music>
    </ul>
</template>

<script>
import music from "./music.vue";

export default {
    components: { music },
    props: {
        list: {
            type: Array,
            required: true,
        },
    },
    computed: {
        musics() {
            return this.list.map((item, index) => {
                return {
                    key: this.hash(item.path),
                    path: item.path,
                    Title: item.Title,
                    day: item.Detastamp,
                };
            });
        },
    },
    methods: {
        hash: function (val) {
            return val.split("").reduce(function (a, b) {
                a = (a << 5) - a + b.charCodeAt(0);
                return a & a;
            }, 0);
        },
        dayText(day) {
            var y = String(Math.floor(day / 10000) + 10000).slice(1, 5);
            var m = String(Math.floor((day % 10000) / 100) + 100).slice(1, 3);
            var d = String(Math.floor(day % 100) + 100).slice(1, 3);
            return y + "/" + m + "/" + d + "";
        },
        play(path, m = 0, Title = "") {
            this.$refs.music.map(function (item) {
                item.stop();
                if (item.path == path) {
                    item.start();
                }
            });
            this.$emit("play", path, m,Title);
        },
    },
};
</script>

<style lang="scss">
.ap-ml-ul {
    border-radius: 0;
    padding: 10px 5px;
    background-color: #999999;
}
</style>
