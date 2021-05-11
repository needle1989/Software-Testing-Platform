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
              <el-form-item label="CALL TIME" prop="time">
                <el-input v-model.number="ruleForm.host"></el-input>
              </el-form-item>
              <el-form-item label="UNPAID NUM" prop="unpaid">
                <el-input v-model.number="ruleForm.monitor"></el-input>
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
          <div style="padding-top: 300px">COST:{{ cost }}</div>
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
    <div>
        <el-table :data="testResult">
          <el-table-column label="TEST CASE ID" align="center" prop="testCaseId"/>
          <el-table-column label="CALL TIME" align="center" prop="callTime"/>
          <el-table-column label="UNPAID NUM" align="center" prop="unpaidNum"/>
          <el-table-column label="EXPECTED OUTPUT" align="center" prop="expectedOutput"/>
          <el-table-column label="ACTUAL OUTPUT" align="center" prop="actualOutput"/>
          <el-table-column label="CORRECTNESS" align="center" prop="correctness"/>
          <el-table-column label="TESTER" align="center" prop="tester"/>
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
    return {
      itemkey: "",
      resultTable: JSON.parse(window.sessionStorage.getItem("table")),
      ruleForm: {
        time: "",
        unpaid: "",
      },
      rules: {
        time: [{ validator: checknum, trigger: "blur" }],
        unpaid: [{ validator: checknum, trigger: "blur" }],
      },
      options: [
        {
          value: "选项1",
          label: "边界类",
        },
        {
          value: "选项2",
          label: "等价类",
        },
      ],
      value: "",
      cost: "",
      commission: "",
      result: "",
      testResult:[]
    };
  },
  methods: {
    submit() {
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
        .post("/api/Q6/manual", this.ruleForm)
        .then(function (response) {
          console.log(response);
          _this.cost = response.data.cost;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    submitForm: function () {
      this.$http.post('http://localhost:10086/api/questionThree/manualtest',this.ruleForm)
      .then((res)=>{
        console.log(res.data.commission)
        this.commission = res.data.commission
        this.message = res.data.message
        this.sales = res.data.sales
        console.log(this.commission)
      })
    },
    submitOption() {
      let _this = this;
      console.log(this.itemkey);
      if (this.value == "选项1") {
        this.$http.post('http://localhost:10086/api/Q6/boundary')
          .then((res)=>{
            this.testResult = res.data.data
          })
      } else {
        this.$http.post('http://localhost:10086/api/Q6/equivalence')
          .then((res)=>{
            this.testResult = res.data.data
          })
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
