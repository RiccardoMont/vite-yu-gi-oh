<script>
export default {
    name: 'AppCardCatalogue',
    props: {
        singlecard: Object
    }
}
</script>
<template>
    <div v-show="singlecard.show" class="card col-20">
        <img :src="singlecard.card_images[0].image_url_small">
        <span class="title">{{ singlecard.name }}</span>
        <br>
        <span class="archetype">{{ singlecard.archetype }}</span>
    </div>
</template>
<style scoped>
.col-20 {
    width: 20%;
    margin: 1rem;
}
.card {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    background-color: chocolate;
    border-bottom-left-radius: 1rem;
    text-align: center;
    transition: border-bottom-right-radius 0.5s, border-bottom-left-radius 0.5s, box-shadow 0.6s, filter 0.6s;
    cursor: pointer;

    & img {
        width: 100%;
        margin-bottom: 0.5rem;
    }

    & span {}

    .title {
        font-weight: 700;
        color: white;
    }
}

.card:hover {

    box-shadow: 10px 5px 5px brown;
    filter: drop-shadow(15px 8px 3px #8f1d1d);
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 1rem;

}
</style>