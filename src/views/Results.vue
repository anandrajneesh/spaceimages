<template>
    <div class="d-flex flex-row searchbanner">
        <div class="align-self-center">
             <h1 class="display-6" style="margin-top: 6px; margin-left: 10px; color:white;"> Search Nasa</h1>
        </div>
        <div style="margin-left: 10px; margin-top: 12px;">
            <Search @search="searchEdited" :text="query"/>
        </div>
    </div>
    <Pagination @pageChange="pageChange" :currentPage="currentPage" :totalPages="totalPages"/>
    <div class="row row-cols-lg-4">
        <div class="col" :key="image.id" v-for="image in data">
            <Image :data="image"></Image>
        </div>
    </div>
    <Pagination @pageChange="pageChange" :currentPage="currentPage" :totalPages="totalPages"/>
</template>

<script>
import Search from '../components/Search.vue'
import Image from '../components/Image.vue'
import Pagination from '../components/Pagination.vue'

export default {
    name : 'Results',
    components: {
        Search,
        Image,
        Pagination
    },
    data(){
        return {
            data : [],
            total: 0,
            totalPages: 0,
            currentPage : 0,
            query : ''
        }
    },
    emits :['search'],
    async created() {
        this.query = this.$route.params.search
        this.currentPage = this.$route.query.page
        console.log(`creating results ${this.query} page ${this.currentPage}`)
        await this.search(this.query)
    },
    methods:{
        async searchEdited(value){
            this.$emit('search', value)
        },
        async search(value){
            let url = `https://images-api.nasa.gov/search?q=${value}&media_type=image`
            if(typeof this.currentPage === 'number' && this.currentPage > 1){
                url+=`&page=${this.currentPage}`
            }
            const res = await fetch(url)
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
            this.total = data.collection.metadata.total_hits
            this.totalPages = Math.ceil(this.total/100)
            this.currentPage = typeof this.currentPage ==='undefined' ? 1: this.currentPage
        },
        async pageChange(page) {
            console.log(`Page change event received for ${page}`)
            this.currentPage = page
            await this.search(this.query)
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
