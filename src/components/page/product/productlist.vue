<template>
        <div class="table" v-loading="loading" element-loading-text="加载中">
            <div class="container">
                <div class="container-title">产品列表</div>
                <div class="mgb20">
                        <el-row >
                                <el-col :span="4">
                                    <router-link :to="{path:'addproduct'}"><el-button type="primary">+新增产品</el-button></router-link>
                                    
                                </el-col>
                                <el-col :span="12" :offset="2">
                                        <el-row :gutter="20">
                                                <el-col :span="6">
                                                        <el-select v-model="region" placeholder="请选择平台">
                                                                <el-option label="区域一" value="shanghai"></el-option>
                                                                <el-option label="区域二" value="beijing"></el-option>
                                                              </el-select>
                                                </el-col>
                                                <el-col :span="6">
                                                        <el-select v-model="region1" placeholder="请选择状态">
                                                                <el-option label="已下架" value="1"></el-option>
                                                                <el-option label="已上架" value="2"></el-option>
                                                              </el-select>
                                                </el-col>
                                                <el-col :span="6">
                                                        <el-input v-model.trim="s_klass" placeholder="请输入产品名称" @keyup.enter.native="search"></el-input>
                                                </el-col>
                                                <el-col :span="6">
                                                        <el-button type="primary" icon="el-icon-search">搜索</el-button>
                                                </el-col>
                                              </el-row>
                                     
                                             
                                                  
                                </el-col>
                              </el-row>
                    </div>
                <el-table :data="tableData" border size="medium" ref="multipleTable">
                    <el-table-column prop="name" label="ID" align="center" width="120"></el-table-column>
                    <el-table-column prop="englishName" label="手机号" align="center"></el-table-column>
                    <el-table-column prop="username" label="是否激活" align="center"></el-table-column>
                    <el-table-column prop="sex" label="激活时间" align="center" width="80"></el-table-column>
                    <el-table-column prop="school" label="最近打开时间" align="center"></el-table-column>
                    <el-table-column prop="grade" label="设备号" align="center"></el-table-column>
                    <el-table-column prop="klass" label="渠道" align="center"></el-table-column>
                    <el-table-column prop="klass" label="版本号" align="center"></el-table-column>
                    <el-table-column prop="klass" label="设备系统" align="center"></el-table-column>
                    <el-table-column label="操作" align="center" width="110">
                        <template slot-scope="scope">
                            <router-link :to="{path:'studentDetail',query:{id:scope.row.id}}" style="color:#35a000;">详情</router-link>
                        </template>
</el-table-column>
</el-table>
<div class="pagination">
    <el-pagination @current-change="handleCurrentChange" layout="prev, pager, next" :page-count="pages">
    </el-pagination>
</div>
</div>
</div>
</template>


<script>
    export default {
        name: 'teacher',
        data() {
            return {
                value1: "",
                value2: "",
                tableData: [],
                cur_page: 1,
                pages: 0,
                s_student: '',
                s_school: '',
                s_grade: '',
                s_klass: '',
                // loading: true,
                region: "",
                region1: ""
            }
        },
        created() {
            // this.$get('students').then((res) => {
            //     if (res.code === 0) {
            //         this.tableData = JSON.parse(JSON.stringify(res.data.list))
            //         this.pages = res.data.pages;
            //         for (var i = 0; i < this.tableData.length; i++) {
            //             if (this.tableData[i].sex) {
            //                 this.tableData[i].sex = '女'
            //             } else {
            //                 this.tableData[i].sex = '男'
            //             }
            //         }
            //     } else {
            //         this.$message.error('学生加载失败');
            //     }
            //     this.loading = false
            // }).catch(() => {
            //     this.loading = false
            // })
        },
        computed: {

        },
        methods: {
            // 分页导航
            handleCurrentChange(val) {
                this.loading = true
                this.$get('students', {
                    pageNum: val,
                    studentName: this.s_student
                }).then((res) => {
                    if (res.code === 0) {
                        this.tableData = JSON.parse(JSON.stringify(res.data.list))
                        this.pages = res.data.pages;
                        for (var i = 0; i < this.tableData.length; i++) {
                            if (this.tableData[i].sex) {
                                this.tableData[i].sex = '女'
                            } else {
                                this.tableData[i].sex = '男'
                            }
                        }
                    } else {
                        this.$message.error('学生加载失败');
                    }
                    this.loading = false
                }).catch(() => {
                    this.loading = false
                })
            },

            // 搜索
            search() {
                this.loading = true
                this.$get('students', {
                    studentName: this.s_student
                }).then((res) => {
                    if (res.code === 0) {
                        this.tableData = JSON.parse(JSON.stringify(res.data.list))
                        this.pages = res.data.pages;
                        for (var i = 0; i < this.tableData.length; i++) {
                            if (this.tableData[i].sex) {
                                this.tableData[i].sex = '女'
                            } else {
                                this.tableData[i].sex = '男'
                            }
                        }
                    } else {
                        this.$message.error('学生加载失败');
                    }
                    this.loading = false
                }).catch(() => {
                    this.loading = false
                })
            }
        }
    }
</script>

<style scoped>

</style>