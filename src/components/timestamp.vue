<template>
    <div v-if="isShow" class="ap-ts-wrapper">
        <h2 class="ap-ts-chapter">チャプター</h2>
        <ul class="ap-ts-list">
            <li
                v-for="(link, i) in list"
                :key="link[0]"
                @click="click(link[0])"
                v-bind:class="isNow(i)"
            >
                <span class="ap-ts-title">{{ link[1] }}</span>
                <span class="ap-ts-time">{{ convTime(link[0]) }}</span>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        list: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            now: 0,
        };
    },
    methods: {
        click(second) {
            this.$emit("click", second);
        },
        convTime(second) {
            let h = Math.floor(second / 3600);
            let m = Math.floor((second % 3600) / 60);
            let s = Math.floor(second % 60);
            return (
                (h > 0 ? h + ":" : "") +
                (m < 10 ? "0" + m : m) +
                ":" +
                (s < 10 ? "0" + s : s)
            );
        },
        changeTime(second) {
            this.now = Math.floor(second);
        },
        isNow(i) {
            let s = true;
            if (this.list[i + 1]) {
                s = this.now < this.list[i + 1][0];
            }
            if (this.now >= this.list[i][0] && s) {
                return "ap-ts-now";
            }else{
                return "";
            }
        },
    },
    computed: {
        isShow() {
            return this.list.length > 0;
        },
    },
};
</script>

<style lang="scss" scoped>
.ap-ts-wrapper {
    background-color: #f0f0f0;
    margin: 5px;
    border: 1px solid #8f8f8f;

    .ap-ts-chapter {
        font-size: 1.5em;
        padding: 5px 10px;
        color: black;
    }
    .ap-ts-list {
        margin-top: 0;
        font-size: initial;
        list-style: none;
        li {
            width: 100%;
            padding: 5px;
            cursor: pointer;
            font-size: initial;
            @media screen and (min-width: 767px) {
                &:hover {
                    background-color: #bfbfbf;
                }
            }
            .ap-ts-title {
                font-size: 1.3em;
                font-weight: bold;
                color: black;
                width: 75%;
            }
            .ap-ts-time {
                font-size: 1em;
                color: #373737;
                width: 25%;
            }
            &.ap-ts-now {
                border-left: blue solid 3px;
                background-color: #bad3ff;
            }
        }
    }
}
</style>
