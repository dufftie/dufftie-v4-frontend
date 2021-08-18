<template>
  <div>
    <UserProfile :user="user"/>
  </div>
</template>
<script>
export default {
  props: ['username'],
  data() {
    return{
      user: {
        name: 'Error 404',
        surname: '',
        location: 'Neverland',
        profileImage: 'death.png',
        email: '',
        links: '',
        description: 'User ' + this.username + ' not found.'
      },
    }
  }
}
</script>
