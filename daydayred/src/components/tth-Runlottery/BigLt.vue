<template>
  <div class="BigLt">
    <div class="bl-nav">
      <div class="bl-nav-left">
        <i class="iconfont icon-jiantou" id="turnback" @click="backRl"></i>
      </div>
      <div class="bl-nav-title">
        <p>超级大乐透开奖</p>
      </div>
      <div class="bl-nav-right">
      </div>
    </div>
    <ul id="all-number">
      <li v-for="(item, index) in ssq" :class="{'active':!index}" @click="numberDetail(index)">
        <span>第{{item['Name']}}期 {{EndTime[index]}}</span>
        <div>
          <p id="getMoney">
            <strong v-for="everyNumber in WinNumber[index]">{{everyNumber}}</strong>
          </p>
          <div v-if="!index" id="final-money">
            <div>
              <span>{{item['Amount']}}<b>注</b> {{item['Money']}}<b>元</b></span>
              <p>一等奖</p>
            </div>
            <div>
              <span>{{item['TotalMoney']}}<b>元</b></span>
              <p>奖池</p>
            </div>
          </div>
        </div>
        <i class="iconfont icon-arrow-right"></i>
      </li>
    </ul>
  </div>
</template>

<script>
  import Test from './test'
  import {Indicator} from 'mint-ui'
  export default {
    name: 'balllist',
    data () {
      return {
        ssq: {},
        getNumbers: [],
        getWeekDay: '',
        getMTime: '',
        WinNumber: [],
        EndTime: [],
        localID: ''
      }
    },
    methods: {
      testData () {
        let that = this
        this.$request({
          type: 'get',
          url: 'api/data/Handler.ashx?action=602&params={%22LotteryID%22:4}',
          headers: {},
          params: {},
          success: function (res) {
            this.ssq = res.data.data
            this.ssq.forEach(function (e, index) {
              that.WinNumber[index] = Test.cutBiglt(e['WinNumber'])
              that.EndTime[index] = Test.cutTime(e['EndTime'])
            })
            Indicator.close()
          },
          failed: function (err) {
            console.log(err)
          }
        })
      },
      backRl () {
        this.$router.go(-1)
      },
      numberDetail (index) {
        this.$router.push({path: '/bigltnext'})
        this.localID = this.ssq[index]['ID']
      }
    },
    mounted () {
      Indicator.open('加载中...')
      this.testData()
    },
    beforeDestroy () {
      this.$bus.emit('get', {
        changeID: this.localID
      })
    }
  }
</script>

<style scoped lang="less">
  @import "../../common/css/style";
  .BigLt{
    background-color: @color-background-white;
      margin:0 auto;
  }
  /*头部*/
  .bl-nav {
    width: 100%;
    height: 12vmin;
    background-color: @color-red;
    display: flex;
    justify-content: space-between;
  }
  .bl-nav > div {
    display: inline-block;
  }
  .bl-nav-left {
    width: 20%;
    height: 100%;
    font-size: 5.6vmin;
    overflow: hidden;

  }
  .bl-nav-left i{
    padding-left: 4vmin;
  }
  .bl-nav-right {
    width: 20%;
    height: 100%;
    // background-color: blue;
  }
  .bl-nav-title {
    /*width: 34.6%;*/
    height: 100%;
    margin: 0 auto;
    overflow: hidden;
  }
  .bl-nav-title p {
    text-align: center;
    font-size: 5.5vmin;
    font-weight: 700;
    color: white;
    line-height: 12vmin;
  }
  /*返回键*/
  #turnback {
    width: 5.86667vmin;
    height: 5.86667vmin;
    color: @color-background-white;
    font-weight: bold;
    font-size: 3.8vmin;
    display: inline-block;
    margin-left: 10px;
    margin-top: 3.7vmin;
  }
  /*中奖号码列表*/
  ul li{
    height: 17.86667vmin;
    box-sizing: border-box;
    border-bottom: 1px solid #e6e6e6;
    padding: 4vmin 3.73333vmin 4vmin 4.53333vmin;
  }
  ul li span{
    display: block;
    color: @color-text-gray;
    line-height: 1;
    font-size: 3.46667vmin;
    margin-bottom: .8vmin;
  }
  ul li p{
    line-height: 1.5;
  }
  ul li p>strong{
    font-size: 4.8vmin;
    font-weight: 400;
    margin-right: 2.4vmin;
    color: #ff5f5f;
  }
  ul li p>strong:last-child{
    color: #6b8dff;
  }
  ul li p>strong:nth-child(6){
    color: #6b8dff;
  }
  li i{
    width: 6.66667vmin;
    height: 6.66667vmin;
    float: right;
    color: @color-text-gray;
    margin-top: -8.13333vmin;
  }
  // 当天号码
  .active{
    padding: 4vmin;
    padding-bottom: 2.66667vmin;
    box-sizing: border-box;
    border-bottom: 1px solid #e6e6e6;
    height: 36.5vmin;
  }
  .active>span{
    color: #000;
  }
  .active>p{
    margin:3.46667vmin 0;
  }
  .active strong {
    width: 8.26667vmin;
    height: 8.26667vmin;
    line-height: 8.26667vmin;
    border-radius: 50%;
    color: #fff;
    text-align: center;
    display: inline-block;
    font-size: 4vmin;
    font-weight: 700;
    margin-right: 1.6vmin;
    box-sizing: border-box;
    background: #ff5f5f;
    font-family: -apple-system,BlinkMacSystemFont,PingFang SC,Helvetica
    Neue,STHeiti,Microsoft Yahei,Tahoma,Simsun,sans-serif;
  }
  .active strong:last-child{
    color: @color-background-white;
    background: @color-blue;
  }
  .active>div p:first-child{
    margin:3.46667vmin 0;
  }
  .active strong:nth-child(6){
    color: @color-background-white;
    background: @color-blue;
  }
  .active i{
    margin-top: -20vmin;
  }
  #final-money{
    width: 100%;
    height: 16vmin;
    margin-top: 2vmin;
  }
  #final-money>div{
    width: 46%;
    height:100%;
    display: inline-block;
    overflow: hidden;
  }
  #final-money>div:first-child{
    margin-left: 4vmin;
    margin-right: 1vmin;
  }
  #final-money>div p{
    font-size: 4vmin;
    text-align: center;
    color: @color-text-gray;
  }
  #final-money>div span{
    font-size: 4vmin;
    text-align: center;
    color: #ff5f5f;
  }
  #final-money>div b{
    color: @color-text-gray;
  }
</style>

