<template>
    <div>
      <el-button type="info" round @click="logOut">退出</el-button>
    </div>
</template>

<script>
export default {
  name: 'Home',
  methods: {
    logOut() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>

</style>
