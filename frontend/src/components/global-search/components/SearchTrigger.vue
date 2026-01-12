<template>
    <input
        type="text"
        placeholder="Search through your team (CTRL + K)"
        class="input-trigger"
        @focusin="interacted"
        @click="interacted"
    >
    <span class="icon-trigger" @click="interacted">
        <SearchIcon class="ff-icon ff-icon-md" />
    </span>
</template>

<script>
import { SearchIcon } from '@heroicons/vue/outline'

export default {
    name: 'SearchTrigger',
    components: { SearchIcon },
    emits: ['interacted'],
    methods: {
        interacted () {
            this.$emit('interacted')
        }
    }
}
</script>

<style>
#global-search .input-trigger {
    display: block;
    height: 34px;
}

#global-search .icon-trigger {
    display: none;
    cursor: pointer;
    padding: 5px;
    transition: ease-in-out .1s;
    object-fit: contain;
}

#global-search .ff-icon-sm.search {
    display: none;
}

#global-search.focused .input-trigger,
#global-search.focused .icon-trigger {
    display: none;
}

#global-search.focused .ff-icon-sm.search {
    display: inline;
}

@media screen and (max-width: 1023px) {
    #global-search .input-trigger {
        display: none;
    }

    #global-search .icon-trigger {
        display: inline-block;
        align-self: flex-end;
    }

    #global-search .icon-trigger:hover {
        will-change: transform ;
        transform: scale(1.25) translateZ(0); /* Using slight adjustments to whole values */
        backface-visibility: hidden;
        perspective: 1000px;
        stroke-width: 1.5px;
        shape-rendering: geometricPrecision;
        text-rendering: geometricPrecision;
        color: var(--color-indigo-500);
    }
}

/*
@media screen and (max-width: 639px) {
    #global-search .content-wrapper .search-wrapper .input-wrapper {
        flex-direction: row
    }
}
*/
</style>
