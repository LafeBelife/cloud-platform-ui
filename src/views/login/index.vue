<template>
  <div class="login-container">


    <a href="https://github.com/LafeBelife/cloud-platform-ui" target="_blank" class="github-corner"
       aria-label="View source on Github">
      <svg width="80" height="80" viewBox="0 0 250 250"
           style="fill:#4AB7BD; color:#fff; position: absolute;border: 0; right: 0px;"
           aria-hidden="true">
        <path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"></path>
        <path
          d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2"
          fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"></path>
        <path
          d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z"
          fill="currentColor" class="octo-body"></path>
      </svg>
    </a>

    <el-form autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left"
             label-width="0px" class="card-box login-form">
      <h3 class="title">wbw - platform</h3>
      <el-form-item prop="username">
        <span class="svg-container"><icon-svg icon-class="account"></icon-svg></span>
        <el-input name="username" type="text" v-model="loginForm.username" autoComplete="on"
                  placeholder="账户"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <span class="svg-container"><icon-svg icon-class="password"></icon-svg></span>
        <el-input name="password" type="password" @keyup.enter.native="handleLogin" v-model="loginForm.password"
                  autoComplete="on" placeholder="密码"></el-input>
      </el-form-item>
      <el-button type="primary" style="width:30%;" :loading="loading" @click.native.prevent="handleLogin">
        登录
      </el-button>
      <el-button type="primary" style="width:50%;float: right" @click.native.prevent="handleExperience">
        获取体验账号
      </el-button>
    </el-form>

    <el-dialog title="第三方验证" :visible.sync="showDialog">
      邮箱登录成功,请选择第三方验证
      <social-sign></social-sign>
    </el-dialog>
  </div>
</template>

<script>
    import socialSign from './socialsignin';

    export default {
        components: {
            socialSign
        },
        name: 'login',
        data() {
            const validatePass = (rule, value, callback) => {
                if (value.length < 5) {
                    callback(new Error('密码不能小于5位'));
                } else {
                    callback();
                }
            };
            return {
                loginForm: {
                    username: '',//admin admin
                    password: ''
                },
                loginRules: {
                    username: [{
                        required: true,
                        message: '账户不能为空',
                        trigger: 'blur'
                    },
                        {
                            min: 3,
                            max: 20,
                            message: '长度在 3 到 20 个字符',
                            trigger: 'blur'
                        }],
                    password: [{
                        required: true,
                        trigger: 'blur',
                        validator: validatePass
                    }]
                },
                loading: false,
                showDialog: false
            }
        },
        methods: {
            handleLogin() {
                this.$refs.loginForm.validate(valid => {
                    if (valid) {
                        this.loading = true;
                        this.$store.dispatch('LoginByEmail', this.loginForm).then(() => {
                            this.loading = false;
                            this.$router.push({
                                path: '/'
                            });
                            // this.showDialog = true;
                        }).catch(() => {
                            this.loading = false;
                        });
                        // this.$http.post('/oauth/token', {
                        //   username: this.loginForm.username,
                        //   password: this.loginForm.password
                        // }, {
                        //   headers: {
                        //     Authorization: 'Basic enV1bDp6dXVs'
                        //   },
                        //   emulateJSON: true,
                        //   emulateHTTP: true
                        // }).then(function(response) {
                        //   if (response.status === 200) {
                        //     this.$store.dispatch('LoginByEmail', this.loginForm).then(() => {
                        //       this.loading = false;
                        //       this.$router.push({
                        //         path: '/'
                        //       });
                        //       // this.showDialog = true;
                        //     }).catch(() => {
                        //       this.loading = false;
                        //     });
                        //   } else {
                        //     this.loading = false;
                        //     return false;
                        //   }
                        // }, function(error) {
                        //   console.log('server error!!');
                        //   this.loading = false;
                        //   return false;
                        // });
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            afterQRScan() {
                // const hash = window.location.hash.slice(1);
                // const hashObj = getQueryObject(hash);
                // const originUrl = window.location.origin;
                // history.replaceState({}, '', originUrl);
                // const codeMap = {
                //   wechat: 'code',
                //   tencent: 'code'
                // };
                // const codeName = hashObj[codeMap[this.auth_type]];
                // if (!codeName) {
                //   alert('第三方登录失败');
                // } else {
                //   this.$store.dispatch('LoginByThirdparty', codeName).then(() => {
                //     this.$router.push({ path: '/' });
                //   });
                // }
            },
            handleExperience() {
                this.loginForm.username = 'admin';
                this.loginForm.password = 'admin';
            }
        },
        created() {
            // window.addEventListener('hashchange', this.afterQRScan);
        },
        destroyed() {
            // window.removeEventListener('hashchange', this.afterQRScan);
        }
    }
</script>

<style rel="stylesheet/scss" lang="scss">
  @import "src/styles/mixin.scss";

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 5px;
  }

  .login-container {
    @include relative;
    height: 100vh;

    input:-webkit-autofill {
      -webkit-box-shadow: 0 0 0 1000px #293444 inset !important;
      -webkit-text-fill-color: #fff !important;
    }

    input {
      background: transparent;
      border: 0;
      -webkit-appearance: none;
      border-radius: 0;
      padding: 12px 5px 12px 15px;
      color: #eeeeee;
      height: 47px;
    }

    .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;
    }

    .svg-container {
      padding: 6px 5px 6px 15px;
      color: #889aa4;
    }

    .title {
      font-size: 26px;
      font-weight: 400;
      color: #eeeeee;
      margin: 0 auto 40px;
      text-align: center;
      font-weight: bold;
    }

    .login-form {
      position: absolute;
      left: 0;
      right: 0;
      width: 400px;
      padding: 35px 35px 15px;
      margin: 120px auto;
    }

    .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }

    .forget-pwd {
      color: #fff;
    }
  }

  .svg-icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.15em;
    fill: currentColor; //此属性为更改svg颜色属性设置
    overflow: hidden;
  }

  #loginSvg {
    width: 3em;
    height: 1em;
  }
</style>
