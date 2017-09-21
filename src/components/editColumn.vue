<template>
  <div class="editColumn">
    <el-table-column label="日期" width="200">
      <template scope="scope">
        <span v-show="!scope.row.editFlag">{{ scope.row.name }}</span>
        <span v-show="scope.row.editFlag" class="cell-edit-input"><el-input v-model="scope.row.name" @keyup.enter.native="handleSave($event, scope.row)" @blur="handleSave($event, scope.row)" ></span>
      </template>
    </el-table-column>
  </div>
</template>

<script>
export default {
  
}
</script>
