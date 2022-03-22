<template>
  <form>
    <div id="bgImage">
      <input type="file" name="image" id="eventImage">
    </div>
    <input type="text" placeholder="Nom de l'évènement" name="eventName" id="eventName">
    <div>
      <img src="" alt="">
      <input type="date" placeholder="date" name="date" id="eventDate">
    </div>
    <div>
      <img src="" alt="">
      <input type="text" placeholder="Adresse" name="place" id="eventPlace">
    </div>
    <div>
      <img src="" alt="">
      <input type="number" placeholder="Prix" name="price" id="eventPrice">
    </div>
    <div>
      <img src="" alt="">
      <input type="number" placeholder="Nombre max de participant" name="maxPerson" id="eventMaxPerson">
    </div>
    <div>
      <label for="eventDescription">A propos</label>
      <textarea placeholder="Description" id="eventDescription" name="description">
      </textarea>
    </div>
    <div>
      <p>Catégories</p>
      <div><img src="" alt="Bouton soirées"></div>
      <div><img src="" alt="Bouton Sport"></div>
      <div><img src="" alt="Bouton Business"></div>
      <div><img src="" alt="Bouton Culture"></div>
    </div>
    <input type="submit" value="AJOUTER">
  </form>
</template>

<script>
export default {
  name: "Home"
}
</script>

<style scoped>

</style>