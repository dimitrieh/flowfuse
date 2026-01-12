<template>
    <div
        class="ff-counter rounded flex-1 p-3 cursor-pointer"
        :class="[backgroundColor, `text-${accent}-500`, accent, emptyCounter]"
        :data-state="state"
        @click="clicked()"
    >
        <label class="block">{{ title }}</label>
        <span class="counter font-bold text-4xl">{{ counter }}</span>
    </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
    name: 'InstanceCounter',
    props: {
        type: {
            required: true,
            type: String
        },
        state: {
            required: true,
            type: String
        },
        counter: {
            required: true,
            type: Number
        },
        darkerGray: {
            type: Boolean,
            required: false,
            default: false
        }
    },
    emits: ['clicked'],
    computed: {
        ...mapGetters('account', ['team']),
        accent () {
            switch (this.state) {
            case 'running':
                return 'green'
            case 'error':
                return 'red'
            case 'stopped':
            default:
                return 'gray'
            }
        },
        title () {
            switch (this.state) {
            case 'running':
                return 'Running'
            case 'error':
                return 'Error'
            case 'stopped':
            default:
                return 'Not Running'
            }
        },
        backgroundColor () {
            const opacity = (this.accent === 'gray' && this.darkerGray) ? 100 : 50
            return `bg-${this.accent}-${opacity}`
        },
        emptyCounter () {
            return this.counter === 0 ? 'empty' : ''
        }
    },
    methods: {
        clicked () {
            this.$emit('clicked', { type: this.type, state: this.state })
        }
    }
}
</script>

<style scoped>
.ff-counter {
    border: 1px solid transparent;
    transition: ease-in-out .15s;
    will-change: border-color;
}
.ff-counter.empty {
    opacity: .3;
}
.ff-counter:hover {
    opacity: 1;
}
.ff-counter:hover.green {
    border-color: var(--color-green-500);
}
.ff-counter:hover.red {
    border-color: var(--color-red-50)0;
}
.ff-counter:hover.gray {
    border-color: var(--color-gray-50)0;
}
</style>
