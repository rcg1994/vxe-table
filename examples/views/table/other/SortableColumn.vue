<template>
  <div>
    <p class="tip"><table-api-link name="vxe-table"/> 方式：使用 <a class="link" href="https://www.npmjs.com/package/sortablejs" target="_blank">sortablejs</a> 实现列移动，由于 sortablejs 操作了 Dom 节点，需要与 Vue 的数据同步，必须设置 <table-api-link prop="column-key"/></p>

    <vxe-table
      border
      column-key
      ref="xTable1"
      class="sortable-column-demo"
      :data="tableData">
      <vxe-table-column field="name" title="Name"></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age"></vxe-table-column>
      <vxe-table-column field="address" title="Address" show-overflow></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[0] }}</code>
      <code class="javascript">{{ demoCodes[1] }}</code>
      <code class="css">{{ demoCodes[2] }}</code>
    </pre>

    <p class="tip">
      <grid-api-link name="vxe-grid"/> 方式，更加简单的配置式调用<br>
      <span class="red">（注：固定列是无法拖动的，需要自行限制）</span>
    </p>

    <vxe-grid
      border
      column-key
      show-footer
      ref="xTable2"
      class="sortable-column-demo"
      :footer-method="footerMethod"
      :toolbar="toolbar"
      :columns="tableColumn"
      :data="tableData"></vxe-grid>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[3] }}</code>
      <code class="javascript">{{ demoCodes[4] }}</code>
      <code class="css">{{ demoCodes[5] }}</code>
    </pre>
  </div>
</template>

<script>
import hljs from 'highlight.js'
import Sortable from 'sortablejs'

