<template>
  <div>
    <div class="total">
      您已经获胜了<p class="red">{{winNum}}次</p>
    </div>
    <div class="compete">
      <div><img :src="'../../images/'+aiIndex+'.png'" alt=""></div>
      <div><img :src="'../../images/'+userIndex+'.png'" alt=""></div>
    </div>
    <p class="result"><span v-if="isStart">{{result}}</span></p>
    <div class="card">
      <p>出拳吧，少年！</p>
      <div class="choose">
        <img src="../../images/1.png" alt="" @click="choose(1)">
        <img src="../../images/2.png" alt="" @click="choose(2)">
        <img src="../../images/3.png" alt="" @click="choose(3)">
      </div>
      <button class="primary" @click="reStart">重新开始</button>
    </div>
  </div>
</template>

<script>
  let timer = '';
  export default {
    data() {
      return {
        winNum: '',
        result: '',
        isStart: false,
        aiIndex: 1,
        userIndex:0
      }
    },

    methods: {
      // 机器人的选择
      setAiChoose() {
        timer = setInterval(()=>{
          if(this.aiIndex>=3) {
            this.aiIndex = 1
          }else{
            this.aiIndex++
          }
        },300)
      },
      // 用户的选择
      choose(index) {
        if(this.isStart) { //如果已经开始 则不可点击
          return
        }
        this.userIndex = index;
        clearInterval(timer);
        this.result = index<this.aiIndex?'您赢了':index===this.aiIndex?'平局':'您输了'
        if(this.aiIndex>=index+2) {
          this.result = '您输了'
        }
        if(this.userIndex>=this.aiIndex+2) {
          this.result = '您赢了'
        }
        this.isStart = true
        this.setWinNum()
      },
      setWinNum() {
       if( this.userIndex<this.aiIndex &&　this.aiIndex==this.userIndex+1 || this.userIndex>=this.aiIndex+2) {
         this.winNum++
         wx.setStorageSync('winNum', this.winNum);
       }
      },
      getWinNum() {
        this.winNum = wx.getStorageSync('winNum')||0
      },
      reStart() {
        this.isStart = false
        this.userIndex = 0
        this.setAiChoose()
      }
    },

    created() {
        this.getWinNum()
        this.setAiChoose()
    }
  }
</script>

<style scoped>
  .total {
    margin-bottom: 18rpx;
    text-align: center;
  }
  .compete{
    display: flex;
    justify-content: space-around;
  }
  .compete div{
    width: 150rpx;
    height: 150rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #eee;
  }
  .compete image{
    width: 120rpx;
    height: 120rpx;
  }

  .result{
    height: 42px;
    line-height: 42px;
    color: red;
    text-align: center;
  }
  .red{
    color: red;
  }
  .card{
    margin: 18rpx;
    padding: 12px;
    background-color: #fff;
    text-align: center;
  }
  .choose{
    display: flex;
    justify-content: space-between;
    margin: 48rpx 0;
  }
  .choose image{
    width: 200rpx;
    height: 200rpx;
  }
  .primary{
    color: #FFF;
    background-color: #87CEEB;
  }
</style>
