<template>
<Sider ref="side1" collapsible :collapsed-width="78" v-model="isCollapsed" hide-trigger :style="{background: background}">
  <Menu :active-name="activedItem" :theme="theme" width="auto" :open-names="parentMenus" :class="menuitemClasses" ref="menu">
    <Submenu v-for="menu in menus" :key="menu.path" :name="menu.path">
      <template slot="title">
        <Icon :type="menu.icon"></Icon>
        <span>{{menu.name}}</span>
      </template>
      <MenuItem v-for="submenu in menu.childs" :key="submenu.path" :name="submenu.path" v-show="!isCollapsed" @click.native="menuClick(submenu.path, submenu.name)">{{submenu.name}}</MenuItem>
    </Submenu>
  </Menu>
</Sider>
</template>
<script>
import {event} from 'spd-webutil'
export default {
  props: {
    background: {
      type: String,
      default: '#fff'
    },
    theme: {
      type: String,
      default: 'light'
    },
    menus: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      parentMenus: [],
      activedItem: null,
      isCollapsed: false,
    }
  },
  created () {
    this.cacheMenuPaths()
    window.menu = this
    event.on('router-enter', (router, params) => {
      if (!this.activedItem) {
        this.activedItem = router[0][1].path
        this.parentMenus = [this.menuPaths[this.activedItem]]
        this.$nextTick(() => {
          this.$refs.menu.updateOpened()
        })
      }
    })
  },
  computed: {
    menuitemClasses () {
      return [
        'menu-item',
        this.isCollapsed ? 'collapsed-menu' : ''
      ]
    }
  },
  methods: {
    cacheMenuPaths () {
      this.menuPaths = {}
      this.menus.forEach((menu) => {
        if (!menu.childs) {
          return
        }
        menu.childs.forEach((submenu) => {
          this.menuPaths[submenu.path] = menu.path
        })
      })
    },
    menuClick (path, name) {
      this.$emit('on-click', {path: path, name: name})
    }
  },

  watch: {
    menus (val) {
      this.cacheMenuPaths()
      this.activedItem = null
    }
  }
}
</script>
