<template>
<header>
    <h1>Välkommen till BurgerOnline</h1>
       
    </header>
    <main> 
        <section id="burgerinformation">
            <h2>Select burger</h2>
            <p>Välj din burgare</p>
            <div class="wrapper">
                <Burger v-for="(b, index) in burgers" :key="index" :burger="b" v-on:orderedBurger="addToOrder($event)"/>
            </div>
        </section>
        <section id="deliveryinformation">
            <h2>Customer information</h2><br>

            <h3>Delivery information</h3>
            <p>
                <label for="name">Full name</label><br>
                <input type="text" id="name" v-model="fullname" required="required" placeholder="First- and Last name">
            </p>
            <p>
                <label for="email">Email</label><br>
                <input type="email" id="email" v-model="emailname" required="required" placeholder="E-mail address">
            </p>
            <p>
                <label for="street">Street</label><br>
                <input type="text" id="street" v-model="streetname" placeholder="Street name">
            </p>
            <p>
                <label for="house">House</label><br>
                <input type="number" id="house" v-model="housenum" placeholder="House number">
            </p>
            <p>
                <label for="payment">Betalningsmetod</label><br>
                <select id="payment" v-model="paym">
                    <option>Kort</option>
                    <option>Swish</option>
                    <option>Avbetalning</option>
                    <option>Betala vid leverans</option>
                </select>
            </p>
            <p>
                <label for="gender">Välj kön:</label>
                <div>
                    <input type="radio" id="alternativt" value="Vill inte ange" v-model="gen">
                    <label for="alternativt">Vill inte ange</label>
                </div>
                <div>  
                     <input type="radio" id="man" value="Man" v-model="gen">
                     <label for="man">Man</label>
                 </div>
                 <div>
                     <input type="radio" id="kvinna" value="Kvinna" v-model="gen">
                     <label for="kvinna">Kvinna</label>
                 </div>
                 <div>
                     <input type="radio" id="ickebinär" value="Ickebinär" v-model="gen">
                     <label for="ickebinär">Ickebinär</label>
                 </div>
             </p>
                
        </section>
        <div id="locationbox">
            <div id="map" v-on:click="setLocation" >
                click here
                <div id="dot" v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}">
                    T
                </div>
            </div>
        </div>
        <button type="submit">
            <img src="/img/order2.jpg" style="width: 70px;" v-on:click="submitOrder">
            Lägg beställning
         </button>
    </main>
        <footer>
            <hr>
            &copy;
        </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, image, kCal, gluten, lactose, ingredients) {
  this.name = name;
  this.image = image;
  this.kCal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
  this.ingredients = ingredients;
}

let burgerlist = [new MenuItem("Vanlig burgare", "/img/burgare1.webp", 500, true, true, ["Ost", "Kött", "Grönsaker"]), 
                  new MenuItem("Bara kött", "/img/burgarkött.jpg", 300, false, false, ["Kött", "Fryst"]),
                  new MenuItem("Fiskburgare", "/img/fiskburgare.jpg", 400, true, false, ["Fisk", "Grönsaker"])
                ]


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fullname: '',
      emailname: '',
      streetname: '',
      housenum: '',
      paym: 'Kort',
      gen: 'Vill inte ange',

      orderedBurgers: {},
      location: {x: 0, y:0}
    }
  },
  methods: {
    submitOrder: function () {
        console.log("Namn: " + this.fullname + " Email: " + this. emailname + " Address: " +this.streetname + " " + this.housenum + " Betalningsmetod: " +this.paym + " Kön: " + this.gen)
        console.log("Beställda hamburgare: ", this.orderedBurgers)

        socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x - 10,
                                           y: this.location.y - 10},
                                orderItems: this.orderedBurgers,
                                orderContact: {fullName: this.fullname, emailName: this.emailname, streetName: this.streetname,
                                    houseNum: this.housenum, payM: this.paym, gender: this.gen
                                 }
                                
                              }
                 );
        console.log(this.orderedContact)
    },

    addToOrder: function (event) {
        this.orderedBurgers[event.name] = event.amount;
        console.log(this.orderedBurgers)
    },

    setLocation: function (event) {
        this.location = {x: event.clientX - event.currentTarget.getBoundingClientRect().left, 
                        y: event.clientY - event.currentTarget.getBoundingClientRect().top}
    },

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
    addOrder: function (event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
        
      
    }
  }
}
</script>

<style>
  body {
    font-size: 14pt;
    font-family: "Times New Roman";
  }

  #burgerinformation p {
      color: white;
  }

  h1 {
      font-family: 'Agbalumo';
      font-size: 36pt;
  }

  main {
      background-color: bisque;
  }

  header {
      display: block;
      margin: 2vw;
      height: 450px;
      overflow: hidden;
      background-image: url(C:\Users\joelt\Documents\1MD031-lab-2024\public\img\header.webp);
      background-position: center;
      background-size: cover;

  }

  header img {
      width: 100%;
      height: auto;
      opacity: 0.7;
  }

  header h1 {
      display: flex;
      justify-content: center;
      align-items: center;
      width:40rem;
      margin: 0 auto;
      color: white;
      
  }

  .allergen {
      font-weight: bold;
  }

  #burgerinformation {
      color: white;
      background-color: black;
      
  }

  button:hover, input[type="radio"] {
      cursor: pointer;
  }

  .wrapper {
      display: grid;
      grid-gap: 100px;
      grid-template-columns: 300px 300px 300px;
  }

  .box {
      background-color: #444;
      color: #fff;
      border-radius: 5px;
      padding: 20px;
      font-size: 100%;
      width: 340px;
  }


  .wrapper img {
      width: 300px;
      height: 250px;
  }


  button {
      margin: 2vw;
  }

  section {
      padding: 2vw;
      margin: 2vw;
      border: 4px dotted white;
  }

#map {
    position: relative;
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
  }

#locationbox {
    width: 90vw;
    padding: 2vw;
    margin: 2vw;
    overflow: scroll;
}

#dot {
    position: absolute;
    width: 15px;
    height: 15px;
    background: red;
    border-radius: 50%;
    transform: translate(-50%, -50%); 
}
</style>