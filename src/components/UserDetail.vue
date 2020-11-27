<template>
  <div id="user_data" class="row" v-if="info">
    <div class="col-ls-3 col-md-3 col-sm-3">
      <img :src="info.avatar_url" class="avatar"/>
      <div class="text-left user-detail">
        <p class="name"><a v-bind:href="info.html_url">{{ info.name }}</a></p>
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
</template>

<script>
  export default {
    name: 'user_data',
    props: {
      info: Object,
      repos:Array
    },
    mounted(){
      console.log(this.info)
      console.log(this.repos)  
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
  .name a{
    color:#2f2f2f;
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