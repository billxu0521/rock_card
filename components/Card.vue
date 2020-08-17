<template>
  <div id="app">
    <div v-for="card in cardlist.rows" :key="card" class="lesson">

      <p>{{card.name}}</p>

      
    </div>
  </div>
</template>

<script>
import CardList from '../card_data.json';

export default {
  name: 'app',
  data () {
    return {
      cardlist:CardList,
    }
  },
  watch: {
   
  },
  mounted () {
    console.log(this.cardlist)
  },
  methods: {
    
  }

}
</script>

