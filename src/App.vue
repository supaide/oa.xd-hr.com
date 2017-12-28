<template>
<div id="app" class="layout">
<Layout>
  <Header>
    <Menu mode="horizontal" theme="dark" active-name="1" class="oa-header">
      <div class="layout-logo">云OA</div>
      <!--Icon @click.native="collapsedSider" :class="rotateIcon" :style="{color: '#fff'}" type="navicon-round" size="24"></Icon-->
      <div class="layout-nav">
        <MenuItem name="1">
          <Icon type="ios-navigate"></Icon>
          HR
        </MenuItem>
        <MenuItem name="2">
          <Icon type="ios-keypad"></Icon>
          财务
        </MenuItem>
        <span style="color: #fff;">你好，张三</span>
      </div>
    </Menu>
  </Header>
  <Layout class="ivu-layout-has-sider">
    <left-menu :menus="menus" @on-click="menuClick"></left-menu>
    <Layout :style="{padding: '0 15px', marginTop: (breads.length<1 ? '15px' : null)}">
      <Breadcrumb :style="{margin: '15px 0'}" v-show="breads.length>0">
        <BreadcrumbItem v-for="(bread,index) in breads" :to="bread.path" :key="index">{{bread.title}}</BreadcrumbItem>
      </Breadcrumb>
      <Content :style="{padding: '15px', minHeight: '280px', background: '#fff'}">
        <page-view/>
      </Content>
      <Footer style="padding-left: 0px;" class="layout-footer-center">&copy;2018 <a href="http://www.xd-hr.com" target="_blank">信德睿尚</a> All Rights Reserved. </Footer>
    </Layout>
  </Layout>
</Layout>
</div>
</template>
<script>
import LeftMenu from './components/LeftMenu'
import {event} from 'spd-webutil'
export default {
  components: {
    LeftMenu
  },
  data () {
    return {
      menus: [],
      breads: [],
      isCollapsed: false,
      spanLeft: 5,
      spanRight: 19
    }
  },
  created () {
    window.app = this
    this.menus = [
      {name: 'Home', path: 'home', icon: 'ios-home', childs: [
        {name: 'Dashboard', path: 'home/dashboard'},
        {name: '日常工作', path: 'home/routines'},
        {name: '审批', path: 'home/workflow'},
      ]},
      {name: '公司管理', path: 'enterprise', icon: 'social-buffer', childs: [
        {name: '组织架构', path: 'enterprise/department'},
        {name: '职位/职级', path: 'enterprise/position'},
        {name: '员工管理', path: 'enterprise/staffs'},
      ]},
      {name: '员工之家', path: 'family', icon: 'ios-people', childs: [
        {name: '档案管理', path: 'family/archives'},
        {name: '审批', path: 'family/workflow'},
        {name: '绩效', path: 'family/performance'}
      ]},
      {name: '薪酬福利', path: 'compensation', icon: 'heart', childs: [
        {name: '薪资体系', path: 'compensation/salaryDesign'},
        {name: '五险一金', path: 'compensation/welfare'}
      ]},
      {name: '绩效', path: 'performance', icon: 'flag', childs: [
        {name: '绩效体系', path: 'performance/design'}
      ]},
      {name: '报表', path: 'bi', icon: 'ios-analytics', childs: [
        {name: 'Dashboard', path: 'bi/dashboard'}
      ]}
    ]
    event.on('router-enter', (routers) => {
      let breads = []
      routers.forEach((router) => {
        if (router[1].ctor.title) {
          breads.push({path: router[1].path, title: router[1].ctor.title})
        }
      })
      if (breads.length==1) {
        breads[0].path = ''
      }
      this.breads = breads
    })

  },
  computed: {
    iconSize () {
      return this.spanLeft === 5 ? 14 : 24;
    },
    rotateIcon () {
      return [
        'menu-icon',
        this.isCollapsed ? 'rotate-icon' : ''
      ];
    },
    menuitemClasses () {
      return [
        'menu-item',
        this.isCollapsed ? 'collapsed-menu' : ''
      ]
    }
  },
  methods: {
    toggleClick () {
      if (this.spanLeft === 5) {
        this.spanLeft = 2;
        this.spanRight = 22;
      } else {
        this.spanLeft = 5;
        this.spanRight = 19;
      }
    },
    onSel (name) {
      this.$router.goto(name)
    },
    collapsedSider () {
      console.log('hello')
      this.$refs.side1.toggleCollapse();
    },
    menuClick (payload) {
      console.log(payload)
      this.$router.goto(payload.path)
    }
  }
}
</script>
<style scoped>
.layout{
  border: 1px solid #d7dde4;
  background: #f5f7f9;
  position: relative;
  border-radius: 4px;
  overflow: hidden;
}
.oa-header {
  display: flex;
  justify-items: center;
  align-items: center;
}
.layout-logo {
  color: #fff;
  font-size: 16px;
  width: 150px;
}
.layout-logo1{
  width: 100px;
  height: 30px;
  background: #5b6270;
  border-radius: 3px;
  float: left;
  position: relative;
  top: 15px;
  left: 20px;
}
.layout-nav{
  margin: 0 auto;
  margin-right: 0px;
}

.layout-header-bar{
  background: #fff;
  box-shadow: 0 1px 1px rgba(0,0,0,.1);
}

.layout-logo-left{
  width: 90%;
  height: 30px;
  background: #5b6270;
  border-radius: 3px;
  margin: 15px auto;
}
.menu-icon{
  transition: all .3s;
}
.rotate-icon{
  transform: rotate(-90deg);
}
.menu-item span{
  display: inline-block;
  overflow: hidden;
  width: 69px;
  text-overflow: ellipsis;
  white-space: nowrap;
  vertical-align: bottom;
  transition: width .2s ease .2s;
}
.menu-item i{
  transform: translateX(0px);
  transition: font-size .2s ease, transform .2s ease;
  vertical-align: middle;
  font-size: 16px;
}
.collapsed-menu span{
  width: 0px;
  transition: width .2s ease;
}
.collapsed-menu i{
  transform: translateX(5px);
  transition: font-size .2s ease .2s, transform .2s ease .2s;
  vertical-align: middle;
  font-size: 22px;
}
.collapsed-menu .ivu-menu-submenu-title-icon {
  display: none;
}
</style>
