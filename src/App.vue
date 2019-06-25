<template>
  <div id="app">
    <div class="header">
      <div class="head-title">
        <h1>
          <span>{{ gameName }}赛事</span>
          <i class="cubeic-select" @click='showGamePick'></i>
        </h1>
        <div class="cont-title">
          <ul class="cont-title-list">
            <li :key='index'
                :class="{active : checkIndex == index}"
                @click='switchTab(index)' 
                v-for='(item,index) in tabList'>{{ item.tabName }}
            </li>
          </ul>
          <i class="triangle-up" :class="{left : checkIndex == 0,right : checkIndex == 2}"></i>
        </div>
      </div>
    </div>
    <div class="content">
      <cube-slide :initialIndex='checkIndex'
                  :loop='false'
                  :autoPlay='false'
                  :showDots='false'
                  @change="slideChange">
        <cube-slide-item :key='index' v-for='(item,index) in tabList'>
          <game-list :gameName='gameName' :type='item.label' />
        </cube-slide-item>
      </cube-slide>
      
    </div>
  </div>
</template>

<script>

  import GameList from './components/GameList'

  const gameNameList = [
    {
      text: 'SOCCER',
      value: 'soccer'
    },
    {
      text: 'NBA',
      value: 'NBA'
    },
    {
      text: 'DOTA',
      value: 'dota'
    }
  ]

  export default {
    data() {
      return {
        checkIndex:1,   //导航条选中索引
        gameName:'soccer',
        // gamePicker:false,
        tabList:[
          {
            tabName:'已结束',
            label:'ENDED'
          },
          {
            tabName:'我的关注',
            label:'CONCERN'
          },
          {
            tabName:'直播中',
            label:'LIVE'
          }
        ]
      }
    },
    components:{
      GameList
    },
    methods: {
      //导航条切换
      switchTab(index){
        this.checkIndex = index
      },
      //slide页面切换
      slideChange(index){
        this.checkIndex = index
      },
      //弹出比赛选择框
      showGamePick(){
        if(!this.gamePicker){
          this.gamePicker = this.$createPicker({
            title: '赛事',
            data: [gameNameList],
            onSelect: this.gameNameSelect,
            onCancle: this.gameNameCancel
          })
        }
        this.gamePicker.show()
      },
      gameNameSelect(val,index,text){
        this.gameName = val[0]
      },
      gameNameCancel(text){
        console.log(text)
      },
    }
  }
</script>

<style scoped lang="scss">
  $black:#15191d;

  #app {
    height:100%;
    text-align: center;
    color: #fff;
    font-size: 16px;

    .header {
      width: 100%;
      height: 80px;
      background-color: $black;

      .head-title{
        
        h1{
          padding: 20px 0;
        }
        
        .cont-title {
          padding-bottom: 12px;
          position:relative;
          
          .cont-title-list{
            display: flex;
            font-size: 12px;
            justify-content: space-around;

            li{
              width: 60px;
              color: #e0e4e8;
            }

            .active{
              color: #fff;
            }
          }

          .triangle-up{
            display: inline-block;
            position:absolute;
            width: 0;
            height: 0;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            border-bottom: 8px solid #e0e4e8;
            border-right: 7px solid transparent;
            border-left: 7px solid transparent;
            border-top: 8px solid transparent;
            transition: all 0.4s;

            &.left{
              left: 16%;
            }

            &.right{
              left: 84%;
            }
          }
        }
      }
    }

    .content{
      overflow: hidden;
      color: $black;
      height: calc(100% - 80px);
    }
  }
</style>