<template>
  <div>
    <p class="tip">展开行</p>

    <vxe-table
      border
      resizable
      :data="tableData"
      :edit-config="{trigger: 'click', mode: 'cell'}">
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column type="expand" width="60">
        <template v-slot="{ row, rowIndex }">
          <ul class="expand-form">
            <li>
              <span class="title">Name：</span>
              <span class="content">
                <vxe-input v-model="row.name"></vxe-input>
              </span>
            </li>
            <li>
              <span class="title">Sex：</span>
              <span class="content">
                <vxe-input v-model="row.sex"></vxe-input>
              </span>
            </li>
            <li>
              <span class="title">Age：</span>
              <span class="content">
                <vxe-input v-model="row.age"></vxe-input>
              </span>
            </li>
          </ul>
        </template>
      </vxe-table-column>
      <vxe-table-column field="name" title="Name" :edit-render="{name: 'input'}"></vxe-table-column>
      <vxe-table-column field="sex" title="Sex" :edit-render="{name: 'input'}"></vxe-table-column>
      <vxe-table-column field="date" title="Date" :edit-render="{name: 'input'}"></vxe-table-column>
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
        <vxe-table
          border
          resizable
          :data="tableData"
          :edit-config="{trigger: 'click', mode: 'cell'}">
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column type="expand" width="60">
            <template v-slot="{ row, rowIndex }">
              <ul class="expand-form">
                <li>
                  <span class="title">Name：</span>
                  <span class="content">
                    <vxe-input v-model="row.name"></vxe-input>
                  </span>
                </li>
                <li>
                  <span class="title">Sex：</span>
                  <span class="content">
                    <vxe-input v-model="row.sex"></vxe-input>
                  </span>
                </li>
                <li>
                  <span class="title">Age：</span>
                  <span class="content">
                    <vxe-input v-model="row.age"></vxe-input>
                  </span>
                </li>
              </ul>
            </template>
          </vxe-table-column>
          <vxe-table-column field="name" title="Name" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="sex" title="Sex" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="date" title="Date" :edit-render="{name: 'input'}"></vxe-table-column>
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
          }
        }
        `,
        `
        .expand-form li {
          margin: 5px 0;
        }
        .expand-form li .title,
        .expand-form li .content {
          display: inline-block;
        }
        .expand-form li .title {
          width: 100px;
        }
        .expand-form li .content {
          width: 200px;
        }
        `
      ]
    }
  },
  created () {
    this.tableData = window.MOCK_DATA_LIST.slice(0, 6)
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  }
}
</script>

<style lang="scss" scoped>
.expand-form li {
  margin: 5px 0;
}
.expand-form li .title,
.expand-form li .content {
  display: inline-block;
}
.expand-form li .title {
  width: 100px;
}
.expand-form li .content {
  width: 200px;
}
</style>
