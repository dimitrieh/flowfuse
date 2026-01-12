<template>
    <div class="segment-wrapper" :class="{open: isSegmentOpen, empty: isEmpty, selected: isSegmentSelected}" data-el="segment-wrapper" :data-value="segment.name">
        <div class="segment flex" @click="toggleChildren();rowClick(segment)">
            <div class="diagram">
                <span v-if="!isRoot" class="connector-elbow" />
                <span v-if="shouldShowTrunk" class="connector-trunk" />
            </div>
            <div class="content flex gap-1.5 items-center font-bold cursor-pointer" :class="{'pl-10': !isRoot}">
                <ChevronRightIcon v-if="hasChildren" class="chevron ff-icon-sm" />
                <p class="flex gap-2.5 items-end" :class="{'ml-2': !hasChildren}">
                    <span class="title">
                        <span class="highlighted-text-parts flex">
                            <span
                                v-for="(part, $key) in highlightedText" :key="$key"
                                :class="{ highlight: part.highlight }"
                            >
                                {{ part.text }}
                            </span>
                        </span>
                        <span
                            v-if="segment.isEndOfTopic && segment.childrenCount"
                            class="separator cursor-help"
                            title="This topic is also able to receive events"
                        >
                            <ArchiveIcon class="ff-icon-sm" />
                        </span>
                    </span>
                    <span v-if="hasChildren" class="font-normal opacity-50 text-xs">{{ topicsCounterLabel }}</span>
                    <text-copier :text="segment.topic" :show-text="false" class="ff-text-copier" />
                </p>
            </div>
        </div>
        <div v-if="hasChildren && isSegmentOpen" class="children" data-el="segment-children" :class="{ 'pl-10': isRoot}">
            <topic-segment
                v-for="(child, key) in childrenSegments"
                :key="'-'+child.path"
                :segment="children[child]"
                :children="children[child].children"
                :has-siblings="Object.keys(children).length > 1"
                :is-last-sibling="key === Object.keys(children).length - 1"
                :class="{'pl-10': !isRoot}"
                :selected-segment="selectedSegment"
                :filter-term="filterTerm"
                @segment-selected="rowClick"
                @segment-state-changed="$emit('segment-state-changed', $event)"
            />
        </div>
    </div>
</template>

<script>
import { ArchiveIcon } from '@heroicons/vue/outline'
import { ChevronRightIcon } from '@heroicons/vue/solid'
import { ref } from 'vue'

import TextCopier from '../../../../../components/TextCopier.vue'
export default {
    name: 'TopicSegment',
    components: { TextCopier, ChevronRightIcon, ArchiveIcon },
    props: {
        segment: {
            required: true,
            type: Object
        },
        children: {
            required: true,
            type: Object
        },
        isRoot: {
            required: false,
            type: Boolean,
            default: false
        },
        hasSiblings: {
            required: true,
            type: Boolean
        },
        isLastSibling: {
            required: true,
            type: Boolean
        },
        selectedSegment: {
            required: false,
            type: Object,
            default: null
        },
        filterTerm: {
            required: true,
            type: String
        }
    },
    emits: ['segment-selected', 'segment-state-changed'],
    setup (props) {
        const isSegmentOpen = ref(props.segment.open)

        return { isSegmentOpen }
    },
    computed: {
        childrenCount () {
            return Object.keys(this.children).length
        },
        hasChildren () {
            return this.childrenCount > 0
        },
        childrenSegments () {
            return Object.keys(this.children).sort()
        },
        topicsCounterLabel () {
            const label = 'topic' + (this.segment.childrenCount <= 1 ? '' : 's')
            return `(${this.segment.childrenCount} ${label})`
        },
        isEmpty () {
            return this.segment.name.length === 0
        },
        segmentText () {
            return !this.isEmpty ? this.segment.name : '(empty)'
        },
        shouldShowTrunk () {
            return !this.isRoot && this.hasSiblings && this.isLastSibling
        },
        isSegmentSelected () {
            return this.segment?.topic === this.selectedSegment?.topic
        },
        highlightedText () {
            if (!this.filterTerm) {
                return [{ text: this.segmentText, highlight: false }]
            }

            const escapedQuery = this.filterTerm.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')
            const regex = new RegExp(escapedQuery, 'gi')

            const parts = []
            let lastIndex = 0
            let match

            while ((match = regex.exec(this.segmentText)) !== null) {
                if (match.index > lastIndex) {
                    parts.push({
                        text: this.segmentText.slice(lastIndex, match.index),
                        highlight: false
                    })
                }

                parts.push({
                    text: match[0],
                    highlight: true
                })

                lastIndex = regex.lastIndex
            }

            if (lastIndex < this.segmentText.length) {
                parts.push({
                    text: this.segmentText.slice(lastIndex),
                    highlight: false
                })
            }

            return parts
        }
    },
    watch: {
        isSegmentOpen: {
            handler () {
                this.$emit('segment-state-changed', {
                    state: this.isSegmentOpen,
                    path: this.segment.path,
                    topic: this.segment.topic
                })
            },
            immediate: false
        }
    },
    methods: {
        rowClick (segment) {
            this.$emit('segment-selected', segment)
        },
        toggleChildren () {
            if (this.hasChildren) {
                this.isSegmentOpen = !this.isSegmentOpen
            }
        }
    }
}
</script>

<style scoped>
.segment-wrapper .segment {
    position: relative;
    margin: 5px 0 0;
    transition: ease .15s;
}

.segment-wrapper .segment:hover {
    color: var(--color-indigo-700);
    cursor: pointer;
}

.segment-wrapper .segment .diagram .connector-elbow {
    border-left: 2px solid  var(--color-indigo-300);
    border-bottom: 2px solid  var(--color-indigo-300);
    border-bottom-left-radius: 7px;
    display: inline-block;
    position: absolute;
    height: 50px;
    width: 25px;
    left: -23px;
    top: -35px;
}

.segment-wrapper .segment .diagram .connector-trunk {
    width: 1px;
    border-left: 2px solid var(--color-indigo-300);
    display: inline-block;
    position: absolute;
    height: 5000px;
    left: -23px;
    top: -5000px;
}

.segment-wrapper .segment .content {
    padding: 5px;
    position: relative;
}

.segment-wrapper .segment .content .chevron {
    transition: ease .15s;
}

.segment-wrapper .segment .content .title {
    align-items: center;
    display: flex;
    gap: 3px;
}

.segment-wrapper .segment .content .title .highlight {
    background-color: var(--color-indigo-100);
}

.segment-wrapper .segment .content .ff-text-copier {
    display: none;
    height: 17px;
}

.segment-wrapper .segment .content:hover .ff-text-copier {
    display: inline-block;
    color: var(--color-gray-400);
}

.segment-wrapper .children {
    overflow: hidden;
}

.segment-wrapper.selected > .segment {
    background: var(--color-indigo-50);
}

.segment-wrapper.open > .segment .content .title {
    color: var(--color-indigo-700);
}

.segment-wrapper.open > .segment .content .chevron {
    transform: rotate(90deg);
}

.segment-wrapper.empty > .segment .content .title {
    color: var(--color-gray-600);
    font-size: 90%;
    font-weight: 300;
}

.segment-wrapper.empty > .segment .content .title .separator {
    color: var(--color-black);
    font-weight: bold;
}
</style>
