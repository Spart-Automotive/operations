<template>
  <master>
    <div class="row">
      <Card v-if = "!user">
        <p class = "coc-text-title coc-error-text center">
          <Icon type = "ios-alert-outline"/>
          You Are Not Logged In
        </p>
        <br>
        <p class="center">
          <button-group>
            <Button
              type = "default"
              size = "large"
              shape = "circle"
              style = "width: 120px"
              @click = "askForLogin">
              Login
            </Button>
          </button-group>
        </p>
      </Card>
    </div>
  </master>
</template>

<script>
import Master from '~/components/common/master'
export default {
  name: 'HomeIndex',
  components: {
    Master
  },
  head() {
    return {
      title: this.$store.state.core.app
        ? `${this.$store.state.core.app.title} | Home`
        : 'Home'
    }
  },
  data() {
    return {
      //
    }
  },
  computed: {
    user() {
      return this.$store.state.core.auth
    }
  },
  mounted() {
    setTimeout(() => {
      this.isMounted = true
    }, 1000)
  },
  methods: {
    askForLogin() {
      this.$root.$emit('askForLogin')
    }
  }
}
</script>

<style lang="css" scoped>
</style>
