<template>
      <div class="runlottery">
            <div class="rl-nav" @click="backHome">
                <div class="rl-nav-title">
                    <p>开奖公告</p>
                </div>
            </div>
            <div class="rl-detail">
                <ul>
                  <li class="double" @click="dndetail">
                    <p>
                      <span>{{double['LotteryName']}}</span>
                      <span>第{{double['IssueName']}}期 {{dlTime}}</span>
                    </p>
                    <div class="double-number">
                      <div v-for="everyNumber in str1">
                        <p>{{everyNumber}}</p>
                      </div>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                  <li class="happy" @click="hndetail">
                    <p>
                      <span>{{happy['LotteryName']}}</span>
                      <span>第{{happy['IssueName']}}期 {{hpTime}}</span>
                    </p>
                    <div class="happy-number">
                      <div v-for="everyNumber in result">
                        <p>{{everyNumber}}</p>
                      </div>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                  <li class="football" @click="fcdetail">
                    <p>
                      <span>{{football['LotteryName']}}</span>
                      <span>第{{football['IssueName']}}期 {{ftTime}}</span>
                    </p>
                    <div>
                      <span id="ftResult">{{football['HTeam']}} {{football['Rz']}} {{football['VTeam']}}</span>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                  <li class="basketball" @click="bkdetail">
                    <p>
                      <span>{{basketball['LotteryName']}}</span>
                      <span>第{{basketball['IssueName']}}期 {{btTime}}</span>
                    </p>
                    <div>
                      <span id="bkResult">{{basketball['HTeam']}} {{basketball['Rz']}} {{basketball['VTeam']}}</span>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                  <li class="fucai3D" @click="fc3detail">
                    <p>
                      <span>{{welfare['LotteryName']}}</span>
                      <span>第{{welfare['IssueName']}}期 {{fcTimeStr[1]}}-{{fcTimeStr[2]}} {{shijian5}}</span>
                    </p>
                    <div class="fucai3D-number">
                      <div v-for="evstr in str51">
                        <p>{{evstr}}</p>
                      </div>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                  <li class="line-3" @click="lidetail">
                    <p>
                      <span>{{rank['LotteryName']}}</span>
                      <span>第{{rank['IssueName']}}期 {{rkTimeStr[1]}}-{{rkTimeStr[2]}} {{shijian6}}</span>
                    </p>
                    <div class="line-3-number">
                      <div v-for="evstr in str61">
                        <p>{{evstr}}</p>
                      </div>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                  <li class="jiangxi" @click="jxdetail">
                    <p>
                      <span>{{choose11['LotteryName']}}</span>
                      <span>第{{choose11['IssueName']}}期 {{jxTimeStr[1]}}-{{jxTimeStr[2]}} {{shijian7}}</span>
                    </p>
                    <div class="jiangxi-number">
                      <div v-for="evstr in str71">
                        <p>{{evstr}}</p>
                      </div>
                      <i class="iconfont icon-arrow-right"></i>
                    </div>
                  </li>
                </ul>
            </div>
        <wsb-footer></wsb-footer>
      </div>
</template>

<script>
    import Test from './test'
    import WsbFooter from '../../components/Footer'
    import {Indicator} from 'mint-ui'
    export default {
      name: '',
      components: {
        WsbFooter
      },
      data () {
        return {
          message: {},
          // 双色球
          str1: [],
          double: {},
          dlTime: '',
          // 大乐透
          result: [],
          happy: {},
          hpTime: '',
          // 足球竞猜
          football: {},
          ftTime: '',
          shijian3: '',
          // 篮球
          basketball: {},
          btTime: '',
          // 福彩3D
          welfare: {},
          str51: [],
          fcTimeStr: {},
          shijian5: '',
          //  排列3
          str61: [],
          rank: {},
          rkTimeStr: {},
          shijian6: '',
          // 江西
          choose11: {},
          str71: [],
          jxTimeStr: {},
          shijian7: ''
        }
      },
      mounted () {
        Indicator.open('加载中...')
        this.testData()
      },
      methods: {
        // 跳转至双色球
        dndetail () {
          this.$router.push({path: '/balllist'})
        },
        // 跳转至大乐透
        hndetail () {
          this.$router.push({path: '/biglt'})
        },
        // 跳转至福彩3
        fc3detail () {
          this.$router.push({path: '/fc3d'})
        },
        // 跳转至排列3
        lidetail () {
          this.$router.push({path: '/line3'})
        },
        // 跳转至江西11选5
        jxdetail () {
          this.$router.push({path: '/choose11in5'})
        },
        // 跳转至足彩
        fcdetail () {
          this.$router.push({path: '/rlfootball'})
        },
        // 跳转至篮球
        bkdetail () {
          this.$router.push({path: '/rlbasketball'})
        },
        // 点击头部返回主页
        backHome () {
          this.$router.push({path: '/home'})
        },
        // 请求接口数据
        testData () {
          let that = this
          this.$request({
            type: 'get',
            url: 'api/data/Handler.ashx?action=600&params={}',
            headers: {},
            params: {},
            success: function (response) {
              this.ssq = response.data.data
              that.double = response.data.data[0]
              that.dlTime = Test.cutTime(that.double['EndTime'])
              that.str1 = Test.cutNumber(that.double['Rz'])
              // 大乐透
              that.happy = response.data.data[1]
              that.result = Test.cutBiglt(that.happy['Rz'])
              that.hpTime = Test.cutTime(that.happy['EndTime'])
              // 足球
              that.football = response.data.data[2]
              that.ftTime = Test.cutTime(that.football['EndTime'])
              // 篮球
              that.basketball = response.data.data[3]
              that.btTime = Test.cutTime(that.basketball['EndTime'])
              // 福彩3D
              that.welfare = response.data.data[4]
              that.str5 = that.welfare['Rz']
              that.str51 = that.str5.split(',')
              that.fcTime = that.welfare['EndTime']
              that.fcTimeStr = that.fcTime.split('/')
              that.try5 = new Date(that.fcTimeStr[0], that.fcTimeStr[1], that.fcTimeStr[2]).getDay()
              that.shijian5 = '周' + '五六日一二三四'.charAt(that.try5)
              // 排列3
              that.rank = response.data.data[5]
              that.str6 = that.rank['Rz']
              that.str61 = that.str6.split(',')
              that.rkTime = that.rank['EndTime']
              that.rkTimeStr = that.rkTime.split('/')
              that.try6 = new Date(that.rkTimeStr[0], that.rkTimeStr[1], that.rkTimeStr[2]).getDay()
              that.shijian6 = '周' + '五六日一二三四'.charAt(that.try6)
              // 江西
              that.choose11 = response.data.data[6]
              that.str7 = that.choose11['Rz']
              that.str71 = that.str7.split(',')
              that.jxTime = that.choose11['EndTime']
              that.jxTimeStr = that.jxTime.split('/')
              that.try7 = new Date(that.jxTimeStr[0], that.jxTimeStr[1], that.jxTimeStr[2]).getDay()
              that.shijian7 = '周' + '五六日一二三四'.charAt(that.try7)
              Indicator.close()
            },
            failed: function (err) {
              console.log(err)
            }
          })
        }
      }
    }
