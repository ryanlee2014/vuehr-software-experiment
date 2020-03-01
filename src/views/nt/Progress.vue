<template>
    <div>
    <div style="display: flex;justify-content: space-between">

    </div>
        <div>
            <div>
                <div class="small">
                    <line-chart :chart-data="datacollection"></line-chart>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import LineChart from '../../lib/chart/LineChart.js'

    export default {
        components: {
            LineChart
        },
        data () {
            return {
                datacollection: null
            }
        },
        mounted () {
            this.fillData()
        },
        methods: {
            fillData () {
                this.datacollection = {
                    labels: [1,2,3,4,5,6,7,8],
                    datasets: [
                        {
                            label: 'Coding time',
                            backgroundColor: '#f87979',
                            data: [5,6,5,7,3,0,0,0]
                        }
                    ]
                }
            },
            getRandomInt () {
                return Math.floor(Math.random() * (50 - 5 + 1)) + 5
            }
        }
    }
</script>

<style scoped>
    .small {
        max-width: 300px;
    }
</style>
