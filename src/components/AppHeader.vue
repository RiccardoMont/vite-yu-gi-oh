<script>

export default {
    name: 'AppHeader'
}

</script>
<template>
    <div class="row">
        <img src="/yu-gi-oh.svg" class="resizeLogo" alt="">
        <h1>Yu-gi-oh! API</h1>
    </div>
</template>
<style scoped>
.row {
    align-items: center;
    background-color: black;
    padding-left: 2rem;
    

    h1 {
        color: white;
        margin-left: 2rem;
    }

}



.resizeLogo {
    width: 80px;
    max-width: 80px;
}
</style>