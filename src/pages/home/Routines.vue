<template>
<div>
<Row>
  <Col span="14">
  <Row>
    <Col span="7" >
      <Card class="routine-card">
        <div>
          <h3>入职/离职</h3>
        </div>
      </Card>
    </Col>
    <Col span="6" offset="1">
      <Card class="routine-card">
        <div>
          <h3>薪资结算</h3>
        </div>
      </Card>
    </Col>
    <Col span="6" offset="1">
      <Card class="routine-card">
        <div>
          <h3>绩效管理</h3>
        </div>
      </Card>
    </Col>
  </Row>
  </Col>
  <Col span="10">
  <DatePicker :options="dateOptions" type="date" placeholder="Select date" class="routine-datepicker" :open="true">
    <span></span>
  </DatePicker>
  </Col>
  </Row>
</div>
</template>
<script>
export default {
  title: '日常工作',
  data () {
    return {
      dateOptions: {
        shortcuts: [
          {
            text: '今日任务',
            value () {
              return new Date();
            },
            onClick: (picker) => {
              this.$Message.info('Click today');
            }
          },
        ]
      }
    }
  }
}
</script>
<style lang="less">
.routine-card {
  margin: 0 auto;
  min-height: 100px;
  max-width: 180px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.routine-datepicker {
  width: 200px;
  height: 300px;
  .ivu-select-dropdown {
    margin-top: 2px;
  }
}
</style>
