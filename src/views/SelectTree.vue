<template>
  <van-dropdown-menu>
    <van-dropdown-item :title="chosen.text" ref="dropitem">
      <van-tree-select :main-active-index.sync="activeIndex" :items="items" @click-nav="handleClickNav">
        <template #content>
          <div class="content-class" v-if="rowList">
            <div v-for="(row, rowIndex) in rowList" :key="rowIndex" class="rowlist">
              <div v-for="(column, cIndex) in row.children" :key="cIndex" class="column-item"
                   :style="{color:row.chosenId === column.id ? 'red' : '#282828'}" @click="handleClick(row, rowIndex, column, cIndex)">
                {{ column.text }}
              </div>
            </div>
          </div>
        </template>
      </van-tree-select>
    </van-dropdown-item>
  </van-dropdown-menu>
</template>

<script>
import { DropdownMenu, DropdownItem, TreeSelect } from 'vant'
export default {
  name: 'SelectTree',
  components: {
    [DropdownMenu.name]: DropdownMenu,
    [DropdownItem.name]: DropdownItem,
    [TreeSelect.name]: TreeSelect
  },
  data () {
    return {
      activeIndex: -1,
      chosen: {},
      items: [
        {
          id: '1',
          text: 'item1',
          children: [{
            id: '11',
            text: 'children11',
            children: [{ id: '111', text: 'children111' }, { id: '112', text: 'children112' }, {
              id: '113',
              text: 'children113',
              children: [{
                id: '1131',
                text: 'children1131',
                children: [{ id: '11311', text: 'children11311' }, { id: '11312', text: 'children11312' }, {
                  id: '11313',
                  text: 'children11313'
                }]
              }, { id: '1132', text: 'children1132' }]
            }]
          },
          { id: '12', text: 'children12' },
          { id: '13', text: 'children13' }]
        },
        {
          id: '2',
          text: 'item2',
          children: [{ id: '21', text: 'children21' }, { id: '22', text: 'children22' }, { id: '23', text: 'children23' }]
        },
        { id: '3', text: 'item3' }
      ],
      rowList: []
    }
  },
  mounted () {
    this.getData()
  },
  methods: {
    getData () {
      this.addNode(this.items)
    },
    addNode (list) {
      if (!list || list.length <= 0) {
        return ''
      }
      list.unshift({ text: '全部', id: '-1' })
      list.map(item => {
        if (item.children && item.children.length > 0) {
          this.addNode(item.children)
        }
      })
    },
    handleClickNav () {
      this.rowList.map((ri, index) => {
        ri.chosenId = ''
      })
      this.rowList = []
      const item = this.items[this.activeIndex]
      this.chosen = { ...item }
      if (item && item.children && item.children.length > 0) {
        this.rowList.push(item)
      } else {
        this.$refs.dropitem.toggle()
      }
    },
    handleClick (row, rowIndex, column, cIndex) {
      this.addData(row, rowIndex, column, cIndex)
    },
    addData (row, rowIndex, column, cIndex) {
      if (rowIndex + 1 === this.rowList.length) {
        row.chosenId = column.id
        this.chosen = { ...column }
        if (column && column.children && column.children.length > 0) {
          this.rowList.push(column)
        } else {
          this.$refs.dropitem.toggle()
        }
      } else {
        this.rowList.map((ri, index) => {
          if (index >= (rowIndex + 1)) {
            ri.chosenId = ''
          }
        })
        this.rowList = this.rowList.slice(0, rowIndex + 1)
        this.addData(row, rowIndex, column, cIndex)
      }
    }
  }
}
</script>

<style scoped lang="scss">
.content-class {
  display: flex;
  flex-direction: row;
  overflow: scroll;
}
.rowlist {
  width: 100px;
  min-height: 300px;
  margin: 0 10px;
  background: white;
  .column-item {
    display: flex;
    flex-direction: row;
    align-items: center;
    height: 40px;
    border-bottom: 1px solid floralwhite;
  }
}
</style>
