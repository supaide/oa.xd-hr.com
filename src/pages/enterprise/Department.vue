<template>
<Row>
  <Col span="10" class="dept-left">
    <department company="A科技有限公司" @on-select="selDept" ref="department" />
  </Col>
  <Col span="13" offset="1">
    <Card v-if="showDeptSetting">
      <p slot="title"><Icon type="android-settings"></Icon> 部门设置</p>
      <div slot="extra">
        <Button type="primary" :loading="false" size="small" @click.native="saveDept">保存</Button>&nbsp;
        <Button type="ghost" :loading="false" size="small" @click.native="delDept">删除</Button>
      </div>
      <Form :model="formItem" :label-width="80" :rules="formValidate">
        <FormItem label="部门名称" prop="name">
          <Input v-model="formItem.name" placeholder="请填写部门名称"></Input>
        </FormItem>
      </Form>
    </Card>
  </Col>
</Row>
</template>
<script>
import Department from '../../components/Department'
export default {
  title: '组织架构',
  components: {
    Department
  },
  data () {
    return {
      showDeptSetting: false,
      formItem: {
        name: ''
      },
      formValidate: {
        name: [
          { required: true, message: '部门名称不能为空', trigger: 'blur' }
        ],
      }
    }
  },
  onCreate () {
    this.currentDeptRoot = null
    this.currentDeptNode = null
    this.currentDeptData = null
  },
  methods: {
    selDept (data) {
      this.showDeptSetting = !!data
      if (data) {
        this.currentDeptRoot = data[0]
        this.currentDeptNode = data[1]
        this.currentDeptData = data[2]
      } else {
        this.currentDeptRoot = null
        this.currentDeptNode = null
        this.currentDeptData = null
      }
    },
    saveDept () {
    },
    delDept () {
      if (this.currentDeptData) {
        this.$refs.department.remove(this.currentDeptRoot, this.currentDeptNode, this.currentDeptData)
      }
    }
  }
}
</script>
<style scoped>
.dept-left {
  min-height: 300px;
  max-height: 450px;
  overflow-y: auto;
}
</style>
