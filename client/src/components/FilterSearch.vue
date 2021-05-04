<template>
    <div>
        <b-row class="filter-wrapper">
            <b-form-input 
                data-filter="filter" 
                class="filter-box" 
                v-model="filter" 
                placeholder="Search Timeline"></b-form-input>  
            <b-button class="search-btn">
                <b-icon icon="search" class="search-btn_icon"></b-icon>
            </b-button>
            <div class="recommendations" v-show="filter.length > 0 && !(selectedFilter === filter)">
                <b-list-group>
                    <b-list-group-item button v-for="item in dataFilter" :key="item.id" @click="setFilter(item)">
                        {{ item.comment }}
                    </b-list-group-item>
                </b-list-group>
            </div>
        </b-row>
        <CategoryFilter 
            :activities="dataFilter" 
            :origActivities="firstActivites"
            :filteredCategories="filteredCategories" />
    </div>
</template>

<script>
import CategoryFilter from './CategoryFilter';
import moment from 'moment';
export default {
  name: 'FilterSearch',
  components: {
      CategoryFilter,
  },
  data() {
      return {
          filter: '',
          firstActivites: null,
          filteredCategories: [],
          selectedFilter: undefined
      }
  },
  computed: {
      
  },
  created () {
    this.fetchFirstActivities();
  },
  updated () {
      this.filterDuplicatedItems();
      this.setCommentsList();
  },
  methods: {
    setFilter(selectedFilter) {
        this.selectedFilter = selectedFilter.comment;
        this.filter = selectedFilter.comment;
    },
    fetchFirstActivities() {
        fetch("http://localhost:3000/activities/v1")
            .then(response => response.json())
            .then(data => (this.firstActivites = data));
    }
  },
  computed: {
      dataFilter() {
          const data = this.firstActivites;
          const activitiesData = data && data.activities ? data.activities : data;
          const self = this;
           if (this.filter) {
                const filteredData = activitiesData.filter((item) => {
                    return item.comment.toLowerCase().includes(self.filter.toLowerCase());
                });

                return filteredData;
           }
           return activitiesData;
      },
      sortedArray() {
          this.firstActivites.sort( ( a, b) => {
                return new Date(parseInt(a.d_created)) - new Date(parseInt(b.d_created));
            });
          return this.firstActivites; 
      },
      activitiesList() {
          const newActivitiesList = [];
          const details = [];
          this.firstActivites.forEach((item, index) => {
              const d = new Date(parseInt(item.d_created));
              const month = moment(d).format('MMMM');
              if(newActivitiesList[index-1].month !== month){
                  details.push(item);
                  newActivitiesList.push({
                    month: month,
                    details 
                  });
              } else {
                  newActivitiesList[index-1].details.push(item);
              }
          });

          return newActivitiesList;
      },
      filterDuplicatedItems() {
        const activities = this.firstActivites;
        const resourceTypes = [];
        if (activities){
            activities.forEach((item, index) => {
                if(resourceTypes.length === 0 || (resourceTypes[index-1] && resourceTypes[index-1].resource_type !== item.resource_type)) {
                    resourceTypes.push(
                        {
                            id: item.id,
                            resource_type: item.resource_type                            
                        }
                    );
                }
            });

            this.filteredCategories = resourceTypes;
        } 
    },
  },
};
</script>

<style scoped lang="scss">
@import './../styles/style.scss';
.filter-wrapper {
    height: 46px;
    line-height: 1.75;
    position: relative;
    .filter-box {
        width: 350px;
        line-height: 1.75;
        border-right: 0;
        float: left;
        height: 2rem;
        border-radius: 0 0 0 0;
        &:focus {
            box-shadow: 0 0 0 $second-color;
        }
    }
    .search-btn {
        border-radius: 0 5px 5px 0;
        height: 2rem;
        width: 2.25rem;
        position: relative;
        background-color: $main-color;
        outline: 0;
        border: 0;
        &_icon {
            position: absolute;
            left: 8px;
            top: 6px;
        }
        &:active, &:focus {
            box-shadow: 0 0 0;
        }
    }
    .recommendations {
        position: absolute;
        left: 0;
        top: 32px;
        width: 100%;
        width: 350px;
        .list-group {
            border-radius: 0;
            &-item{
                padding: 0 0 0 10px;
                border-bottom: 0;
                &:last-child {
                    border-bottom: 1px solid $outline-color;
                }
            }
        }
    }
}
</style>