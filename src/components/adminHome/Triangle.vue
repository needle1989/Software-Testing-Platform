<template>
  <div class="all-container">
    <div class="thetop"><h1>自定义测试用例</h1></div>
    <div class="tableTitle"></div>
	  <section class="theform">
    <el-form :model="ruleForm"
             status-icon
             :rules="rules"
             ref="ruleForm"
             label-width="100px"
             class="demo-ruleForm">
      <el-form-item label="Edge A"
                    prop="edgeA">
        <el-input v-model.number="ruleForm.edgeA"
                  style="width:300px;"></el-input>
      </el-form-item>
      <el-form-item label="Edge B"
                    prop="edgeB">
        <el-input v-model.number="ruleForm.edgeB"
                  style="width:300px;"></el-input>
      </el-form-item>
      <el-form-item label="Edge C"
                    prop="edgeC">
        <el-input v-model.number="ruleForm.edgeC"
                  style="width:300px;"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary"
                           @click="submitForm('ruleForm')" style="width:100%;">SUBMIT</el-button>
      </el-form-item>
    </el-form></section>
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
    <div>
        <el-table :data="testResult">
          <el-table-column label="Edge A" align="center" prop="side1"/>
          <el-table-column label="Edge A" align="center" prop="side2"/>
          <el-table-column label="Edge A" align="center" prop="side3"/>
          <el-table-column label="Expected Output" align="center" prop="expectType"/>
          <el-table-column label="Actual Output" align="center" prop="actualType"/>
          <el-table-column label="Correctness" align="center" prop="ifCorrect"/>
        </el-table>
      </div>
  </div>
</template>
<script>
export default {
  data () {
    var checknum = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('Number can not be null'))
      }
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error('Please enter a number'))
        } else {
            callback()
          }
      }, 1000)
    }
    var check = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入内容'))
      } else { callback() }
    }
    return {
      itemkey: "",
      ruleForm: {
        edgeA: '',
        edgeB: '',
        edgeC: '',
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
      rules: {
        edgeA: [
          { validator: checknum, trigger: 'blur' }
        ],
        edgeB: [
          { validator: checknum, trigger: 'blur' }
        ],
        edgeC: [
          { validator: checknum, trigger: 'blur' }
        ]

      },
      testResult:[]
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs.ruleForm.validate(async valid => {
        if (!valid) return;
        let Token = window.sessionStorage.getItem('token');
        let msg = "";
        let status = 200;
        let edgeA = this.ruleForm.edgeA;
        let edgeB = this.ruleForm.edgeB;
        let degeC = this.ruleForm.edgeC;
        names = encodeURI(encodeURI(names));
        genders = encodeURI(encodeURI(genders));
        let result = await this.$http.put('/apip/api/adminop/modreaderinfo?id=' + ids + '&name=' + names + '&contact=' + contacts + '&gender=' + genders,
          {
            hearders: {
              'Authorization': Token,
            }
          })
          .catch(function (error) {
            if (error.response) {
              status = error.response.status;
              msg = error.response.data.msg;
            }
          });
        let inf = result.data.error;
        if (inf === undefined) {
          inf = 'submit successfully'
          this.$message.info(inf);
        }
        else {
          this.$message.info(inf);
        }
      });
    },
    submitOption() {
      let _this = this;
      console.log(this.itemkey);
      if (this.value == "选项1") {
        this.$http.post('http://localhost:10086/api/questionOne/boundary')
          .then((res)=>{
            this.testResult = res.data.data
          })
      } else {
        this.$http.post('http://localhost:10086/api/questionOne/equivalent')
          .then((res)=>{
            this.testResult = res.data.data
          })
      }
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>
<style scoped>
</style>
