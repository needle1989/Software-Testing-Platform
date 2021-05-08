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
      ruleForm: {
        edgeA: '',
        edgeB: '',
        edgeC: '',
      },
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

      }
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
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>
<style scoped>
</style>
