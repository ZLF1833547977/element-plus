<template>
  <div>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="name" label="姓名">
        <template #default="{ row, $index }">
          <el-form-item :prop="`tableData[${$index}].name`" :rules="rules.name">
            <el-input v-model="row.name" @blur="validateCell(row, 'name')"></el-input>
          </el-form-item>
        </template>
      </el-table-column>
      <el-table-column prop="age" label="年龄">
        <template #default="{ row, $index }">
          <el-form-item :prop="`tableData[${$index}].age`" :rules="rules.age">
            <el-input v-model="row.age" @blur="validateCell(row, 'age')"></el-input>
          </el-form-item>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { ElTable, ElTableColumn, ElInput, ElFormItem } from 'element-plus';

const tableData = ref([
  { name: '', age: '' },
  { name: '', age: '' }
]);

const rules = ref({
  name: [
    { required: true, message: '姓名不能为空', trigger: 'blur' }
  ],
  age: [
    { required: true, message: '年龄不能为空', trigger: 'blur' },
    { type: 'number', message: '年龄必须为数字', trigger: 'blur' }
  ]
});

const validateCell = (row, prop) => {
  const formItem = document.querySelector(`[prop="tableData[${tableData.value.indexOf(row)}].${prop}"]`);
  if (formItem) {
    const form = formItem.closest('.el-form');
    if (form) {
      const formInstance = form.__vueParentComponent;
      if (formInstance) {
        formInstance.validateField(`tableData[${tableData.value.indexOf(row)}].${prop}`, (errors) => {
          if (errors) {
            console.log(errors);
          }
        });
      }
    }
  }
};
</script>

<style scoped>
/* 可根据需要添加样式 */
</style>   
