<template>
  <div>
    <p class="tip">实现简单的唯一下拉选项列表</p>

    <vxe-toolbar>
      <template v-slot:buttons>
        <vxe-button @click="insertEvent()">新增</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      border
      resizable
      ref="xTable"
      max-height="400"
      :data="tableData"
      :edit-config="{trigger: 'click', mode: 'row'}"
      @edit-actived="editActivedEvent">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" :edit-render="{name: 'input'}"></vxe-table-column>
      <vxe-table-column field="role1" title="Role" :edit-render="{name: 'select', options: roleList, events: {change: roleChangeEvent}}"></vxe-table-column>
      <vxe-table-column field="date12" title="Date" :edit-render="{name: 'input', attrs: {type: 'date'}}"></vxe-table-column>
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
      roleList: [
        {
          label: '',
          value: '',
          disabled: false
        },
        {
          label: '前端',
          value: '1',
          disabled: false
        },
        {
          label: '后端',
          value: '2',
          disabled: false
        },
        {
          label: '项目经理',
          value: '3',
          disabled: false
        },
        {
          label: '设计师',
          value: '4',
          disabled: false
        },
        {
          label: '运维',
          value: '5',
          disabled: false
        }
      ],
      demoCodes: [
        `
        <vxe-toolbar>
          <template v-slot:buttons>
            <vxe-button @click="insertEvent()">新增</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          border
          resizable
          ref="xTable"
          max-height="400"
          :data="tableData"
          :edit-config="{trigger: 'click', mode: 'row'}"
          @edit-actived="editActivedEvent">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="role1" title="Role" :edit-render="{name: 'select', options: roleList, events: {change: roleChangeEvent}}"></vxe-table-column>
          <vxe-table-column field="date12" title="Date" :edit-render="{name: 'input', attrs: {type: 'date'}}"></vxe-table-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              tableData: [],
              roleList: [
                {
                  label: '',
                  value: '',
                  disabled: false
                },
                {
                  label: '前端',
                  value: '1',
                  disabled: false
                },
                {
                  label: '后端',
                  value: '2',
                  disabled: false
                },
                {
                  label: '项目经理',
                  value: '3',
                  disabled: false
                },
                {
                  label: '设计师',
                  value: '4',
                  disabled: false
                },
                {
                  label: '运维',
                  value: '5',
                  disabled: false
                }
              ]
            }
          },
          created () {
            this.$nextTick(() => {
              this.tableData = window.MOCK_DATA_LIST.slice(0, 2)
              this.updateRoleList()
            })
          },
          methods: {
            insertEvent () {
              let record = {

              }
              this.$refs.xTable.insert(record)
            },
            updateRoleList () {
              // 获取表格中的全量数据
              let { fullData } = this.$refs.xTable.getTableData()
              this.roleList.forEach(item => {
                if (item.value) {
                  // 如果当前选项已经被选过，则禁用
                  item.disabled = fullData.some(row => row.role1 === item.value)
                }
              })
            },
            roleChangeEvent ({ row }, evnt) {
              // 使用内置 select 需要手动更新，使用第三方组件如果是 v-model 就不需要手动赋值
              row.role1 = evnt.target.value
              this.updateRoleList()
            },
            editActivedEvent ({ row }) {
              // 激活编辑时检查剩余选项是否可选择
              this.updateRoleList()
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.$nextTick(() => {
      this.tableData = window.MOCK_DATA_LIST.slice(0, 2)
      this.updateRoleList()
    })
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    insertEvent () {
      let record = {

      }
      this.$refs.xTable.insert(record)
    },
    updateRoleList () {
      // 获取表格中的全量数据
      let { fullData } = this.$refs.xTable.getTableData()
      this.roleList.forEach(item => {
        if (item.value) {
          // 如果当前选项已经被选过，则禁用
          item.disabled = fullData.some(row => row.role1 === item.value)
        }
      })
    },
    roleChangeEvent ({ row }, evnt) {
      // 使用内置 select 需要手动更新，使用第三方组件如果是 v-model 就不需要手动赋值
      row.role1 = evnt.target.value
      this.updateRoleList()
    },
    editActivedEvent ({ row }) {
      // 激活编辑时检查剩余选项是否可选择
      this.updateRoleList()
    }
  }
}
</script>
