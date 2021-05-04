<template>
    <div>
        <b-row class="filter-btns">
            <b-row class="filter-btns_title">
                Filter by:
            </b-row>
            <b-row class="filter-btns_btns">
                <b-button 
                    variant="outline-primary"
                    @click="filterCategories('All')"
                    class="{selected: !selectedCategory}">
                    <b-iconstack v-show="!selectedCategory">        
                        <b-icon icon="circle-fill"></b-icon>
                        <b-icon icon="check"></b-icon>
                    </b-iconstack>
                    All work
                </b-button>
                <b-button 
                    variant="outline-primary" 
                    v-for="item in filteredCategories" 
                    :key="item.id" 
                    @click="filterCategories(item)"
                    class="{selected: selectedCategory === item.resource_type}">
                        <b-iconstack v-show="selectedCategory === item.resource_type">        
                            <b-icon icon="circle-fill"></b-icon>
                            <b-icon icon="check"></b-icon>
                        </b-iconstack>
                        {{ item.resource_type | formatCategoryText }}
                </b-button>
            </b-row>
            <span>{{dateDayMonth}}, {{dateYear}}</span>
        </b-row>
        <ActivitiesItems :items="activities" />
    </div>
</template>

<script>
import ActivitiesItems from './ActivitiesItems';

export default {
    name: 'CategoryFilter',
    components: {
        ActivitiesItems,
    },
    props: {
        activities: {
            type: Array,
            default: () => [] 
        },
        origActivities: {
            type: Array,
            default: () => [] 
        },
        filteredCategories: {
            type: Array,
            default: () => [] 
        },
        dateDayMonth: String,
        dateYear: String,
    },
    data() {
        return {
            //filteredCategories: [],
            selectedCategory: undefined
        }
    },
    filters: {
        formatCategoryText: function(value) {
            if (!value) return '';
            
            if (value.indexOf('_') < 0) {
                // one word category name
                return value.charAt(0).toUpperCase() + value.slice(1);
            } else {
                // multiple words category name
                const words = value.split('_');
                value = '';
                words.forEach((word, index) => {
                    let space = index == words.length - 1 ? '' : ' ';
                    value += word.charAt(0).toUpperCase() + word.slice(1) + space;
                });
                return value;
            }
        }
    },
    methods: {
        filterCategories(category) {
            let activities = this.origActivities;
            if (category === 'All') {
                this.selectedCategory = undefined;
                this.activities = activities;
                return;
            }
            this.selectedCategory = category.resource_type;
            this.activities = activities && activities.filter((activity) => {
                return activity.resource_type === category.resource_type;
            });
        },
        clearCategoryFilter() {
            const originalActivities = this.origActivities;
            this.activities = originalActivities;
            this.selectedCategory = undefined;
        }
    },
};
</script>

<style scoped lang="scss">
@import './../styles/style.scss';
// override bootstrapa active btn behavior
.btn-outline-primary:not(:disabled):not(.disabled):active {
    @extend .category-btn-active;
}
.filter-btns {
    &_title {
        color: $text-color;
        width: 100%;
        padding: 0 0 0.45rem 0.25rem;
    }
    &_btns {
        padding: 0.25rem 0.75rem 0.25rem 0.25rem;
        .btn {
            color: $main-color;
            border-color: $main-color;
            margin-right: .25rem;
            font-size: 1rem;
            .b-icon {
                font-size: 1rem;
                &.bi-check {
                    color: $second-color;
                }
            }
            &:hover, 
            &:focus, 
            &:active {
                @extend .category-btn-active;
            }
            &.selected {
                color: $main-color;
                border-color: $main-color;
                margin-right: .25rem;
                background-color: $second-color;
                &:hover, 
                &:focus, 
                &:active {
                    background-color: $second-color;
                    color: $main-color;
                }
            }
        }
    }
}
</style>

