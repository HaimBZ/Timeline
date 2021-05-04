        
<template>
    <b-row>
        <!-- iterates the months of activities-->
        <b-container class="activities-items">
            
            <b-row class="activities-items_month">
                <b-badge variant="light">{{ getDateMonth(items && items[0].d_created) }}</b-badge>
            </b-row>
            <!-- itterates activities within months and displaying each in card -->
            <div class="wrapper">
                <b-row class="activities-items_item" v-for="item in items" :key="item.id">
                <!-- itterates activities within months and displaying each in card -->
                    <b-row class="activity-item" sm="12">
                        <b-col sm="8" class="inner-col">
                            <b-col sm="1" class="inner-col_img">
                                <img :src="`${item.topic_data.icon_path}`" alt="" width="50" />
                            </b-col>
                            <b-col sm="11" class="inner-col_desc">    
                                <div>{{ item.comment }}</div>
                                <div>{{ item.d_created | dateFormat }}</div>
                            </b-col>
                        </b-col>
                        <b-col sm="2" class="activity-row">
                            <div class="activity-row_link">
                                <span>Score</span> <span>{{ item.score | getScore }}/{{ item.possible_score | getScore }}</span>
                            </div>
                        </b-col>
                        <b-col sm="2" class="activity-row">
                            <div class="activity-row_link">
                                <b-icon icon="eye" aria-hidden="true"></b-icon>
                                <span v-b-modal="'activity-details'">View Work</span>
                            </div>
                        </b-col>
                    </b-row>
                </b-row>
            </div>

            <div class="time-line"></div>
        </b-container>

        <b-row class="more">
            <div class="more-link">
                <b-icon rotate="90" icon="chevron-right" shift-h="-4"></b-icon>
                <span>Load More</span>
            </div>
        </b-row>

        <b-modal id="activity-details" hide-footer>            
            <div class="modal-wrapper">
                <template>
                    <div class="img flexer">
                        <img src="../assets/topics/dna.png" alt="" width="50">
                    </div>
                    <div class="activity-title flexer">DNA Movie Quiz</div>
                    <div class="date flexer">Oct 28, 2019 . 6:25pm</div>
                </template>
                <div class="middle">
                    <div class="comment">Great Job!</div>
                </div>
                <div class="bottom">
                    <div class="score">Score <span>3/10</span></div>
                </div>
            </div>
        </b-modal>
    </b-row>
</template>

<script>
import moment from 'moment';
export default {
    name: 'ActivitiesItems',
    data() {
        return {
            imagePath: ''
        }
    },
    props: {
        items: {
            type: Array,
            default: () => []
        },
    },
    methods: {
        getHumanDate(date) {
            const d = new Date(date);
            return moment(d, 'YYYY-MM-DD').format('DD/MM/YYYY');
        },
        getDateMonth(timestamp) {
            if(timestamp) {
                const d = new Date(parseInt(timestamp));
                return moment(d).format('MMMM');   
            }
            return null;
        },
    },
    filters: {
        dateFormat: function(value) {
            if (value) {
                return moment(parseInt(value)).format('MMMM Do, YYYY . h:mm:ss a')
            }
        },
        getScore(value) {
            if(!value){
                return "0";
            }
            return value;
        }
    },
};
</script>

<style scoped lang="scss">
@import './../styles/style.scss';
.activities-items {
    padding-left: 0;
    position: relative;
    &_month {
        margin: 1.25rem 0;
        .badge {
            padding: 3px 23px;
            background: $badge-color;
        }
    }
    &_item {
        padding: 1.5rem 0;
        border: 2px solid $outline-color;
        border-radius: 7px;
        margin-bottom: 1rem;
        background: $second-color;
        .activity-row {
            align-self: center;
            color: $main-color;
            font-weight: 700;
            &_link {
                cursor: pointer;
            }
        }
        .inner-col {
            display: flex;
            padding-left: 0;
            &_desc {
                color: $text-color;
                div:first-child {
                    font-weight: 700;
                }
            }
        }
    }
    .time-line {
        border-left: 1px solid $outline-color;
        position: absolute;
        left: 3rem;
        top: 2rem;
        z-index: -1;
        height: 90%;
    }
}
.activity-item {
    width: 100%;
}
.more {
    display: flex;
    width: 100%;
    justify-content: center;
    &-link {
        color: $main-color;
        font-weight: 700;
        .b-icon, span {
            color: $main-color;
        }
    }
}
#activity-details {
    .modal-content {
        border: 3px solid red !important;
    }
    .flexer {
        display: flex;
        justify-content: center;
    }
    .activity-title {
        font-size: 1.5rem;
    }
    .date {
        font-size: .75rem;
    }
    .middle {
        font-size: 1rem;
    }
    .bottom {
        color: $main-color;
        span {
            font-weight: 700;
        }
    }
}
</style>