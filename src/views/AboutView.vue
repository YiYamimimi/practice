<template>
  <div>
    <!-- <span style="margin-right:10px">
      <input class="input-file" type="file" @change="exportData"
        accept=".csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel" />
      <el-button type="primary" size="mini" icon="el-icon-upload" @click="btnClick">上传Excel表格数据</el-button>
    </span> -->
    <!-- 导出按钮 -->
    <el-button type="primary" style="margin:20px;" @click="exportExcelSelect">导出Excel</el-button>

    <!-- 原始表格 -->
    <el-table :data="tableData" @selection-change="handleSelectionChange">
      <el-table-column type="selection"></el-table-column>
      <el-table-column prop="date" label="日期"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
      <el-table-column label="详细地址">
        <el-table-column prop="province" label="省份"></el-table-column>
        <el-table-column prop="city" label="市区"></el-table-column>
        <el-table-column prop="address" label="地址"></el-table-column>
        <el-table-column prop="email" label="邮编"></el-table-column>
      </el-table-column>
      <el-table-column fixed="right" label="操作">
        <template>
          <el-button type="text" size="small">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 预览弹窗表格 -->
    <el-dialog title="表格保存预览" width="70%" :visible.sync="selectWindow">
      <el-table :data="selectData" id="selectTable" height="380px">
        <el-table-column prop="date" label="日期"></el-table-column>
        <el-table-column prop="name" label="姓名"></el-table-column>
        <el-table-column label="详细地址">
          <el-table-column prop="province" label="省份"></el-table-column>
          <el-table-column prop="city" label="市区"></el-table-column>
          <el-table-column prop="address" label="地址"></el-table-column>
          <el-table-column prop="email" label="邮编"></el-table-column>
        </el-table-column>
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="exportExcel(selectData)">确定保存</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import XLSX from "xlsx";

export default {
  data() {
    return {
      // 表格数据
      tableData: [
        { date: '2023-03-31', name: '表格导出', province: '北京', city: '朝阳区', address: '北京市朝阳区红海路123号', email: '123456' },
        { date: '2023-03-31', name: '表格导出', province: '北京', city: '朝阳区', address: '北京市朝阳区红海路123号', email: '123456' },
        { date: '2023-03-31', name: '表格导出', province: '北京', city: '朝阳区', address: '北京市朝阳区红海路123号', email: '123456' },
      ],
      // 表格中选中的数据
      selectData: [],
      selectWindow: false,
    };
  },
  methods: {
    // 格式化数据
    // getExcel(dom, title = '默认标题') {
    //   var excelTitle = title;
    //   var wb = XLSX.utils.table_to_book(document.querySelector(dom));
    //   /* 获取二进制字符串作为输出 */
    //   var wbout = XLSX.write(wb, { bookType: "xlsx", bookSST: true, type: "array" });
    //   try {
    //     FileSaver.saveAs(new Blob([wbout], { type: "application/octet-stream" }), excelTitle + ".xlsx");
    //   } catch (e) {
    //     if (typeof console != "undefined") console.log(e, wbout);
    //   }
    //   return wbout;
    // },
    exportExcel(selectData) {
  const ws = XLSX.utils.json_to_sheet(selectData);
  const wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, 'Sheet1');
  XLSX.writeFile(wb, `111.xlsx`);
},
    // 导出
    // exportExcel() {
    //   this.getExcel('#selectTable', '导出的自定义标题');
    // },
    // 显示预览弹窗
    exportExcelSelect() {
      if (this.selectData.length < 1) {
        this.$message.error('请选择要导出的内容!');
        return false;
      }
      this.selectWindow = true;
    },
    // 选中数据
    handleSelectionChange(val) {
      this.selectData = val;
    },
  }
}
</script>
