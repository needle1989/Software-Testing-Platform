<template>
    <div class="room_reserves">
        <el-row style="margin-top: 20px;">
            <el-col :span="14" :offset="4">
                <header class="form_header"><h1>续借图书</h1></header>
                <el-form ref="roomFormRef" :model="roomForm" :rules="roomFormRules" label-width="110px" class="demo-now_Data">
                    <el-form-item label="借书ID" prop="room_id">
                        <el-input v-model="roomForm.room_id" placeholder="借书ID"></el-input>
                    </el-form-item>
                    <el-button type="primary" @click="booking" class="submit_btn" style="width:40%; margin-left: 230px;">提交申请</el-button>
                </el-form>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                //登录表单数据绑定
                roomForm: {
                    room_id: '',
                },
                //表单的验证规则
                roomFormRules: {
                    //    验证用户名是否合法
                    room_id: [
                        { required: true, message: "请输入借书ID", trigger: "blur" },
                    ],
                }
            }
        },
        methods: {
            //异步操作
            booking() {
                this.$refs.roomFormRef.validate(async valid => {
                    if (!valid) return;
                    let msg = "";
                    let status = 200;
                    let key = this.roomForm.room_id;
                    let Token = window.sessionStorage.getItem('token');
                    let result = await this.$http.put('/apip/api/extendborrow/extendborrow?borrowid=' + this.roomForm.room_id, {
                    })
                    if (status === 400) {
                        this.$message.info(msg);
                    }
                    else {
                        if (result.data.success !=undefined ) {
                            this.$message.info("续借成功");
                        }
                        else {
                            this.$message.info(result.data.error);
                        }

                    }
                });


            }
        }
    }
</script>

<style lang="less">

    @import '../style/mixin';

    .button_submit {
        text-align: center;
    }

    .form_header {
        text-align: center;
        margin-bottom: 30px;
    }

    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }

        .avatar-uploader .el-upload:hover {
            border-color: #20a0ff;
        }

    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 120px;
        height: 120px;
        line-height: 120px;
        text-align: center;
    }

    .avatar {
        width: 120px;
        height: 120px;
        display: block;
    }

    .el-table .info-row {
        background: #c9e5f5;
    }

    .el-table .positive-row {
        background: #e2f0e4;
    }
</style>
