<template>
  <form @submit.prevent="addEvent" id="form" v-if="access">
    <div id="bgImage">
      <input type="file" name="image" id="eventImage">
    </div>
    <input type="text" placeholder="Nom de l'évènement" name="eventName" id="eventName" v-model="name">
    <div>
      <img src="" alt="">
      <input type="datetime-local" placeholder="date" name="date" id="eventDate" v-model="date">
    </div>
    <div>
      <img src="" alt="">
      <input type="text" placeholder="Adresse" name="place" id="eventPlace" v-model="place">
    </div>
    <div>
      <img src="" alt="">
      <input type="number" placeholder="Prix" name="price" id="eventPrice" v-model="price">
    </div>
    <div>
      <img src="" alt="">
      <input type="number" placeholder="Nombre max de participant" name="maxPerson" id="eventMaxPerson" v-model="person">
    </div>
    <div>
      <label for="eventDescription">A propos</label>
      <textarea placeholder="Description" id="eventDescription" name="description" v-model="description">
      </textarea>
    </div>
    <div>
      <p>Catégories</p>
      <div v-for="item in categories" :id="item.id" @click="this.setCategory(item.id)"><img :src="item.logo" :alt="'Bouton '+ item.name "> {{item.name}}</div>
    </div>
    <input type="submit" value="AJOUTER">
  </form>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data(){
    return{
      jwt:'',
      access: false,
      categories: [],
      name: "",
      description: "",
      price: 0,
      person: 0,
      place: "",
      date: "",
      categoryId: 0,
    }
  },

  created() {
    if (typeof this.$route.query.jwt !== "undefined"){
      this.jwt = this.$route.query.jwt
      this.access = true
      axios.get('http://192.46.237.170:1337/categories').then(r=>{
        this.categories = r.data
      })
    }
    else {
      alert('Vous n\'êtes pas connecté')
    }
  },

  methods:{
    addEvent(){
      const form = document.getElementById('form');
      const img = document.getElementById('eventImage').files;
      const formData = new FormData();

      formData.append('files', img[0])
      if (this.name !== "" && this.description !== "" && this.date !== "" && this.categoryId !== 0 && this.place !== ""){
        axios.post("http://192.46.237.170:1337/upload", formData, {
          headers: {
            Authorization: 'Bearer ' + this.jwt,
          }
        }).then(r=>{
          console.log(r)
          axios.post('http://192.46.237.170:1337/events', {
            "name": this.name,
            "description": this.description,
            "Date_and_time": this.date,
            "price": this.price,
            "place": this.place,
            "ImageLink": r.data[0].url,
            "categories": [
              {
                "id": this.categoryId
              }
            ]

          }, {
            headers: {
              Authorization: 'Bearer ' + this.jwt,
            }
          })
        })
      }
      else {
        alert('Vous devez remplir tout les champs')
      }
    },

    setCategory(id){
      this.categoryId = id
    }
  }
}
</script>

<style scoped>

</style>