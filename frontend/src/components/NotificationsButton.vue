<template>
    <div class="notifications-button-wrapper">
        <button class="notifications-button" data-el="notifications-button" data-click-exclude="right-drawer" @click="onClick">
            <MailIcon />
            <ff-notification-pill v-if="hasNotifications" data-el="notification-pill" class="ml-3" :count="notificationsCount" />
        </button>
    </div>
</template>

<script>
import { MailIcon } from '@heroicons/vue/outline'
import { markRaw } from 'vue'
import { mapActions, mapGetters, mapState } from 'vuex'

import NotificationsDrawer from './drawers/notifications/NotificationsDrawer.vue'

export default {
    name: 'NotificationsButton',
    components: { MailIcon },
    computed: {
        ...mapState('ux/drawers', ['rightDrawer']),
        ...mapGetters('account', ['hasNotifications']),
        ...mapGetters('account', ['unreadNotificationsCount']),
        notificationsCount: function () {
            // Return null if count = 0 so we don't show a 0 in the pill
            if (!this.unreadNotificationsCount) {
                return null
            }
            return this.unreadNotificationsCount
        }
    },
    methods: {
        ...mapActions('ux/drawers', ['openRightDrawer', 'closeRightDrawer']),
        onClick () {
            this.openRightDrawer({ component: markRaw(NotificationsDrawer) })
        }
    }
}
</script>

<style scoped>
.notifications-button-wrapper .notifications-button {
    color: var(--color-gray-800);
    display: flex;
    align-items: center;
    flex: 1;
    justify-content: center;
    width: 100%;
    height: 100%;
    padding: 18px;
    position: relative;
}
.notifications-button-wrapper .notifications-button > * {
    pointer-events: none;
}
.notifications-button-wrapper .notifications-button svg {
    flex: 1;
    width: 24px;
    height: 24px;
    transition: ease-in-out .1s;
    object-fit: contain;
}
.notifications-button-wrapper .notifications-button:hover svg {
    will-change: transform;
    color: var(--color-indigo-600);
    transform: scale(1.25) translateZ(0);
    backface-visibility: hidden;
    perspective: 1000px;
    stroke-width: 1.5px;
    shape-rendering: geometricPrecision;
    text-rendering: geometricPrecision;
}
.notifications-button-wrapper .notifications-button .ff-notification-pill {
    bottom: 10px;
    right: 5px;
    position: absolute;
    font-size: 0.65rem;
    padding: 0 7px;
    background-color: var(--color-red-500);
}
</style>
