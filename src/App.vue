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
      <span class="fa fa-github fa-3x"></span>
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
        <ul class="repositori" v-for="(reponame, index) in repos" :key="index">
          {{ reponame.repo }}
        </ul>
      </div>
    </div>
  </div>
</template>

<style>
* {
  margin: 0 auto;
  font-family: "courier", "Roboto Mono", "Vazirmatn";
  font-weight: bold;
}

.maintitle {
  text-align: center;
  letter-spacing: -0.5px;
  position: relative;
  top: 9px;
}

header {
  background-color: rgb(26, 20, 20);
  color: #fff;
  height: 55px;
}

main {
  background-color: #1f938f;
  height: 90px;
  border-bottom: 3px solid black;
}

.loading {
  background-color: #156766;
  text-align: center;
  font-size: 50px;
  padding-top: 50px;
}

.contents {
  background-color: #156766;
  height: 800px;

}

.NameInfo {
  display: inline-block;
  margin-left: 460px;
  margin-top: 20px;
}

.typename {
  font-size: 1.5em;
  margin-right: 20px;
}

.searchinfo {
  font-size: 1.2em;
  position: relative;
  bottom: 2px;
  cursor: pointer;
}

.mt-10 {
  margin-top: 10px;
}

.mb-10 {
  margin-bottom: 10px;
}

.fa {
  text-align: left;
  position: relative;
  bottom: 48px;
  left: 5px;
}

.infoimg {
  width: 300px;
  position: relative;
  left: 100px;
  top: 30px;
  border: 2px solid rgb(56, 11, 11);
}

h2 {
  text-align: center;
  position: relative;
  bottom: 250px;
  color: #e4efef;
  font-size: 40px;
  margin-bottom: 10px;
}

ul {
  position: relative;
  text-align: center;
  bottom: 250px;
  list-style: none;
  margin-right: 40px;
}

h3 {
  text-align: center;
  position: relative;
  bottom: 225px;
  color: #e4efef;
  font-size: 40px;
  margin-bottom: 10px;
}

.repositori {
  position: relative;
  text-align: center;
  bottom: 225px;
  list-style: none;
  margin-right: 40px;
}
</style>