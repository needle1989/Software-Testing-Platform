<template>
  <div class="all-container">
	  <section class="formCon">
		  <div class="libtipin"><h1>修改个人信息</h1></div>
    <el-form :model="ruleForm"
             status-icon
             :rules="rules"
             ref="ruleForm"
             label-width="100px"
             class="demo-ruleForm">
      <el-form-item label="用户名"
                    prop="name">
        <el-input v-model.number="ruleForm.name"
                  style="width:300px;"></el-input>
      </el-form-item>
      <el-form-item label="性别"
                    prop="gender">
        <el-input v-model.number="ruleForm.gender"
                  style="width:300px;"></el-input>
      </el-form-item>
      <el-form-item label="手机号"
                    prop="contact">
        <el-input v-model.number="ruleForm.contact"
                  style="width:300px;"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" 
                           @click="submitForm('ruleForm')" style="width:100%;">提交</el-button>
                <el-button @click="resetForm('ruleForm')" style="width:100%; margin-top:20px; margin-left:0rem;">重置</el-button>
      </el-form-item>
    </el-form>
	</section>
  </div>
</template>
<script>
export default {
  data () {
    /*var checknum = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('联系方式不能为空'))
      }
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error('请输入数字值'))
        } else {
          if (value.toString().length !== 11) {
            callback(new Error('必须为11位数'))
          } else {
            callback()
          }
        }
      }, 1000)
    }
    var check = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入内容'))
      } else { callback() }
    }*/
    return {
      ruleForm: {
        pass: '',
        checkPass: '',
        gender: '',
        name: '',
        contact: ''
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
        let names = this.ruleForm.name;
        let contacts = this.ruleForm.contact;
        let genders = "" + this.ruleForm.gender;
        names = encodeURI(encodeURI(names));
        genders = encodeURI(encodeURI(genders));
        let result = await this.$http.put('/apip/api/infomodify/modinfo?name=' + names + '&contact=' + contacts + '&gender=' + genders,
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
