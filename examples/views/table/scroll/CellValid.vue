<template>
  <div>
    <p class="tip">虚拟滚动渲染，可编辑表格<br><span class="red">当数据量非常大时，由于需要进行大量数据运算，所以数据校验或者获取数据...等函数执行会相对耗时</span></p>

    <vxe-toolbar :refresh="{query: findList}">
      <template v-slot:buttons>
        <vxe-button>
          <template>新增操作</template>
          <template v-slot:dropdowns>
            <vxe-button @click="insertEvent(null)">从第一行插入</vxe-button>
            <vxe-button @click="insertEvent(-1)">从最后插入</vxe-button>
            <vxe-button @click="insertEvent($refs.xTable.getData(100))">插入到 100 行</vxe-button>
            <vxe-button @click="insertEvent($refs.xTable.getData(2000))">插入到 2000 行</vxe-button>
          </template>
        </vxe-button>
        <vxe-button>
          <template>删除操作</template>
          <template v-slot:dropdowns>
            <vxe-button @click="$refs.xTable.removeSelecteds()">删除选中</vxe-button>
            <vxe-button @click="$refs.xTable.remove($refs.xTable.getData(0))">删除第一行</vxe-button>
            <vxe-button @click="$refs.xTable.remove($refs.xTable.getData($refs.xTable.getData().length - 1))">删除最后一行</vxe-button>
            <vxe-button @click="$refs.xTable.remove($refs.xTable.getData(100))">删除第 100 行</vxe-button>
          </template>
        </vxe-button>
        <vxe-button>
          <template>校验操作</template>
          <template v-slot:dropdowns>
            <vxe-button @click="validEvent">基本校验</vxe-button>
            <vxe-button @click="fullValidEvent">完整校验</vxe-button>
            <vxe-button @click="selectValidEvent">选中校验</vxe-button>
          </template>
        </vxe-button>
        <vxe-button @click="getInsertEvent">获取新增</vxe-button>
        <vxe-button @click="getRemoveEvent">获取删除</vxe-button>
        <vxe-button @click="getUpdateEvent">获取修改</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      border
      resizable
      show-overflow
      ref="xTable"
      height="300"
      :loading="loading"
      :edit-rules="validRules"
      :edit-config="{trigger: 'click', mode: 'cell', showStatus: true}"
      :checkbox-config="{checkField: 'checked'}">
      <vxe-table-column type="checkbox" width="60"></vxe-table-column>
      <vxe-table-column type="index" width="100"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable width="200" :edit-render="{name: 'input'}"></vxe-table-column>
      <vxe-table-column field="age" title="Age" width="200" :edit-render="{name: 'input'}"></vxe-table-column>
      <vxe-table-column field="sex" title="Sex" width="200" :edit-render="{name: 'input'}"></vxe-table-column>
      <vxe-table-column field="rate" title="Rate" width="200"></vxe-table-column>
      <vxe-table-column field="region" title="Region" width="200"></vxe-table-column>
      <vxe-table-column field="time" title="Time" width="200"></vxe-table-column>
      <vxe-table-column field="address" title="Address" width="300" show-overflow></vxe-table-column>
      <vxe-table-column field="updateTime" title="UpdateTime" width="200"></vxe-table-column>
      <vxe-table-column field="createTime" title="CreateTime" width="200"></vxe-table-column>
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
      loading: false,
      validRules: {
        name: [
          { required: true, message: '名称必须填写' },
          { min: 3, max: 50, message: '名称长度在 3 到 50 个字符' }
        ],
        sex: [
          { required: true, message: '性别必须填写' }
        ]
      },
      demoCodes: [
        `
        <vxe-toolbar :refresh="{query: findList}">
          <template v-slot:buttons>
            <vxe-button>
              <template>新增操作</template>
              <template v-slot:dropdowns>
                <vxe-button @click="insertEvent(null)">从第一行插入</vxe-button>
                <vxe-button @click="insertEvent(-1)">从最后插入</vxe-button>
                <vxe-button @click="insertEvent($refs.xTable.getData(100))">插入到 100 行</vxe-button>
                <vxe-button @click="insertEvent($refs.xTable.getData(2000))">插入到 2000 行</vxe-button>
              </template>
            </vxe-button>
            <vxe-button>
              <template>删除操作</template>
              <template v-slot:dropdowns>
                <vxe-button @click="$refs.xTable.removeSelecteds()">删除选中</vxe-button>
                <vxe-button @click="$refs.xTable.remove($refs.xTable.getData(0))">删除第一行</vxe-button>
                <vxe-button @click="$refs.xTable.remove($refs.xTable.getData($refs.xTable.getData().length - 1))">删除最后一行</vxe-button>
                <vxe-button @click="$refs.xTable.remove($refs.xTable.getData(100))">删除第 100 行</vxe-button>
              </template>
            </vxe-button>
            <vxe-button>
              <template>校验操作</template>
              <template v-slot:dropdowns>
                <vxe-button @click="validEvent">基本校验</vxe-button>
                <vxe-button @click="fullValidEvent">完整校验</vxe-button>
                <vxe-button @click="selectValidEvent">选中校验</vxe-button>
              </template>
            </vxe-button>
            <vxe-button @click="getInsertEvent">获取新增</vxe-button>
            <vxe-button @click="getRemoveEvent">获取删除</vxe-button>
            <vxe-button @click="getUpdateEvent">获取修改</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          border
          resizable
          show-overflow
          ref="xTable"
          height="300"
          :loading="loading"
          :edit-rules="validRules"
          :edit-config="{trigger: 'click', mode: 'row', showStatus: true}"
          :checkbox-config="{checkField: 'checked'}">
          <vxe-table-column type="checkbox" width="60"></vxe-table-column>
          <vxe-table-column type="index" width="100"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable width="200" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="age" title="Age" width="200" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="sex" title="Sex" width="200" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="rate" title="Rate" width="200"></vxe-table-column>
          <vxe-table-column field="region" title="Region" width="200"></vxe-table-column>
          <vxe-table-column field="time" title="Time" width="200"></vxe-table-column>
          <vxe-table-column field="address" title="Address" width="300" show-overflow></vxe-table-column>
          <vxe-table-column field="updateTime" title="UpdateTime" width="200"></vxe-table-column>
          <vxe-table-column field="createTime" title="CreateTime" width="200"></vxe-table-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              loading: false,
              validRules: {
                name: [
                  { required: true, message: '名称必须填写' },
                  { min: 3, max: 50, message: '名称长度在 3 到 50 个字符' }
                ],
                sex: [
                  { required: true, message: '性别必须填写' }
                ]
              }
            }
          },
          created () {
            this.findList()
          },
          methods: {
            findList () {
              this.loading = true
              return new Promise(resolve => {
                setTimeout(() => {
                  let tableData = window.MOCK_DATA_LIST.slice(0, 1000)
                  // 阻断 vue 对大数组的双向绑定，避免 vue 绑定大数据造成短暂的卡顿
                  if (this.$refs.xTable) {
                    this.$refs.xTable.loadData(tableData)
                  }
                  resolve()
                  this.loading = false
                }, 300)
              })
            },
            validEvent () {
              this.$refs.xTable.validate(valid => {
                if (valid) {
                  this.$XModal.message({ status: 'success', message: '校验成功！' })
                } else {
                  this.$XModal.message({ status: 'error', message: '校验不通过！' })
                }
              })
            },
            fullValidEvent () {
              this.$refs.xTable.fullValidate((valid, errMap) => {
                if (valid) {
                  this.$XModal.message({ status: 'success', message: '校验成功！' })
                } else {
                  let msgList = []
                  Object.values(errMap).forEach(errList => {
                    errList.forEach(params => {
                      let { rowIndex, column, rules } = params
                      rules.forEach(rule => {
                        msgList.push(\`第 \${rowIndex} 行 \${column.title} 校验错误：\${rule.message}\`)
                      })
                    })
                  })
                  this.$XModal.message({
                    status: 'error',
                    message: () => {
                      return [
                        <div class="red" style="max-height: 400px;overflow: auto;">
                          {
                            msgList.map(msg => {
                              return <div>{ msg }</div>
                            })
                          }
                        </div>
                      ]
                    }
                  })
                }
              })
            },
            selectValidEvent () {
              let selectRecords = this.$refs.xTable.getSelectRecords()
              if (selectRecords.length > 0) {
                this.$refs.xTable.validate(selectRecords, valid => {
                  if (valid) {
                    this.$XModal.message({ status: 'success', message: '校验成功！' })
                  } else {
                    this.$XModal.message({ status: 'error', message: '校验不通过！' })
                  }
                })
              } else {
                this.$XModal.message({ status: 'warning', message: '未选中数据！' })
              }
            },
            insertEvent (row) {
              let xTable = this.$refs.xTable
              let record = {}
              xTable.insertAt(record, row).then(({ row }) => {
                xTable.setActiveRow(row)
              })
            },
            getInsertEvent () {
              let insertRecords = this.$refs.xTable.getInsertRecords()
              this.$XModal.alert(insertRecords.length)
            },
            getRemoveEvent () {
              let removeRecords = this.$refs.xTable.getRemoveRecords()
              this.$XModal.alert(removeRecords.length)
            },
            getUpdateEvent () {
              let updateRecords = this.$refs.xTable.getUpdateRecords()
              this.$XModal.alert(updateRecords.length)
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.findList()
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    findList () {
      this.loading = true
      return new Promise(resolve => {
        setTimeout(() => {
          let tableData = window.MOCK_DATA_LIST.slice(0, 1000)
          // 阻断 vue 对大数组的双向绑定，避免 vue 绑定大数据造成短暂的卡顿
          if (this.$refs.xTable) {
            this.$refs.xTable.loadData(tableData)
          }
          resolve()
          this.loading = false
        }, 300)
      })
    },
    validEvent () {
      this.$refs.xTable.validate(valid => {
        if (valid) {
          this.$XModal.message({ status: 'success', message: '校验成功！' })
        } else {
          this.$XModal.message({ status: 'error', message: '校验不通过！' })
        }
      })
    },
    fullValidEvent () {
      this.$refs.xTable.fullValidate((valid, errMap) => {
        if (valid) {
          this.$XModal.message({ status: 'success', message: '校验成功！' })
        } else {
          let msgList = []
          Object.values(errMap).forEach(errList => {
            errList.forEach(params => {
              let { rowIndex, column, rules } = params
              rules.forEach(rule => {
                msgList.push(`第 ${rowIndex} 行 ${column.title} 校验错误：${rule.message}`)
              })
            })
          })
          this.$XModal.message({
            status: 'error',
            message: () => {
              return [
                <div class="red" style="max-height: 400px;overflow: auto;">
                  {
                    msgList.map(msg => {
                      return <div>{ msg }</div>
                    })
                  }
                </div>
              ]
            }
          })
        }
      })
    },
    selectValidEvent () {
      let selectRecords = this.$refs.xTable.getSelectRecords()
      if (selectRecords.length > 0) {
        this.$refs.xTable.validate(selectRecords, valid => {
          if (valid) {
            this.$XModal.message({ status: 'success', message: '校验成功！' })
          } else {
            this.$XModal.message({ status: 'error', message: '校验不通过！' })
          }
        })
      } else {
        this.$XModal.message({ status: 'warning', message: '未选中数据！' })
      }
    },
    insertEvent (row) {
      let xTable = this.$refs.xTable
      let record = {}
      xTable.insertAt(record, row).then(({ row }) => {
        xTable.setActiveRow(row)
      })
    },
    getInsertEvent () {
      let insertRecords = this.$refs.xTable.getInsertRecords()
      this.$XModal.alert(insertRecords.length)
    },
    getRemoveEvent () {
      let removeRecords = this.$refs.xTable.getRemoveRecords()
      this.$XModal.alert(removeRecords.length)
    },
    getUpdateEvent () {
      let updateRecords = this.$refs.xTable.getUpdateRecords()
      this.$XModal.alert(updateRecords.length)
    }
  }
}
</script>
