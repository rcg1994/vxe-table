<template>
  <div>
    <p class="tip">
      通过给需要排序功能的列加上 <table-api-link prop="sortable"/> 属性可以支持排序，还可以通过设置 <table-api-link prop="sort-by"/> 多字段进行排序<br>
      如果是服务端排序，只需加上 <table-column-api-link prop="remote-sort"/> 和 <table-api-link prop="sort-change"/> 事件就可以实现
    </p>

    <vxe-table
      border
      highlight-hover-row
      height="300"
      :data="tableData">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
      <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age"></vxe-table-column>
      <vxe-table-column field="time" title="Time" sortable></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[0] }}</code>
      <code class="javascript">{{ demoCodes[1] }}</code>
    </pre>

    <p class="tip">配置 <table-column-api-link prop="sort-by"/> 多个字段组合排序</p>

    <vxe-table
      border
      highlight-hover-row
      height="300"
      :data="tableData">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
      <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age"></vxe-table-column>
      <vxe-table-column field="time" title="Time" sortable :sort-by="['time', 'name']"></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[2] }}</code>
      <code class="javascript">{{ demoCodes[3] }}</code>
    </pre>

    <p class="tip">配置 <table-column-api-link prop="sort-method"/> 自定义排序</p>

    <vxe-table
      border
      highlight-hover-row
      height="300"
      :data="tableData"
      @sort-change="sortChangeEvent">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable :sort-method="sortNameMethod"></vxe-table-column>
      <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age"></vxe-table-column>
      <vxe-table-column field="time" title="Time"></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[4] }}</code>
      <code class="javascript">{{ demoCodes[5] }}</code>
    </pre>

    <p class="tip">点击表头排序，可以通过配置 trigger 设置触发源</p>

    <vxe-table
      border
      highlight-hover-row
      highlight-hover-column
      ref="xTable"
      height="300"
      :data="tableData"
      :sort-config="{trigger: 'cell'}"
      @sort-change="sortChangeEvent">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name"></vxe-table-column>
      <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
      <vxe-table-column field="sex" title="Sex" sortable></vxe-table-column>
      <vxe-table-column field="age" title="Age" sortable></vxe-table-column>
      <vxe-table-column field="time" title="Time" sortable></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[6] }}</code>
      <code class="javascript">{{ demoCodes[7] }}</code>
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
        <vxe-table
          border
          highlight-hover-row
          height="300"
          :data="tableData">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
          <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age"></vxe-table-column>
          <vxe-table-column field="time" title="Time" sortable></vxe-table-column>
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
            this.tableData = window.MOCK_DATA_LIST.slice(0, 50)
          }
        }
        `,
        `
        <vxe-table
          border
          highlight-hover-row
          height="300"
          :data="tableData">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
          <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age"></vxe-table-column>
          <vxe-table-column field="time" title="Time" sortable :sort-by="['time', 'name']"></vxe-table-column>
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
            this.tableData = window.MOCK_DATA_LIST.slice(0, 50)
          }
        }
        `,
        `
        <vxe-table
          border
          highlight-hover-row
          height="300"
          :data="tableData"
          @sort-change="sortChangeEvent">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable :sort-method="sortNameMethod"></vxe-table-column>
          <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age"></vxe-table-column>
          <vxe-table-column field="time" title="Time"></vxe-table-column>
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
            this.tableData = window.MOCK_DATA_LIST.slice(0, 50)
          },
          methods: {
            sortNameMethod (a, b) {
              // 例如：名称不区分大小写的排序
              var v1 = (a.name || '').toLowerCase()
              var v2 = (b.name || '').toLowerCase()
              return v1 < v2 ? -1 : v1 > v2 ? 1 : 0
            },
            sortChangeEvent ({ column, property, order }) {
              console.info(property, order)
            }
          }
        }
        `,
        `
        <vxe-table
          border
          highlight-hover-row
          highlight-hover-column
          ref="xTable"
          height="300"
          :data="tableData"
          :sort-config="{trigger: 'cell'}"
          @sort-change="sortChangeEvent">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name"></vxe-table-column>
          <vxe-table-column field="nickname" title="Nickname" sortable></vxe-table-column>
          <vxe-table-column field="sex" title="Sex" sortable></vxe-table-column>
          <vxe-table-column field="age" title="Age" sortable></vxe-table-column>
          <vxe-table-column field="time" title="Time" sortable></vxe-table-column>
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
            this.tableData = window.MOCK_DATA_LIST.slice(0, 50)
          },
          methods: {
            sortChangeEvent ({ column, property, order }) {
              console.info(property, order)
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.tableData = window.MOCK_DATA_LIST.slice(0, 50)
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    sortNameMethod (a, b) {
      // 例如：名称不区分大小写的排序
      var v1 = (a.name || '').toLowerCase()
      var v2 = (b.name || '').toLowerCase()
      return v1 < v2 ? -1 : v1 > v2 ? 1 : 0
    },
    sortChangeEvent ({ column, property, order }) {
      console.info(property, order)
    }
  }
}
</script>
