<template>
    <div>
        <div class="watch-container">
            <div class="watch-content" v-for="dayItem in fixedShedule">
                <div class="day-date">{{ dayItem.daydate }}</div>
                <div class="day-descr">{{ dayItem.daydescr | deleteQuotes}}</div>
                <div class="day-content" v-for="contentItem in dayItem.daycontent">
                    <div class="event-header">
                        <div class="event-time">
                            <div class="event-time-begin">{{ contentItem.event.timebegin }}</div>
                        </div>
                        <div class="event-type">{{ contentItem.event.eventtype }}</div>
                    </div>
                    <div class="event-descr-row">
                        <div class="event-descr-cell">{{ contentItem.event.description | deleteQuotes}}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style scoped lang="less">
    .watch-container {
        width: 100%;
        /*height: 100vh;*/
        display: block;
        text-align: center;
        vertical-align: top;
        overflow-y: auto;
        overflow-x: hidden;
        padding-top: 40px;

        .watch-content {
            width: 28vw;
            display: inline-block;
            vertical-align: top;
            padding: 30px;
        }
        .day-date {
            text-align: left;
            padding: 10px;
            background-color: #F7F7F7;
            color: #000000;
            font-size: 36px;
        }
        .day-descr {
            text-align: left;
            padding-left: 10px;
            padding-right: 10px;
            padding-bottom: 10px;
            background-color: #F7F7F7;
            color: #000000;
            font-size: 26px;
            margin-bottom: 15px;
        }
        .day-content {
            text-align: left;
            display: table;
            .event-header {
                display: block;
                width: 100%;
                margin-top: 15px;
                padding-top: 5px;
                padding-bottom: 5px;
                .event-time {
                    font-size: 24px;
                    display: inline-block;
                    width: 76px;
                    text-align: center;
                    background-color: #F7F7F7;
                    color: #000000;
                    .event-time-begin {
                    }
                }
                .event-type {
                    font-size: 24px;
                    display: inline-table;
                    width: calc(28vw - 80px);
                    color: #FFFFFF;
                }
            }
            .event-descr-row {
                display: block;
                width: 28vw;
                margin-bottom: 15px;
                padding-left: 80px;
                .event-descr-cell {
                    display: block;
                    width: 100%;
                    font-size: 18px;
                }
            }

        }
    }
</style>
<script>
    import ajax from './components/helpers/ajax.js';

    export default{
        data(){
            return {
                rasp: []
            }
        },
        computed: {
            fixedShedule: function () {
                return this.rasp;
            }
        },

        methods: {
            getShow: function () {
                const self = this;
                const payload = {
                    callback: function () {
                        self.rasp = _.map(self.$store.state.app.show, function (item) {
                            return item;
                        });
                    }
                };
                this.$store.dispatch('GET_SHOW', payload);
            }
        },
        filters: {
            deleteQuotes: function (value) {
                if (!value) return '';
                value = value.toString();
                return value.replace(/&quot/g, '"');
            }
        },

        mounted() {
            const self = this;
            if (this.$store.state.app.show.length === 0) {
                //console.log('Заполняем');
                this.getShow();
            } else {
                //console.log('Из кэша');
                this.rasp = _.map(this.$store.state.app.show, function (item) {
                    return item;
                });
            }
        }
    }
</script>
