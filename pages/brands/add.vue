
<template>
  <master sidebar-starter = "brands">
    <div class = "row coc-house-keeper">
      <div class="col coc-house-keeper">
        <span class = "coc-text-title coc-text-bold">Add Brand</span>
      </div>
    </div>
    <div class="row">
      <coc-input
        v-model = "xdata.name"
        :rules = "{ HasValue: true }"
        :scope = "['add-brand']"
        placeholder = "Brand Name"
        labeled />
      <coc-select
        v-model = "xdata.keywords"
        :rules = "{ HasValue: true }"
        :scope = "['add-brand']"
        placeholder = "Brand Keywords"
        multiple
        allow-create
        filterable
        labeled />
      <coc-select
        v-model = "xdata.vehicles"
        :rules = "{ }"
        :scope = "['add-brand']"
        placeholder = "Brand Vehicles"
        multiple
        filterable
        labeled />
      <coc-input
        v-model = "xdata.about"
        :rules = "{ HasValue: false }"
        :scope = "['add-brand']"
        placeholder = "About Brand"
        type = "textarea"
        labeled />
      <coc-button
        :scope = "['add-brand']"
        :request = "{ url: '/brand', xdata }"
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
      this.$router.push(`/brands/${e.response.data._id}`)
    }
  }
}
</script>

<style lang="css" scoped>

</style>
