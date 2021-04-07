<template>
  <div class="home">
    <el-card class="box-card">
      <template #header>
        <div class="card-header">
          <h2>Convert XLSX to CSV</h2>
        </div>
      </template>

      <el-upload
        class="upload-demo"
        action="https://jsonplaceholder.typicode.com/posts/"
        :on-success="handleSuccess"
        :on-remove="handleRemove"
        :limit="1"
        :file-list="fileList"
        :multiple="false"
      >
        <el-button size="small" type="primary">Click to upload</el-button>
        <template #tip>
          <div class="el-upload__tip">xlsx file</div>
        </template>
      </el-upload>
      <br />

      <el-button type="success">Download CSV</el-button>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "Home",
  props: {},
  data() {
    return {
      fileList: [],
    };
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handleSuccess(response, file, fileList) {
      console.log(response, file, fileList);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.box-card {
  width: 480px;
}
</style>
