<template>
    <div style="margin-top: 60px;margin-left:330px;width: 300px;height: 500px;border: 0px solid red;" >
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="楼宇" prop="buildingId">
                <el-select v-model="ruleForm.buildingId" placeholder="请选择楼宇">
                    <el-option v-for="item in buildingList" :label="item.name" :value="item.id"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="名称" prop="name">
                <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>
            <el-form-item label="几人间" prop="type">
                <el-select v-model="ruleForm.type" placeholder="请选择几人间">
                    <el-option label="4人间" value="4"></el-option>
                    <el-option label="6人间" value="6"></el-option>
                    <el-option label="8人间" value="8"></el-option>
                    <el-option label="10人间" value="10"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="联系电话" prop="telephone">
                <el-input v-model="ruleForm.telephone"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                <el-button @click="resetForm('ruleForm')">重置</el-button>
            </el-form-item>
        </el-form>
    </div>

</template>

<script>
    export default {
        data() {
            return {
                category:null,
                ruleForm: {
                    buildingId: '',
                    name: '',
                    type: '',
                    telephone: ''
                },
                buildingList: '',
                rules: {
                    buildingId: [
                        { required: true, message: '请选择楼宇', trigger: 'change' }
                    ],
                    name: [
                        { required: true, message: '请输入名称', trigger: 'blur' }
                    ],
                    telephone: [
                        { required: true, message: '请输入联系电话', trigger: 'blur' }
                    ],
                    type: [
                        { required: true, message: '请选择几人间', trigger: 'change' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                const _this = this
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        axios.post('http://localhost:8181/dormitory/save',_this.ruleForm).then(function (resp) {
                            if(resp.data.code == 0){
                                _this.$alert(_this.ruleForm.name+'添加成功', '', {
                                    confirmButtonText: '确定',
                                    callback: action => {
                                        _this.$router.push('/dormitoryManager')
                                    }
                                });
                            }
                        })
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        },
        created() {
            const _this = this
            axios.get('http://localhost:8181/building/list').then(function (resp) {
                _this.buildingList = resp.data.data
            })
        }
    }
</script>
