<template>
    <div class="ff-pipeline" :data-pipeline="slugify(pipeline.name)">
        <router-link
            :to="{name: 'ApplicationPipelines', params: {id: pipeline.application.id}}"
            class="ff-pipeline-header flex gap-5 self-end items-center truncate"
        >
            <div class="flex flex-col gap-0.5">
                <span class="name">
                    {{ pipeline.name }}
                </span>
                <span class="text-xs text-gray-500">{{ pipeline.application.name }}</span>
            </div>
            <span class="to">
                <ChevronRightIcon class="ff-icon " />
            </span>
        </router-link>
        <div class="ff-pipeline-content">
            <ul v-if=" pipeline.stages.length > 0" class="ff-pipeline-stages-list">
                <li v-for="stage in pipeline.stages" :key="stage.id">
                    <TeamPipelineStage :stage="stage" :application="pipeline.application" />
                    <ChevronRightIcon class="ff-icon" />
                </li>
            </ul>
            <p v-else class="ff-empty-stages-message">No stages in sight just yet!</p>
        </div>
    </div>
</template>

<script>
import { ChevronRightIcon } from '@heroicons/vue/outline'

import { slugify } from '../../../../composables/String.js'

import TeamPipelineStage from './TeamPipelineStage.vue'
export default {
    name: 'TeamPipeline',
    components: {
        TeamPipelineStage,
        ChevronRightIcon
    },
    props: {
        pipeline: {
            required: true,
            type: Object
        }
    },
    methods: {
        slugify
    }
}
</script>

<style scoped>
.ff-pipeline {
    border: 1px solid var(--color-gray-300);
    border-radius: 5px;
    overflow: hidden;
}

.ff-pipeline > .ff-pipeline-header {
    background: var(--color-white);
    padding: 15px;
    border-bottom: 1px solid var(--color-gray-300);
    transition: ease-in-out .3s;
}

.ff-pipeline > .ff-pipeline-header:hover {
    color: var(--color-white);
    background: var(--color-indigo-700);
}

.ff-pipeline > .ff-pipeline-header:hover .ff-pipeline-application-name {
    transition: ease-in-out .3s;
    color: var(--color-gray-400);
}

.ff-pipeline > .ff-pipeline-header:has(.ff-pipeline-application-name:hover) {
    color: var(--color-gray-50)0;
}

.ff-pipeline > .ff-pipeline-header:has(.ff-pipeline-application-name:hover) .ff-pipeline-application-name:hover {
    color: var(--color-white);
}

.ff-pipeline > .ff-pipeline-header .ff-application-name {
    transition: ease-in-out .3s;
    color: var(--color-gray-400);
}

.ff-pipeline > .ff-pipeline-header .ff-application-name:hover {
    color: var(--color-indigo-700);
}

.ff-pipeline > .ff-pipeline-header .to {
    display: flex;
    flex: 1;
    justify-content: end;
}

.ff-pipeline > .ff-pipeline-content {
    padding: 15px;
    overflow: auto;
}

.ff-pipeline > .ff-pipeline-content .ff-pipeline-stages-list {
    display: flex;
    flex-direction: row;
    gap: 15px;
}

.ff-pipeline > .ff-pipeline-content .ff-pipeline-stages-list li {
    display: flex;
    gap: 15px;
    align-items: center;
}

.ff-pipeline > .ff-pipeline-content .ff-pipeline-stages-list li:last-child > .ff-icon {
    display: none;
}

.ff-pipeline > .ff-pipeline-content .ff-empty-stages-message {
    text-align: center;
    color: var(--color-gray-50)0;
}
</style>
