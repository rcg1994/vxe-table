<template>
  <div>
    <p class="tip">
      虚拟滚动渲染，加载 10 万行，左右固定列<br>
      大数据不建议使用双向绑定的 data 属性（vue 监听会大数据会短暂的卡顿），建议使用 <table-api-link prop="loadData"/>/<table-api-link prop="reloadData"/> 函数<br>
      对于多选 type=<table-column-api-link prop="checkbox"/> 当数据量海量时应该绑定 <table-api-link prop="checkField"/> 属性渲染速度更快<br>
      <span class="red">注意：如果要启用纵向虚拟滚动，所有的行高必须一致，否则无法兼容</span>
    </p>

    <vxe-toolbar>
      <template v-slot:buttons>
        <vxe-button @click="loadList(100000)">加载10w条</vxe-button>
        <vxe-button @click="$refs.xTable.toggleRowSelection($refs.xTable.getData(1))">切换第二行选中</vxe-button>
        <vxe-button @click="$refs.xTable.setSelection([$refs.xTable.getData(2), $refs.xTable.getData(3)], true)">设置第三、四行选中</vxe-button>
        <vxe-button @click="$refs.xTable.setAllSelection(true)">设置所有行选中</vxe-button>
        <vxe-button @click="$refs.xTable.clearSelection()">清除所有行选中</vxe-button>
        <vxe-button @click="getSelectEvent">获取选中</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      ref="xTable"
      border
      resizable
      show-overflow
      show-header-overflow
      highlight-hover-row
      highlight-current-row
      height="600"
      :loading="loading"
      :checkbox-config="{checkField: 'checked'}">
      <vxe-table-column type="checkbox" width="60" fixed="left"></vxe-table-column>
      <vxe-table-column type="index" width="100" fixed="left"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable width="200"></vxe-table-column>
      <vxe-table-column field="sex" title="Sex" width="200"></vxe-table-column>
      <vxe-table-column field="rate" title="Rate" width="200"></vxe-table-column>
      <vxe-table-column field="region" title="Region" width="200"></vxe-table-column>
      <vxe-table-column field="time" title="Time" width="200"></vxe-table-column>
      <vxe-table-column field="address" title="Address" width="300"></vxe-table-column>
      <vxe-table-column field="updateTime" title="UpdateTime" width="200"></vxe-table-column>
      <vxe-table-column field="createTime" title="CreateTime" width="200"></vxe-table-column>
      <vxe-table-column field="attr1" title="Attr1" width="200"></vxe-table-column>
      <vxe-table-column field="attr2" title="Attr2" width="200"></vxe-table-column>
      <vxe-table-column field="attr3" title="Attr3" width="200"></vxe-table-column>
      <vxe-table-column field="attr4" title="Attr4" width="200"></vxe-table-column>
      <vxe-table-column field="attr5" title="Attr5" width="200"></vxe-table-column>
      <vxe-table-column field="attr6" title="Attr6" width="200"></vxe-table-column>
      <vxe-table-column field="attr7" title="Attr7" width="200"></vxe-table-column>
      <vxe-table-column field="attr8" title="Attr8" width="200"></vxe-table-column>
      <vxe-table-column field="attr9" title="Attr9" width="200"></vxe-table-column>
      <vxe-table-column field="createTime" title="CreateTime" width="200"></vxe-table-column>
      <vxe-table-column field="age" title="Age" width="200" fixed="right"></vxe-table-column>
    </vxe-table>

    <pre>
      <code>
        | Arrow Up ↑ | 匀速向上滚动数据 |
        | Arrow Down ↓ | 匀速向下滚动数据 |
        | Arrow Left ← | 匀速向左滚动数据 |
        | Arrow Right → | 匀速向右滚动数据 |
        | Page Up | 向上翻页滚动 |
        | Page Down | 向下翻页滚动 |
        | Spacebar | 翻页滚动 |
        | Home | 滚动到顶部 |
        | End | 滚动到底部 |
      </code>
    </pre>

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
      demoCodes: [
        `
        <vxe-toolbar>
          <template v-slot:buttons>
            <vxe-button @click="loadList(100000)">加载10w条</vxe-button>
            <vxe-button @click="$refs.xTable.toggleRowSelection($refs.xTable.getData(1))">切换第二行选中</vxe-button>
            <vxe-button @click="$refs.xTable.setSelection([$refs.xTable.getData(2), $refs.xTable.getData(3)], true)">设置第三、四行选中</vxe-button>
            <vxe-button @click="$refs.xTable.setAllSelection(true)">设置所有行选中</vxe-button>
            <vxe-button @click="$refs.xTable.clearSelection()">清除所有行选中</vxe-button>
            <vxe-button @click="getSelectEvent">获取选中</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          ref="xTable"
          border
          resizable
          show-overflow
          show-header-overflow
          highlight-hover-row
          highlight-current-row
          height="600"
          :loading="loading"
          :checkbox-config="{checkField: 'checked'}">
          <vxe-table-column type="checkbox" width="60" fixed="left"></vxe-table-column>
          <vxe-table-column type="index" width="100" fixed="left"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable width="200"></vxe-table-column>
          <vxe-table-column field="sex" title="Sex" width="200"></vxe-table-column>
          <vxe-table-column field="rate" title="Rate" width="200"></vxe-table-column>
          <vxe-table-column field="region" title="Region" width="200"></vxe-table-column>
          <vxe-table-column field="time" title="Time" width="200"></vxe-table-column>
          <vxe-table-column field="address" title="Address" width="300"></vxe-table-column>
          <vxe-table-column field="updateTime" title="UpdateTime" width="200"></vxe-table-column>
          <vxe-table-column field="createTime" title="CreateTime" width="200"></vxe-table-column>
          <vxe-table-column field="attr1" title="Attr1" width="200"></vxe-table-column>
          <vxe-table-column field="attr2" title="Attr2" width="200"></vxe-table-column>
          <vxe-table-column field="attr3" title="Attr3" width="200"></vxe-table-column>
          <vxe-table-column field="attr4" title="Attr4" width="200"></vxe-table-column>
          <vxe-table-column field="attr5" title="Attr5" width="200"></vxe-table-column>
          <vxe-table-column field="attr6" title="Attr6" width="200"></vxe-table-column>
          <vxe-table-column field="attr7" title="Attr7" width="200"></vxe-table-column>
          <vxe-table-column field="attr8" title="Attr8" width="200"></vxe-table-column>
          <vxe-table-column field="attr9" title="Attr9" width="200"></vxe-table-column>
          <vxe-table-column field="createTime" title="CreateTime" width="200"></vxe-table-column>
          <vxe-table-column field="age" title="Age" width="200" fixed="right"></vxe-table-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              loading: false
            }
          },
          created () {
            this.loadList(600)
          },
          methods: {
            loadList (size) {
              this.loading = true
              setTimeout(() => {
                let xTable = this.$refs.xTable
                if (xTable) {
                  // 使用函数式加载，阻断 vue 对大数组的双向绑定，大数据性能翻倍提升
                  xTable.reloadData(window.MOCK_DATA_LIST.slice(0, size)).then(() => {
                    this.loading = false
                  })
                } else {
                  this.loading = false
                }
              }, 300)
            },
            getSelectEvent () {
              let selectRecords = this.$refs.xTable.getSelectRecords()
              this.$XModal.alert(selectRecords.length)
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.loadList(600)
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    loadList (size) {
      this.loading = true
      setTimeout(() => {
        let xTable = this.$refs.xTable
        if (xTable) {
          // 使用函数式加载，阻断 vue 对大数组的双向绑定，大数据性能翻倍提升
          xTable.reloadData(window.MOCK_DATA_LIST.slice(0, size)).then(() => {
            this.loading = false
          })
        } else {
          this.loading = false
        }
      }, 300)
    },
    getSelectEvent () {
      let selectRecords = this.$refs.xTable.getSelectRecords()
      this.$XModal.alert(selectRecords.length)
    }
  }
}
</script>
