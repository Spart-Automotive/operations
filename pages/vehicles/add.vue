
<template>
  <master sidebar-starter = "vehicles">
    <div class = "row coc-house-keeper">
      <div class="col coc-house-keeper">
        <span class = "coc-text-title coc-text-bold">Add Vehicle</span>
      </div>
    </div>
    <div class="row">
      <coc-input
        v-model = "xdata.name"
        :rules = "{ HasValue: true }"
        :scope = "['add-vehicle']"
        placeholder = "Vehicle Name"
        labeled />
      <coc-select
        v-model = "xdata.keywords"
        :rules = "{ HasValue: true }"
        :scope = "['add-vehicle']"
        placeholder = "Vehicle Keywords"
        multiple
        allow-create
        filterable
        labeled />
      <coc-select
        v-model = "xdata.sections"
        :rules = "{ }"
        :scope = "['add-vehicle']"
        :autocomplete-remote = "model => ({ url: '/section', method: 'get', xdata: { name: model && model.meta ? model.meta.query: '' } })"
        :autocomplete-map-response = "res => res.sections.map(s => s.name)"
        placeholder = "Vehicle Sections"
        multiple
        filterable
        allow-autocomplete
        labeled />
      <coc-input
        v-model = "xdata.about"
        :rules = "{ HasValue: false }"
        :scope = "['add-vehicle']"
        placeholder = "About Vehicle"
        type = "textarea"
        labeled />
      <coc-button
        :scope = "['add-vehicle']"
        :request = "{ url: '/vehicle', xdata }"
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
      this.$router.push(`/vehicles/${e.response.data._id}`)
    }
  }
}
</script>

<style lang="css" scoped>

</style>
