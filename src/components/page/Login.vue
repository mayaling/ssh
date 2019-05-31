<template>
  <div class="login-wrap" v-loading="loading" element-loading-text="登录中">
  	<!-- <img class="ms-logo" src="../../assets/logo.png"/> -->
  	<!-- <div class="ms-title">立洋在线测评  ——</div> -->
    <div class="ms-login clearfix">
    	<div class="ms-login-left"></div>
    	<div class="ms-login-right">
    		<div class="login-title">登录</div>
    		<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
    			<el-form-item prop="type">
            <el-select v-model="ruleForm.type" class="login-input" placeholder="选择角色" @change="typeChange">
					    <el-option key="0" label="用户" value="用户"></el-option>
					    <el-option key="1" label="管理员" value="管理员"></el-option>
					  </el-select>
					  <i class="el-input__icon el-icon-lx-group"></i>
          </el-form-item>
          <el-form-item prop="username">
            <el-input class="login-input" v-model="ruleForm.username" placeholder="用户名"><i slot="prefix" class="el-input__icon el-icon-lx-people"></i></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input class="login-input" type="password" placeholder="密码" v-model="ruleForm.password" @keyup.enter.native="submitForm('ruleForm')"><i slot="prefix" class="el-input__icon el-icon-lx-lock"></i></el-input>
          </el-form-item>
          <div class="login-btn">
            <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
          </div>
        </el-form>
    	</div>
    </div>
  </div>
</template>

<script>
    export default {
        data: function() {
            return {
                loading: false,
                ruleForm: {
                    type: "",
                    username: '',
                    password: ''
                },
                rules: {
                    type: [{
                        required: true,
                        message: '请选择角色类型',
                        trigger: 'change'
                    }],
                    username: [{
                        required: true,
                        message: '请输入用户名',
                        trigger: 'blur'
                    }],
                    password: [{
                        required: true,
                        message: '请输入密码',
                        trigger: 'blur'
                    }]
                }
            }
        },
        created() {

        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        localStorage.setItem('token', '');
                        this.loading = true
                        if (this.ruleForm.type === "管理员") {
                            this.$post('teachers/login', {
                                loginId: this.ruleForm.username,
                                passwd: this.ruleForm.password
                            }).then((res) => {
                                if (res.code === 0) {
                                    res.data.role = 'teacher'
                                    localStorage.setItem('userInfo', JSON.stringify(res.data));
                                    localStorage.setItem('token', res.data.token);
                                    this.$router.push('/adminlist');
                                } else {
                                    this.$message.error(res.msg);
                                }
                                this.loading = false
                            }).catch(() => {
                                this.loading = false
                            })
                        } else {
                            this.$post('students/login', {
                                username: this.ruleForm.username,
                                password: this.ruleForm.password
                            }).then((res) => {
                                if (res.code === 0) {
                                    res.data.role = 'student'
                                    localStorage.setItem('userInfo', JSON.stringify(res.data));
                                    localStorage.setItem('token', res.data.token);
                                    //this.$router.push('/grades_exam_7_8');
                                    this.$router.push('/index');
                                } else {
                                    this.$message.error(res.msg);
                                }

                                this.loading = false
                            }).catch(() => {
                                this.loading = false
                            })
                        }
                    } else {
                        return false;
                    }
                });
            },
            typeChange() {
                this.ruleForm.username = ''
                this.ruleForm.password = ''
            }
        }
    }
</script>