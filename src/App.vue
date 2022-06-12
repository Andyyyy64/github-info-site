<script>
import axios from "axios"
export default {
  data() {
    return {
      Names: null,
      inputname: "",
      infoFlg: "",
      Followers: "",
      Following: "",
      repos: "",
    }
  },
  methods: {
    async getgitinfo() {
      const image = document.getElementById('image')
      const url = `https://api.github.com/users/${this.inputname}`
      axios.get(url)
        .then(Response => {
          this.Names = Response.data
          this.Followers = Response.data.followers
          this.Following = Response.data.following
          image.src = this.Names["avatar_url"]
        })
        .catch(err => {
          alert("ユーザーは存在しません", err.statusText)
        })
      console.log(JSON.stringify(this.Names["avatar_url"]))
      axios.get(`https://api.github.com/users/${this.inputname}/repos`)
        .then(Response => {
          this.repos = Response.data
          const reposName = this.repos.map((_, index) => {
            return {
              repo: Response.data[index].name
            }
          })
          this.repos = reposName
        })
      this.infoFlg = true
      this.$nextTick(function () {
        document.getElementById("typename").focus()
      })
    },
  },
  mounted: () => {
    this.getgitinfo();
  },
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
    <div class="contents">
      <img v-if="infoFlg" class="infoimg" id="image" src="image.src">
      <div v-if="infoFlg" class="userinfo">
        <h2>{{ this.inputname }}</h2>
        <ul>
          <li>{{ this.Followers }} followers</li>
          <li>{{ this.Following }} following</li>
        </ul>
        <h3>repos</h3>
        <ul class="repositori" v-for="(reponame, index) in repos" :key="index">
          {{ reponame }}
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
}

header {
  background-color: #16e0d9;
  color: #fff;
  height: 55px;
}

main {
  background-color: #1f938f;
  height: 90px;
  border-bottom: 3px solid black;
}

.contents {
  background-color: #156766;
  height: 500px;
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
  color: #b8c1c1;
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
  color: #b8c1c1;
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