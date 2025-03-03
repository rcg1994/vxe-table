<template>
  <div>
    <p class="tip">
      设置 <table-api-link prop="show-footer"/> show-footer 和 <table-api-link prop="footer-method"/> 自定义表尾合计<br>
      需要注意的是表尾的的计算并非实时的，而是在 data 初始化时才会触发自动计算；如果要达到实时计算请手动调用 <table-api-link prop="updateFooter"/> 方法<br>
      <span class="red">（注：<table-api-link prop="footer-method"/> 合计的逻辑都是自行实现的，该示例仅供参考）</span>
    </p>

    <vxe-table
      class="mytable-footer"
      border
      highlight-hover-row
      show-footer
      height="400"
      :footer-method="footerMethod"
      :data="tableData">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age"></vxe-table-column>
      <vxe-table-column field="rate" title="Rate"></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[0] }}</code>
      <code class="javascript">{{ demoCodes[1] }}</code>
    </pre>

    <p class="tip">还可以配合 <table-api-link prop="footer-cell-class-name"/> 自定义不同列颜色</p>

    <vxe-table
      class="mytable-footer"
      border
      show-footer
      height="400"
      :footer-method="footerMethod"
      :footer-cell-class-name="footerCellClassName"
      :data="tableData">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age"></vxe-table-column>
      <vxe-table-column field="rate" title="Rate"></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[2] }}</code>
      <code class="javascript">{{ demoCodes[3] }}</code>
    </pre>

    <p class="tip">还可以固定列</p>

    <vxe-table
      class="mytable-footer"
      border
      show-footer
      height="400"
      show-overflow
      :footer-method="footerMethod"
      :footer-cell-class-name="footerCellClassName"
      :data="tableData">
      <vxe-table-column type="index" width="60" fixed="left"></vxe-table-column>
      <vxe-table-column title="基本信息">
        <vxe-table-column field="name" title="Name" min-width="600" sortable></vxe-table-column>
        <vxe-table-column field="age" title="Age" min-width="600"></vxe-table-column>
      </vxe-table-column>
      <vxe-table-column field="date" title="Date" min-width="600"></vxe-table-column>
      <vxe-table-column field="rate" title="Rate" width="200" fixed="right"></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[4] }}</code>
      <code class="javascript">{{ demoCodes[5] }}</code>
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
          class="mytable-footer"
          border
          highlight-hover-row
          show-footer
          height="400"
          :footer-method="footerMethod"
          :data="tableData">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age"></vxe-table-column>
          <vxe-table-column field="rate" title="Rate"></vxe-table-column>
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
            footerMethod ({ columns, data }) {
              return [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '平均'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.$utils.mean(data, column.property)
                  }
                  return null
                }),
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '和值'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.$utils.sum(data, column.property)
                  }
                  return null
                })
              ]
            }
          }
        }
        `,
        `
        <vxe-table
          class="mytable-footer"
          border
          show-footer
          height="400"
          :footer-method="footerMethod"
          :footer-cell-class-name="footerCellClassName"
          :data="tableData">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age"></vxe-table-column>
          <vxe-table-column field="rate" title="Rate"></vxe-table-column>
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
            footerCellClassName ({ $rowIndex, column, columnIndex }) {
              if (columnIndex === 0) {
                if ($rowIndex === 0) {
                  return 'col-blue'
                } else {
                  return 'col-red'
                }
              }
            },
            footerMethod ({ columns, data }) {
              return [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '平均'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.$utils.mean(data, column.property)
                  }
                  return null
                }),
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '和值'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.$utils.sum(data, column.property)
                  }
                  return null
                })
              ]
            }
          }
        }
        `,
        `
        <vxe-table
          class="mytable-footer"
          border
          show-footer
          height="400"
          show-overflow
          :footer-method="footerMethod"
          :footer-cell-class-name="footerCellClassName"
          :data="tableData">
          <vxe-table-column type="index" width="60" fixed="left"></vxe-table-column>
          <vxe-table-column title="基本信息">
            <vxe-table-column field="name" title="Name" min-width="600" sortable></vxe-table-column>
            <vxe-table-column field="age" title="Age" min-width="600"></vxe-table-column>
          </vxe-table-column>
          <vxe-table-column field="date" title="Date" min-width="600"></vxe-table-column>
          <vxe-table-column field="rate" title="Rate" width="200" fixed="right"></vxe-table-column>
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
            footerCellClassName ({ $rowIndex, column, columnIndex }) {
              if (columnIndex === 0) {
                if ($rowIndex === 0) {
                  return 'col-blue'
                } else {
                  return 'col-red'
                }
              }
            },
            footerMethod ({ columns, data }) {
              const footerData = [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '平均'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.$utils.mean(data, column.property)
                  }
                  return null
                }),
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '和值'
                  }
                  if (['age', 'rate'].includes(column.property)) {
                    return this.$utils.sum(data, column.property)
                  }
                  return null
                })
              ]
              return footerData
            }
          }
        }
        `
      ]
    }
  },
  created () {
    let list = window.MOCK_DATA_LIST.slice(0, 50)
    this.tableData = list
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    footerCellClassName ({ $rowIndex, column, columnIndex }) {
      if (columnIndex === 0) {
        if ($rowIndex === 0) {
          return 'col-blue'
        } else {
          return 'col-red'
        }
      }
    },
    footerMethod ({ columns, data }) {
      const footerData = [
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '平均'
          }
          if (['age', 'rate'].includes(column.property)) {
            return this.$utils.mean(data, column.property)
          }
          return null
        }),
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '和值'
          }
          if (['age', 'rate'].includes(column.property)) {
            return this.$utils.sum(data, column.property)
          }
          return null
        })
      ]
      return footerData
    }
  }
}
</script>

<style>
.mytable-footer .vxe-footer--column.col-blue {
  background-color: #2db7f5;
  color: #fff;
}
.mytable-footer .vxe-footer--column.col-red {
  background-color: red;
  color: #fff;
}
</style>
