<template>
  <div :class="['layout', { print: onPrint }]">
    <coc-watch-my-window v-model = "win" />
    <Modal
      v-model = "authModal"
      class = "coc-background-bg"
      footer-hide>
      <div class = "coc-margin-top-30px coc-margin-bottom-0">
        <h3 class="text-title coc-primary-text center">
          {{ authMode === 'login' ? 'Login' : 'Register' }}
        </h3>
        <transition
          enter-active-class = "animated zoomIn"
          leave-active-class = "animated zoomOut">
          <login
            v-if = "authMode === 'login'"
            @success = "authModal = false"/>
        </transition>
        <transition
          enter-active-class = "animated zoomIn"
          leave-active-class = "animated zoomOut">
          <register
            v-if = "authMode === 'register'"
            class = "coc-margin-bottom-20px"
            @success = "authModal = false"/>
        </transition>
        <div class="row">
          <a
            class = "coc-padding-left-0 no-outline right"
            @click = "authMode = authMode === 'login' ? 'register' : 'login'">
            {{ authMode === 'login' ? 'Create an account' : 'Already have an account? Login' }}
          </a>
        </div>
      </div>
    </Modal>
    <Layout>
      <Layout>
        <Sider 
          ref="side1" 
          v-model="isCollapsed"
          :collapsed-width="win && win.isSmall ? 0 : 78" 
          :class = "[{hidden: onPrint}, { responsivesider: win && win.isSmall && !isCollapsed }]"
          class = "coc-primary-background-bg coc-border-0 coc-border-right-1 coc-border-border"
          hide-trigger 
          collapsible>
          <div
            v-if = "win && win.isSmall && !isCollapsed"
            class = "coc-primary-text coc-padding-y-10px coc-padding-x-5px"
            @click = "isCollapsed = !isCollapsed">
            <i
              class="ivu-icon ivu-icon-ios-menu" />
            <span>Hide Menu</span>
          </div>
          <Menu 
            :class="menuitemClasses"
            :active-name="sidebarActive" 
            theme="light" 
            width="auto"
            class = "coc-primary-background-bg coc-content-text" 
            @on-select = "handleSidebarSelect">
            <!-- menu items to go -->
            <menu-item
              v-if = "$utils.roles.hasRole('jobs',user)"
              name="sections">
              <i class="knocks-flow-tree" />
              <span>Sections</span>
            </menu-item>
            <menu-item
              v-if = "$utils.roles.hasRole('jobs',user)"
              name="vehicles">
              <i class="knocks-car3" />
              <span>Vehicles</span>
            </menu-item>
            <menu-item
              v-if = "$utils.roles.hasRole('jobs',user)"
              name="brands">
              <i class="knocksapp-shine" />
              <span>Brands</span>
            </menu-item>
          </Menu>
        </Sider>
        <Layout>
          <Header :class = "[{hidden: onPrint}]">
            <Menu 
              mode="horizontal" 
              theme="dark" 
              active-name="1">
              <div class="layout-logo">
                <div class="row">
                  <Icon 
                    :class="rotateIcon" 
                    class = "coc-secondary-text col" 
                    type="ios-menu" 
                    size="33" 
                    @click.native="collapsedSider"/>
                  <nuxt-link to = "/">
                    <coc-avatar
                      v-coc-mouse-over="'pulse'"
                      v-coc-mouse-leave="'jello'"
                      :source="$coc.App.logo.primary"
                      scale = "40px"
                      class="logo col coc-padding-0 animated"/>
                  </nuxt-link>
                </div>
              </div>
              <div class="layout-nav">
                <!--to go -->
                <menu-item
                  name="4"
                  class = "right">
                  <span 
                    v-if = "!user" 
                    class = " coc-text-normal-1 ivu-icon ivu-icon-ios-log-in hide-on-med-and-up" 
                    @click = "activateAuthModal" />
                  <span 
                    v-if = "!user" 
                    class = " coc-text-normal-1 hide-on-med-and-down" 
                    @click = "activateAuthModal"> Login </span>
                  <Dropdown v-else>
                    <Button
                      type="text" 
                      size = "large"
                      class="client-navbar-action coc-primary-hover-tint-9-bg coc-padding-x-10px">
                      <span class="coc-primary-text coc-text-normal-1"/>
                      <Avatar 
                        :icon="!user.avatar ? 'ios-person' : null"
                        :src="avatar" 
                        size="large"
                        class = "coc-primary-tint-9-bg coc-primary-tint-7-text coc-margin-y-0" />
                    </Button>
                    <DropdownMenu slot="list">
                      <DropdownItem
                        class = "coc-border-0 coc-border-bottom-1 coc-border-border"
                        style = "min-width: 150px">
                        <nuxt-link
                          class = "coc-primary-shade-2-text coc-primary-hover-shade-4-text coc-text-normal-1 text coc-text-hover-normal-2 coc-smooth-font-size full-width block"
                          to = "/profile">
                          <icon type = " knocks-user2" />
                          Profile
                        </nuxt-link>
                      </DropdownItem>
                      <DropdownItem
                        v-if = "$utils.roles.dashboarder(user)"
                        class = "coc-border-0 coc-border-bottom-1 coc-border-border">
                        <nuxt-link
                          class = "coc-primary-shade-2-text coc-primary-hover-shade-4-text coc-text-normal-1 text coc-text-hover-normal-2 coc-smooth-font-size  full-width block"
                          to = "/users">
                          <icon type = " knocks-users" />
                          Manage Users
                        </nuxt-link>
                      </DropdownItem>
                      <DropdownItem
                        v-if = "$utils.roles.dashboarder(user)"
                        class = "coc-border-0 coc-border-bottom-1 coc-border-border">
                        <nuxt-link
                          class = "coc-primary-shade-2-text coc-primary-hover-shade-4-text coc-text-normal-1 text coc-text-hover-normal-2 coc-smooth-font-size  full-width block"
                          to = "/app">
                          <icon 
                            type = " knocks-cogs" />
                          App Configurations
                        </nuxt-link>
                      </DropdownItem>
                      <DropdownItem style = "min-width: 150px">
                        <a
                          class = "coc-primary-shade-2-text coc-primary-hover-shade-4-text coc-text-normal-1 text coc-text-hover-normal-2 coc-smooth-font-size  full-width block"
                          @click = "logout">
                          <icon type = " knocks-log-out" />
                          Logout
                        </a>
                      </DropdownItem>
                    </DropdownMenu>
                  </Dropdown>
                </menu-item>
              </div>
            </Menu>
          </Header>
          <Content
            :style="{padding: '0'}"
            :class = "[
              { 'white content-print': onPrint },
              { 'black-text': onPrint },
              { 'coc-primary-background-bg': win && !win.isSmall },
              { 'coc-background-bg': win && win.isSmall },
              'coc-content-text'
          ]">
            <Breadcrumb
              v-if = "!onPrint"
              :style="{margin: '20px 0'}"
              class = "coc-margin-left-10px">
              <BreadcrumbItem 
                v-for = "(crumb, c) in analysisBreadcrump()" 
                :key = "c"
                :to = "`/${ analysisBreadcrump(false).splice( 0, c ).join('/') }`"
                class = "coc-content-text">
                <span class="coc-content-text">{{ crumb | CocCapitalizeName }}</span>
              </BreadcrumbItem>
            </Breadcrumb>
            <Card 
              v-else 
              class = "row coc-margin-top-5px animated fadeIn coc-border-0 coc-border-bottom-2 white black-text coc-none-border-radius">
              <p
                v-if = "$store.state.core.app"
                class="coc-text-lg-1 coc-text-bold coc-margin-y-0 coc-padding-y-0">
                <coc-avatar
                  :source = "$coc.App.logo.primary"
                  scale = "50px"
                  class = "col coc-margin-right-10px coc-margin-y-0 coc-padding-y-0"/>
                {{ $store.state.core.app.title }}
                <small 
                  v-if = "$store.state.core.app.subtitle" 
                  class = "coc-text-thin">{{ $store.state.core.app.subtitle }}</small>
                <small class="right coc-text-body"> {{ $moment().format('D/M/YYYY h:m A') }} </small>
              </p>
              <p
                v-if = "$store.state.core.app"
                class="left">
                <span
                  v-for = "(phone, p) in $store.state.core.app.phones"
                  :key = "p">
                  <span class="coc-text-md-2"><Icon type= "ios-call"/> {{ phone }}</span><br>
                </span>
                <span class="coc-text-md-1"><Icon type= "ios-navigate"/>{{ $store.state.core.app.address }}</span>
              </p>
            </Card>
            <Card 
              :padding = "win && win.isSmall ? 5 : 16"
              :class = "[
                'coc-none-border-radius',
                'coc-border-right-0',
                'coc-border-left-0',
                { white: onPrint },
                { 'black-text coc-border-0': onPrint },
                { 'coc-border-0': win && win.isSmall },
            ]">
              <div style="min-height: 70vh;">
                <slot />
              </div>
            </Card>
          </Content>
          <Footer
            v-if = "!onPrint"
            :class = "[ { white: onPrint }, { 'black-text': onPrint } ]"
            class="layout-footer-center coc-primary-background-bg">
            <span 
              class = "">
              <b style="font-family: jura; font-weight: bold; font-size: 120%">Spart</b>  by
              <icon 
                type = "logo-twitter" 
                class = "blue-text" /> @coc.js 2020&copy; </span>
          </Footer>
          <Footer
            v-else
            :class = "[ { white: onPrint }, { 'black-text': onPrint } ]"
            class="layout-footer-center coc-primary-background-bg print-footer">
            <span 
              class = "">
              <b style="font-family: jura; font-weight: bold; font-size: 120%">Spart</b>  by
              <icon 
                type = "logo-twitter" 
                class = "blue-text" /> @coc.js 2020 &copy; </span>
          </Footer>
        </Layout>
      </Layout>
    </Layout>
  </div>
