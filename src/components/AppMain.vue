<script>
import axios from 'axios';

export default {
    name: 'AppMain',
    data() {
        return {
            list: []
        }
    },
    mounted() {
        axios
            .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
            .then((response) => {
                console.log(response.data);
                this.list = response.data.data;
            })
    }
}
</script>
<template>
    <div class="container">
        <div class="row" v-if="list != undefined">
            <div class="card col-20" v-for="(singlecard, index) in list">
                <img :src="singlecard.card_images[0].image_url_small">
                <span class="title">{{ singlecard.name }}</span>
                <br>
                <span class="archetype">{{ singlecard.archetype }}</span>
            </div>
        </div>
    </div>
</template>
<style scoped>
.container {
    background-color: white;
    display: flex;
    justify-content: center;
}

.row {
    width: 90%;
    justify-content: center;
}

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
    transition: border-bottom-right-radius 0.5s,  border-bottom-left-radius 0.5s, box-shadow 0.6s, filter 0.6s ;

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
    
    box-shadow: 10px 5px 5px #a72222;
    filter: drop-shadow(15px 8px 3px #8f1d1d);
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 1rem;
    
}
</style>