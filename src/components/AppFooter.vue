<script>
export default {
    name: 'AppFooter'
}
</script>
<template>

    <div class="row">
        <img src="/Konami-Logo.wine.svg" class="resizeLogo" alt="logo_konami">
        <p>&copy;1996 KAZUKI TAKAHASHI</p>
    </div>

</template>
<style scoped>
.row{
    
    background-color: black;
    padding: 0 2rem;
    align-items: center;
    justify-content: space-between;

    p {
        color: white;
    }
}

.resizeLogo{
    width: 100px;
}
</style>