<template>
  <div class="burgerBox">
    <h3>{{burger.name}}</h3>
      
      <img :src="burger.image" :alt="burger.name" :title="burger.name"/>
      
      <ul id="information">
        <li v-for="x in burger.ingredients">{{ x }}</li>
        <li v-if="burger.gluten" class="allergen">Innehåller gluten</li>
        <li v-if="burger.laktos" class="allergen">Innehåller laktos</li>
        <li>{{ burger.kCal }} kCal</li>
      </ul><br>

      <p>Lägg till i beställning</p>
      <button type="button" v-on:click="changeOrder(-1)">-</button>
      {{ amountOrdered }}
      <button type="button" v-on:click="changeOrder(+1)">+</button>
      
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function (){
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    changeOrder: function (ammount){
      if((this.amountOrdered == 0 && ammount == -1) == false){
        this.amountOrdered += ammount;
      };
      this.$emit('orderedBurger', {name: this.burger.name, amount: this.amountOrdered}); 
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .burgerBox {
    background-color: #444;
      color: #fff;
      border-radius: 5px;
      padding: 20px;
      width: 340px;
  }

  .allergen {
    font-weight: bold;
  }

  #information{
    height: 10vh;
  }
</style>