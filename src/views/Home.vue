<template>
  <div class="container">
    <form @submit.prevent="addEvent" id="form" v-if="access">
      <div id="bgImage">
        <input type="file" name="file" id="eventImage" accept="image/*">
        <label for="eventImage">Ajouter une image</label>
      </div>
      <div class="form-container">
        <input type="text" placeholder="Nom de l'évènement" name="eventName" id="eventName" v-model="name">
        <div>
          <img src="../assets/calendar.png" alt="calendar icon">
          <input type="datetime-local" placeholder="date" name="date" id="eventDate" v-model="date">
        </div>
        <div>
          <img src="../assets/location.png" alt="location icon">
          <input type="text" placeholder="Adresse" name="place" id="eventPlace" v-model="place">
        </div>
        <div>
          <img src="../assets/location.png" alt="location icon">
          <input type="text" placeholder="Nom du lieu" name="place" id="eventPlaceName" v-model="placeName">
        </div>
        <div>
          <img src="../assets/cash.png" alt="cash icon">
          <input type="number" placeholder="Prix" name="price" id="eventPrice" v-model="price">
        </div>
        <div>
          <img src="../assets/people.png" alt="people icon">
          <input type="number" placeholder="Nombre max de participant" name="maxPerson" id="eventMaxPerson"
                 v-model="person">
        </div>
        <div class="test">
          <label for="eventDescription">A propos</label>
          <textarea placeholder="Description" id="eventDescription" name="description" v-model="description">
          </textarea>
        </div>
        <input type="submit" value="AJOUTER">
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      jwt: '',
      access: false,
      categories: [],
      name: "",
      description: "",
      price: 0,
      person: 0,
      place: "",
      placeName: "",
      date: "",
      categoryId: 0,
    }
  },

  created() {
    if (typeof this.$route.query.jwt !== "undefined") {
      this.jwt = this.$route.query.jwt
      this.access = true
    } else {
      alert('Vous n\'êtes pas connecté')
    }
  },

  methods: {
    addEvent() {
      const form = document.getElementById('form');
      const img = document.getElementById('eventImage').files;
      const formData = new FormData();

      formData.append('file', img[0])
      formData.append('name', this.name)
      formData.append('description', this.description)
      formData.append('date', this.date)
      formData.append('price', this.price)
      formData.append('place', this.person)
      formData.append('adresse', this.place)
      formData.append('namePlace', this.placeName)
      if (this.name !== "" && this.description !== "" && this.date !== "" && this.place !== "") {
          axios.post('https://127.0.0.1:8000/api/events', formData, {
            headers: {
              Authorization: 'Bearer ' + this.jwt,
            }
          })
      } else {
        alert('Vous devez remplir tout les champs')
      }
    },
  }
}
</script>

<style scoped>
* {
  font-family: 'Open Sans', sans-serif;
}

.container {
  max-width: 900px;
  height: 100vh;
  overflow-x: hidden;
  position: relative;
  background-color: #FAFAFA;
  margin: 0 auto;
}

#bgImage {
  background: #414BCD;
  height: 212px;
  width: 100%;
  position: relative;
}

#eventImage {
  display: none;
}

#bgImage label {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 16px;
  font-weight: 600;
  color: white;
  border: solid 3px white;
  border-radius: 10px;
  padding: 9px 14px;
}

.form-container {
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  padding: 24px;
  position: absolute;
  top: 190px;
  left: 0;
  width: 100%;
  z-index: 99;
  background: #FAFAFA;
}

.form-container input {
  background: white;
  height: 36px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  box-shadow: 0px 4px 8px hsla(0, 0%, 0%, 0.06);
  border: none;
  width: 100%;
  padding: 13px;
  font-size: 14px;
  color: #9798A6;
}

.form-container > div {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 18px;
}

#eventName {
  margin-bottom: 18px;
}

.test {
  flex-direction: column;
  align-items: flex-start !important;
}

.test label {
  font-weight: bold;
  font-size: 16px;
  margin-top: 20px;
  margin-bottom: 13px;
}

.test textarea {
  background: white;
  height: 85px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  box-shadow: 0px 4px 8px hsla(0, 0%, 0%, 0.06);
  border: none;
  width: 100%;
  padding: 13px;
  font-size: 14px;
  color: #9798A6;
}

input[type="submit"] {
  background: #414BCD;
  height: 43px;
  border-radius: 54px;
  text-transform: uppercase;
  color: white;
  font-weight: 600;
  font-size: 16px;
}

textarea {
  min-width: 100%;
  max-width: 100%;
}

.categoriesContainer.categoriesContainer {
  flex-direction: column;
  align-items: start;
}

.categoriesContainer > div{
  display: flex;
  gap: 2rem;
  padding: 10px;
  width: 90vw;
  overflow-x: scroll;
}

</style>