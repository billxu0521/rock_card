<template>
  
  <div id="app">
    <el-container>
      <el-main>
      已持有手牌(點擊可以取消選擇)：
      <el-row  type="flex" justify="center" class="row-bg selectcardcontainer" :gutter="10">
          <el-col  :span="6" v-for="(select_card,index) in select" :key="index"  class="selectcard">
            <el-card @click.native="removeCard(index)" :value="index" shadow="always"> {{select_card}}</el-card>
        </el-col>
      </el-row>
        <transition name="score" ><p id="score" v-show="scoreshow">結果：{{score}}分</p></transition>
      <br>
      手牌選擇區：
      <el-row type="flex" class="row-bg cardcontainer" :gutter="1">
        <el-col :span="12" class="allcard" v-for="card in allCardData.rows" :key="card.name"  >
            <el-button  @click="addCard(card.name)" :value="card.name" round>{{card.name}}</el-button>
          
        </el-col>
      </el-row>
    </el-main>
  </el-container>
  </div>
</template>

<script>
import CardList from '../card_data.json';

export default {
  name: 'app',
  data () {
    return {
      allCardData:CardList,
      cardlist:[],
      select:[],
      card_combine:[],
      score:999,
      scoreshow:false,
      noscoreshow:true
    }
  },
  watch: {
    select(){
      this.scoreshow = false
      this.noscoreshow = false
      console.log('新增卡片')
      //this.checkCombine()
      let checkscore = this.getRandom(0,10)
      if(this.select.length > 1 && checkscore > 3){
        this.score = this.getRandom(0,999)
        this.scoreshow = true
        this.noscoreshow = false
      }else{
        this.scoreshow = false
        this.noscoreshow = true
      }
      
    }
   
  },
  mounted () {
    console.log(this.allCardData)
    for(let i in this.allCardData['rows']){
      //console.log(this.cardlist['rows'][i])
      let card_combine = this.allCardData['rows'][i]
      let card = card_combine['name']
      this.cardlist.push(card)
      let _c = []
      //console.log(card_combine['name'])
      if(card_combine['name'] !== ''){
        _c.push({'card':card_combine['name'],'score':card_combine['score']})
      }
      if(card_combine['name2'] !== ''){
        _c.push({'card':card_combine['name2'],'score':card_combine['score2']})
      }
      if(card_combine['name3'] !== ''){
        _c.push({'card':card_combine['name3'],'score':card_combine['score3']})
      }
      if(card_combine['name4'] !== ''){
        _c.push({'card':card_combine['name3'],'score':card_combine['score4']})
      }
      this.card_combine.push(_c)
    }
    //console.log(this.card_combine)
    //console.log(this.cardlist)
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
      return Math.floor(Math.random()*(max-min+1))+min;
    },
    checkCombine(){
      if(this.select.length > 1){
        for(let i in this.select){
          console.log(this.select[i])
          //let check_combine = []
          //檢查組合
          //let current_combine = []
          //let current_combine = []
          for(let a in this.card_combine){
            let _c = this.card_combine[a] //每組組合
            
            for(let x in _c){
              console.log(_c[x].card)
              
            }
            //如果check_combine > 2 才會算分
            
          }


        }
      }
    }
  }

}
</script>

<style>
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
  }

  body > .el-container {
    margin-bottom: 40px;
  }
  
  .selectcardcontainer{
    flex-wrap: wrap;    
    max-width: 90vw;
    min-height: 10vh;
    margin-bottom: 40px;
  }

  .selectcard{
    margin-top: 10px;
  }

  .cardcontainer{
    overflow: auto;
    flex-wrap: wrap;    
    max-height: 60vh;
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

