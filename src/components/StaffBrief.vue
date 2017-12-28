<template>
<Row>
  <Col span="4">姓名: {{row.name}}</Col>
  <Col span="4">生日: {{row.birthday}}</Col>
</Row>
</template>
<script>
export default {
  props: {
    row: Object
  }
}
</script>
