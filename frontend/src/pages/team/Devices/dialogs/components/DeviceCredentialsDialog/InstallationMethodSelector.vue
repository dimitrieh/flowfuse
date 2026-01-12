<template>
    <ul class="installation-method-selector">
        <li
            v-for="method in installationMethods"
            :key="method.slug"
            :class="{active: selectedMethod === method.slug}"
            class="ff-method-option"
            @click="$emit('select-method', method.slug)"
        >
            {{ method.label }}
        </li>
    </ul>
</template>

<script>
export default {
    name: 'InstallationMethodSelector',
    props: {
        installationMethods: {
            required: true,
            type: Array
        },
        selectedMethod: {
            required: true,
            type: String
        }
    },
    emits: ['select-method']
}
</script>

<style scoped>
.installation-method-selector {
    display: flex;
    gap: 50px;
    margin-bottom: 15px;
    position: relative;
}

.installation-method-selector .ff-method-option {
    flex: 1;
    text-align: center;
    border-bottom: 2px solid var(--color-border);
    padding-bottom: 5px;
    cursor: pointer;
    transition: ease-in-out .3s;
}

.installation-method-selector .ff-method-option.active {
    border-color: var(--color-indigo-500);
}

.installation-method-selector .ff-method-option:first-of-type:after {
    content: 'or';
    position: absolute;
    top: 5%;
    left: 49%;
    font-style: italic;
    font-size: var(--font-size-sm);
    color: var(--color-gray-500);
}
</style>