</script>

<style scoped lang="less">

  @import "../../common/css/style";
  .runlottery{
    background-color:@color-background-white;
    margin:0 auto;
    height: 115vmin;
  }
  ul li{
    background-color:@color-background-white;
  }
  .rl-nav{
        width:100%;
        height: 12vmin;
        background-color: @color-red;
      }
  .rl-nav-title{
        width:30.6%;
        height:100%;
        margin:0 auto;
      }
  .rl-nav-title p{
    text-align: center;
    font-size: 5.5vmin;
    font-weight: 700;
    color: white;
    line-height: 12vmin;
  }
  /*列表样式*/
  .rl-detail li{
    height: 22.66667vmin;
    box-sizing: border-box;
    padding: 4vmin 3.2vmin 2.66667vmin;
    border-bottom: 1px solid #e6e6e6;
  }
  /*第一个p标签*/
  .rl-detail li span:first-child{
    font-size: 4vmin;
    margin-right: 2.13333vmin;
  }
  .rl-detail li span:nth-child(2){
    font-size: 3.46667vmin;
    color: #999;
    font-weight: 400;
    float: right;
    padding-right: 5.33333vmin;
  }
  .rl-detail p:first-child{
    margin-bottom: 3.06667vmin;
  }
  /*第二个p标签*/
  .rl-result{
    height: 8.8vmin;
  }
  .double-number>div,.happy-number>div,.fucai3D-number div,.line-3-number div,.jiangxi-number div{
    display: inline-block;
    width: 8.26667vmin;
    height: 8.26667vmin;
    line-height: 8.26667vmin;
    border-radius: 50%;
    background: #ff5f5f;
    text-align: center;
    margin-right: 1.6vmin;
    color: white;
  }
  .double-number>div p,.happy-number>div p,
  .fucai3D-number div p,.line-3-number div p,.jiangxi-number div p{
    font-weight: bolder;
    font-size: 4vmin;
    font-family: -apple-system,BlinkMacSystemFont,PingFang SC,Helvetica
    Neue,STHeiti,Microsoft Yahei,Tahoma,Simsun,sans-serif;
  }

  /*双色球*/
  /*超级大乐透*/

  .happy-number>div p{
    font-size: 4vmin;
    font-weight: 700;
  }
  .happy-number>div:nth-child(7),.double-number>div:nth-child(7){
    background-color: #6b8dff;
  }
  .happy-number div:nth-child(6){
    background-color: #6b8dff;
  }
  /*篮球 足球*/
  .football div{
    background-image: url("../../assets/tth-runlottery/download.png");
    background-repeat: no-repeat;
    background-size: contain;
  }
  .basketball div{
    background-image: url("../../assets/tth-runlottery/download-1.png");
    background-repeat: no-repeat;
    background-size: contain;
  }
  .football div,.basketball div{
    height: 8.8vmin;
  }
  #ftResult,#bkResult{
    color: #fff;
    text-align: center;
    line-height: 1;
    display: block;
    max-width: 60vmin;
    padding-left: 8vmin;
    padding-top: 3.46667vmin;
    font-weight: 300;
    width: 58vmin;
    font-size: 3.63333vmin;
    margin-left: 2.93333vmin;
  }
  /*箭头*/
  i{
    float: right;
    margin-top: 8px;
    color:@color-text-gray;
    width: 5.86667vmin;
    height: 5.86667vmin;
  }
  .football i,.basketball i{
    margin-top: -18px;
  }
</style>

