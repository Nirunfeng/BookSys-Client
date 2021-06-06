<template>
    <div class="register">
        <Form ref="formItem" :model="formItem" :rules="ruleItem" :label-width="80">
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <h2>{{msg}}</h2>
                </Col>
            </Row>
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <FormItem prop="account" label="账号">
                        <Input type="text" v-model="formItem.account" placeholder="请填写账号"></Input>
                    </FormItem>
                </Col>
            </Row>
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <FormItem prop="name" label="姓名">
                        <Input type="text" v-model="formItem.name" placeholder="请填写姓名"></Input>
                    </FormItem>
                </Col>
            </Row>
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <FormItem prop="password" label="密码">
                        <Input type="password" v-model="formItem.password" placeholder="请填写密码"></Input>
                    </FormItem>
                </Col>
            </Row>
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <FormItem prop="confirmpassword" label="重复密码">
                        <Input type="password" v-model="formItem.confirmpassword" placeholder="请重复密码"></Input>
                    </FormItem>
                </Col>
            </Row>
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <FormItem prop="sex" label="性别">
                        <RadioGroup v-model="formItem.sex">
                            <Radio label="男">男</Radio>
                            <Radio label="女">女</Radio>
                        </RadioGroup>
                    </FormItem>
                </Col>
            </Row>
            <Row>
                <Col :xs="23" :sm="23" :md="23" :lg="23">
                    <FormItem label="身份" prop="condi">
                        <Select v-model="formItem.condi">
                            <Option value="0">学生</Option>
                        </Select>
                    </FormItem>
                </Col>
            </Row>
            <Row>
                <Col :xs="24" :sm="24" :md="24" :lg="24">
                    <Button id="login_btn" shape="circle" type="primary" :loading="loading"  @click.native="handleSubmit('formItem')">
                        <span v-if="!loading">提交</span>
                    </Button>
                </Col>
            </Row>
        </Form>
    </div>
</template>

<script>
export default {
    name: 'Register',
    data () {
        // 初始化
        return {
            msg: '图书馆',
            formItem: {
                account: '',
                name: '',
                password: '',
                confirmpassword: '',
                sex: '男',
                condi: '0'
            },
            ruleItem: {
                account: [{
                    required: true,
                    message: '请填写账号！',
                    trigger: 'blur'
                }],
                name: [{
                    required: true,
                    message: '请填写姓名！',
                    trigger: 'blur'
                }],
                password: [{
                    required: true,
                    message: '请填写密码',
                    trigger: 'blur'
                }, {
                    type: 'string',
                    min: 3,
                    message: '密码长度不能小于6位',
                    trigger: 'blur'
                }],
                confirmpassword: [{
                    required: true,
                    message: '请填写密码！',
                    trigger: 'blur'
                }]
            },
            loading: false
        }
    },
    methods: {
        handleSubmit (name) {
            var that = this
            this.loading = true
            this.$refs[name].validate((valid) => {
                if (valid) {
                    if (that.formItem.password === that.formItem.confirmpassword) {
                        that.$http.post(that.GLOBAL.serverPath + '/user/register',
                            {
                                account: that.formItem.account,
                                name: that.formItem.name,
                                password: that.formItem.password,
                                sex: that.formItem.sex,
                                condi: that.formItem.condi
                            },
                            {
                                emulateJSON: true
                            }
                        ).then(function (res) {
                            if (res.data.result === 'yes') {
                                this.$router.replace({path: '/'})
                            } else {
                                this.loading = false
                            }
                        })
                    } else {
                        this.$Message.error('密码不正确!')
                        this.loading = false
                    }
                } else {
                    this.loading = false
                }
            })
        }
    }
}
</script>

<style scoped>
    Form{
        margin:0 auto;
        width:350px;
        border: 2px solid grey;
        padding:50px 0;
    /*background-color:red;*/
    }
    
    h2{
        color:#2D8CF0;
        margin-bottom:20px;
    }

    #login_btn{
        width:70%;
    }
</style>