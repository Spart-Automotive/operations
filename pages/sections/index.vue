
<template>
  <master
    v-coc-loading = "sections && sections.loading"
    sidebar-starter = "sections">
    <coc-axios
      v-model = "sections"
      method = "get"
      url = "/section" />
    <div>
      <div class = "row coc-padding-x-10px">
        <div class="col coc-house-keeper">
          <span class = "coc-text-title coc-text-bold">Sections</span>
        </div>
        <div class = "col coc-house-keeper right">
          <div class="coc-margin-top-10px">
            <coc-button
              v-if = "sections"
              icon = " knocks-refresh2"
              type = "primary"
              size = "large"
              class = "coc-margin-x-5px"
              circle
              @clicked = "sections.retrieve"/>
            <coc-button
              to = "/sections/add"
              icon = " knocks-plus4"
              type = "primary"
              size = "large"
              circle/>
          </div>
        </div>
      </div>
      <div>
        <div
          v-if = "sections && sections.response && sections.response.sections && sections.response.sections.length"
          class = "row">
          <div
            v-for = "(section, s) in sections.response.sections"
            :key = "s"
            class = "col s12 l4  coc-margin-bottom-5px">
            <div class="col s12 coc-padding-y-5px coc-standard-border-radius coc-primary-background-bg">
              <div class = "col">
                <coc-avatar
                  :source = "`${config.baseURL}/section/${section._id}/image`"
                  class = "coc-margin-top-10px"
                  scale = "40px"
                  @click = "$router.push(`/sections/${section._id}`)" />
              </div>
              <div class = "col">
                <p class = "coc-text-heading coc-text-bold">
                  <nuxt-link
                    :to = "`/sections/${section._id}`"
                    class = "coc-content-text">
                    {{ section.name }}
                  </nuxt-link>
                </p>
                <p class = "coc-text-body ">{{ section.about }}</p>
              </div>
              <div class="col right coc-house-keeper">
                <nuxt-link
                  :to = "`/sections/${section._id}`"
                  class = "right coc-margin-top-10px">
                  <span class = "knocks-dots-three-horizontal circle coc-primary-bg coc-background-text coc-padding-5px" />
                </nuxt-link>
              </div>
            </div>
          </div>
        </div>
        <div
          v-else>
          <p class = "center coc-text-subtitle">
            <span class = "knocks-alert-circle" />
            There is no sections.
          </p>
        </div>
      </div>
    </div>
  </master>
</template>

<script>
import config from '~/config'
import Master from '~/components/common/master'
export default {
  name: 'Index',
  head: {
    title: 'Dashboard | Manage App data'
  },
  components: {
    Master
  },
  data() {
    return {
      sections: null,
      config
    }
  },
  computed: {
    init() {
      return this.$store.state.core.app
    },
    auth() {
      return this.$store.state.core.auth
    },
    user() {
      return this.$store.state.core.auth
    },
    avatarUrl() {
      return this.user ? `${config.baseURL}/app/background` : null
    },
    avatar() {
      return this.init && this.init.background
        ? `${config.baseURL}/app/background`
        : null
    }
  },
  watch: {
    //
  },
  mounted() {
    // get all sections
  },
  methods: {
    //
  }
}
</script>

<style lang="css" scoped>
.home-background-cover{
  background-size: cover !important;
  background-repeat: no-repeat !important;
  background-position: center !important;
}
.coc-browser-container{
  border : 3px solid #546e7a;
  border-radius : 25px;
  min-height : 200px;
  padding: 10px 20px 0px 20px;
  background-color : #FBF9F5;
}
.coc-browser-screen{
  border : 3px solid #455a64;
  border-radius : 5px;
  min-height : 200px;
  padding : 0px;
  margin : 0px;
  background-color : #FFF;
}
#coc-browser-main-screen{
  max-height : 600px;
  overflow-y : auto
}
.coc-browser-dot-small{
  height : 20px;
  width : 20px;
  border-radius : 50%;
}
.ui.button {
  font-weight : 400; 
  font-family : titillium
}
.coc-eventbrite:before{
  color : #fff;
}
.demo-box-container{
  width: 98vw;
  position: absolute;
  left: 1vw;
  z-index: 100;
}
</style>
