<template>
    <el-container>
      <el-main>
      <div id="selectcardcontainer">
      <div id="selectcardcontainertext">已持有手牌(點擊可以取消選擇)：</div>
      <el-row  type="flex" justify="center" class="row-bg selectcardcontainer" :gutter="20" >
          <el-col  :span="6" v-for="(select_card,index) in select" :key="index"  class="selectcard">
            <el-card :class="cardlist[select_card]" @click.native="removeCard(index)" :value="index" shadow="always"> {{select_card}}</el-card>
        </el-col>
      </el-row>
        <transition name="score" ><span id="score" v-show="scoreshow">結果：{{score}}分</span></transition>
      </div>
      <div id="cardcontainer">
      <el-button  @click="removeAllCard" size="mini">清除所有持有手牌</el-button>
      <br>
      <br>
      <div id="cardcontainertext">手牌選擇區：</div>
      <el-row type="flex" class="row-bg cardcontainer" :gutter="1">
        <el-col :span="12" class="allcard" v-for="card in allcarddata.rows" :key="card.name"  >
            <el-button  :type="card.color"  @click="addCard(card.name)" :value="card.name" round>{{card.name}}</el-button>
          
        </el-col>
        <el-col>Powered by: <a href="mailto:billxu0521@gmail.com">橫隔膜</a> </el-col>
      </el-row>
    </div>
    </el-main>

  </el-container>
</template>

<script>
import CardList from '../card_data_cal.json';

export default {
  name: 'app',
  data () {
    return {
      allcarddata:CardList,
      cardlist:[],
      select:[],
      cardcombine:[],
      score:999,
      scoreshow:false,
      bg1:'bg1.jpg'
    }
  },
  watch: {
    select(){
      this.scoreshow = false
      this.noscoreshow = false
      console.log('新增卡片')
      let score = this.checkCombine()
      this.score = score
      this.scoreshow = true
      /*
      let checkscore = this.getRandom(0,10)
      if(this.select.length > 1 && checkscore > 3){
        //this.checkCombine()
        this.score = this.getRandom(0,999)
        this.scoreshow = true
        this.noscoreshow = false
      }else{
        this.scoreshow = false
        this.noscoreshow = true
      }
      */
    }
  },
  mounted () {
    console.log(this.allcarddata['rows'])
    let _combine = []
    for(let i in this.allcarddata['rows']){
      //console.log(this.cardlist['rows'][i])
      let card_combine = this.allcarddata['rows'][i]
      let card = card_combine['name']
      this.cardlist[card] = card_combine['color']
      _combine[card] = {'ref':this.allcarddata['rows'][i]['ref'],'score':this.allcarddata['rows'][i]['score']}
    }
    this.cardcombine = _combine
    console.log(this.cardlist)
  },
  methods: {
    addCard(value){
      if(this.select.length < 7){
        this.select.push(value)
      }
    },
    removeCard(value){
      if(this.select !== null){
        (this.select).splice(value, 1)
      }
    },
    getRandom(min,max){
      return Math.floor(Math.random()*(max-min+1))+min
    },
    removeAllCard(){
      this.select = []
    },
    checkCombine(){
      let checkedItems = this.select
      let score_cal = 0
      let allcardcombine = this.cardcombine
      for(let i in checkedItems){
        //console.log(checkedItems[i])
        let cardname = checkedItems[i]
        let _cardcombine = allcardcombine[cardname]
        let num = parseInt(allcardcombine[cardname]['score'])
        score_cal += num
        console.log(i+"/卡片: " + cardname + " 得分:" + num)
        let picked_item = [parseInt(i)]
        console.log(picked_item)
        for(let a in _cardcombine['ref']){
          let _ref = _cardcombine['ref'][a]
          for(let x = 0 ; x < checkedItems.length ; ++x) {
            if (picked_item.indexOf(x) > -1) continue;
            if(_ref['name'] === checkedItems[x]) {
              let _ref_num = parseInt(_ref['score'])
              score_cal += _ref_num
              picked_item.push(x)
              console.log(i+"/參考卡片: " + _ref['name'] + " 得分:" + _ref_num)
              break
            }
          }
        }
      }
    console.log(score_cal)
    //this.score = score_cal
    return score_cal
    }

  }

}
</script>

<style>
  body{
    margin:0px;
  }

  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }
  
  .el-aside {
    background-color: #D3DCE6;
    color: #333;
    text-align: center;
    line-height: 200px;
  }
  
  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
    line-height: 16px;
    padding: 0px !important; 
    height: 100vh;
  }

  body > .el-container {
    margin-bottom: 40px;
    height: 100vh;
  }
  
  .selectcardcontainer{
    flex-wrap: wrap;    
    max-width: 90vw;
    height: 35vh;
  }

  #selectcardcontainer{   
    background: url('~@/../src/assets/bg1.jpg') center ;
    background-size: cover;
    padding-bottom: 10px;
    min-height: 45%;
    padding-top: 10px;
  }

  .selectcard{
    margin-top: 5px;
  }

  .cardcontainer{
    overflow: auto;
    flex-wrap: wrap;  
    max-height: 43vh;  
  }

  #cardcontainer{
    background: url('~@/../src/assets/bg2.jpg') ;
    background-size: cover;

  }

  .allcard{
    margin-top:2%;
    margin-bottom:2%;
  }

  #score{
    font-size: 42px;
    color: red;
  }

  .allcard button{
    font-size: 20px !important;
  }
  .allcard{
    margin-bottom: 2%;
  }

  .selectcard button{
    font-size: 28px !important; 
  }

  #selectcardcontainertext{
    font-weight:bold;
    font-size: 24px;
    -webkit-text-stroke: 1px white;
    color: black;
    margin-bottom: 10px;
  }

  #cardcontainertext {
    font-weight:bold;
    font-size: 24px;
    -webkit-text-stroke: 1px white;
    color: black;
    margin-bottom: 10px;
  }

  .el-card{
    font-size: 18px;
  }

  .el-card.danger{
    background:#f78989;
    color: white;
  }

  .el-card.primary{
    background:#409EFF;
    color: white;
  }

  .score-enter-active  {
    transition: opacity .5s;
  }
  .score-enter ,.score-leave-to/* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

  .score-enter-to ,.score-leave/* .fade-leave-active below version 2.1.8 */ {
    opacity: 1;
  }

  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 260px;
  }
  
  .el-container:nth-child(7) .el-aside {
    line-height: 320px;
  }

  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
</style>

