<template>
  <div>
    <Header />
    <div v-if="$fetchState.fetch">
      <Loading />
    </div>
    <div v-else-if="$fetchState.errno">
      Fuck... Error
    </div>
    <div v-else>
      <div class="body">
        <div class="creator-image-block">
          <div class="image-container" id="image-container">
            <div class="creator-image" id="creator-image" v-bind:style="{ backgroundImage: 'url(' + profileImage + ')' }">
            </div>
          </div>
          <div class="name-description">
            <div class="name">{{ this.name }}</div>
            <div class="description">{{ this.description }}</div>
          </div>
        </div>
        <div class="creator-info-block">
          <div class="location-container">
            <svg id="map-icon" class="map-icon" data-name="map-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 67 100">
              <defs><style>.cls-1{fill:#ff0303;}</style></defs>
              <title>map-tag</title>
              <path class="cls-1" d="M74.5,51A33.5,33.5,0,0,0,41,84.5C41,121,74.5,151,74.5,151S108,120,108,84.5A33.5,33.5,0,0,0,74.5,51Zm0,53A16.5,16.5,0,1,1,91,87.5,16.5,16.5,0,0,1,74.5,104Z" transform="translate(-41 -51)"></path>
            </svg>
            <div class="location">{{ this.location }}</div>
          </div>
          <div class="contact">
            <div class="mail"><a :href="this.emailTo">{{ this.email }}</a></div>
          </div>
          <div class="links" v-for="link in links">
            <div class="link">
              <a :href="link.href">{{ link.title }}</a>
            </div>
          </div>
        </div>
      </div>
      </div>
    </div>
</template>
<script>
import Header from "../../components/Header";
import Loading from "../../components/Loading";
import "~/assets/styles/creator.css";
export default {
  components: {Loading, Header},
  data() {
    return {
      userData: [],
      username: this.$route.params['index']
    }
  },
  head() {
    return {
      'title': this.name
    }
  },
  computed: {
    name () {
      return this.userData.name + ' ' + this.userData.surname
    },
    links () {
      return this.convertLinks(this.userData.links)
    },
    email () {
      return this.userData.email
    },
    emailTo () {
      return "mail:" + this.userData.email
    },
    location () {
      return this.userData.location
    },
    description () {
      return this.userData.description
    },
    profileImage () {
      return require('@/assets/images/' + this.userData.profileImage)
    },
  },
  methods: {
    convertLinks(links) {
      let _linksToReturn = [];
      let _linksArr = links.split(";");
      for (let l=0; l < _linksArr.length; l++)
      {
        let linkInfo = _linksArr[l].split("|");
        let link = {
          title: linkInfo[0],
          href: linkInfo[1],
        };
        _linksToReturn.push(link);
      }
     return _linksToReturn
    },
  },
  async fetch() {
    this.userData = await fetch(
      process.env.apiUrl + '/user/' + this.username,
      {
        headers: {
          'userToken': process.env.apiKey
        }
      }
    ).then(res => res.json())
  }
}
</script>
