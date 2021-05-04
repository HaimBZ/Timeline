<template>
  <div class="timeline">
    <h1>Timeline</h1>
    <FilterSearch />
    <!--CategoryFilter />
    <ActivitiesItems /-->
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue';
import FilterSearch from '@/components/FilterSearch.vue';
/* import CategoryFilter from '@/components/CategoryFilter.vue';
import ActivitiesItems from '@/components/ActivitiesItems.vue'; */
//import moment from 'moment';
/* const date = new Date(1567015526);
const dateDayMonth = moment(date).format('MMMM Do');
const dateYear = moment(date).format('YYYY'); */

export default {
  name: 'home',
  components: {
    FilterSearch
  },
}
</script>

<style scoped lang="scss">
@import './../styles/style.scss';
h1 {
  font-size: 2rem;
  font-weight: 700;
  line-height: 1.5;
}
</style>
