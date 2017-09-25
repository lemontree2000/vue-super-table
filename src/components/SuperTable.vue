<template>
  <div class="super-table">
    <div class="table-wrapper">
      <el-table :data="data" @cell-dblclick="handleEdit">
        <slot></slot>
        <el-table-column fixed="right" label="操作" align="center" v-if="isOperation" width="120">
          <template scope="scope">
            <el-button type="success" size="mini" @click="addRow">添加</el-button>
            <el-button type="danger" size="mini" @click="deleteRow(scope.$index, scope.rows)">删除</el-button>
          </template>
        </el-table-column>
        <el-table-column :label="item.label" width="200" :prop="item.prop" v-for="item in editData" :key="item.prop" align="center">
          <template scope="scope">
            <span>{{ scope.row[item.prop] }}</span>
            <span class="cell-edit-input" style="display: none;" id="span">
              <el-input v-model="scope.row[item.prop]" class="e-input"  @blur="handleSave($event, scope.row)">
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
    },
    editData: {
      type: Array,
      defalut: []
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
    _handleEdit: function(row, column, cell, event) {
      var oInput = cell.querySelector('.e-input input');
      if (!oInput || row.disable) return;
      var oSpan2 = cell.getElementsByTagName('span')[1];
      var oSpan1 = cell.getElementsByTagName('span')[0];
      oSpan2.style.display = 'block';
      oSpan1.style.display = 'none';
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
      var textSpan = e.path[3].getElementsByTagName('span')[0];
      var inputSpan = e.path[3].getElementsByTagName('span')[1];
      inputSpan.style.display = 'none';
      textSpan.style.display = 'block';
    },
    handleSave: function(e, row) {
      this.$emit('handle-save', e, row);
      this._handleSave(e, row);
    }
  },
  created() {
    this._initAll();
  }
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.super-table {}
</style>
