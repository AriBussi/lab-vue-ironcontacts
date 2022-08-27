<script setup>
import contactsData from './contacts.json';
import { ref } from 'vue';

const displayedContacts = ref(contactsData.slice(0, 5));
const availableContacts = ref(contactsData.slice(6, contactsData.length));

const addContact = () => {
  const randomInt = Math.floor(Math.random() * availableContacts.value.length);
  const newContact = availableContacts.value[randomInt];
  availableContacts.value.splice(randomInt, 1);
  displayedContacts.value.unshift(newContact);
};

const sortByName = () => {
  displayedContacts.value.sort((a, b) => {
    if(a.name < b.name) { return -1; }
    if(a.name > b.name) { return 1; }
    return 0;
  })
};

const sortByPopularity = () => {
  displayedContacts.value.sort((a, b) => {
    if(a.popularity < b.popularity) { return 1; }
    if(a.popularity > b.popularity) { return -1; }
    return 0;
  })
};

const deleteContact = (id) => {
  const filteredContacts = displayedContacts.value.filter(contact => contact.id !== id);
  displayedContacts.value = filteredContacts;
}

</script>

<template>
  <h1>IronContacts</h1>
  <button @click="addContact">Add Random Contact</button>
  <button @click="sortByPopularity">Sort by popularity</button>
  <button @click="sortByName">Sort by name</button>
  <table>
    <tr>
      <th>Picture</th>
      <th>Name</th>
      <th>Popularity</th>
      <th>Won Oscar</th>
      <th>Won Emmy</th>
    </tr>
    <tr v-for="contact in displayedContacts">
      <td>
        <img :src="contact.pictureUrl" :alt="contact.name + ' profile picture'">
      </td>
      <td>
        {{contact.name}}
      </td>
      <td>
        {{contact.popularity.toFixed(2)}}
      </td>
      <td>
        <img class="award" src="./assets/trophy-solid.svg" alt="Awarded" v-show="contact.wonOscar">
      </td>
      <td>
        <img class="award" src="./assets/trophy-solid.svg" alt="Awarded" v-show="contact.wonEmmy">
      </td>
      <td>
        <button @click="() => deleteContact(contact.id)">Delete</button>
      </td>
    </tr>
  </table>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

img {
  max-width: 80px;
}

table {
  width: 100%;
}

.award {
  max-width: 25px;
}

</style>
