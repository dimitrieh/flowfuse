<template>
    <div class="ff-layout--docs">
        <div class="ff-layout--docs-contents">
            <div id="platform-banner" />
            <slot />
        </div>
    </div>
</template>

<script>
export default {
    name: 'ff-layout-docs',
    data () {
        return { }
    }
}
</script>

<style>
.ff-layout--docs {
    background-color: white;
}

.ff-layout--docs-contents {
    max-width: 1012px;
    margin: auto;
    padding: 16px;
}

.ff-layout--docs h1 {
    margin: 16px 0;
}
</style>
