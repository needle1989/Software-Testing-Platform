<template>
  <div class="all-container">
    <div class="thetop"><h1>自定义测试用例</h1></div>
    <div class="tableTitle"></div>
    <section>
      <el-row style="padding-top: 20px; padding-left: 50px" :gutter="100">
        <el-col :span="8">
          <div>
            <el-form
              :model="ruleForm"
              status-icon
              :rules="rules"
              ref="ruleForm"
              class="demo-ruleForm"
            >
              <el-form-item label="主机" prop="host">
                <el-input v-model.number="ruleForm.host"></el-input>
              </el-form-item>
              <el-form-item label="显示器" prop="monitor">
                <el-input v-model.number="ruleForm.monitor"></el-input>
              </el-form-item>
              <el-form-item label="外设" prop="io">
                <el-input v-model.number="ruleForm.io"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button
                  type="primary"
                  @click="submitForm()"
                  style="width: 100%"
                  >SUBMIT</el-button
                >
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="10">
          <div style="padding-top: 300px">销售额：{{ sales }}</div>
          <div>销售佣金：{{ commission }}</div>
        </el-col>
      </el-row>
    </section>

    <div class="thetop"><h1>预设测试用例</h1></div>
    <div class="tableTitle"></div>
    <el-row :gutter="20">
      <el-col :span="6">
        <div style="padding-left: 50px; padding-top: 20px">
          <el-select v-model="value" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </div>
      </el-col>
      <el-col :span="6">
        <div style="padding-top: 20px">
          <el-button type="primary" @click="submitOption()" style="width: 100px"
            >SUBMIT</el-button
          >
        </div>
      </el-col>
    </el-row>
    <div style="padding-left: 50px; padding-top: 30px">
      result:<br />{{ result }}
    </div>
    <div style="padding-left: 50px">
      <el-table
        :key="itemkey"
        :data="resultTable"
        style="width: 62.5rem"
        border
        :stripe="true"
      >
        <el-table-column prop="host" label="主机"> </el-table-column>
        <el-table-column prop="monitor" label="显示器"> </el-table-column>
        <el-table-column prop="io" label="外设"> </el-table-column>
        <el-table-column prop="expectSales" label="预期销售额">
        </el-table-column>
        <el-table-column prop="actualSales" label="实际销售额">
        </el-table-column>
        <el-table-column prop="expectCommission" label="预期佣金">
        </el-table-column>
        <el-table-column prop="actualCommission" label="实际佣金">
        </el-table-column>
        <el-table-column prop="ifCorrect" label="是否正确"> </el-table-column>
      </el-table>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    var checknum = (rule, value, callback) => {
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error("Please enter a number"));
        } else if (value < 0) {
          callback(new Error("Please enter a natural number"));
        } else {
          callback();
        }
      }, 1000);
    };
    var check = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入内容"));
      } else {
        callback();
      }
    };
    return {
      itemkey: "",
      resultTable: JSON.parse(window.sessionStorage.getItem("table")),
      ruleForm: {
        host: "",
        monitor: "",
        io: "",
      },
      rules: {
        host: [{ validator: checknum, trigger: "blur" }],
        monitor: [{ validator: checknum, trigger: "blur" }],
        io: [{ validator: checknum, trigger: "blur" }],
      },
      options: [
        {
          value: "选项1",
          label: "根据输入划分边界值",
        },
        {
          value: "选项2",
          label: "根据输出划分边界值",
        },
      ],
      value: "",
      sales: "",
      commission: "",
      result: "",
    };
  },
  methods: {
    submitForm() {
      //console.log(this.ruleForm);
      let _this = this;
      if (
        this.ruleForm.host == "0" &&
        this.ruleForm.monitor == "0" &&
        this.ruleForm.io == "0"
      ) {
        alert("至少卖出一台设备！");
        return;
      }
      this.$http
        .post("/api/questionThree/manualtest", this.ruleForm)
        .then(function (response) {
          console.log(response);
          _this.sales = response.data.sales;
          _this.commission = response.data.commission;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    submitOption() {
      let _this = this;
      console.log(this.itemkey);
      if (this.value == "选项1") {
        _this.$http
          .post("/api/questionThree/boundaryInput")
          .then(function (response) {
            var data = JSON.stringify(response.data.data);
            console.log(data);
            window.sessionStorage.setItem("table", data);
            this.$forceUpdate();
            _this.itemkey = !_this.itemkey;
          });
      } else {
        _this.$http
          .post("/api/questionThree/boundaryOutput")
          .then(function (response) {
            var data = JSON.stringify(response.data.data);
            console.log(data);
            window.sessionStorage.setItem("table", data);
            this.$forceUpdate();
            _this.itemkey = !_this.itemkey;
          });
      }
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};
</script>
<style scoped>
</style>
