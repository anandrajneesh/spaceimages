<template>
    <div class="d-flex flex-row searchbanner">
        <div class="align-self-center">
             <h1 class="display-6" style="margin-top: 6px; margin-left: 10px; color:white"> Search Nasa</h1>
        </div>
        <div style="margin-left: 10px; margin-top: 12px;">
            <Search @search="searchEdited" :text="query"/>
        </div>
    </div>
    <div class="row row-cols-lg-4">
        <div class="col" :key="image.id" v-for="image in data">
            <Image :data="image"></Image>
        </div>
    </div>
</template>

<script>
import Search from '../components/Search.vue'
import Image from '../components/Image.vue'

export default {
    name : 'Results',
    components: {
        Search,
        Image
    },
    data(){
        return {
            data : [],
            query : ''
        }
    },
    emits :['search'],
    async created(){
        this.query = this.$route.params.search
        console.log(`creating results ${this.query}`)
        await this.search(this.query)
    },
    methods:{
        async searchEdited(value){
            this.$emit('search', value)
        },
        async search(value){
            const res = await fetch(`https://images-api.nasa.gov/search?q=${value}&media_type=image`)
            const data = await res.json()
            console.log(`searching ${value}`)
            this.data = data.collection.items.map((item) => {
                return {
                    id: item.data[0].nasa_id,
                    title: item.data[0].title,
                    image: item.links[0].href,
                    description: item.data[0].description,
                    created:item.data[0].date_created
                }
            })
        }
    }
}
</script>
<style scoped>
.searchbanner{
    background-image: url("../assets/home_banner.jpeg");
    margin-top: 15px;
    display: flex;
}
</style>
