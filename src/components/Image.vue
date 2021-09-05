<template>
    <div class="card text-dark bg-light" style="margin-top: 50px">
        <img :alt="data.title" :src="data.image" class="img-fluid card-img-top">
        <div class="card-body">
            <h5 class="card-title">{{data.title}}</h5>
            <h6 class="card-subtitle mb-2 text-muted">{{createdDate}}</h6>
            <p class="card-text">{{normalizedDesc}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name : 'Image',
    props: {
        data : Object
    },
    computed :{
        createdDate() {
            const date = new Date(this.data.created)
            return date.toDateString()
        },
        normalizedDesc() {
            return this.data.description.length > 200 ? this.data.description.substring(0, 200) : this.data.description
        }
    }
}
</script>
<style scoped>

</style>
