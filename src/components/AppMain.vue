<script>
import axios from 'axios';



export default {
    components: {

    },
    data() {
        return {
            store,
        }
    },
    methods: {
        fetchCard() {
            console.log('fetching data')
            const offset = this.store.offset //1. richiamo e salvo la variabile
            axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php`, {
                params: {
                    num: 20,
                    offset: this.store.pageOffset,
                    fname: this.store.searchName,
                    type: this.store.filteredCardType,
                }
            }) //2. inserisco la var tra le option
                .then((res) => {
                    //decide chi implementa l'API che ti po di risposta viene data
                    const cards = res.data.data;
                    this.store.cards = cards;
                    this.store.count = cards.length;
                })
                .catch((error) => { //serve per recuperare gli errori generati dal server (da concatenare subito dopo il then.)
                    console.log(error);
                    this.store.filteredCardType = '';
                })
                .finally(() => { //serve per eseguire comunque il codice anche in caso di errore

                })
        },

    },

    computed: {
        setPageOffset() {
            return this.store.pageOffset;
        }
    },

    watch: {
        setPageOffset() {
            this.fetchCard();
            console.log('watch works')
        }
    },

    created() {
        this.fetchCard()
    },


}
</script>

<template>
    <main class="main">

        <div class="container">





            <div class="counter">
                {{ store.count }}
            </div>

            <div class="cards">


            </div>

        </div>

    </main>
</template>


<style lang="scss" scoped>
.main {
    background-color: burlywood;

    .container {
        padding: 50px 0;

        .wrapper {
            display: flex;
            gap: 50px;
        }

    }

}

.select-wrapper,
.counter {
    margin-bottom: 30px;
}

.cards {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 20px;


}
</style>