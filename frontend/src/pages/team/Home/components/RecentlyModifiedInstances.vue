<template>
    <div class="recently-modified">
        <p class="text-gray-400 text-sm">Recently Modified</p>
        <ul v-if="instances.length" class="flex flex-1 flex-col gap-1">
            <li v-for="instance in instances" :key="instance.id" class="instance-wrapper flex">
                <InstanceTile :instance="instance" :minimal-view="true" @delete-instance="$emit('delete-instance', $event)" />
            </li>
            <li v-if="hasMore" class="instance-wrapper flex">
                <team-link :to="{name: 'Instances'}" class="instance-tile has-more hover:text-indigo-700" data-el="has-more">
                    <span>{{ instancesLeft }} More</span>
                    <span>
                        <ChevronRightIcon class="ff-icon ff-icon-sm" />
                    </span>
                </team-link>
            </li>
        </ul>
        <div v-else class="no-instances flex flex-col flex-1 justify-center text-gray-500 italic">
            <p class="text-center self-center">
                It's looking a little empty.
                <team-link :to="{name: 'CreateInstance'}" class="text-indigo-500">Create a Hosted Instance</team-link>
                to get started.
            </p>
        </div>
    </div>
</template>

<script>
import { ChevronRightIcon } from '@heroicons/vue/outline'
import { mapGetters } from 'vuex'

import teamAPI from '../../../../api/team.js'
import TeamLink from '../../../../components/router-links/TeamLink.vue'
import InstanceTile from '../../Applications/components/compact/InstanceTile.vue'

export default {
    name: 'RecentlyModifiedInstances',
    components: { TeamLink, InstanceTile, ChevronRightIcon },
    props: {
        totalInstances: {
            type: Number,
            required: true
        }
    },
    emits: ['delete-instance'],
    data () {
        return {
            hasMore: false,
            instances: []
        }
    },
    computed: {
        ...mapGetters('account', ['team']),
        instancesLeft () {
            return this.totalInstances - this.instances.length
        }
    },
    watch: {
        totalInstances () {
            // if the no. of total instances changed, it must mean one was deleted, so we need to refresh our list
            this.getInstances()
        }
    },
    mounted () {
        this.getInstances()
            .catch(e => e)
    },
    methods: {
        getInstances () {
            let limit

            if (this.totalInstances <= 4) {
                limit = 3
                this.hasMore = this.totalInstances === 4
            } else {
                limit = 3
                this.hasMore = true
            }
            return teamAPI.getInstances(this.team.id, {
                limit,
                includeMeta: true,
                orderByMostRecentFlows: true
            })
                .then(res => {
                    this.instances = res.projects
                })
        }
    }
}
</script>

<style>
.recently-modified {
    display: flex;
    flex: 1;
    flex-direction: column;
}

.recently-modified > p {
    border-bottom: 1px solid var(--color-gray-100);
    margin-bottom: 10px;
    line-height: 2rem;
}

.recently-modified .instance-wrapper {
    height: fit-content;
}

.recently-modified .instance-wrapper .instance-tile {
    border: 1px solid var(--color-gray-100);
    padding: 2px 10px;
    border-radius: 5px;
    display: flex;
    width: 100%;
    height: fit-content;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
    will-change: border-color;
    transition: ease-in-out .3s;
}

.recently-modified .instance-wrapper .instance-tile.has-more {
    padding: 10px;
}

.recently-modified .instance-wrapper .instance-tile:hover {
    border-color: var(--color-gray-200);
}

.recently-modified .instance-wrapper .instance-tile .details {
    flex: 1;
}

.recently-modified .instance-wrapper .instance-tile .details .detail-wrapper.detail {
    font-size: var(--font-size-sm);
    color: var(--color-gray-400);
}

.recently-modified .instance-wrapper .instance-tile .details .detail-wrapper .name:hover,
.recently-modified .instance-wrapper .instance-tile .details .detail-wrapper .editor-link:not(.inactive):hover {
    color: var(--color-indigo-700);
}

.recently-modified .instance-wrapper .instance-tile .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.recently-modified .no-instances {
    min-height: 130px;
}
</style>
