<template>
    <div class="modal-wrap modal-signin-wrap" :class="show ? 'active' : ''"><span class="center-helper"></span>
        <div class="modal modal-signup">
            <h2 class="modal-title">登录</h2><a @click="close" href="javascript:;" class="modal-close"><i class="icon icon-close-black"></i></a>
            <div class="modal-content">
                <div class="signup-form">
                    <div class="input-wrap">
                        <input v-model="form.username" type="text" placeholder="昵称" class="base-input">
                        <p class="error-info input-info hidden">长度至少 6 位</p>
                    </div>
                    <div class="input-wrap">
                        <input v-model="form.password" type="password" placeholder="密码" class="base-input">
                        <p class="error-info input-info hidden">长度至少 6 位</p>
                    </div>
                    <a @click="login" href="javascript:;" class="btn signup-btn btn-yellow">确认登录</a>
                    <a @click="register" href="javascript:;" class="btn signup-btn btn-blue">我要注册</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { showMsg } from '~utils'
// import api from '~api'
export default {
    name: 'sign-in',
    props: ['show'],
    data() {
        return {
            form: {
                username: '',
                password: ''
            }
        }
    },
    methods: {
        close() {
            this.$store.commit('global/showLoginModal', false)
        },
        register() {
            this.$store.commit('global/showLoginModal', false)
            this.$store.commit('global/showRegisterModal', true)
        },
        async login() {
            if (!this.form.username || !this.form.password) {
                showMsg('请将表单填写完整!')
                return
            }
            const { code, message } = await this.$store.$api.post('frontend/user/login', this.form)
            if (code === 200) {
                showMsg({
                    type: 'success',
                    content: message
                })
                this.$router.go(0)
            }
        }
    }
}
</script>
