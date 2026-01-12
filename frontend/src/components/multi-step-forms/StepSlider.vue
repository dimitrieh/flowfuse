<template>
    <div class="step-slider">
        <div class="wrapper">
            <ul class="progress" :class="{'multi-step': entries.length > 1, 'single-step': entries.length === 1}">
                <li
                    v-for="(entry, $key) in entries"
                    :key="$key"
                    class="st"
                    data-el="slider-step"
                    :class="{completed: $key <= currentEntry, disabled: entry.disabled}"
                >
                    <span />
                </li>
            </ul>
            <ul class="steps" :class="{'justify-center': entries.length === 1, 'justify-between': entries.length > 1}">
                <li
                    v-for="(entry, $key) in entries"
                    :key="$key"
                    class="step cursor-pointer"
                    data-el="slider-title"
                    :class="{active: $key === currentEntry, completed: $key <= currentEntry, disabled: entry.disabled}"
                    @click="select($key, entry.disabled)"
                >
                    <span class="label">
                        {{ entry.title }}
                    </span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'StepSlider',
    props: {
        entries: {
            type: Array,
            required: true
        },
        currentEntry: {
            type: Number,
            required: true
        },
        disableNextStep: {
            type: Boolean,
            required: false,
            default: false
        }
    },
    emits: ['step-selected'],
    methods: {
        select (key, disabled) {
            if (this.disableNextStep) {
                return
            }
            if (disabled !== true) {
                this.$emit('step-selected', key)
            }
        }
    }
}
</script>

<style scoped>
.step-slider {
    width: 100%;
}

.step-slider .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    position: relative;
    max-width: 600px;
    margin: auto;
    min-height: 50px;
}

.step-slider .wrapper .progress {
    position: absolute;
    left: 0;
    top: 15px;
    height: 4px;
    background: var(--color-gray-300);
    transform: translateY(-50%);
    z-index: 1;
    display: flex;
    justify-content: space-between;
    overflow: hidden;
}

.step-slider .wrapper .progress.multi-step {
    width: 99%;
}

.step-slider .wrapper .progress.single-step {
    width: 0;
}

.step-slider .wrapper .progress .st {
    position: relative;
}

.step-slider .wrapper .progress .st span {
    width: 1000px;
    height: 4px;
    background: var(--color-indigo-600);
    z-index: 3;
    display: block;
    right: 0;
    position: absolute;
    transform: translateX(-1000%);
    transition: transform .3s ease-in-out;
}

.step-slider .wrapper .progress .st.completed span {
    transform: translateX(0);
}

.step-slider .wrapper .steps {
    width: 100%;
    position: absolute;
    left: 0;
    top: 5px;
    display: flex;
}

.step-slider .wrapper .steps .step {
    position: relative;
    width: 20px;
    height: 20px;
    background-color: var(--color-gray-400);
    border-radius: 50%;
    z-index: 2;
    transition: ease-in-out .3s;
}

.step-slider .wrapper .steps .step.completed {
    background-color: var(--color-indigo-600);
}

.step-slider .wrapper .steps .step.active {
    transform: scale(1.1);
    background-color: var(--color-indigo-600);
}

.step-slider .wrapper .steps .step.active .label {
    color: var(--color-indigo-700);
}

.step-slider .wrapper .steps .step.disabled {
    cursor: default;
}

.step-slider .wrapper .steps .step .label {
    position: absolute;
    left: 50%;
    transform: translate(-50%, 150%);
    font-weight: bold;
    color: var(--color-gray-300);
    transition: ease-in-out .3s;
}
</style>
