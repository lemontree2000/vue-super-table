<template>
  <div class="super-table">
    <div class="table-wrapper">
      <el-table :data="data" @cell-dblclick="handleEdit">
        <slot></slot>
        <el-table-column
          fixed="right"
          label="操作"
          align="center"
          v-if="isOperation"
          width="120">
          <template scope="scope">
            <el-button type="success" size="mini" @click="addRow">添加</el-button>
            <el-button type="danger" size="mini" @click="deleteRow(scope.$index, scope.rows)">删除</el-button>
          </template>
        </el-table-column>
        <el-table-column label="日期" width="200" prop="name">
          <template scope="scope">
            <span v-show="!scope.row.editFlag">{{ scope.row.name }}</span>
            <span v-show="scope.row.editFlag" class="cell-edit-input">
              <el-input v-model="scope.row.name" @keyup.enter.native="handleSave($event, scope.row)" @blur="handleSave($event, scope.row)" >
              </el-input>
            </span>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'v-super-table',
  props: {
    propData: {
      type: Object,
      defalut: null
    },
    isOperation: {
      type: Boolean,
      defalut: false
    }
  },
  data() {
    return {
      data: []
    };
  },
  methods: {
    _initAll() {
      this._getProps();
    },
    _getProps() {
      if (this.propData) {
        this.data.push(this.propData);
      } else {
        throw new Error('请传入propDatas');
      }
    },
    addRow() {
      if (this.propData) {
        let opData = JSON.parse(JSON.stringify(this.propData));
        this.data.push(opData);
      }
    },
    deleteRow(index) {
      if (this.data.length !== 1) {
        this.data.splice(index, 1);
      }
    },
    _handleEdit: function(row, column, event) {
      this.$set(row, 'editFlag', true);
      var oInput = event.getElementsByTagName('input')[0];
      setTimeout(function() {
        oInput.focus();
      }, 0);
    },
    handleEdit(row, column, event) {
      this.$emit('cell-dblclick', row, column, event);
      this._handleEdit(row, column, event);
    },
    _handleSave: function(e, row) {
      // 保存数据，向后台取数据
      this.$set(row, 'editFlag', false);
    }
  },
  created() {
    this._initAll();
  }
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
  .super-table {

  }
</style>