export default {
  data () {
    return {
      tableColumn: [
        { field: 'name', title: 'Name', fixed: 'left', minWidth: 200 },
        { field: 'role', title: 'Role', minWidth: 220 },
        { field: 'sex', title: 'Sex', minWidth: 220 },
        { field: 'age', title: 'Age', minWidth: 220 },
        { field: 'date3', title: 'Date', minWidth: 220 },
        { field: 'address', title: 'Address', minWidth: 300, showOverflow: true }
      ],
      toolbar: {
        setting: true
      },
      tableData: [],
      demoCodes: [
        `
        <vxe-table
          border
          column-key
          ref="xTable"
          class="sortable-column-demo"
          :data="tableData">
          <vxe-table-column field="name" title="Name"></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age"></vxe-table-column>
          <vxe-table-column field="address" title="Address" show-overflow></vxe-table-column>
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
            this.tableData = window.MOCK_DATA_LIST.slice(0, 6)
          },
          beforeDestroy () {
            if (this.sortable) {
              this.sortable.destroy()
            }
          },
          methods: {
            columnDrop () {
              this.$nextTick(() => {
              let xTable = this.$refs.xTable
              this.sortable = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--header .vxe-header--row'), {
                handle: '.vxe-header--column:not(.col--fixed)',
                onEnd: ({ item, newIndex, oldIndex }) => {
                  let { fullColumn, tableColumn } = xTable.getTableColumn()
                  let targetThElem = item
                  let wrapperElem = targetThElem.parentNode
                  let newColumn = fullColumn[newIndex]
                  if (newColumn.fixed) {
                    // 错误的移动
                    if (newIndex > oldIndex) {
                      wrapperElem.insertBefore(targetThElem, wrapperElem.children[oldIndex])
                    } else {
                      wrapperElem.insertBefore(wrapperElem.children[oldIndex], targetThElem)
                    }
                    return this.$XModal.message({ message: '固定列不允许拖动！', status: 'error' })
                  }
                  // 转换真实索引
                  let oldColumnIndex = xTable.getColumnIndex(tableColumn[oldIndex])
                  let newColumnIndex = xTable.getColumnIndex(tableColumn[newIndex])
                  // 移动到目标列
                  let currRow = fullColumn.splice(oldColumnIndex, 1)[0]
                  fullColumn.splice(newColumnIndex, 0, currRow)
                  xTable.loadColumn(fullColumn)
                }
              })
            })
            }
          }
        }
        `,
        `
        .sortable-column-demo .vxe-header--row .vxe-header--column.sortable-ghost,
        .sortable-column-demo .vxe-header--row .vxe-header--column.sortable-chosen {
          background-color: #dfecfb;
        }
        `,
        `
        <vxe-grid
          border
          column-key
          show-footer
          ref="xTable"
          class="sortable-column-demo"
          :footer-method="footerMethod"
          :toolbar="toolbar"
          :columns="tableColumn"
          :data="tableData"></vxe-grid>
        `,
        `
        export default {
          data () {
            return {
              tableColumn: [
                { field: 'name', title: 'Name', fixed: 'left', minWidth: 200 },
                { field: 'role', title: 'Role', minWidth: 220 },
                { field: 'sex', title: 'Sex', minWidth: 220 },
                { field: 'age', title: 'Age', minWidth: 220 },
                { field: 'date3', title: 'Date', minWidth: 220 },
                { field: 'address', title: 'Address', minWidth: 300, showOverflow: true }
              ],
              toolbar: {
                setting: true
              },
              tableData: []
            }
          },
          created () {
            this.tableData = window.MOCK_DATA_LIST.slice(0, 6)
          },
          beforeDestroy () {
            if (this.sortable) {
              this.sortable.destroy()
            }
          },
          methods: {
            footerMethod ({ columns, data }) {
              return [
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '平均'
                  }
                  if (['age', 'sex'].includes(column.property)) {
                    return this.$utils.mean(data, column.property)
                  }
                  return null
                }),
                columns.map((column, columnIndex) => {
                  if (columnIndex === 0) {
                    return '和值'
                  }
                  if (['age', 'sex'].includes(column.property)) {
                    return this.$utils.sum(data, column.property)
                  }
                  return null
                })
              ]
            },
            columnDrop () {
              this.$nextTick(() => {
              let xTable = this.$refs.xTable
              this.sortable = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--header .vxe-header--row'), {
                handle: '.vxe-header--column:not(.col--fixed)',
                onEnd: ({ item, newIndex, oldIndex }) => {
                  let { fullColumn, tableColumn } = xTable.getTableColumn()
                  let targetThElem = item
                  let wrapperElem = targetThElem.parentNode
                  let newColumn = fullColumn[newIndex]
                  if (newColumn.fixed) {
                    // 错误的移动
                    if (newIndex > oldIndex) {
                      wrapperElem.insertBefore(targetThElem, wrapperElem.children[oldIndex])
                    } else {
                      wrapperElem.insertBefore(wrapperElem.children[oldIndex], targetThElem)
                    }
                    return this.$XModal.message({ message: '固定列不允许拖动！', status: 'error' })
                  }
                  // 转换真实索引
                  let oldColumnIndex = xTable.getColumnIndex(tableColumn[oldIndex])
                  let newColumnIndex = xTable.getColumnIndex(tableColumn[newIndex])
                  // 移动到目标列
                  let currRow = fullColumn.splice(oldColumnIndex, 1)[0]
                  fullColumn.splice(newColumnIndex, 0, currRow)
                  xTable.loadColumn(fullColumn)
                }
              })
            })
            }
          }
        }
        `,
        `
        .sortable-column-demo .vxe-header--row .vxe-header--column.sortable-ghost,
        .sortable-column-demo .vxe-header--row .vxe-header--column.sortable-chosen {
          background-color: #dfecfb;
        }
        .sortable-column-demo .vxe-header--row .vxe-header--column.col--fixed {
          cursor: no-drop;
        }
        `
      ]
    }
  },
  created () {
    this.tableData = window.MOCK_DATA_LIST.slice(0, 6)
    this.columnDrop1()
    this.columnDrop2()
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  beforeDestroy () {
    if (this.sortable1) {
      this.sortable1.destroy()
    }
    if (this.sortable2) {
      this.sortable2.destroy()
    }
  },
  methods: {
    footerMethod ({ columns, data }) {
      return [
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '平均'
          }
          if (['age', 'sex'].includes(column.property)) {
            return this.$utils.mean(data, column.property)
          }
          return null
        }),
        columns.map((column, columnIndex) => {
          if (columnIndex === 0) {
            return '和值'
          }
          if (['age', 'sex'].includes(column.property)) {
            return this.$utils.sum(data, column.property)
          }
          return null
        })
      ]
    },
    columnDrop1 () {
      this.$nextTick(() => {
        let xTable = this.$refs.xTable1
        this.sortable1 = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--header .vxe-header--row'), {
          handle: '.vxe-header--column:not(.col--fixed)',
          onEnd: ({ item, newIndex, oldIndex }) => {
            let { fullColumn, tableColumn } = xTable.getTableColumn()
            let targetThElem = item
            let wrapperElem = targetThElem.parentNode
            let newColumn = fullColumn[newIndex]
            if (newColumn.fixed) {
              // 错误的移动
              if (newIndex > oldIndex) {
                wrapperElem.insertBefore(targetThElem, wrapperElem.children[oldIndex])
              } else {
                wrapperElem.insertBefore(wrapperElem.children[oldIndex], targetThElem)
              }
              return this.$XModal.message({ message: '固定列不允许拖动！', status: 'error' })
            }
            // 转换真实索引
            let oldColumnIndex = xTable.getColumnIndex(tableColumn[oldIndex])
            let newColumnIndex = xTable.getColumnIndex(tableColumn[newIndex])
            // 移动到目标列
            let currRow = fullColumn.splice(oldColumnIndex, 1)[0]
            fullColumn.splice(newColumnIndex, 0, currRow)
            xTable.loadColumn(fullColumn)
          }
        })
      })
    },
    columnDrop2 () {
      this.$nextTick(() => {
        let xTable = this.$refs.xTable2
        this.sortable2 = Sortable.create(xTable.$el.querySelector('.body--wrapper>.vxe-table--header .vxe-header--row'), {
          handle: '.vxe-header--column:not(.col--fixed)',
          onEnd: ({ item, newIndex, oldIndex }) => {
            let { fullColumn, tableColumn } = xTable.getTableColumn()
            let targetThElem = item
            let wrapperElem = targetThElem.parentNode
            let newColumn = fullColumn[newIndex]
            if (newColumn.fixed) {
              // 错误的移动
              if (newIndex > oldIndex) {
                wrapperElem.insertBefore(targetThElem, wrapperElem.children[oldIndex])
              } else {
                wrapperElem.insertBefore(wrapperElem.children[oldIndex], targetThElem)
              }
              return this.$XModal.message({ message: '固定列不允许拖动！', status: 'error' })
            }
            // 转换真实索引
            let oldColumnIndex = xTable.getColumnIndex(tableColumn[oldIndex])
            let newColumnIndex = xTable.getColumnIndex(tableColumn[newIndex])
            // 移动到目标列
            let currRow = fullColumn.splice(oldColumnIndex, 1)[0]
            fullColumn.splice(newColumnIndex, 0, currRow)
            xTable.loadColumn(fullColumn)
          }
        })
      })
    }
  }
}
</script>

<style lang="scss">
.sortable-column-demo .vxe-header--row .vxe-header--column.sortable-ghost,
.sortable-column-demo .vxe-header--row .vxe-header--column.sortable-chosen {
  background-color: #dfecfb;
}
.sortable-column-demo .vxe-header--row .vxe-header--column.col--fixed {
  cursor: no-drop;
}
</style>
