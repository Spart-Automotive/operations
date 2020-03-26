
<template>
  <master
    :crumbs = "['sections', init ? init.name : 'Loading..']"
    sidebar-starter = "sections">
    <div
      class="coc-container">
      <div
        v-if = "init"
        class = "col s12 coc-border-0">
        <h1 class = "coc-content-text coc-text-title">{{ init.name }}</h1>
        <coc-input
          v-model = "appForm.name"
          :rules = "{ HasValue: true }"
          :scope = "['edit-section']"
          placeholder = "Name"
          size = "large"
          icon = "ios-card"
          light-model
          labeled/>
        <coc-input
          v-model = "appForm.about"
          :rules = "{ HasValue: true }"
          :scope = "['edit-section']"
          placeholder = "About"
          size = "large"
          icon = "ios-card-outline"
          light-model
          labeled/>
        <coc-select
          v-model = "appForm.vehicles"
          :rules = "{ }"
          :scope = "['edit-section']"
          placeholder = "Section Vehicles"
          multiple
          filterable
          labeled />
        <coc-select
          v-model = "appForm.keywords"
          :rules = "{ HasValue: true }"
          :scope = "['edit-section']"
          placeholder = "Section Keywords"
          multiple
          allow-create
          filterable
          labeled />
      </div>
      <div
        v-if = "init"
        class = "row">
        <coc-button
          v-if = "init"
          :scope = "['edit-section']"
          :request = "{
            method: 'put',
            xdata: $_.omit(appForm, ['_id', '__v']),
            url: `/section/${$route.params.id}`
          }"
          type = "primary"
          size = "large"
          classes = "right"
          class = "row coc-house-keeper"
          placeholder = "Save"
          icon = "md-checkmark"
          long
          @coc-submit-accepted = "handleUpdateSuccess"
          @coc-validation-refused = "handleValidationRefused"/>
      </div>
      <div class="row">
        <divider class = "coc-border-bg coc-border-border"/>
        <div
          v-if = "init"
          class = "col s12">
          <h3 class = "coc-content-text coc-text-heading">Background</h3>
          <div
            v-if = "user"
            class="container">
            <coc-axios
              v-model = "deleteRetriever"
              :xdata = "{}"
              :url = "avatarUrl"
              prevent-on-mount
              method = "delete"
              @success = "handleDeleteSuccess"/>
            <div class="row">
              <div class="col s12 coc-section">
                <div class="col s12 l6 push-l3">
                  <div class="col s6">
                    <Upload
                      id = "avatar-uploader"
                      ref = "upload"
                      :show-upload-list = "false"
                      :on-change="handleAvatarChange"
                      :before-upload="beforeAvatarUpload"
                      :auto-upload="false"
                      :format="['jpg','jpeg','png']"
                      accept = "image/*"
                      action="#"
                      style="border-radius: 5px !important;"
                      class="avatar-uploader">
                      <img
                        v-if = "!imageUrl.length && avatar"
                        :src="avatar"
                        class="full-width">
                      <img
                        v-else-if = "imageUrl"
                        :src="imageUrl"
                        class="full-width">
                      <coc-avatar
                        v-else
                        :parent-class = "[
                          'row',
                          'house-keeper',
                          'coc-inline-block'
                        ]"
                        :child-classes = "[
                          'coc-primary-background-bg',
                          'coc-border-border',
                          'coc-border-1',
                          'coc-border-text'
                        ]"
                        icon = "ivu-icon ivu-icon-md-images"
                        scale = "178px"
                        border-radius = "standard"/>
                      <p class = "coc-content-text center coc-padding-top-5px coc-text-md-1">
                        <Icon
                          v-if = "imagesFiles && imagesFiles.length"
                          type = "md-create"/>
                        <Icon
                          v-else
                          type = "ios-add-circle-outline"/>
                        Click To 
                        <span
                          v-if = "imagesFiles && imagesFiles.length"
                          class = "animated fadeIn">Change</span>
                        <span
                          v-else
                          class = "animated fadeIn">Select</span>
                      </p>
                    </Upload>
                  </div>
                  <div class = "col s6">

                    <Button
                      :loading = "uploadRetriever.loading"
                      :disabled = "!imagesFiles || !imagesFiles.length"
                      type = "success"
                      size = "large"
                      class = "coc-padding-x-10px coc-margin-bottom-10px"
                      icon = "ios-cloud-upload-outline coc-text-md-2"
                      long
                      @click = "uploadAvatar">
                      <span class="coc-text-md-2">Upload</span>
                    </Button>
                    <Button
                      :loading = "deleteRetriever.loading"
                      :disabled = "!avatar"
                      type = "error"
                      size = "large"
                      class = "coc-padding-x-10px coc-margin-bottom-10px"
                      icon = "ios-trash-outline coc-text-md-2"
                      long
                      @click = "deleteAvatar">
                      <span class="coc-text-md-2">Remove</span>
                    </Button>
                    <Button
                      :loading = "deleteRetriever.loading"
                      :disabled = "!imagesFiles || !imagesFiles.length"
                      type = "warning"
                      size = "large"
                      class = "coc-padding-x-10px"
                      icon = "ios-close-circle-outline coc-text-md-2"
                      long
                      @click = "cancelAvatarSelection">
                      <span class="coc-text-md-2">Cancel</span>
                    </Button>
                  </div>
                </div>
              </div>
            </div>
          </div>
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
      init: null,
      phoneInput: '',
      source: null,
      onPrintDemo: false,
      app: null,
      appForm: { name: '', roles: [] },
      // appForm.background_redirect: { url: '', placeholder: '' },
      upload: null,
      imageUrl: '',
      imagesFiles: null,
      uploadRetriever: { loading: false },
      deleteRetriever: { loading: false },
      uploadFD: null
    }
  },
  computed: {
    auth() {
      return this.$store.state.core.auth
    },
    user() {
      return this.$store.state.core.auth
    },
    avatarUrl() {
      return `${config.baseURL}/section/${this.$route.params.id}/image`
    },
    avatar() {
      return this.init && this.init.image
        ? `${config.baseURL}/section/${this.$route.params.id}/image`
        : null
    }
  },
  watch: {
    //
  },
  mounted() {
    //
    this.getInit()
  },
  methods: {
    getInit() {
      this.$axios.get(`/section/${this.$route.params.id}`).then(res => {
        this.init = res.data
        this.appForm = this.$_.cloneDeep(this.init)
      })
    },
    handleUpdateSuccess({ meta: { response } }) {
      this.$store.dispatch('setApp', response)
    },
    handleAvatarChange(res, file) {
      // console.log(file);
      this.imagesFiles = file
      this.imageUrl = URL.createObjectURL(file[0])
    },
    beforeAvatarUpload(file) {
      this.$refs.upload.clearFiles()
      const isLt2M = file.size / 1024 / 1024 < 2

      if (!isLt2M) {
        this.$Message.error('Avatar picture size can not exceed 2MB!')
      }
      this.imagesFiles = [file]
      this.imageUrl = URL.createObjectURL(file)
      return isLt2M
    },
    uploadAvatar() {
      if (this.imagesFiles && this.imagesFiles.length) {
        this.uploadFD = new FormData()
        this.uploadFD.append('file', this.imagesFiles[0])
        this.uploadRetriever.loading = true
        setTimeout(() => {
          this.$axios({
            url: this.avatarUrl,
            method: 'put',
            data: this.uploadFD
          })
            .then(res => {
              this.handleUploadSuccess({ response: res.data })
            })
            .catch(err => {
              this.uploadRetriever.loading = false
              this.$Message.error({
                content: error.response
              })
              this.uploadRetriever.loading = false
            })
        }, 500)
      } else {
        this.$Message.error({
          content: 'No Image Provided'
        })
      }
    },
    deleteAvatar() {
      this.deleteRetriever.retrieve()
    },
    handleUploadSuccess(e) {
      this.uploadRetriever.loading = true
      const data = e.response
      const tempData = this.$_.cloneDeep(data)
      tempData.file = null
      this.$store.dispatch('setApp', tempData)
      setTimeout(() => {
        this.$Message.success({
          content: 'Done'
        })
        this.$store.dispatch('setApp', data)
        this.imageUrl = ''
        this.imagesFiles = null
        this.uploadRetriever.loading = false
      }, 1000)
    },
    cancelAvatarSelection() {
      this.imageUrl = ''
      this.imagesFiles = null
    },
    handleDeleteSuccess(e) {
      this.imageUrl = ''
      this.imagesFiles = null
      this.$Message.success({
        content: 'Hell Yeah'
      })
      this.$store.dispatch('app', e.response)
    },
    handleValidationRefused(e) {
      console.log(e)
    }
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
