<template>
  <div class="body">
    <div class="creator-image-block">
      <div class="image-container" id="image-container">
        <div class="creator-image" id="creator-image" :style="{ backgroundImage: 'url(' + this.profileImage + ')' }">
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
</template>
<script>
export default {
  props: ['user'],
  computed: {
    name () {
      return this.user.name + ' ' + this.user.surname
    },
    links () {
      return this.convertLinks(this.user.links)
    },
    email () {
      return this.user.email
    },
    emailTo () {
      return "mailto:" + this.user.email
    },
    location () {
      return this.user.location
    },
    description () {
      return this.user.description
    },
    profileImage () {
      return require('@/assets/images/' + this.user.profileImage)
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
}
</script>
