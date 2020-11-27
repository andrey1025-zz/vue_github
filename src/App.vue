<template>
  <div id="app">
    <div class="container">
      <div class="form-group row">
        <label class="control-label col-sm-3 page-title text-left" for="username"><strong>Github</strong> <i>Search</i></label>
        <div class="col-sm-9 input-group">
          <input type="text" class="form-control" id="username" placeholder="Enter github username" v-model="username" v-on:keyup="enterKey">
          <div class="input-group-append">
            <button class="btn btn-pink" type="submit">
              <i class="fa fa-search"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="search-result">
        <div class="empty-user" v-if="isEmpty">
          <h1 class="pink">User not found :(</h1>
        </div>
      </div>
      <user_data :info="info" :repos="repos" v-if="showUserDetail && !isEmpty"/>
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
      isEmpty:false,
      showUserDetail:false
    }
  },
  methods:{
    async searchGithubUser(){
      var userInfoUrl = "https://api.github.com/users/" + this.username
      var userReposUrl = "https://api.github.com/users/" + this.username + "/repos"
      try {
        const userInfo = await fetch(userInfoUrl, {
          method: 'GET',
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const data = await userInfo.json()
        if(data.avatar_url){
          this.info = data
          const userRepos = await fetch(userReposUrl, {
            method: 'GET',
            headers: { 'Content-type': 'application/vnd.github.mercy-preview+json; charset=UTF-8' },
          })
          const repos = await userRepos.json()
          this.repos = repos
          this.showUserDetail = true
          this.isEmpty = false
        } else{
          this.isEmpty = true
        }
      } catch (error) {
        console.error(error)
      }
    },
    enterKey:function(e){
      if(e.keyCode == 13){
        this.searchGithubUser()
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
</style>
