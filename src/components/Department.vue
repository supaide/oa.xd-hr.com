<template>
  <Tree :data="data" :render="renderContent" style="overflow-x:hidden;" />
</template>
<script>
export default {
  props: {
    company: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      id: 1,
      data: [],
      buttonProps: {
        type: 'ghost',
        size: 'small',
      }
    }
  },
  created () {
    this.data = [{
      id: 'id_0',
      top: true,
      title: this.company,
      expand: true
    }]
    this.append(this.data)
  },
  methods: {
    renderContent (h, { root, node, data }) {
      let buttons = [h('Button', {
        props: Object.assign({}, this.buttonProps, {
          icon: 'ios-plus-empty',
          type: data.top ? 'primary' : 'ghost'
        }),
        style: {
          //width: data.top ? '52px' : null,
          width: '40px'
          //marginRight: data.top ? null : '8px'
        },
        on: {
          click: () => { this.append(data) }
        }
      })]
      if (false && !data.top) {
        buttons.push(h('Button', {
          props: Object.assign({}, this.buttonProps, {
            icon: 'ios-minus-empty'
          }),
          on: {
            click: () => { this.remove(root, node, data) }
          }
        }))
      }
      return h('span', {
        attrs: {
          id: data.id
        },
        'class': {
          'tree-item': true
        //},
        //style: {
        //  display: 'inline-block',
        //  width: '100%',
        //  paddingLeft: '5px'
        }}, [
        h('span', {
          on: {
            click: () => {this.selItem(root, node, data)}
          }
        }, [h('span', data.title)]),
        h('span', {
          style: {
            marginRight: '15px'
          }}, buttons)
      ]);
    },
    append (data) {
      const children = data.children || [];
      children.push({
        id: 'id-'+this.id++,
        title: 'appended node',
        expand: true
      });
      console.log(data)
      this.$set(data, 'children', children);
    },
    remove (root, node, data) {
      const parentKey = root.find(el => el === node).parent;
      const parent = root.find(el => el.nodeKey === parentKey).node;
      const index = parent.children.indexOf(data);
      parent.children.splice(index, 1);
    },
    selItem (root, node, data) {
      if (data.top) {
        return
      }
      let currentItemSelected = false
      this.$el.querySelectorAll('.tree-item_selected').forEach((node)=> {
        node.classList.remove('tree-item_selected')
        if (node.id == data.id) {
          currentItemSelected = true
        }
      })
      let selData = null
      if (!currentItemSelected) {
        this.$el.querySelector('#'+data.id).classList.add('tree-item_selected')
        selData = [root, node, data]
      }
      this.$emit('on-select', selData)
    }
  }
}
</script>
<style lang="less">
.tree-item {
  cursor: pointer;
  width: 100%;
  padding: 0 10px 0 5px;
  display: inline-flex;
  align-items: center;
  justify-content: space-between;
}
.tree-item_selected {
  background: #eee;
  border-radius: 5px;
}
</style>
