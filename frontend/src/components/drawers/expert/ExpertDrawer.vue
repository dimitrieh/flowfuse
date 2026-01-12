<template>
    <div ref="drawer" class="ff-expert-drawer" data-el="expert-drawer" tabindex="-1">
        <div class="header">
            <div class="flex items-center gap-1.5">
                <img src="/ff-minimal-red.svg" alt="FlowFuse" class="w-5 h-5 flex-shrink-0">
                <h2 class="title">Expert</h2>
            </div>
            <div class="agent-mode">
                <toggle-button-group
                    v-model="agentModeWrapper"
                    :buttons="agentModeButtons"
                    :usesLinks="false"
                    :visually-hide-title="true"
                />
            </div>
            <div class="header-actions">
                <button
                    v-if="shouldAllowPinning()"
                    class="header-button pin-button"
                    :class="{ 'is-pinned': isPinned }"
                    :title="isPinned ? 'Unpin drawer' : 'Pin drawer open'"
                    data-el="expert-drawer-pin-button"
                    @click="togglePin"
                >
                    <LockClosedIcon v-if="isPinned" class="ff-icon" />
                    <LockOpenIcon v-else class="ff-icon" />
                </button>
                <button
                    class="header-button"
                    :title="'Close Expert'"
                    data-el="expert-drawer-close-button"
                    @click="closeDrawer"
                >
                    <XIcon class="ff-icon" />
                </button>
            </div>
        </div>
        <ExpertPanel />
    </div>
</template>

<script>
import { LockClosedIcon, LockOpenIcon, XIcon } from '@heroicons/vue/solid'
import { mapActions, mapState } from 'vuex'

import ToggleButtonGroup from '../../elements/ToggleButtonGroup.vue'

import ExpertPanel from '../../expert/Expert.vue'

export default {
    name: 'ExpertDrawer',
    components: {
        ToggleButtonGroup,
        ExpertPanel,
        XIcon,
        LockClosedIcon,
        LockOpenIcon
    },
    inject: ['togglePinWithWidth', 'shouldAllowPinning'],
    computed: {
        ...mapState('ux/drawers', ['rightDrawer']),
        ...mapState('product/expert', ['agentMode']),
        agentModeButtons () {
            return [
                { title: 'Support', value: 'ff-agent' },
                { title: 'Insights', value: 'operator-agent' }
            ]
        },
        isPinned () {
            return this.rightDrawer.fixed
        },
        agentModeWrapper: {
            get () {
                return this.agentMode
            },
            set (value) {
                this.setAgentMode(value)
            }
        }
    },
    mounted () {
        // Wait for drawer slide-in animation to complete (300ms) before focusing
        setTimeout(() => {
            this.$refs.drawer?.focus()
        }, 350)
    },
    methods: {
        ...mapActions('ux/drawers', ['closeRightDrawer']),
        ...mapActions('product/expert', ['setAgentMode']),
        closeDrawer () {
            this.closeRightDrawer()
        },
        togglePin () {
            this.togglePinWithWidth()
        }
    }
}
</script>

<style scoped>
.ff-expert-drawer {
    display: flex;
    flex-direction: column;
    height: 100%;
    overflow: hidden; /* Prevent drawer from scrolling */
}

.ff-expert-drawer:focus {
    outline: none;
}

.ff-expert-drawer .header {
    padding: 1rem 1.5rem;
    background: linear-gradient(white, white) padding-box,
                linear-gradient(90deg, var(--color-red-600), #5048e5, var(--color-red-600), #5048e5, var(--color-red-600)) border-box;
    border: none;
    border-bottom: 1px solid transparent;
    background-size: 200% 100%;
    animation: gradient-flow-lr 4s linear infinite;
    flex-shrink: 0; /* Prevent header from shrinking */
    display: flex;
    align-items: center;
    justify-content: space-between;
}

/* Logo + Expert title on left */
.ff-expert-drawer .header > .flex:first-child {
    flex: 1;
    justify-content: flex-start;
}

/* Mode selector in center */
.ff-expert-drawer .header .agent-mode {
    flex: 0 0 auto;
}

.ff-expert-drawer .header .agent-mode :deep(.ff-btn) {
    min-width: 5.5rem;
    justify-content: center;
}

/* Actions on right */
.ff-expert-drawer .header .header-actions {
    flex: 1;
    justify-content: flex-end;
    display: flex;
    align-items: center;
    gap: 0.25rem;
}

.ff-expert-drawer .header .flex {
    display: flex;
}

.ff-expert-drawer .header .flex.items-center {
    align-items: center;
}

.ff-expert-drawer .header .flex.gap-1 {
    gap: 0.25rem; /* gap-1 */
}

.ff-expert-drawer .header .title {
    font-size: 1rem;
    font-weight: 700; /* font-bold */
    line-height: 20px;
    color: #1f2937; /* text-gray-800 */
    margin: 0;
}

.ff-expert-drawer .header .header-actions .header-button {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
    padding: 0;
    background: none;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    color: inherit;
    font: inherit;
    transition: background-color 0.15s ease;
}

.ff-expert-drawer .header .header-actions .header-button:hover {
    cursor: pointer;
    background: var(--color-gray-100);
}

.ff-expert-drawer .header .header-actions .header-button:focus-visible {
    outline: 2px solid var(--color-indigo-700);
    outline-offset: 1px;
}

.ff-expert-drawer .header .header-actions .header-button.pin-button.is-pinned {
    background: var(--color-indigo-800);
    color: white;
}

.ff-expert-drawer .header .header-actions .header-button.pin-button.is-pinned:hover {
    background: var(--color-indigo-900);
}

/* Ensure ExpertPanel fills remaining space */
.ff-expert-drawer > *:not(.header) {
    flex: 1;
    min-height: 0; /* Important for flex child overflow */
}

@keyframes gradient-flow-lr {
    0% {
        background-position: 0% 0%;
    }
    100% {
        background-position: 100% 0%;
    }
}
</style>
