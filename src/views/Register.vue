<template>
    <div>
        <el-form class="loginContainer">
            <h3 class="loginTitle">系统注册</h3>
            <el-form-item label="name">
                <el-input v-model="uname"></el-input>
            </el-form-item>
            <el-form-item label="phone">
                <el-input v-model="phone"></el-input>
            </el-form-item>
            <el-form-item label="telephone">
                <el-input v-model="telephone"></el-input>
            </el-form-item>
            <el-form-item label="address">
                <el-input v-model="address"></el-input>
            </el-form-item>
            <el-form-item label="username">
                <el-input v-model="username"></el-input>
            </el-form-item>
            <el-form-item label="password">
                <el-input v-model="password"></el-input>
            </el-form-item>
            <el-form-item label="userface">
                <el-input v-model="userface"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click.prevent="submit">Submit</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script lang="ts">
    import {putRequest} from "@/utils/api";

    interface IHrDTO {
        name: string,
        phone: string,
        telephone: string,
        address: string,
        username: string,
        password: string,
        userface: string
    }

    import Vue from "vue";
    import {Component} from "vue-property-decorator";

    @Component
    export default class Register extends Vue {
        uname = "";
        phone = "";
        telephone = "";
        address = "";
        username = "";
        password = "";
        userface = "";

        submit() {
            const data = Object.assign({}, this.$data);
            data.name = data.uname;
            putRequest("/system/hr/add", data)
            .then(resp => {
                if (resp) {
                    console.log(resp);
                    this.$router.push('/');
                }
            })
        }
    }
</script>

<style scoped>
    .loginContainer {
        border-radius: 15px;
        background-clip: padding-box;
        margin: 180px auto;
        width: 350px;
        padding: 15px 35px 15px 35px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
    }


    .loginTitle {
        margin: 15px auto 20px auto;
        text-align: center;
        color: #505458;
    }

    .loginRemember {
        text-align: left;
        margin: 0px 0px 15px 0px;
    }

    .el-form-item__content {
        display: flex;
        align-items: center;
    }
</style>
