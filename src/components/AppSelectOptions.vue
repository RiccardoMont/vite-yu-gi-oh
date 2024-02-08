<script>
export default {
    name: 'AppSelectOptions',
    props: {
        singleArchetype: Object
    },
}
</script>
<template>
    <option :value="singleArchetype">{{ singleArchetype }}</option>
</template>
<style scoped></style>