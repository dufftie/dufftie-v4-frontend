<template>
  <div>
    <div v-if="loading">
      <Loading />
    </div>
    <div v-else-if="error">
      <DeadHeader />
      <EmptyUserProfile :username="username" />
    </div>
    <div v-else>
      <Header />
      <UserProfile :user="user" />
    </div>
  </div>
</template>
<script>
import Header from "../../components/Essentials/Header";
import "~/assets/styles/creator.css";
import EmptyUserProfile from "../../components/User/EmptyUserProfile";
import Loading from "../../components/Essentials/Loading";
import DeadHeader from "../../components/Essentials/DeadHeader";
export default {
  components: {DeadHeader, Loading, EmptyUserProfile, Header},
  data() {
    return {
      username: this.$route.params['id'],
      user: {},
      loading: true,
      error: false
    }
  },
  head() {
    return {
      'title': this.user.name === undefined?
        'User not found | ' + this.username
        : this.user.name + ' ' + this.user.surname + ' | ' + this.username
    }
  },
  async fetch() {
    await fetch(
      process.env.apiUrl + '/user/' + this.username,
      {
        headers: {
          'userToken': process.env.apiKey,
        }
      }).then(res => res.json())
      .then(json => {
        this.user = json
      })
      .catch(() => {
        this.error = true
      })
    .finally(() => {
      this.loading = false
    })
  }
}
</script>
