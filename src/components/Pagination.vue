<template>
  <nav aria-label="Search results page">
      <ul class="pagination justify-content-center">
          <li :class="[this.currentPage === 1 ? 'disabled' : '', 'page-item']">
              <a class="page-link" href="javascript:void(0)" @click="previous()">Previous</a>
          </li>
          <li v-for="n in this.totalPages" :key="n" :class="[current(n) ? 'active' : '', 'page-item']">
              <span v-if="current(n)" class="page-link">{{n}}</span>
              <a v-if="!current(n)" class="page-link" href="javascript:void(0)" @click="pageChange(n)">{{n}}</a>
          </li>
          <li :class="[this.currentPage === this.totalPages ? 'disabled' : '', 'page-item']">
              <a class="page-link" href="javascript:void(0)" @click="next()">Next</a>
          </li>
      </ul>
  </nav>
</template>
<script>
export default {
    name: 'Pagination',
    props: {
        currentPage: Number,
        totalPages: Number
    },
    emits: ['pageChange'],
    methods: {
        current(n) {
            return n === this.currentPage
        },
        next() {
            this.$emit('pageChange', this.currentPage+1)
        },
        previous() {
            this.$emit('pageChange', this.currentPage-1)
        },
        pageChange(n) {
            console.log(`Page change ${n}`)
            this.$emit('pageChange', n)
        }
    }
}
</script>
