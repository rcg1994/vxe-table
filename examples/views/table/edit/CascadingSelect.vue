<template>
  <div>
    <p class="tip">实现简单的级联下拉选项列表</p>

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
      <vxe-table-column field="attr3" title="Project type" :edit-render="{name: 'select', options: ptypeList, events: {change: ptypeChangeEvent}}"></vxe-table-column>
      <vxe-table-column field="attr4" title="Project name" :formatter="formatPanmeLabel" :edit-render="{}">
        <template v-slot:edit="{ row }">
          <select class="vxe-default-select" v-model="row.attr4">
            <option v-for="item in pnameList" :key="item.value" :value="item.value">{{ item.label }}</option>
          </select>
        </template>
      </vxe-table-column>
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
      ptypeList: [
        {
          label: '',
          value: ''
        },
        {
          label: '项目1',
          value: '1'
        },
        {
          label: '项目2',
          value: '2'
        },
        {
          label: '项目3',
          value: '3'
        }
      ],
      pnameList: [],
      cachePnameList: [],
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
          <vxe-table-column field="attr3" title="Project type" :edit-render="{name: 'select', options: ptypeList, events: {change: ptypeChangeEvent}}"></vxe-table-column>
          <vxe-table-column field="attr4" title="Project name" :formatter="formatPanmeLabel" :edit-render="{}">
            <template v-slot:edit="{ row }">
              <select class="vxe-default-select" v-model="row.attr4">
                <option v-for="item in pnameList" :key="item.value" :value="item.value">{{ item.label }}</option>
              </select>
            </template>
          </vxe-table-column>
          <vxe-table-column field="date12" title="Date" :edit-render="{name: 'input', attrs: {type: 'date'}}"></vxe-table-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              tableData: [],
              ptypeList: [
                {
                  label: '',
                  value: ''
                },
                {
                  label: '项目1',
                  value: '1'
                },
                {
                  label: '项目2',
                  value: '2'
                },
                {
                  label: '项目3',
                  value: '3'
                }
              ],
              pnameList: [],
              cachePnameList: []
            }
          },
          created () {
            this.$nextTick(() => {
              this.tableData = window.MOCK_DATA_LIST.slice(0, 4)
              this.updateRoleList()
            })
          },
          methods: {
            insertEvent () {
              let record = {

              }
              this.$refs.xTable.insert(record)
            },
            // 格式化显示名称
            formatPanmeLabel ({ cellValue, row }) {
              let ptype = row.attr3
              let ptypeItem = this.cachePnameList.find(item => item.ptype === ptype)
              if (ptypeItem && ptypeItem.pnameList) {
                let pnameItem = ptypeItem.pnameList.find(item => item.value === cellValue)
                if (pnameItem) {
                  return pnameItem.label
                }
              }
              return ''
            },
            // 更新级联选项列表
            updatePnameList (row) {
              let ptype = row.attr3
              let pnameList = [
                {
                  label: '',
                  value: ''
                }
              ]
              if (ptype) {
                let item = this.cachePnameList.find(item => item.ptype === ptype)
                if (item) {
                  pnameList = item.pnameList
                } else {
                  // 模拟后台数据
                  Array.from(new Array(this.$utils.random(3, 8))).forEach((item, index) => {
                    pnameList.push({
                      label: \`\${ptype}-名称\${index}\`,
                      value: \`\${ptype}_\${index}\`
                    })
                  })
                  this.cachePnameList.push({ ptype, pnameList })
                }
              }
              this.pnameList = pnameList
            },
            ptypeChangeEvent ({ row }, evnt) {
              // 使用内置 select 需要手动更新，使用第三方组件如果是 v-model 就不需要手动赋值
              row.attr3 = evnt.target.value
              // 类型切换时更新级联的下拉数据
              row.attr4 = ''
              this.updatePnameList(row)
            },
            editActivedEvent ({ row }) {
              this.updatePnameList(row)
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.$nextTick(() => {
      this.tableData = window.MOCK_DATA_LIST.slice(0, 4)
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
    // 格式化显示名称
    formatPanmeLabel ({ cellValue, row }) {
      let ptype = row.attr3
      let ptypeItem = this.cachePnameList.find(item => item.ptype === ptype)
      if (ptypeItem && ptypeItem.pnameList) {
        let pnameItem = ptypeItem.pnameList.find(item => item.value === cellValue)
        if (pnameItem) {
          return pnameItem.label
        }
      }
      return ''
    },
    // 更新级联选项列表
    updatePnameList (row) {
      let ptype = row.attr3
      let pnameList = [
        {
          label: '',
          value: ''
        }
      ]
      if (ptype) {
        let item = this.cachePnameList.find(item => item.ptype === ptype)
        if (item) {
          pnameList = item.pnameList
        } else {
          // 模拟后台数据
          Array.from(new Array(this.$utils.random(3, 8))).forEach((item, index) => {
            pnameList.push({
              label: `${ptype}-名称${index}`,
              value: `${ptype}_${index}`
            })
          })
          this.cachePnameList.push({ ptype, pnameList })
        }
      }
      this.pnameList = pnameList
    },
    ptypeChangeEvent ({ row }, evnt) {
      // 使用内置 select 需要手动更新，使用第三方组件如果是 v-model 就不需要手动赋值
      row.attr3 = evnt.target.value
      // 类型切换时更新级联的下拉数据
      row.attr4 = ''
      this.updatePnameList(row)
    },
    editActivedEvent ({ row }) {
      this.updatePnameList(row)
    }
  }
}
</script>
