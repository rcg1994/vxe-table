<template>
  <div>
    <p class="tip">树表格，固定列</p>

    <vxe-toolbar>
      <template v-slot:buttons>
        <vxe-button @click="$refs.xTree.toggleTreeExpansion(tableData[0], true)">切换第一个</vxe-button>
        <vxe-button @click="$refs.xTree.setTreeExpansion(tableData[2], true)">展开第三个</vxe-button>
        <vxe-button @click="$refs.xTree.setAllTreeExpansion(true)">展开所有</vxe-button>
        <vxe-button @click="$refs.xTree.clearTreeExpand()">关闭所有</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      resizable
      ref="xTree"
      :tree-config="{children: 'children'}"
      :checkbox-config="{labelField: 'id'}"
      :data="tableData">
      <vxe-table-column type="checkbox" title="ID" fixed="left" width="200" tree-node></vxe-table-column>
      <vxe-table-column field="name" title="Name" width="300"></vxe-table-column>
      <vxe-table-column field="size" title="Size" width="300"></vxe-table-column>
      <vxe-table-column field="type" title="Type" width="300"></vxe-table-column>
      <vxe-table-column field="date" title="Date" width="300"></vxe-table-column>
    </vxe-table>

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
      tableData: [],
      demoCodes: [
        `
        <vxe-toolbar>
          <template v-slot:buttons>
            <vxe-button @click="$refs.xTree.toggleTreeExpansion(tableData[0], true)">切换第一个</vxe-button>
            <vxe-button @click="$refs.xTree.setTreeExpansion(tableData[2], true)">展开第三个</vxe-button>
            <vxe-button @click="$refs.xTree.setAllTreeExpansion(true)">展开所有</vxe-button>
            <vxe-button @click="$refs.xTree.clearTreeExpand()">关闭所有</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          resizable
          ref="xTree"
          :tree-config="{children: 'children'}"
          :checkbox-config="{labelField: 'id'}"
          :data="tableData">
          <vxe-table-column type="checkbox" title="ID" fixed="left" width="200" tree-node></vxe-table-column>
          <vxe-table-column field="name" title="Name" width="300"></vxe-table-column>
          <vxe-table-column field="size" title="Size" width="300"></vxe-table-column>
          <vxe-table-column field="type" title="Type" width="300"></vxe-table-column>
          <vxe-table-column field="date" title="Date" width="300"></vxe-table-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              tableData: []
            }
          },
          created () {
            this.tableData = window.MOCK_TREE_DATA_LIST.slice(0)
          }
        }
        `
      ]
    }
  },
  created () {
    this.tableData = this.$utils.clone(window.MOCK_TREE_DATA_LIST, true)
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  }
}
</script>
