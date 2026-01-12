<template>
    <vue-date-picker v-bind="$props" :locale="$props.locale ?? defaultLocale" class="ff-date-picker" />
</template>

<script>

import VueDatePicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'

// documentation https://vue3datepicker.com/props/modes/
export default {
    name: 'ff-date-time-picker',
    components: { VueDatePicker },
    computed: {
        defaultLocale () {
            return window.navigator.language
        }
    }
}
</script>

<style>
.ff-date-picker.disabled input {
    cursor: not-allowed !important;
}

.ff-date-picker .dp__action_button.dp__action_select {
    background: var(--color-indigo-700);
}

.ff-date-picker .dp__action_button.dp__action_select[disabled] {
    background: var(--color-gray-300);
}
</style>
