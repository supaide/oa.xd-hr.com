<template>
<div>
  <Table :columns="columns" :data="data0"></Table>
</div>
</template>
<script>
import StaffBrief from '../../components/StaffBrief'
export default {
  components: {
    StaffBrief
  },
  onSaveData (data) {
    if (data) {
      delete data.columns
    }
    return data
  },
  data () {
    return {
      columns: [
        {
          type: 'expand',
          width: 50,
          render: (h, params) => {
            return h(StaffBrief, {
              props: {
                row: params.row
              }
            })
          }
        },
        {
          title: '#工号',
          key: 'id'
        },
        {
          title: '姓名',
          key: 'name'
        },
        {
          title: '部门',
          key: 'dept'
        },
        {
          title: '入职日期',
          key: 'inDay'
        },
      ],
      data0: [
        {
          id: 1,
          dept: '技术部',
          inDay: '2017-12-28',
          name: '张三',
          age: 18,
          address: 'New York No. 1 Lake Park',
          job: 'Data engineer',
          interest: 'badminton',
          birthday: '1991-05-14',
          book: 'Steve Jobs',
          movie: 'The Prestige',
          music: 'I Cry'
        },
        {
          id: 2,
          dept: '技术部',
          inDay: '2017-12-28',
          name: '李四',
          age: 25,
          address: 'London No. 1 Lake Park',
          job: 'Data Scientist',
          interest: 'volleyball',
          birthday: '1989-03-18',
          book: 'My Struggle',
          movie: 'Roman Holiday',
          music: 'My Heart Will Go On'
        },
        {
          id: 3,
          dept: '技术部',
          inDay: '2017-12-28',
          name: '王五',
          age: 30,
          address: 'Sydney No. 1 Lake Park',
          job: 'Data Product Manager',
          interest: 'tennis',
          birthday: '1992-01-31',
          book: 'Win',
          movie: 'Jobs',
          music: 'Don’t Cry'
        },
        {
          id: 4,
          dept: '技术部',
          inDay: '2017-12-28',
          name: '赵六',
          age: 26,
          address: 'Ottawa No. 2 Lake Park',
          job: 'Data Analyst',
          interest: 'snooker',
          birthday: '1988-7-25',
          book: 'A Dream in Red Mansions',
          movie: 'A Chinese Ghost Story',
          music: 'actor'
        }
      ]
    }
  }  
}
</script>
