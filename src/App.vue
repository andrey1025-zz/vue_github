<template>
  <div id="app">
    <div class="container" v-if="searchStart">
      <user_data :info="info" :repos="repos" :username="username" v-if="showUserDetail"/>
    </div>
    <div class="home-wrapper" v-if="!searchStart">
      <div class="form-group">
        <label class="control-label page-title"><strong>Github</strong> <i>Search</i></label>
        <div class="input-group">
          <input type="text" class="form-control" id="username" placeholder="Enter github username" v-model="username" v-on:keyup="enterKey">
          <div class="input-group-append">
            <button class="btn btn-pink" type="button" v-on:click="start">
              <i class="fa fa-search"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import user_data from './components/UserDetail.vue'

export default {
  name: 'App',
  components: {
    user_data
  },
  data() {
    return {
      username : "",
      info:{},
      repos : [],
      showUserDetail:false,
      searchStart:false
    }
  },
  methods:{
    async start(){
    this.showUserDetail = true
    this.searchStart = true;

    },
    enterKey:function(e){
      if(e.keyCode == 13){
        this.start()
      }
    }
  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.search-result{
  margin:30px 0px;
}
.pink{
  color:#ac53f2;
}
.page-title{
  margin-bottom: 0px;
}
.page-title strong{
  font-size: 30px;
  margin-right: 10px;
}
.page-title i{
  font-size: 30px;
}
.input-group #username{
  height:100%;
}
.input-group-append button{
  padding:0px 30px;
  color:white;
}
.btn-pink{
  background-color: #ac53f2 !important;
}
.home-wrapper{
  width:600px;
  margin:0px auto;
}
</style>
