<template>
    <div v-if="isShow" class="ap-ts-wrapper js-accordion">
        <h2
            class="ap-ts-chapter js-accordion--trigger"
            @click="isOpened = !isOpened"
        >
            チャプター
            <span class="ap-ts-hid-title" v-if="!isOpened">{{ nowTitle }}</span>
        </h2>
        <div
            class="js-accordion--target"
            :class="{ '_state-open': isOpened }"
            v-if="isOpened"
        >
            <ul class="js-accordion--body ap-ts-list">
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
            isOpened: false,
            nowTitle: "",
            nowIndex: NaN,
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
            second = Math.floor(second);
            for (let i = 0; i < this.list.length; i++) {
                let s = true;
                if (this.list[i + 1]) {
                    s = second < this.list[i + 1][0];
                }
                if (second >= this.list[i][0] && s) {
                    this.nowTitle = this.list[i][1];
                    this.nowIndex = i;
                    break;
                }
            }
        },
        isNow(i) {
            return {
                "ap-ts-now": i === this.nowIndex,
            };
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
    .ap-ts-hid-title {
        color: #8f8f8f;
        font-size: 0.7em;
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
.js-accordion {
    &--trigger {
        position: relative;
        transition: all 0.2s ease-in;
        &:after {
            display: inline-block;
            width: 0;
            height: 0;
            border: solid transparent;
            content: "";
            border-top-color: #5f6569;
            border-width: 7px;
            position: absolute;
            top: 50%;
            right: 1em;
            margin-top: -5px;
            transition: all 0.2s ease-in;
        }
        &._state-open {
            background-color: #f1f1f1;
            text-decoration: none;
            &:after {
                transform: rotateX(180deg);
                margin-top: -10px;
            }
        }
        &:hover {
            background-color: #f1f1f1;
            text-decoration: none;
        }
    }
    &--target {
        overflow: hidden;
        transition: 0.4s ease-in-out;
    }
    &-enter-active {
        animation-duration: 1s;
        animation-fill-mode: both;
        animation-name: js-accordion--anime__opend;
    }
    &-leave-active {
        animation-duration: 1s;
        animation-fill-mode: both;
        animation-name: js-accordion--anime__closed;
    }
}

@keyframes js-accordion--anime__opend {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}
@keyframes js-accordion--anime__closed {
    0% {
        opacity: 1;
    }

    100% {
        opacity: 0;
    }
}
</style>
