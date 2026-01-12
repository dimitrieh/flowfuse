<template>
    <div class="ff-box" :data-el="`dashboard-section-${type}`">
        <div class="ff-box-header">
            <div class="flex items-center gap-2">
                <span v-if="$slots.icon" class="icon">
                    <slot name="icon" />
                </span>
                <h3 v-if="title" class="ff-box-title">
                    <span>
                        {{ title }}
                    </span>
                </h3>
            </div>
            <slot name="actions" />
        </div>
        <div class="ff-box-content flex flex-col flex-1 overflow-auto">
            <slot />
        </div>
    </div>
</template>

<script>
export default {
    name: 'DashboardSection',
    props: {
        title: {
            type: String,
            required: false,
            default: null
        },
        type: {
            type: String,
            required: true
        }
    }
}
</script>

<style scoped>
.ff-box {
    border: 1px solid var(--color-border);
    background: var(--color-white);
    padding: 10px;
    display: flex;
    flex-direction: column;
    flex: 1;
}

.ff-box .ff-box-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
}

.ff-box .ff-box-header .ff-box-title {
    margin: 0;
    padding: 0;
    font-size: 1.3rem;
}
</style>
