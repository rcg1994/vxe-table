<template>
  <div>
    <p class="tip">
      导入数据：通过 <table-api-link prop="importData"/> 函数可以直接将数据导入表格中<br>
      <span class="red">注：附件中的字段名必须和表格一致，数据格式不正确将无法导入</span>
    </p>

    <vxe-toolbar>
      <template v-slot:buttons>
        <vxe-button @click="clearDataEvent">清空数据</vxe-button>
        <vxe-button @click="exportDataEvent">导出数据</vxe-button>
        <vxe-button @click="importDataEvent">导入数据</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      ref="xTable"
      highlight-hover-row
      height="400"
      :data="tableData">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name"></vxe-table-column>
      <vxe-table-column field="sex" title="Sex"></vxe-table-column>
      <vxe-table-column field="age" title="Age" sortable></vxe-table-column>
      <vxe-table-column field="address" title="Address" show-overflow></vxe-table-column>
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
            <vxe-button @click="clearDataEvent">清空数据</vxe-button>
            <vxe-button @click="exportDataEvent">导出数据</vxe-button>
            <vxe-button @click="importDataEvent">导入数据</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          ref="xTable"
          highlight-hover-row
          height="400"
          :data="tableData">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name"></vxe-table-column>
          <vxe-table-column field="sex" title="Sex"></vxe-table-column>
          <vxe-table-column field="age" title="Age" sortable></vxe-table-column>
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
            this.tableData = window.MOCK_DATA_LIST.slice(0, 10)
          },
          methods: {
            clearDataEvent () {
              this.tableData = []
            },
            exportDataEvent () {
              this.$refs.xTable.openExport()
            },
            importDataEvent () {
              this.$refs.xTable.importData(status => {
                if (status) {
                  this.$XModal.message({ message: '导入成功', status: 'success' })
                } else {
                  this.$XModal.alert({ message: '导入失败，请检查格式是否正确', status: 'error' })
                }
              })
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.tableData = window.MOCK_DATA_LIST.slice(0, 10)
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    clearDataEvent () {
      this.tableData = []
    },
    exportDataEvent () {
      this.$refs.xTable.openExport()
    },
    importDataEvent () {
      this.$refs.xTable.importData(status => {
        if (status) {
          this.$XModal.message({ message: '导入成功', status: 'success' })
        } else {
          this.$XModal.alert({ message: '导入失败，请检查格式是否正确', status: 'error' })
        }
      })
    }
  }
}
</script>
