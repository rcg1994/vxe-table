<template>
  <div>
    <p class="tip">树表格、数据代理</p>

    <vxe-grid
      border
      resizable
      :proxy-config="tableProxy"
      :columns="tableColumn"
      :tree-config="{children: 'children'}"></vxe-grid>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[0] }}</code>
      <code class="javascript">{{ demoCodes[1] }}</code>
    </pre>
  </div>
</template>

<script>
import hljs from 'highlight.js'

export default {
  data () {
    return {
      tableProxy: {
        ajax: {
          // 处理树结构转换
          query: () => this.$ajax.getJSON('/api/file/list').then(data => this.$utils.toArrayTree(data, { key: 'id', parentKey: 'parentId', children: 'children' }))
        }
      },
      tableColumn: [
        { field: 'id', title: 'ID', width: 120, treeNode: true },
        { field: 'name', title: '名称' },
        { field: 'size', title: '大小' },
        { field: 'createTime', title: '创建时间', formatter: this.formatterDate },
        { field: 'updateTime', title: '修改时间', formatter: this.formatterDate }
      ],
      demoCodes: [
        `
        <vxe-grid
          border
          resizable
          :proxy-config="tableProxy"
          :columns="tableColumn"
          :tree-config="{children: 'children'}"></vxe-grid>
        `,
        `
        export default {
          data () {
            return {
              tableProxy: {
                ajax: {
                  // 处理树结构转换
                  query: () => this.$ajax.getJSON('/api/file/list').then(data => this.$utils.toArrayTree(data, { key: 'id', parentKey: 'parentId', children: 'children' }))
                }
              },
              tableColumn: [
                { type: 'checkbox', width: 100, treeNode: true },
                { field: 'id', title: 'ID', width: 0 },
                { field: 'name', title: '名称' },
                { field: 'size', title: '大小' },
                { field: 'createTime', title: '创建时间' },
                { field: 'updateTime', title: '修改时间' }
              ]
            }
          },
          methods: {
            formatterDate ({ cellValue }) {
              return this.$utils.toDateString(cellValue, 'yyyy-MM-dd HH:mm:ss')
            }
          }
        }
        `
      ]
    }
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    formatterDate ({ cellValue }) {
      return this.$utils.toDateString(cellValue, 'yyyy-MM-dd HH:mm:ss')
    }
  }
}
</script>
