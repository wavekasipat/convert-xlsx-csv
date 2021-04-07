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
        :on-change="handleChange"
        :on-remove="handleRemove"
        :limit="2"
        :multiple="false"
        accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
        :auto-upload="false"
        ref="upload"
        :on-exceed="handleExceed"
        :file-list="fileList"
      >
        <el-button size="small" type="primary">Click to upload</el-button>
        <template #tip>
          <div class="el-upload__tip">xlsx file</div>
        </template>
      </el-upload>
      <br />

      <a v-if="file" :href="csv" :download="csvName"
        ><el-button type="success">Download CSV</el-button></a
      >
    </el-card>
  </div>
</template>

<script>
const XLSX = require("xlsx");

function readFileAsync(file) {
  return new Promise((resolve, reject) => {
    let reader = new FileReader();
    reader.onload = () => {
      resolve(reader.result);
    };
    reader.onerror = reject;
    reader.readAsArrayBuffer(file);
  });
}

export default {
  name: "Home",
  props: {},
  data() {
    return {
      fileList: [],
      file: null,
      csv: null,
      csvName: null,
    };
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
      this.file = null;
    },
    handleExceed(files, fileList) {
      console.log(files, fileList);
      // this.file = null;
    },
    async handleChange(file, fileList) {
      console.log(file, fileList);
      if (fileList.length > 1) {
        this.fileList = fileList.slice(1);
      }
      this.file = file;
      this.csvName = file.name.replace(".xlsx", ".csv").replace(".xls", ".csv");

      let contentBuffer = await readFileAsync(file.raw);
      // console.log("contentBuffer", contentBuffer);
      let data = new Uint8Array(contentBuffer);
      let workbook = XLSX.read(data, { type: "array" });
      let csvText = XLSX.utils.sheet_to_csv(
        workbook.Sheets[workbook.SheetNames[0]]
      );
      // console.log(csvText);
      this.csv = "data:text/csv;charset=utf-8," + csvText;
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
h2 {
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>
