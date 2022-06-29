<script>
import axios from "axios"
export default {
  data() {
    return {
      Names: "",
      inputname: "",
      Followers: "",
      Following: "",
      repos: "",
      infoFlg: "",
      message: "",
      image: "",
    }
  },
  methods: {
    async getgitinfo() {
      this.message = "Loading..."
      const url = `https://api.github.com/users/${this.inputname}`
      await axios.get(url)
        .then(Response => {
          this.infoFlg = true
          this.Names = Response.data
          this.Followers = Response.data.followers
          this.Following = Response.data.following
          this.image = this.Names["avatar_url"]
        })
        .catch(err => {
          alert("ユーザーが存在しません", err);
        })
      await axios.get(`https://api.github.com/users/${this.inputname}/repos`)
        .then(Response => {
          this.repos = Response.data
          const reposName = this.repos.map((_, index) => {
            return {
              repo: Response.data[index].name
            }
          })
          this.repos = reposName
        })
        .finally(() => {
          this.message = ""
        })
    },
  },
  watch: {
    inputname: function () {
      this.infoFlg = false
    }
  }
}
</script>

<template>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
  <div class="contaier">
    <header>
      <h1 class="maintitle mb-10">Github Infomation Site</h1>
      <a href="https://github.com/Andyyyy64" class="gitjump"><span class="fa fa-github fa-3x"></span></a>
    </header>
    <main>
      <div class="NameInfo">
        <input type="text" class="typename" v-model="inputname" placeholder="Please enter a name">
        <button class="searchinfo" @click="getgitinfo">Search</button>
      </div>
    </main>
    <div class="loading">{{ message }}</div>
    <div class="contents">
      <img v-if="infoFlg" class="infoimg" id="image" :src="this.image">
      <div v-if="infoFlg" class="userinfo">
        <h2>{{ this.inputname }}</h2>
        <ul>
          <li>{{ this.Followers }} followers</li>
          <li>{{ this.Following }} following</li>
        </ul>
        <h3>repos</h3>
        <ul class="repozitori" v-for="(reponame, index) in repos" :key="index">
          {{ reponame.repo }}
        </ul>
      </div>
    </div>
  </div>
</template>

<style>
@import "../css/mainstyle.css"
</style>