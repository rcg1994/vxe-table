<template>
  <div>
    <p class="tip">与 <a class="link" href="https://www.npmjs.com/package/element-ui">element-ui</a> 组合渲染 + 使用分页</p>

    <el-form ref="tableform" :model="formData" inline>
      <el-form-item title="名字" prop="name">
        <el-input v-model="formData.name" placeholder="名字"></el-input>
      </el-form-item>
      <el-form-item title="性别" prop="sex">
        <el-select v-model="formData.sex" placeholder="性别">
          <el-option v-for="item in sexList" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="searchEvent">查询</el-button>
        <el-button @click="$refs.tableform.resetFields()">重置</el-button>
      </el-form-item>
    </el-form>

    <vxe-toolbar export setting>
      <template v-slot:buttons>
        <el-button @click="insertEvent">新增</el-button>
        <el-button @click="saveEvent">保存</el-button>
        <el-button @click="vaildEvent">校验</el-button>
        <el-dropdown @command="dropdownMenuEvent">
          <el-button>
            操作<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="remove">删除选中</el-dropdown-item>
            <el-dropdown-item command="export">导出数据</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </template>
    </vxe-toolbar>

    <vxe-table
      border
      resizable
      show-overflow
      highlight-hover-row
      ref="xTable"
      class="vxe-table-element"
      height="460"
      :loading="loading"
      :data="tableData"
      :edit-rules="validRules"
      :edit-config="{trigger: 'click', mode: 'row'}">
      <vxe-table-column type="checkbox" width="60" fixed="left"></vxe-table-column>
      <vxe-table-column type="index" width="60" fixed="left"></vxe-table-column>
      <vxe-table-column field="name" title="ElInput" min-width="140" fixed="left" :edit-render="{name: 'ElInput'}"></vxe-table-column>
      <vxe-table-column field="role" title="ElAutocomplete" width="160" :edit-render="{name: 'ElAutocomplete', props: {fetchSuggestions: roleFetchSuggestions}}"></vxe-table-column>
      <vxe-table-column field="age" title="ElInputNumber" width="160" :edit-render="{name: 'ElInputNumber', props: {max: 35, min: 18}}"></vxe-table-column>
      <vxe-table-column field="sex" title="ElSelect" width="140" :edit-render="{name: 'ElSelect', options: sexList}"></vxe-table-column>
      <vxe-table-column field="region" title="ElCascader" width="200" :edit-render="{name: 'ElCascader', props: {options: regionList}}"></vxe-table-column>
      <vxe-table-column field="date" title="ElDatePicker" width="200" :edit-render="{name: 'ElDatePicker', props: {type: 'date', format: 'yyyy/MM/dd'}}"></vxe-table-column>
      <vxe-table-column field="date1" title="DateTimePicker" width="220" :edit-render="{name: 'ElDatePicker', props: {type: 'datetime', format: 'yyyy-MM-dd HH:mm:ss'}}"></vxe-table-column>
      <vxe-table-column field="date5" title="ElTimeSelect" width="200" :edit-render="{name: 'ElTimeSelect', props: {pickerOptions: {start: '08:30', step: '00:15', end: '18:30'}}}"></vxe-table-column>
      <vxe-table-column field="flag" title="ElSwitch" width="100" :edit-render="{name: 'ElSwitch', type: 'visible'}"></vxe-table-column>
      <vxe-table-column field="slider" title="ElSlider" width="200" :edit-render="{name: 'ElSlider', type: 'visible'}"></vxe-table-column>
      <vxe-table-column field="rate" title="ElRate" width="200" fixed="right" :edit-render="{name: 'ElRate', type: 'visible'}"></vxe-table-column>
    </vxe-table>

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="tablePage.currentPage"
      :page-sizes="[5, 10, 15, 20, 50, 100, 150, 200]"
      :page-size="tablePage.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="tablePage.totalResult">
    </el-pagination>

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
      tableData: [],
      validRules: {
        name: [
          { required: true, message: '名称必须填写' },
          { min: 3, max: 50, message: '名称长度在 3 到 50 个字符' }
        ],
        sex: [
          { required: true, message: '性别必须填写' }
        ]
      },
      sexList: [],
      regionList: [],
      restaurants: [
        { value: '前端', name: '前端' },
        { value: '后端', name: '后端' }
      ],
      tablePage: {
        currentPage: 1,
        pageSize: 10,
        totalResult: 0
      },
      formData: {
        name: null,
        sex: null
      },
      demoCodes: [
        `
          <el-form ref="tableform" class="click-table2-form" size="mini" :inline="true" :model="formData">
            <el-form-item title="名字" prop="name">
              <el-input v-model="formData.name" placeholder="名字"></el-input>
            </el-form-item>
            <el-form-item title="角色" prop="role">
              <el-input v-model="formData.role" placeholder="请输入角色"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="searchEvent">查询</el-button>
              <el-button @click="$refs.tableform.resetFields()">重置</el-button>
            </el-form-item>
          </el-form>

          <vxe-toolbar export setting>
            <template v-slot:buttons>
              <el-button @click="insertEvent">新增</el-button>
              <el-button @click="saveEvent">保存</el-button>
              <el-button @click="vaildEvent">校验</el-button>
              <el-dropdown @command="dropdownMenuEvent">
                <el-button>
                  操作<i class="el-icon-arrow-down el-icon--right"></i>
                </el-button>
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item command="remove">删除选中</el-dropdown-item>
                  <el-dropdown-item command="export">导出数据</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>
            </template>
          </vxe-toolbar>

          <vxe-table
            border
            resizable
            show-overflow
            highlight-hover-row
            ref="xTable"
            class="vxe-table-element"
            height="460"
            :loading="loading"
            :data="tableData"
            :edit-rules="validRules"
            :edit-config="{trigger: 'click', mode: 'row'}">
            <vxe-table-column type="checkbox" width="60" fixed="left"></vxe-table-column>
            <vxe-table-column type="index" width="60" fixed="left"></vxe-table-column>
            <vxe-table-column field="name" title="ElInput" min-width="140" :edit-render="{name: 'ElInput'}"></vxe-table-column>
            <vxe-table-column field="role" title="ElAutocomplete" width="160" :edit-render="{name: 'ElAutocomplete', props: {fetchSuggestions: roleFetchSuggestions}}"></vxe-table-column>
            <vxe-table-column field="age" title="ElInputNumber" width="160" :edit-render="{name: 'ElInputNumber', props: {max: 35, min: 18}}"></vxe-table-column>
            <vxe-table-column field="sex" title="ElSelect" width="140" :edit-render="{name: 'ElSelect', options: sexList}"></vxe-table-column>
            <vxe-table-column field="region" title="ElCascader" width="200" :edit-render="{name: 'ElCascader', props: {options: regionList}}"></vxe-table-column>
            <vxe-table-column field="date" title="ElDatePicker" width="200" :edit-render="{name: 'ElDatePicker', props: {type: 'date', format: 'yyyy/MM/dd'}}"></vxe-table-column>
            <vxe-table-column field="date1" title="DateTimePicker" width="220" :edit-render="{name: 'ElDatePicker', props: {type: 'datetime', format: 'yyyy-MM-dd HH:mm:ss'}}"></vxe-table-column>
            <vxe-table-column field="date5" title="ElTimeSelect" width="200" :edit-render="{name: 'ElTimeSelect', props: {pickerOptions: {start: '08:30', step: '00:15', end: '18:30'}}}"></vxe-table-column>
            <vxe-table-column field="flag" title="ElSwitch" width="100" :edit-render="{name: 'ElSwitch', type: 'visible'}"></vxe-table-column>
            <vxe-table-column field="slider" title="ElSlider" width="200" :edit-render="{name: 'ElSlider', type: 'visible'}"></vxe-table-column>
            <vxe-table-column field="rate" title="ElRate" width="200" fixed="right" :edit-render="{name: 'ElRate', type: 'visible'}"></vxe-table-column>
          </vxe-table>

          <el-pager
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="tablePage.currentPage"
            :page-sizes="[5, 10, 15, 20, 50, 100, 150, 200]"
            :page-size="tablePage.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="tablePage.totalResult">
          </el-pager>
        `,
        `
        export default {
          data () {
            return {
              loading: false,
              tableData: [],
              validRules: {
                name: [
                  { required: true, message: '名称必须填写' },
                  { min: 3, max: 50, message: '名称长度在 3 到 50 个字符' }
                ],
                sex: [
                  { required: true, message: '性别必须填写' }
                ]
              },
              sexList: [],
              regionList: [],
              restaurants: [
                { value: '前端', name: '前端' },
                { value: '后端', name: '后端' }
              ],
              tablePage: {
                currentPage: 1,
                pageSize: 10,
                totalResult: 0
              },
              formData: {
                name: null,
                sex: null
              }
            }
          },
          created () {
            this.findList()
            this.findSexList()
            this.findRegionList()
          },
          methods: {
            findList () {
              // 模拟后台数据
              this.loading = true
              this.$ajax.doGet(\`/api/user/page/list/\${this.tablePage.pageSize}/\${this.tablePage.currentPage}\`, this.formData).then(response => {
                let { page, result } = response.data
                this.tableData = result
                this.tablePage.totalResult = page.totalResult
                this.loading = false
              }).catch(e => {
                this.loading = false
              })
            },
            findSexList () {
              return this.$ajax.doGet('/api/conf/sex/list').then(({ data }) => {
                this.sexList = data
                return data
              })
            },
            findRegionList () {
              return this.$ajax.doGet('/api/conf/region/list').then(({ data }) => {
                this.regionList = data
                return data
              })
            },
            insertEvent () {
              let record = {
                role: '',
                age: 18,
                region: [],
                flag: false,
                rate: 2
              }
              this.$refs.xTable.insert(record).then(({ row }) => this.$refs.xTable.setActiveRow(row))
            },
            saveEvent () {
              let { insertRecords, removeRecords, updateRecords } = this.$refs.xTable.getRecordset()
              if (insertRecords.length || removeRecords.length || updateRecords.length) {
                this.$alert('保存成功！')
                this.searchEvent()
              } else {
                this.$alert('数据未改动！')
              }
            },
            dropdownMenuEvent (name) {
              switch (name) {
                case 'remove': {
                  let selectRecords = this.$refs.xTable.getSelectRecords()
                  if (selectRecords.length) {
                    this.$refs.xTable.removeSelecteds()
                  } else {
                    this.$alert('请至少选择一条数据！')
                  }
                  break
                }
                case 'export': {
                  this.$refs.xTable.exportData()
                  break
                }
              }
            },
            searchEvent () {
              this.tablePage.currentPage = 1
              this.findList()
            },
            vaildEvent () {
              this.$refs.xTable.validate(valid => {
                if (valid) {
                  this.$XModal.message({ status: 'success', message: '校验成功！' })
                } else {
                  this.$XModal.message({ status: 'error', message: '校验不通过！' })
                }
              })
            },
            handleSizeChange (pageSize) {
              this.tablePage.pageSize = pageSize
              this.searchEvent()
            },
            handleCurrentChange (currentPage) {
              this.tablePage.currentPage = currentPage
              this.findList()
            },
            roleFetchSuggestions (queryString, cb) {
              var restaurants = this.restaurants
              var results = queryString ? restaurants.filter(this.createStateFilter(queryString)) : restaurants
              clearTimeout(this.timeout)
              this.timeout = setTimeout(() => {
                cb(results)
              }, 3000 * Math.random())
            },
            createStateFilter (queryString) {
              return (state) => {
                return (state.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0)
              }
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.findList()
    this.findSexList()
    this.findRegionList()
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    findList () {
      this.loading = true
      this.$ajax.doGet(`/api/user/page/list/${this.tablePage.pageSize}/${this.tablePage.currentPage}`, this.formData).then(response => {
        let { page, result } = response.data
        this.tableData = result
        this.tablePage.totalResult = page.totalResult
        this.loading = false
      }).catch(e => {
        this.loading = false
      })
    },
    findSexList () {
      return this.$ajax.doGet('/api/conf/sex/list').then(({ data }) => {
        this.sexList = data
        return data
      })
    },
    findRegionList () {
      return this.$ajax.doGet('/api/conf/region/list').then(({ data }) => {
        this.regionList = data
        return data
      })
    },
    insertEvent () {
      let record = {
        role: '',
        age: 18,
        region: [],
        flag: false,
        rate: 2
      }
      this.$refs.xTable.insert(record).then(({ row }) => this.$refs.xTable.setActiveRow(row))
    },
    saveEvent () {
      let { insertRecords, removeRecords, updateRecords } = this.$refs.xTable.getRecordset()
      if (insertRecords.length || removeRecords.length || updateRecords.length) {
        this.$alert('保存成功！')
        this.searchEvent()
      } else {
        this.$alert('数据未改动！')
      }
    },
    vaildEvent () {
      this.$refs.xTable.validate(valid => {
        if (valid) {
          this.$XModal.message({ status: 'success', message: '校验成功！' })
        } else {
          this.$XModal.message({ status: 'error', message: '校验不通过！' })
        }
      })
    },
    dropdownMenuEvent (name) {
      switch (name) {
        case 'remove': {
          let selectRecords = this.$refs.xTable.getSelectRecords()
          if (selectRecords.length) {
            this.$refs.xTable.removeSelecteds()
          } else {
            this.$alert('请至少选择一条数据！')
          }
          break
        }
        case 'export': {
          this.$refs.xTable.exportData()
          break
        }
      }
    },
    searchEvent () {
      this.tablePage.currentPage = 1
      this.findList()
    },
    handleSizeChange (pageSize) {
      this.tablePage.pageSize = pageSize
      this.searchEvent()
    },
    handleCurrentChange (currentPage) {
      this.tablePage.currentPage = currentPage
      this.findList()
    },
    roleFetchSuggestions (queryString, cb) {
      var restaurants = this.restaurants
      var results = queryString ? restaurants.filter(this.createStateFilter(queryString)) : restaurants
      clearTimeout(this.timeout)
      this.timeout = setTimeout(() => {
        cb(results)
      }, 3000 * Math.random())
    },
    createStateFilter (queryString) {
      return (state) => {
        return (state.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0)
      }
    }
  }
}
</script>
