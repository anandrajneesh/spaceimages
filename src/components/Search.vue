<template>
    <form @submit="onSubmit" class="search-form">
        <div class="input-group">
            <input type="text" class="form-control" v-model="value" placeholder="Search anything in Space" aria-label="Search anything in space" aria-describedby="button-addon2">
            <button class="btn btn-outline-secondary" type="submit" id="button-addon2"><i class="fas fa-search"></i></button>
        </div>
  </form>
</template>
<script>
export default {
    name : 'Search',
    props: {
        text : String
    },
    data (){
        return {
            value :''
        }
    },
    created (){
        this.value = this.text === 'undefined' ? this.value : this.text
    },
    methods : {
        onSubmit(e){
            e.preventDefault()
            this.$emit('search', this.value)
        }
    },
    emits: ['search']
}
</script>