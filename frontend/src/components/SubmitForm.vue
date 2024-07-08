<template>
  <div>
    <el-select v-model="selectedOption" placeholder="请选择提交选项">
      <el-option label="Cookie" value="cookie"></el-option>
      <el-option label="WSCK" value="wsck"></el-option>
      <el-option label="Token1" value="token1"></el-option>
      <el-option label="Token2" value="token2"></el-option>
    </el-select>
    <el-input
      v-model="inputValue"
      placeholder="请输入对应的值"
      clearable
      class="my-4 w-full"
    ></el-input>
    <el-button type="primary" @click="submit">提交</el-button>
  </div>
</template>

<script>
import { ref } from "vue";
import { ElMessage } from "element-plus";
import axios from "axios";

export default {
  setup() {
    const selectedOption = ref("");
    const inputValue = ref("");

    const submit = async () => {
      if (!selectedOption.value || !inputValue.value) {
        ElMessage.error("请选择一个选项并输入对应的值");
        return;
      }

      let apiUrl;
      switch (selectedOption.value) {
        case "cookie":
          apiUrl = "/api/cookie";
          break;
        case "wsck":
          apiUrl = "/api/wsck";
          break;
        case "token1":
          apiUrl = "/api/token1";
          break;
        case "token2":
          apiUrl = "/api/token2";
          break;
        default:
          ElMessage.error("无效的选项");
          return;
      }

      try {
        const response = await axios.post(apiUrl, { value: inputValue.value });
        if (response.data.success) {
          ElMessage.success("提交成功");
        } else {
          ElMessage.error("提交失败");
        }
      } catch (error) {
        ElMessage.error("提交失败");
      }
    };

    return {
      selectedOption,
      inputValue,
      submit,
    };
  },
};
</script>

<style scoped>
.el-select {
  width: 100%;
  margin-bottom: 1rem;
}
</style>
