<template>
<div class="container-fluid">
     <ul >
         <li v-for="film of films" :key="film.id">
          <div class="product-wrap">
            <div class="product-image" >
              <img :src="film.posterUrl">
              <div class="shadow" ></div>
                <div class="detail-link " @click="moreInfo(film.filmId)" title="Подробнее">
                  <span class="title">Название:</span> <p>{{film.nameRu}}</p>
                  <span class="title">Рейтинг кинопоиска:</span> <p>{{film.rating}}</p>
                  <span class="title">Краткая информация:</span> <p>{{film.description}}</p>
                </div>
            </div>
          </div>
         </li>
     </ul>
<div class="row">
  <div class="col"></div>
   <div class="col">
     <div class="btn-toolbar" role="toolbar" aria-label="Toolbar with button groups">
    <div class="btn-group mr-2" role="group" aria-label="First group">
      <button v-if=" currentPage > 3 " type="button" class="btn" @click="changePage(1)">1</button>
      <button v-if=" currentPage > 4 " type="button" class="btn"> ... </button>
      <button v-if=" currentPage > 2 " type="button" class="btn" @click="changePage(currentPage - 2)">{{currentPage - 2}}</button>
      <button v-if=" currentPage > 1 " type="button" class="btn" @click="changePage(currentPage - 1)">{{currentPage - 1}}</button>
      <button type="button" class="btn currentpage">{{currentPage}}</button>
      <button v-if=" currentPage < numOfPages - 1 " type="button" class="btn" @click="changePage(currentPage + 1)">{{currentPage + 1}}</button>
      <button v-if=" currentPage < numOfPages - 2 " type="button" class="btn" @click="changePage(currentPage + 2)">{{currentPage + 2}}</button>
      <button v-if=" currentPage < numOfPages - 3 " type="button" class="btn"> ... </button>
      <button v-if="!(currentPage >= numOfPages)" type="button" class="btn" @click="changePage(numOfPages)">{{numOfPages}}</button>
    </div>
  </div></div>
    <div class="col"></div>
    
  
</div>
  
</div>
</template>



<script>
import axios from 'axios';

export default {
    name: 'Films',
    data:function(){
        return{
            films: String,
            numOfPages: '',
            currentPage: 1,
        }
    },
    created: function () {
      axios.get('https://kinopoiskapiunofficial.tech//api/v2.1/films/search-by-keyword', 
      {
      headers: {'X-API-KEY':'27b18e18-799e-4f9a-bdcc-bc8e6a3348a6'},
      params: { keyword: this.$route.params.name}
      })
        .then(response =>  {
          console.log(response.data)
          this.films = response.data.films
          this.numOfPages = response.data.pagesCount
        })
        .catch(e => console.log(e))
        .finally(() => console.log('Data loading complete'))
    },
    methods:{
    changePage(page){
      axios.get('https://kinopoiskapiunofficial.tech//api/v2.1/films/search-by-keyword', 
      {
      headers: {'X-API-KEY':'27b18e18-799e-4f9a-bdcc-bc8e6a3348a6'},
      params: { keyword: this.$route.params.name, page: page}
      })
        .then(response =>  {
          console.log(response.data)
          this.films = response.data.films
          this.currentPage = page
        })
        .catch(e => console.log(e))
        .finally(() => console.log('Data loading complete'))
    },
    moreInfo(filmID){
          this.$router.push({ name:"Film", params: { id: filmID } }).catch(() => {})
    }
    },
    watch: {
    '$route.params.data'(){
        this.films = this.$route.params.data.films
        this.numOfPages = this.$route.params.data.pagesCount
    }
    }
}
</script>

<style scoped>

.product-wrap {
  width: 200px;
  text-align: left;
  margin: 0 auto;
  background: white;
}
.product-image {
  position: relative;
  overflow: hidden;
}
.product-image a {display: block;}
.product-image img {
  display: block;
  width: 100%;
}
.shadow {
  background: rgba(0, 0, 0, 0.74);
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: .3s ease-in-out;
}
.product-wrap:hover .shadow {opacity: 1;}
.detail-link {
  opacity: 0;
  color:  white;
  position: absolute;
  left: 50%;
  top: 100%;
  width: 100%;
  transform: translate(-50%,-50%);
  transition: .3s ease-in-out;
}
.detail-link:before {
  color: white;
  font-size: 20px;
  display: block;
  height: 100%;
  line-height: -50px;
  width: 100%;
  transition: .3s ease-in-out;
}
.product-wrap:hover .detail-link {
  opacity: 1;
  top: 50%;
}

.title{
padding-right: 5px;
padding-left: 5px ;
color: white;
background: rgba(89, 0, 172, 0.795);
border: none;
border-radius: 20px;
}

span ~ p {
  color: rgb(233, 233, 233);
  padding-left: 5px ;
}

.container-fluid{
  padding-top: 50px;
}

li{
  display: inline-block;
  margin: 0 10px;
}

.currentpage{
font-weight: bold; 

font-family: 'Montserrat', sans-serif; 
background: #8301ceb7
}

button {
  outline: none;
  display: flex;
  margin: 20px 7px 0px 0px;
  padding: 10px 15px 10px 15px;
  border: 2px solid #5b24c0;
  box-shadow: 0px 0px 40px rgba(143, 1, 199, 0.644);
  color: #ffffff;
  font-family: 'Montserrat', sans-serif; 
}

button:hover{
color: white;
background: rgb(55, 0, 78);
font-family: 'Montserrat', sans-serif; 
font-weight: bold;
}

</style>
