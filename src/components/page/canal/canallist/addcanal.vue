<template>
    <div class="table" v-loading="loading" element-loading-text="加载中">
        <div class="container">
            <div class="container-title">新增渠道用户</div>
            <el-form :model="form" ref="form" :rules="rules" class="item-add-list">
                    <el-row :gutter="10" class="clearfix">
                        <el-col :span="12">
                            <el-form-item label="渠道名称:" prop="projectNo">
                            <el-input v-model.trim="form.projectNo" type="text"></el-input>
                          </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="APP下载链接:" prop="projectName">
                            <el-input v-model.trim="form.projectName" ></el-input>
                          </el-form-item>
                        </el-col>                   
                    </el-row>
                    <el-row :gutter="10" class="clearfix">
                        <el-col :span="12">
                            <el-form-item label="渠道联系人名称" prop="amt">
                                    <el-select v-model="region1" placeholder="请选择联系人名称">
                                            <el-option label="启动" value="1"></el-option>
                                            <el-option label="关闭" value="2"></el-option>
                                          </el-select>
                          </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="渠道模板类型" prop="memo">
                                <el-select v-model="region1" placeholder="请选择模板类型">
                                    <el-option label="启动" value="1"></el-option>
                                    <el-option label="关闭" value="2"></el-option>
                                  </el-select>
                          </el-form-item>
                        </el-col>
                    </el-row>         
                    <el-row :gutter="10" class="clearfix">
                            <el-col :span="12">
                                <el-form-item label="合作方式" prop="amt">
                                        <el-select v-model="region1" placeholder="请选择合作方式">
                                                <el-option label="启动" value="1"></el-option>
                                                <el-option label="关闭" value="2"></el-option>
                                              </el-select>
                              </el-form-item>
                            </el-col>
                        </el-row>               
                    <el-row :gutter="10" class="clearfix">
                      <el-col :span="20" :offset="4">
                        <el-button type="primary" @click="onSubmit('form')" style='margin-top:40px'>确认添加</el-button>
                        <el-button @click="resetForm('form')">重置</el-button>
                      </el-col>
                    </el-row>
                    </el-form>
</div>
</div>
</template>

<script>
    export default {
        name: 'addcanal',
        data() {
            // var re = /^(((13[0-9]{1})|(15[0-9]{1})|(17[0-9]{1})|(18[0-9]{1})|(198)|(199)|(166)|(165)|(146)|(148))+\d{8})$/;
            var re = /(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/;
            // var amtRule = (rule, value, callback) => {
            //     if (!re.test(value)) {
            //         callback(new Error('请输入正确的格式'));
            //     } else {
            //         callback();
            //     }
            // };
            return {
                form: {
                    projectNo: "",
                    projectName: "",
                    startDate: "",
                    expDate: "",
                    amt: "",
                    memo: "",
                },
                // rules: {
                //     projectNo: [{
                //         required: true,
                //         message: '不可为空！',
                //         trigger: 'blur'
                //     }],
                //     projectNo: [{
                //         required: true,
                //         message: '不可为空！',
                //         trigger: 'blur'
                //     }],
                //     amt: [{
                //         required: true,
                //         message: '不可为空！',
                //         trigger: 'blur'
                //     }],
                //     startDate: [{
                //         required: true,
                //         message: '不可为空！',
                //         trigger: 'blur'
                //     }],
                //     expDate: [{
                //         required: true,
                //         message: '不可为空！',
                //         trigger: 'blur'
                //     }],
                // }
            }
        },
        created() {

        },
        computed: {

        },
        methods: {
            //提交数据
            onSubmit(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        this.$post('projects', {
                            projectNo: this.form.projectNo,
                            projectName: this.form.projectName,
                            startDate: this.form.startDate,
                            expDate: this.form.expDate,
                            amt: this.form.amt,
                            memo: this.form.memo,
                        }).then((res) => {
                            if (res.code === 0) {
                                this.$message({
                                    message: res.msg,
                                    type: 'success'
                                });
                                this.$emit('closedialog');
                                // this.$router.push('/projecttable');
                            } else {
                                this.$message.error(res.msg);
                            }
                        })
                    } else {
                        return false;
                    }
                });
                // this.$emit('closedialog');  
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>


<style scoped>

</style>