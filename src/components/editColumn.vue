<template>
  <div class="editColumn">
    <el-table-column label="日期" width="200">
      <template scope="scope">
        <span v-show="!scope.row.editFlag">{{ scope.row.name }}</span>
        <span v-show="scope.row.editFlag" class="cell-edit-input">
          <el-input v-model="scope.row.name" @keyup.enter.native="handleSave($event, scope.row)" @blur="handleSave($event, scope.row)" >
          </el-input>
        </span>
      </template>
    </el-table-column>
  </div>
</template>

<script>
export default {
  props: {
    prop: {
      type: String,
      default: ''
    }
  },
  methods: {
    handleEdit: function(row, column, event) {
      this.$set(row, 'editFlag', true);
      var oInput = event.getElementsByTagName('input')[0];
      setTimeout(function() {
        oInput.focus();
      }, 0);
    },
    handleSave: function(e, row) {
      // 保存数据，向后台取数据
      this.$set(row, 'editFlag', false);
    }
  }
};
</script>
