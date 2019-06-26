<template>
    <div class="game-list">
        <cube-scroll ref="scroll"
                     :data='list'
                     :options="options"
                     @pulling-down='pullingDownList'
                     @pulling-up='pullingUplist'>
                <div class="game-detail" v-for='(item,index) in list' :key='index'>
                    <div class="left-team">
                        <div class="team-logo">
                            <img :src="item.hostLogoUrl" alt="">
                            <p>{{ item.hostTeamName }}</p>
                        </div>
                    </div>
                    <div class="mid-score">
                        <span class="status-logo" v-if='item.live'>{{ item.live }}</span>
                        <span class="order-logo" v-if='item.order'>{{ item.order }}</span>
                        <div class="team-score" :class="{ end : item.live == '已结束' }">{{ item.hostScore }} - {{ item.guestScore }}</div>
                        <div class="end-time" v-if='item.live !== "已结束"'>{{ item.endTime }}</div>
                    </div>
                    <div class="right-team">
                        <div class="team-logo">
                            <img :src="item.guestLogoUrl" alt="">
                            <p>{{ item.guestTeamName }}</p>
                        </div>
                    </div>
                </div>
        </cube-scroll>
    </div>
</template>

<script>

import gameList from '../common/data/game-data'
import { getData } from '../common/data/game-data'

    export default {
        props: {
            gameName:{
                type: String,
                default: 'soccer'
            },
            type:{
                type: String,
                default: 'CONCERN'
            }
        },
        data() {
            return {
                list:[],     //对应类型比赛列表
                options: {
                    scrollbar: true,
                    pullDownRefresh: {
                        threshold:60,
                        stop:60,
                        stopTime:200,
                        txt:'刷新成功'
                    },
                    pullUpLoad: {
                        threshold:0,
                        txt: {
                            more: '加载更多',
                            noMore: '没有更多的比赛啦'
                        }
                    }
                }          
            };
        },
        components: {

        },
        computed: {
            
        },
        created() {
            this.list = getData(gameList,this.gameName,this.type)
        },
        mounted() {

        },
        watch: {
            gameName(){
                this.list = getData(gameList,this.gameName,this.type)
            }
        },
        methods: {
            //下拉加载
            pullingDownList(){
                //模拟请求
                let timer = setInterval(() => {
                    console.log('down')
                    this.$refs.scroll.forceUpdate()
                    clearInterval(timer)    
                }, 2000);
            },
            //上拉加载
            pullingUplist(){
                console.log('up')
                this.$refs.scroll.forceUpdate()
            }
        },
    };
</script>

<style scoped lang="scss">

    .game-list{
        height: 100%;

        .game-detail {
        display: flex;
        justify-content: space-around;
        padding: 10px 0;
        border-bottom: 1px solid #e4e4e4;

        .left-team{
            width: 80px;
        }

        .mid-score{
            width: 80px;

            .status-logo{
                display:inline-block;
                padding: 3px 10px;
                background-color: #3d8f29;
                border-radius: 25px;
                margin-bottom: 7px;
                color: #fff;
                font-size: 12px;
                line-height: 100%;
            }

            .order-logo{
                display:inline-block;
                padding: 3px 20px;
                background-color: #fff;
                border-radius: 25px;
                border: 1px #3d8f29 solid;
                margin-bottom: 7px;
                color: #3d8f29;
                font-size: 12px;
                line-height: 100%;
            }

            .team-score{
                font-size: 14px;
                margin-top: 5px;
                font-weight: 500;

                &.end{
                    font-size: 22px;
                    color: #878f98;
                }
            }

            .end-time{
                margin-top: 7px;
                color: #878f98;
                font-size: 14px;
            }
        }

        .right-team{
            width: 80px;
        }

        .team-logo{

            img{
                display: inline-block;
                height: 38px;
                margin-bottom: 9px;
            }

            p{
                font-size: 14px;
            }
        }
    }
    }
</style>