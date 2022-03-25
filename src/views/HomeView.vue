<template>
  <div>
    <div v-if="!logged" class="d-flex flex-row-reverse p-4">
      <div class="p-2">
        <button type="submit" class="btn btn-outline-primary" @click="login()">Login</button>
      </div>
      <div class="p form-floating">
        <input type="password" v-model="$user.pass" class="form-control" id="password" required>
        <label for="password">password</label>
      </div>
      <div class="p form-floating pl-2">
        <input type="username" v-model="$user.name" class="form-control" id="username" required>
        <label for="username">username</label>
      </div>
    </div>
    <div v-else >
      <div align="right" class="p-4" >
        <router-link to="/about" style="color: #2c3e50;align:right;font-weight: bold;">Movies I like</router-link>
      </div>
    </div>
    <br><br>
    <h2>Welcome! </h2>
    <div class="row g-2 p-4">
      <div class="col-md">
        <div>
          <b-form-input type="search" v-model="text" class="form-control" id="inputSearch"
                        placeholder="Start typing a title..."
                        @keypress="getFromImdb({ text })"
                        @keyup.enter="searching=true; typing=false"
                        @click="typing=true;"/>
        </div>
        <div v-if="text.length >= 3 && typing==true" style="text-align:left; border:lightskyblue; border-width:2px; border-style:solid;">
          <div v-for="obj in objs">
            <p @click="text=obj.title" style="cursor: pointer; padding-left:20px;">{{obj.title}}</p>
          </div>
        </div>
      </div>
      <div class="col-md">
        <div>
            <b-form-select  class="form-select" id="selectCategory" v-model="params.category">
              <b-form-select-option value="0" selected>Both Movies & Series</b-form-select-option>
              <b-form-select-option value="1">Movies Only</b-form-select-option>
              <b-form-select-option value="2">Series Only</b-form-select-option>
            </b-form-select>
        </div>
      </div>
      <div v-if="searching == true">
        <div v-if="objs.length">
          <Card v-for="obj in objs" :obj="obj"></Card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
import Card from '@/components/Card.vue'

export default {
  name: 'HomeView',
  components: {
    Card,
  },
  data() {
    return {
      text:'',
      searching : false,
      logged: false,
      typing: true,
      objs: [],
      url: '',
      params:{
        category: 0
      },
      optionUrls: {
        0: 'SearchTitle',
        1: 'SearchMovie',
        2: 'SearchSeries'
      },
      apiKey: process.env.VUE_APP_API_KEY
    }
  },
  methods: {
    getFromImdb(txt) {
      this.url= `https://imdb-api.com/en/API/${this.optionUrls[this.params.category]}/${this.apiKey}`;

      this.url=[this.url,Object.values(txt)].join('/');
      console.log('Searching for this url: '+ this.url);
      axios.get(this.url).then(response => {
        console.log(response.data.results);
        this.objs = response.data.results;
      }).catch(function (error) {
        console.error(error);
      });
    },
    login(){
      this.logged=true;
      console.log(this.$user.name);
      console.log(this.$user.pass);
    },
  },

}

</script>