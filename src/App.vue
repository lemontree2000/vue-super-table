<template>
  <div id="app">
    <img src="./assets/logo.png" @click="getData">
    <super-table :editData="editDatas" @handle-save="save" @cell-dblclick="handleEdit" :propData="propDatas" :isOperation="true" ref="superTable">
      <el-table-column prop="date" label="日期" align="center">
      </el-table-column>
      <el-table-column prop="address" label="地址" align="center">
      </el-table-column>
      <el-table-column prop="tel" label="电话" align="center">
      </el-table-column>
      <el-table-column prop="address" label="地址" align="center">
      </el-table-column>
      <el-table-column prop="address" label="地址" align="center">
      </el-table-column>
    </super-table>
  </div>
</template>

<script>
import SuperTable from './components/SuperTable';

export default {
  name: 'app',
  components: {
    SuperTable
  },
  data() {
    return {
      propDatas: {
        name: '郭涛',
        date: '今天',
        address: '我家',
        tel: '13755551'
      },
      editDatas: [
        {
          prop: 'name',
          label: '名称'
        },
        {
          prop: 'address',
          label: '地址'
        }
      ]
    };
  },
  methods: {
    getData() {
      console.log(this.$refs.superTable.data);
    },
    handleEdit(row, column, cell, event) {
      console.log(423423);
    },
    save(e, row) {
      console.log(e, row);
    }
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  padding: 30px;
  text-align: center;
}
</style>
