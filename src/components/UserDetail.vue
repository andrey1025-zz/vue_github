<template>
  <div id="user_data">
    <div class="form-group row" v-if="info">
      <label class="control-label col-sm-3 page-title text-left" for="username"><strong>Github</strong> <i>Search</i></label>
      <div class="col-sm-9 input-group">
        <input type="text" class="form-control" id="username" v-model="username" placeholder="Enter github username" v-on:keyup="enterKey">
        <div class="input-group-append">
          <button class="btn btn-pink" type="button" v-on:click="searchGithubUser">
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
    <div class="row" v-if="!isEmpty">
      <div class="col-ls-3 col-md-3 col-sm-3">
        <a v-bind:href="info.html_url" target="_blank"><img :src="info.avatar_url" class="avatar"/></a>
        <div class="text-left user-detail">
          <p class="name">{{ info.name }}</p>
          <p class="username">{{ info.login }}</p>
          <p class="extra"><i class="fa fa-empire"></i><span>{{ info.bio }}</span></p>
          <p class="extra"><i class="fa fa-dribbble	"></i><span>{{ info.company }}</span></p>
          <p class="extra"><i class="fa fa-star-o"></i><span>{{ info.following }}</span></p>
          <p class="extra"><i class="fa fa-cube"></i><span>{{ info.public_repos }}</span></p>
          <p class="extra"><i class="fa fa-users"></i><span>{{ info.followers }}</span></p>
        </div>
      </div>
      <div class="col-ls-9 col-md-9 col-sm-9">
        <div v-for="repo in repos" :key="repo.id" class="repo text-left">
          <a v-bind:href="repo.clone_url" class="pink">{{ repo.name }}</a>
          <p>{{ repo.description != null ? repo.description : "No description"}}</p>
          <p><i class="fa fa-star-o"></i> {{ repo.watchers_count }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'user_data',
    props: {
      username:String,
    },
    data(){
      return {
        info:{},
        repos : [],
        isEmpty : false,
        showUserDetail : false,
        searchStart : false
      }
    },
    mounted(){
      this.searchGithubUser()
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
            repos.sort((a, b) => parseInt(b.watchers_count) - parseInt(a.watchers_count));
            this.repos = repos
            this.showUserDetail = true
            this.isEmpty = false
            this.searchStart = true;
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

<style scoped>
  .avatar{
    width:100%;
  }
  .user-detail p{
    margin:5px 0px;
  }
  .name{
    font-size: 25px;
    font-weight: 500;
  }
  .username{
    font-weight: 300;
    margin-bottom: 20px;
  }
  .extra i{
    font-size: 20px;
    margin-right: 10px;
  }
  .repo{
    margin-bottom: 15px;
  }
  .repo p{
    margin-bottom: 5px;
  }
  .repo a{
    font-size: 20px;
  }
</style>