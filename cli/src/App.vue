<template>
<div id="body">
    <div class="container" >
      <form method="post" onkeypress="if(event.keyCode == 13) return false;">
      <input type="text"  placeholder="Название фильма..." v-model="search" @keyup.enter="shareData()" > 
      <button type="button" @click="shareData()"></button>
    </form>
    <router-view></router-view>
  </div>
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data: () =>{
    return{
      search: '',
      info: String, 
    }
  },
  methods:{
    shareData(){
      if( this.search.trim() != ""){
      axios.get('https://kinopoiskapiunofficial.tech//api/v2.1/films/search-by-keyword', 
      {
      headers: {'X-API-KEY':'27b18e18-799e-4f9a-bdcc-bc8e6a3348a6'},
      params: { keyword: this.search}
      })
        .then(response =>  {
          console.log(response.data)
          this.info = response.data
          this.$router.push({ name:"Films", params: {name: this.search, data: this.info} }).catch(() => {})
        })
        .catch(e => console.log(e))
        .finally(() => console.log('Data loading complete'))
      } 
    }
  }
}

</script>

<style scoped>
#body{
    background: url('../public/christmas-dark.png');
    color: white;
   }
.container{
  background: url('../public/55.png');
  width: 1000px;
  height: 3500px;
  padding-top: 10px;
  text-align: center;
  box-shadow: 0px 0px 20px rgba(68, 0, 77, 0.644);
}
form {
  position: relative;
  margin: 0 auto;
  width: 500px;
  background: #331d66;
  border: none;
  border-radius: 20px;
}
input, button {
  border: none;
  outline: none;
  background: transparent;
}
input {
  border-radius: 20px ;
  width: 100%;
  height: 42px;
  padding-left: 15px;
  color: rgb(201, 201, 201);
}
button {
  border-radius: 20px ;
  height: 42px;
  width: 42px;
  position: absolute;
  top: 0;
  right: 0;
  cursor: pointer;
}
button:before {
  content: "\f002";
  font-family: FontAwesome;
  font-size: 16px;
  color: #F9F0DA;
}
input:focus{
  border: 2px solid #b6b6b6;
  transition: all .2s;
}
input:focus::after{
  border:2px solid #b6b6b6;
  transition: all .2s;
}

</style>
