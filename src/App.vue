<script>
import axios from "axios"
export default {
  data() {
    return {
      Names: null,
      inputname: "",
      infoFlg: "",
      Followers: "",
      Followering: "",
      repos:[1,2,3]
    }
  },
  methods: {
    async getgitinfo() {
      const url = `https://api.github.com/users/${this.inputname}`
      axios.get(url)
        .then(Response => {
          this.Names = Response.data
        })
        .catch(err => {
          alert(err.statusText)
        })
      console.log(JSON.stringify(this.Names))
      this.infoFlg = true

    },
    filterdname: () => {
      const filtered = [];
      for (let i in this.Names) {
        const Name = this.Names[i];
        if (Name.Names.indexOf(this.inputname) !== -1) {
          filtered.push(Name);
        }
      }
      return filtered;
    }

  },

  mounted: () => {
    this.getgitinfo();
  },
  computed: {
    filterdUsers: () => {
      return this.filterdname();
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
    <div class="contents">
      <img v-if="infoFlg" class="infoimg" src="https://avatars.githubusercontent.com/u/105579829?v=4">
      <div v-if="infoFlg" class="userinfo">
        <h2>{{ this.inputname }}</h2>
        <ul>
          <li>{{ this.Followers }} followers</li>
          <li>{{ this.Followering }} followering</li>
        </ul>
        <h3>repos</h3>
        <ul class="repositori" v-for="(reponame, index) in repos" :key="index">
          <li>{{reponame}}</li>
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
  top: px;
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
h3{
  text-align: center;
  position: relative;
  bottom: 225px;
  color: #b8c1c1;
  font-size: 40px;
  margin-bottom: 10px;
}
.repositori{
  position: relative;
  text-align: center;
  bottom: 225px;
  list-style: none;
  margin-right: 40px;
}
</style>