</template>
<script>
import config from '~/config'
import Login from '../auth/Login.vue'
import Register from '../auth/Register.vue'
export default {
  name: 'Master',
  components: {
    Login,
    Register
  },
  props: {
    crumbs: {
      type: Array,
      default: null
    },
    onPrint: {
      type: Boolean,
      default: false
    },
    sidebarStarter: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      stockData: {
        mode: 'post',
        init: null
      },
      win: {
        isSmall: false
      },
      loaders: { jobs: false },
      events: new this.$coc.Event({ api: this.$root }),
      authMode: 'login',
      authModal: false,
      addStockModal: false,
      isCollapsed: false,
      sidebarActive: 'jobs',
      input: {
        job: {
          search: ''
        }
      }
    }
  },
  computed: {
    user() {
      return this.$store.state.core.auth
    },
    avatar() {
      if (this.user && this.user.avatar && this.user.auth_type !== 'local') {
        return this.user.avatar
      }
      return this.user && this.user.avatar
        ? `${config.baseURL}/users/${this.user._id}/avatar`
        : null
    },
    rotateIcon() {
      return ['menu-icon', this.isCollapsed ? 'rotate-icon' : '']
    },
    menuitemClasses() {
      return ['menu-item', this.isCollapsed ? 'collapsed-menu' : '']
    }
  },
  watch: {
    isCollapsed(e) {
      this.$emit('collapse', e)
      this.$emit(e ? 'sider-on' : 'sider-off')
    },
    onPrint(v) {
      if (process.browser) {
        const body = document.body
        if (v) {
          if (!body.classList.contains('white')) {
            body.classList.add('white')
          }
        } else if (body.classList.contains('white'))
          body.classList.remove('white')
      }
    }
  },
  mounted() {
    if (this.sidebarStarter) this.sidebarActive = this.sidebarStarter
    const vm = this
    this.events.OnMap({
      askForLogin(e) {
        if (e && e.mode && e.mode === 'register') {
          vm.authMode = 'register'
        } else {
          vm.authMode = 'login'
        }
        vm.authModal = true
      },
      updateRunningJobs() {
        vm.getJobs()
      },
      LoggedIn() {
        setTimeout(() => {
          vm.getJobs()
        }, 3000)
      }
    })
    if (this.win && this.win.isSmall) this.isCollapsed = true
  },
  methods: {
    handleSidebarSelect(e) {
      this.sidebarActive = e
      this.isCollapsed = false
      if (e === 'sections') {
        this.$router.push('/sections')
      }
      if (e === 'brands') {
        this.$router.push('/brands')
      }
      if (e === 'vehicles') {
        this.$router.push('/vehicles')
      }
    },
    collapsedSider() {
      this.$refs.side1.toggleCollapse()
    },
    activateAuthModal() {
      this.authModal = true
    },
    handleName(name) {
      if (!name) return ''
      return name.length > 11 ? name.split(' ')[0] : name
    },
    logout() {
      this.$cookies.set('auth', null, {
        path: '/',
        maxAge: 60 * 60 * 24 * 7
      })
      this.$axios.defaults.headers.common['Authorization'] = null
      this.$store.dispatch('setAuth', null)
    },
    analysisBreadcrump(appendDashboard = true) {
      const psudoAppend = []
      if (appendDashboard) psudoAppend.push('Dashboard')
      {
        const parts =
          this.crumbs ||
          this.$route.path.split('/').filter(c => c !== '/' && c.length)
        return [...psudoAppend, ...parts]
      }
    }
  }
}
</script>
<style scoped>
@font-face {
  font-family: 'jura';
  src: url('/fonts/Jura/Jura-Regular.ttf');
  font-weight: 250;
  font-style: normal;
}
@font-face {
  font-family: 'jura';
  src: url('/fonts/Jura/Jura-Bold.ttf');
  font-weight: bold;
  font-style: normal;
}
.layout {
  position: relative;
  border-radius: 4px;
  overflow: hidden;
}
.layout.print {
  background-color: white !important;
  min-height: 100vh;
  margin: 0 !important;
  border-radius: 0 !important;
  border: 0px solid #fff !important;
}
.layout-header-bar {
  background: #fff;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
}
.layout-logo-left {
  width: 90%;
  height: 30px;
  border-radius: 3px;
  margin: 15px auto;
}
.menu-icon {
  transition: all 0.3s;
}
.rotate-icon {
  transform: rotate(-90deg);
}
.menu-item span {
  display: inline-block;
  overflow: hidden;
  width: 69px;
  text-overflow: ellipsis;
  white-space: nowrap;
  vertical-align: bottom;
  transition: width 0.2s ease 0.2s;
}
.menu-item i {
  transform: translateX(0px);
  transition: font-size 0.2s ease, transform 0.2s ease;
  vertical-align: middle;
  font-size: 23px;
}
.collapsed-menu span {
  width: 0px;
  transition: width 0.2s ease;
}
.collapsed-menu i {
  transform: translateX(5px);
  transition: font-size 0.2s ease 0.2s, transform 0.2s ease 0.2s;
  vertical-align: middle;
  font-size: 22px;
}
.layout-logo {
  width: 100px;
  height: 30px;
  background: transparent;
  border-radius: 3px;
  float: left;
  position: relative;
  top: 15px;
  left: 20px;
}
.layout-nav {
  width: 420px;
  margin: 0 auto;
  margin-right: 20px;
}
.layout-footer-center {
  text-align: center;
}
.side-jobs {
  min-height: 50vh;
  max-height: 70vh;
  overflow-y: auto;
}
.show-item-name {
  width: 100px;
  white-space: nowrap;
  overflow: hidden !important;
  text-overflow: ellipsis;
}
.ivu-layout-header,
.ivu-menu-dark {
  background-color: #000;
}
.responsivesider {
  position: fixed;
  z-index: 999;
  min-height: 100vh;
}
.print-footer {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100px;
}
.content-print {
  padding-bottom: 103px !important;
}
.ivu-layout-sider-children {
  min-height: 100vh !important;
}
</style>
