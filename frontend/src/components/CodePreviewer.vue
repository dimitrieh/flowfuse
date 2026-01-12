<template>
    <pre class="ff-code-previewer" data-el="ff-code-previewer"><code>{{ strippedSnippet }}</code></pre>
</template>

<script>
export default {
    name: 'ff-code-previewer',
    props: {
        snippet: {
            required: true,
            type: [Object, Array, String]
        }
    },
    computed: {
        strippedSnippet: function () {
            return this.snippet
        }
    }
}
</script>

<style scoped>
/* Component styles are managed globally */
</style>
