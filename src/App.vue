<script setup>
import {ref} from 'vue'

const showModal=ref(false)
const newNote=ref("")
const notes=ref([])
const errorMessage=ref("")
function randomColor(){
  return "hsl("+Math.random()*360+",100%,75%)"
}
const addNote=()=>{
  if(newNote.value.length<10){

    return errorMessage.value="Note needs to be atleast 10 characters"
  }
  notes.value.push({
    id:Math.floor(Math.random()*100),
    text:newNote.value,
    date:new Date(),
    backgroundColor:randomColor()
  })
  newNote.value=''
  showModal.value=false
  errorMessage.value=''
}
</script>
<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button class="add-note" @click="addNote">Add Note</button>
        <button class="add-close" @click="showModal=false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal=true">+</button>
      </header>
      <div class="cards-container">
        <div class="card"  v-for="note in notes" :key="note.id" :style="{backgroundColor:note.backgroundColor}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{note.date.toLocaleDateString("en-US")}} - {{note.date.toLocaleTimeString("en-US")}}</p>
        </div>
      </div>
    </div>
  </main>
</template>


<style scoped>
main{
  height: 100vh;
  width: 100vw;

}
.container{
  max-width: 1100px;
  padding: 2rem;
  margin: 0 auto;
}
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}
h1{
  font-weight: bold;
  font-size: 4rem;
}
header button{
  background-color: black;
  height: 50px;
  color: white;
  width: 50px;
  border: none;
  border-radius: 100%;
  font-size: 1.2rem;
  padding: 5px;
  cursor: pointer;
}
.card{
  height: 14rem;
  width: 14rem;
  background-color: orange;
  border-radius: 10px;
  margin: 0 1rem 1rem 0;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.date{
  font-size: .7rem;
  font-weight: bold;
}
.cards-container{
  display: flex;
  flex-wrap: wrap;
}
.overlay{
  width: 100%;
  height: 100%;
  background-color: rgb(0,0,0,.70);
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}
.modal{
  width: 750px;
  padding: 2rem;
  border-radius: 1rem;
  position: relative;
  background-color: white;
  display: flex;
  flex-direction: column;
}
.modal button{
  padding: .7rem 1.5rem;
  background-color: blueviolet;
  font-size: 1.5rem;
  margin-top: .5rem;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}
.modal .add-close{
  background-color: red;
}
.modal p{
  color: red;
}
</style>