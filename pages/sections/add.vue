
<template>
  <master sidebar-starter = "sections">
    <div class = "row coc-house-keeper">
      <div class="col coc-house-keeper">
        <span class = "coc-text-title coc-text-bold">Add Section</span>
      </div>
    </div>
    <div class="row">
      <coc-input
        v-model = "xdata.name"
        :rules = "{ HasValue: true }"
        :scope = "['add-section']"
        placeholder = "Section Name"
        labeled />
      <coc-select
        v-model = "xdata.keywords"
        :rules = "{ HasValue: true }"
        :scope = "['add-section']"
        placeholder = "Section Keywords"
        multiple
        allow-create
        filterable
        labeled />
      <coc-select
        v-model = "xdata.vehicles"
        :rules = "{ }"
        :scope = "['add-section']"
        placeholder = "Section Vehicles"
        multiple
        filterable
        labeled />
      <coc-input
        v-model = "xdata.about"
        :rules = "{ HasValue: false }"
        :scope = "['add-section']"
        placeholder = "About Section"
        type = "textarea"
        labeled />
      <coc-button
        :scope = "['add-section']"
        :request = "{ url: '/section', xdata }"
        class = "coc-margin-top-5px right"
        placeholder = "Add"
        icon = "md-add"
        type = "primary"
        @coc-submit-accepted = "handleSuccess" />
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
      xdata: {
        name: ''
      }
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
    }
  },
  watch: {
    //
  },
  mounted() {
    //
  },
  methods: {
    handleSuccess(e) {
      this.$router.push(`/sections/${e.response.data._id}`)
    }
  }
}
</script>

<style lang="css" scoped>

</style>
