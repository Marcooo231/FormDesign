<template>
  <div>
    <p class="table-title">
      <span>联系人</span>
      <Button size="small"
              type="primary"
              @click="addData">新增</Button>
    </p>
    <i-table :columns="columns"
             :data="originData"
             width="100%"></i-table>
    <showColumnDetailModal :visible="visible"
                           @close="visible = false"
                           :columnsDetailForModal="columnsDetailForModal"></showColumnDetailModal>
    <createItem :visible="createItemModalVisible"
                :isEdit="isEdit"
                :defaultData="defaultData"
                :editData="editDetailForrModal"
                :tableColumnConfigList="tableConfig"
                @close="createItemModalVisible = false"
                @getVal="getVal"></createItem>
  </div>
</template>
<script>
import common from './detail.common.js'
export default {
  mixins: [common],
  props: {
    customerData: Object
  },
  data () {
    return {
      defaultData: {
        compange_name: this.customerData.id
      }
    }
  },
  created () {
  },
  watch: {
    originData (v) {
      console.log(v)
    }
  },
  computed: {
    columns () {
      return [...this.originColumns, {
        title: '操作',
        key: 'action',
        width: this.isWechat ? 100 : 150,
        fixed: 'right',
        align: 'center',
        render: (h, params) => {
          return h('div', !this.isWechat ? [
            h(
              'Button',
              {
                props: {
                  type: 'primary',
                  size: 'small'
                },
                on: {
                  click: () => {
                    this.checkDetail(params)
                  }
                },
                style: {
                  'margin-right': '5px'
                }
              },
              '查看'
            ),
            h(
              'Button',
              {
                props: {
                  type: 'info',
                  size: 'small'
                },
                on: {
                  click: () => {
                    this.editData(params)
                  }
                }
              },
              '修改'
            )
          ]
            : [
              h(
                'Button',
                {
                  props: {
                    type: 'primary',
                    size: 'small'
                  },
                  on: {
                    click: () => {
                      this.checkDetail(params)
                    }
                  },
                  style: {
                    'margin-right': '5px'
                  }
                },
                '查看/修改'
              )
            ])
        }
      }]
    }
  }
}
</script>
<style scoped>
.table-title {
  display: flex;
  justify-content: space-between;
  font-size: 15px;
  margin: 0 10px;
  margin-bottom: 10px;
  font-weight: bold;
}
</style>
