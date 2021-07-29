<template>
  <div class="hello">
    <Form ref="formItem" :model="formItem" :rules="ruleItem" :label-width="80">
      <Row>
        <Col :xs="23" :sm="23" :md="23" :lg="23">
        <h2>{{msg}}</h2>
        </Col>
      </Row>
      <Row>
        <Col :xs="23" :sm="23" :md="23" :lg="23">
        <FormItem prop="account" label="注册账号">
          <Input type="text" v-model="formItem.account" placeholder="填写注册邮箱"></Input>
        </FormItem>
        </Col>
      </Row>
      <Row>
        <Col :xs="23" :sm="23" :md="23" :lg="23">
        <FormItem prop="name" label="姓名">
          <Input type="text" v-model="formItem.name" placeholder="输入姓名"></Input>
        </FormItem>
        </Col>
      </Row>
      <Row>
        <Col :xs="23" :sm="23" :md="23" :lg="23">
        <FormItem prop="sex" label="性别">
            <input type="radio" value="男" v-model="sex"><label>男</label>
            <input type="radio" value="女" v-model="sex"><label>女</label>
        </FormItem>
        </Col>
      </Row>
      <Row>
        <Col :xs="23" :sm="23" :md="23" :lg="23">
        <FormItem prop="password" label="注册密码">
          <Input type="password" v-model="formItem.password" placeholder="填写密码"></Input>
        </FormItem>
        </Col>
      </Row>
      <Row>
        <Col :xs="23" :sm="23" :md="23" :lg="23">
        <FormItem prop="conf_password" label="确认密码">
          <Input type="password" v-model="formItem.conf_password" placeholder="重复密码"></Input>
        </FormItem>
        </Col>
      </Row>
      <Row>
        <Col :xs="24" :sm="24" :md="24" :lg="24">
        <Button id="login_btn" shape="circle" type="primary" :loading="loading" @click.native="handleSubmit('formItem')">
          <span v-if="!loading">登录</span>
          <span v-else>登录中...</span>
        </Button>
        </Col>
      </Row>
       <!-- 注册按钮 -->
      <br>
      <Button id="resgiter_btn" shape="circle" type="primary" @click="register()">
          <span>注册</span>
      </Button>
        
      <Row class="tip">
        <Col :xs="10" :sm="10" :md="10" :lg="10" offset="12">
          <span class="login_font"><a>忘记密码？点击邮箱找回</a></span>
        </Col>
      </Row>
    </Form>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      sex: '男',
      msg: '图书馆',
      verifyCode: 'hello',
      formItem: {
        account: '',
        password: '',
        sex: '男',
        name: ''
      },
      ruleItem: {
        account: [{
          required: true,
          message: '请填写账号！',
          trigger: 'blur'
        }],
        name: [{
          required: true,
          message: '请输入姓名！',
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
        conf_password: [{
          required: true,
          message: '请填写密码',
          trigger: 'blur'
        }, {
          type: 'string',
          min: 3,
          message: '密码长度不能小于6位',
          trigger: 'blur'
        }]
      }
    }
  },
  methods: {
    handleSubmit (name) {
      var that = this
      this.loading = true
      this.$refs[name].validate((valid) => {
        if (valid) {
          if (that.verifyCode === that.formItem.code) {
            that.$http.post(that.GLOBAL.serverPath + '/excise/login',
              {
                account: that.formItem.account,
                password: that.formItem.password
              },
              {
                emulateJSON: true
              }
            ).then(function (res) {
              console.log(res.data.loginUser)
              if (res.data.result === 'yes') {
                this.$Message.success('登录成功!')
                window.localStorage.setItem('userId', res.data.loginUser.rid)
                window.localStorage.setItem('account', res.data.loginUser.account)
                window.localStorage.setItem('username', res.data.loginUser.name)
                window.localStorage.setItem('sex', res.data.loginUser.sex)
                window.localStorage.setItem('condi', res.data.loginUser.condi)
                console.log('hahaha' + res.data.condi)
                if (res.data.condi === 2) {
                  this.$router.replace({path: '/index'})
                } else if (res.data.condi === 1) {
                  this.$router.replace({path: '/manager'})
                } else {
                  this.$router.replace({path: '/reader'})
                }
              } else {
                this.$Message.error('账号或密码有误！')
                this.loading = false
              }
            })
          } else {
            that.$Message.error('请填写正确的验证码!')
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .hello{
    margin: auto;
  }
  h2{
    color:#2D8CF0;
    margin-bottom:20px;
  }
  Form{
    margin:0 auto;
    width:350px;
    border: 2px solid grey;
    padding:50px 0;
    /*background-color:red;*/
  }
  #login_btn{
    width:70%;
  }
  .tip{
    margin-top:10px;
    color:darkgrey;
  }
  #verifyCode{
  }
  /* 注册按钮 */
  #resgiter_btn{
    width:70%;
  }
</style>
