<template>
    <router-link
        v-slot="{ href, navigate }"
        :to="to"
        class="ff-icon-link"
        :title="title"
    >
        <a :href="href" class="flex" @click.stop="navigate">
            <component :is="icon" :class="`ff-icon ${iconColor}`" />
            <slot />
        </a>
    </router-link>
</template>

<script>
export default {
    name: 'IconLink',
    props: {
        icon: {
            type: Function,
            required: true
        },
        to: {
            type: [String, Object],
            required: true
        },
        title: {
            type: String,
            default: undefined
        },
        iconColor: {
            type: String,
            default: 'text-teal-700'
        }
    }
}
</script>

<style scoped>
.ff-icon-link {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;

    border-radius: var(--spacing-sm);
    border: 1px solid var(--color-gray-300);

    height: 28px;
    padding: var(--spacing-sm) 5px;

    font-size: var(--font-size-sm);
    line-height: 20px;
}

.ff-icon-link svg {
    margin-right: 5px;
}

.ff-icon-link:hover {
    cursor: pointer;
    border-color: var(--color-blue-600);

    color: var(--color-blue-600);
    background-color: var(--color-gray-50);
}

.ff-icon-link:hover .ff-icon {
    color: var(--color-blue-600);
}
</style>
