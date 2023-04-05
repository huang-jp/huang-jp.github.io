---
title:  Vue+element-plus+SpringBoot搭建管理系统
---
前言
Vue+element-plus的管理系统模板有很多，其开发步骤类似

一、大致页面
![1](/images/StudyVue/1.png)


二、工程结构
![2](/images/StudyVue/2.png)

二、创建步骤
1.安装Vue
全局安装@vue/cli（仅第一次执行）
注意：要以管理员的身份打开cmd

1.如果出现下载缓慢可以先配置淘宝镜像：

npm config set registry https://registry.npm.taobao.org
npm install -g @vue/cli
创建脚手架工程

在目录位置打开cmd，运行命令行：

       

 vue create xxxx
选择Vue版本
![3](/images/StudyVue/3.png)


运行项目:

1.先进入创建的文件夹的路径：

cd vue_text

2. 然后输入 ：

npm run serve
2.安装/引入element-plus
在项目中打开cmd命令窗口，输入命令

npm install element-plus --save

安装成功，在main.js将其引入到自己的项目中
![4](/images/StudyVue/4.png)


 三、部分代码
1.main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import 'element-plus/dist/index.css'
import store from './store'
import '@/assets/css/global.css'
import ElementPlus from 'element-plus'
createApp(App).use(store).use(router).use(ElementPlus).mount('#app')
2.App.vue

    <template>
        <div>
            <el-config-provider :locale="locale">
                <router-view />
            </el-config-provider>
        </div>
    </template>

    <script>
        import locale from "element-plus/lib/locale/lang/zh-cn";
        export default {
            name: 'App',
            components: {
            },
            setup() {
                return { locale };
            }
        }
    </script>
    
    <style>
    
    </style>

3.布局框架文件layout.vue
<!--用作框架-->

    <template>
        <div>
            <el-config-provider :locale="locale">
                <!--    头部-->
                <Header/>
                <!--    主体-->
                <div style="display: flex">
                    <!--          侧边栏-->
                    <Aside/>
                    <!--          主体内容区域-->
                    <router-view style="flex: 1"/>
                </div>
            </el-config-provider>
        </div>
    </template>
    
    <script>
        import Header from "../components/Header";
        import Aside from "../components/Aside";
        import locale from "element-plus/lib/locale/lang/zh-cn";
        export default {
            name: "Layout",
            components:{
                Header,
                Aside,
            },
            setup() {
                return { locale };
            }
        }
    </script>
    
    <style scoped>
    
    </style>

4.头部栏components/Header.vue
![5](/images/StudyVue/5.png)

<!--头部导航栏-->

    <template>
        <div class="topNavigation">
            <div class="headerMenu">后台管理</div>
            <div style="flex: 1"></div>
            <div style="width: 100px">
                <el-dropdown>
                    <el-button>{{user.nickname}}<el-icon><arrow-down /></el-icon></el-button>
                    <template #dropdown>
                        <el-dropdown-menu>
                            <el-dropdown-item>个人信息</el-dropdown-item>
                            <el-dropdown-item @click="quit">退出系统</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>
            </div>
        </div>
    </template>
    <script>
        import { ArrowDown  } from '@element-plus/icons-vue'
        import router from "../router";
        export default {
            name: "Header",
            components:{
                ArrowDown
            },
            data(){
            return{
                user:{},
            }
            },
            created() {
                this.user = JSON.parse(sessionStorage.getItem("user")||"{}");
            },
            methods:{
                quit(){
                    sessionStorage.clear();
                    router.push("/login");
                },
            }
        }
    </script>
    
    <style>
        .topNavigation {
            height: 50px;
            line-height: 50px;
            border-bottom: 1px solid #f6eaff;
            display: flex;
        }
    
        .headerMenu {
            width: 200px;
            padding-left: 30px;
            font-weight: bold;
            color: dodgerblue;
        }
    
    </style>

5.左侧导航栏components/Asid.vue
![6](/images/StudyVue/6.png)

<!--左侧边栏-->

    <template>
        <nav>
            <el-row class="tac">
    <!--            加入outer实现导航栏点击跳转,index项值为对应的路由-->
                <el-menu
                        router
                        style="width: 200px ; min-height: 90vh"
                        active-text-color="#ffd04b"
                        class="el-menu-vertical-demo"
                        default-active="user"
                        text-color="black"
                        :default-openeds="['数据管理']"
                >
                    <el-sub-menu index="系统管理">
                        <template #title>
                            <el-icon>
                                <user/>
                            </el-icon>
                            <span>系统管理</span>
                        </template>
                        <el-menu-item index="user">用户管理</el-menu-item>
                        <el-menu-item index="book">图书管理</el-menu-item>
                    </el-sub-menu>
                    <el-sub-menu index="数据管理">
                        <template #title>
                            <el-icon>
                                <grid/>
                            </el-icon>
                            <span>登录与注册</span>
                        </template>
                        <el-menu-item index="login">登录</el-menu-item>
                        <el-menu-item index="register">注册</el-menu-item>
                    </el-sub-menu>
                </el-menu>
            </el-row>
        </nav>
    </template>
    <script>
        import {Location, Grid, Document, Setting,User} from '@element-plus/icons-vue'
    
        export default {
            name: "Aside",
            components: {
                Location,
                Grid,
                Document,
                Setting,
                User
            }
        }
    </script>
    
    <style scoped>
    
    </style>

6.布局框架Layout.vue
用以控制各个组件在页面中的布局

<!--用作框架-->

    <template>
        <div>
            <el-config-provider :locale="locale">
                <!--    头部-->
                <Header/>
                <!--    主体-->
                <div style="display: flex">
                    <!--          侧边栏-->
                    <Aside/>
                    <!--          主体内容区域-->
                    <router-view style="flex: 1"/>
                </div>
            </el-config-provider>
        </div>
    </template>
    
    <script>
        import Header from "../components/Header";
        import Aside from "../components/Aside";
        import locale from "element-plus/lib/locale/lang/zh-cn";
        export default {
            name: "Layout",
            components:{
                Header,
                Aside,
            },
            setup() {
                return { locale };
            }
        }
    </script>
    
    <style scoped>
    
    </style>

7.路由配置router/index.js
// 路由
import { createRouter, createWebHistory } from 'vue-router'
import Layout from "../layout/Layout";
 
const routes = [
  {
    path: '/', //当访问根路径时，会转向访问Layout界面
    name: 'Layout',
    component: Layout,
    redirect: 'user',//重定向,访问根目录重定向到/home
    // 嵌套路由配置
    children: [
      { path: 'user', name: 'User', component:()=>import("../views/UserView") },
      { path: 'book', name: 'Book', component:()=>import("../views/BookView") },
    ]
  },
  {
    path: '/login',
    name: 'Login',
    component: ()=>import("../views/LoginView")
  },
  {
    path: '/register',
    name: 'Register',
    component: ()=>import("../views/RegisterView")
  }
];
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
});
 
export default router

8.封装axios工具 utils/reques.js
// 封装axios
import axios from 'axios'
import router from "@/router";
 
const request = axios.create({
    baseURL: '/api',  // 注意！！ 这里是全局统一加上了 '/api' 前缀，也就是说所有接口都会加上'/api'前缀在，页面里面写接口的时候就不要加 '/api'了，否则会出现2个'/api'，类似 '/api/api/user'这样的报错，切记！！！
    timeout: 5000
})
 
// request 拦截器
// 可以自请求发送前对请求做一些处理
// 比如统一加token，对请求参数统一加密
request.interceptors.request.use(config => {
    config.headers['Content-Type'] = 'application/json;charset=utf-8';
    // config.headers['token'] = user.token;  // 设置请求头
 
    //判断用户是否登录，没有登陆就跳转到login界面
    let userJson = sessionStorage.getItem("user");
    if(!userJson){
        router.push("/login");
    }
 
    return config
}, error => {
    return Promise.reject(error)
});
 
// response 拦截器
// 可以在接口响应后统一处理结果
request.interceptors.response.use(
    response => {
        let res = response.data;
        // 如果是返回的文件
        if (response.config.responseType === 'blob') {
            return res
        }
        // 兼容服务端返回的字符串数据,如果返回的是String，就转换一下
        if (typeof res === 'string') {
            res = res ? JSON.parse(res) : res
        }
        return res;
    },
    error => {
        console.log('err' + error) // for debug
        return Promise.reject(error)
    }
)
 
 
export default request

四、功能页面
1.登陆页面 views/LoginView.vue


    <template>
        <div class="contanier">
            <div class="loginArea">
                <h2 style="text-align: center;padding: 10px 0">系统登录</h2>
                <el-form :model="loginForm" :rules="rules" ref="ruleForm">
                    <el-form-item label="用户名:" prop="username">
                        <el-input v-model.trim="loginForm.username" clearable></el-input>
                    </el-form-item>
                    <el-form-item label="密 码:" prop="password">
                        <el-input type="password" v-model.trim="loginForm.password" show-password clearable></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button @click="loginBut('ruleForm')" style="width: 40%" type="primary">登 录</el-button>
                        <el-button @click="registerBut" style="width: 40%;margin-left: 80px" type="primary">注 册</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </template>
    
    <script>
        import {User, Key, Lock} from '@element-plus/icons-vue'
        import request from "../utils/reques";
    
        export default {
            name: "LoginView",
            components: {
                User, Key, Lock
            },
            data() {
                return {
                    loginForm: {
                        username: '',
                        password: '',
                    },
                    // 表单校验
                    rules: {
                        username: [
                            {required: true, message: '请输入账号', trigger: 'blur'},
                            {min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ],
                        password: [
                            {required: true, message: '请输入密码', trigger: 'blur'},
                            {min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ]
                    }
                }
            },
            methods: {
                //登录按钮
                loginBut(formName) {
                    /**
                     * 配置axios请求,请求的url和请求参数
                     * .then（）链式操作，前一步完成后，结果传入
                     */
                    //表单校验
                    this.$refs[formName].validate((valid) => {
                        if (valid) {
                            request.post("/user/login", this.loginForm).then(res => {
                                if (res.code == "0") {
                                    this.$message({
                                        message: '登录成功o(*￣▽￣*)ブ',
                                        type: 'success'
                                    });
                                    //缓存用户信息，将登录的用户信息存入到sessionStorage
                                    sessionStorage.setItem("user",JSON.stringify(res.data));
                                    //登陆成功页面跳转,跳转到到根目录
                                    this.$router.push("/")
                                } else {
                                    this.$message({
                                        message: "登录失败,,ԾㅂԾ,,"+res.msg,
                                        type: 'error'
                                    });
                                }
                            });
                        } else {
                            this.$message({
                                message: '请确认信息(ˉ▽ˉ；)...',
                                type: 'warning'
                            });
                            return false;
                        }
                    });
                },
                //注册按钮
                registerBut() {
                    this.$router.push("/register")
                },
            }
        }
    </script>
    
    <style scoped>
        .contanier {
            width: 100%;
            height: 100%;
            overflow: hidden;
        }
    
        .loginArea {
            width: 400px;
            border-radius: 30px;
            background-clip: border-box;
            margin: 200px auto;
            padding: 15px 35px 15px 35px;
            background: #d6e8ee;
            border: 3px solid #e6fffc;
            box-shadow: 0 0 30px #c5c7ff;
        }
    </style>

2.注册页面views/RegisterView.vue


    <template>
        <div class="contanier">
            <div class="loginArea">
                <h2 style="text-align: center;padding: 10px 0">用户注册</h2>
                <el-form :model="registerForm" :rules="rules" ref="ruleForm">
                    <el-form-item label="用户名:" prop="username">
                        <el-input v-model.trim="registerForm.username" clearable></el-input>
                    </el-form-item>
                    <el-form-item label="密 码:" prop="password">
                        <el-input type="password" v-model.trim="registerForm.password" show-password clearable></el-input>
                    </el-form-item>
                    <el-form-item label="确认密码:" prop="againPassword">
                        <el-input type="password" v-model.trim="registerForm.againPassword" show-password
                                clearable></el-input>
                    </el-form-item>
                    <el-form-item label="昵 称:" prop="nickname">
                        <el-input type="text" v-model.trim="registerForm.nickname" clearable></el-input>
                    </el-form-item>
                    <el-form-item label="年 龄:" prop="age">
                        <el-input type="number" v-model.trim="registerForm.age" clearable></el-input>
                    </el-form-item>
                    <el-form-item label="性 别:" prop="sex">
                        <el-checkbox-group v-model="registerForm.sex">
                            <el-radio label="男" v-model="registerForm.sex"/>男
                            <el-radio label="女" v-model="registerForm.sex"/>女
                            <el-radio label="未知" v-model="registerForm.sex"/>未知
                        </el-checkbox-group>
                    </el-form-item>
                    <el-form-item label="地 址:" prop="address">
                        <el-input v-model.lazy="registerForm.address" type="textarea"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button @click="registerSure('ruleForm')" style="width: 100%" type="primary">确 认</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </template>
    
    <script>
        import {User, Key, Lock} from '@element-plus/icons-vue'
        import request from "../utils/reques";
    
        export default {
            name: "RegisterView",
            components: {
                User, Key, Lock
            },
            data() {
                return {
                    registerForm: {
                        username: '',
                        password: '',
                        againPassword: '',
                        nickname: '',
                        age: '',
                        sex: '',
                        address: ''
                    },
                    // 表单校验
                    rules: {
                        username: [
                            {required: true, message: '请输入账号', trigger: 'blur'},
                            {min: 2, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ],
                        password: [
                            {required: true, message: '请输入密码', trigger: 'blur'},
                            {min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ],
                        againPassword: [
                            {required: true, message: '请再次输入密码', trigger: 'blur'},
                            {min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ],
                        nickname: [
                            {required: true, message: '请输入昵称', trigger: 'blur'},
                            {min: 2, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ],
                        age: [
                            {required: true, message: '请输入年龄', trigger: 'blur'},
                            {min: 1, max: 3, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                        ],
                        sex: [
                            {required: true, message: '请选择性别', trigger: 'change'}
                        ],
                        address: [
                            {required: true, message: '请输入地址', trigger: 'blur'},
                            {min: 2, message: '长度要大于2个字符', trigger: 'blur'}
                        ]
                    }
                }
            },
            methods: {
                //登录按钮
                registerSure(formName) {
                    if (this.registerForm.password != this.registerForm.againPassword) {
                        this.$message({
                            message: '两次密码不一致(ˉ▽ˉ；)..',
                            type: 'warning'
                        });
                        return;
                    }
                    /**
                     * 配置axios请求,请求的url和请求参数
                     * .then（）链式操作，前一步完成后，结果传入
                     */
                    //表单校验
                    this.$refs[formName].validate((valid) => {
                        if (valid) {
                            request.post("/user/register", this.registerForm).then(res => {
                                if (res.code == "0") {
                                    this.$message({
                                        message: '注册成功o(*￣▽￣*)ブ',
                                        type: 'success'
                                    });
                                    //注册成功，跳转到登陆界面
                                    this.$router.push("/login")
                                } else {
                                    this.$message({
                                        message: "注册失败,,ԾㅂԾ,," + res.msg,
                                        type: 'error'
                                    });
                                }
                            });
                        } else {
                            this.$message({
                                message: '请确认信息(ˉ▽ˉ；)...',
                                type: 'warning'
                            });
                            return false;
                        }
                    });
                },
            }
        }
    </script>
    
    <style scoped>
        .contanier {
            width: 100%;
            height: 100%;
            overflow: hidden;
        }
    
        .loginArea {
            width: 400px;
            border-radius: 30px;
            background-clip: border-box;
            margin: 200px auto;
            padding: 15px 35px 15px 35px;
            background: #d6e8ee;
            border: 3px solid #e6fffc;
            box-shadow: 0 0 30px #c5c7ff;
        }
    </style>

3.用户管理页面views/UserView.vue


    <template>
        <div style="width: 100%">
            <!--    操作区域-->
            <div style="margin: 10px 5px">
                <el-button type="primary" @click="addUser">新增</el-button>
                <el-button type="primary" @click="importUser">导入</el-button>
                <el-button type="primary" @click="exportUser">导出</el-button>
            </div>
            <!--    搜索区域-->
            <div style="margin: 10px 5px">
                <el-input v-model="search" clearable type="text" placeholder="请输入关键字" style="width: 200px"></el-input>
                <el-button @click="load" type="primary" style="margin: 0 5px">
                    <el-icon>
                        <search/>
                    </el-icon>
                </el-button>
            </div>
            <el-table v-model:data="tableData"
                    stripe
                    border
                    style="width: 100%"
            >
                <el-table-column prop="id" label="id"/>
                <el-table-column prop="username" label="用户名"/>
                <el-table-column prop="nickname" label="昵称"/>
                <el-table-column prop="age" label="年龄"/>
                <el-table-column prop="sex" label="性别"/>
                <el-table-column prop="address" label="地址"/>
                <el-table-column label="操作">
                    <template #default="scope">
                        <el-button size="mini" @click="updateUser(scope.row)">编辑</el-button>
                        <el-popconfirm title="你确定要删除该数据吗?" @confirm="deleteUser(scope.row.id)">
                            <template #reference>
                                <el-button size="mini" type="danger">删除</el-button>
                            </template>
                        </el-popconfirm>
                    </template>
                </el-table-column>
            </el-table>
            <div class="demo-pagination-block">
                <el-pagination
                        v-model:currentPage="currentPage"
                        v-model:page-size="pageSize"
                        :page-sizes="[5, 10, 20]"
                        :small="small"
                        :disabled="disabled"
                        :background="background"
                        layout="total, sizes, prev, pager, next, jumper"
                        :total="totals"
                        @size-change="handleSizeChange"
                        @current-change="handleCurrentChange"
                />
            </div>
            <div>
                <el-dialog
                        v-model="dialogVisible"
                        title="新增数据"
                        width="60%">
                    <el-form :model="form" :rules="rules" ref="ruleForm" label-width="140px">
                        <el-form-item label="用户名:" prop="username">
                            <el-input v-model.trim="form.username" style="width: 80%"/>
                        </el-form-item>
                        <el-form-item label="昵  称:" prop="nickname">
                            <el-input v-model.trim="form.nickname" style="width: 80%"/>
                        </el-form-item>
                        <el-form-item label="密  码:" prop="password">
                            <el-input type="password" show-password v-model.trim="form.password" style="width: 80%"/>
                        </el-form-item>
                        <el-form-item label="年  龄:" prop="age">
                            <el-input type="number" v-model.trim="form.age" style="width: 80%"/>
                        </el-form-item>
                        <el-form-item label="性  别:" prop="sex">
                            <el-checkbox-group v-model="form.sex">
                                <el-radio label="男" v-model="form.sex"/>男
                                <el-radio label="女" v-model="form.sex"/>女
                                <el-radio label="未知" v-model="form.sex"/>未知
                            </el-checkbox-group>
                        </el-form-item>
                        <el-form-item label="地址:" prop="address">
                            <el-input v-model.lazy="form.address" type="textarea" style="width: 80%"></el-input>
                        </el-form-item>
                    </el-form>
                    <template #footer>
                    <span class="dialog-footer">
                        <el-button @click="dialogVisible = false">取消</el-button>
                        <el-button type="primary" @click="addFormSave('ruleForm')">确定</el-button>
                    </span>
                    </template>
                </el-dialog>
            </div>
        </div>
    </template>
    <script>
        import {Search} from '@element-plus/icons-vue'
        import request from "../utils/reques";
    
        export default {
            name: 'HomeView',
            components: {
                Search
            },
            data() {
                return {
                    search: '',
                    totals: 0,
                    currentPage: '',
                    pageSize: 10,
                    dialogVisible: false,
                    tableData: [],
                    form: {
                        username: '',
                        password: '',
                        nickname: '',
                        age: 0,
                        sex: '',
                        address: '',
                    },
                    // 表单校验
                    rules: {
                        username: [
                            { required: true, message: '请输入用户名', trigger: 'blur' },
                            { min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                        ],
                        nickname: [
                            { required: true, message: '请输入昵称', trigger: 'blur' },
                            { min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                        ],
                        password: [
                            { required: true, message: '请输入密码', trigger: 'blur' },
                            { min: 3, max: 11, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                        ],
                        age: [
                            { required: true, message: '请输入年龄', trigger: 'blur' }
                        ],
                        sex: [
                            {required: true, message: '请选择性别', trigger: 'change' }
                        ],
                        address: [
                            { required: true, message: '请输入地址', trigger: 'blur' },
                        ]
                    }
                }
            },
            created() {
                this.load()
            },
            methods: {
                // 加载初始数据
                load(){
                    request.get("user/",{
                        params:{
                            pageNum: this.currentPage,
                            pageSize: this.pageSize,
                            search: this.search
                        }
                    }).then(res => {
                        this.tableData = res.data.records;
                        this.totals = res.data.total;
                    })
                },
                //新增数据按钮
                addUser() {
                    this.dialogVisible = true;
                    this.form = {};
                },
                // 点击新增按钮后在弹出的表单点击确定按钮
                addFormSave(formName) {
                    /**
                    * 配置axios请求,请求的url和请求参数
                    * .then（）链式操作，前一步完成后，结果传入
                    */
                    //表单校验
                    this.$refs[formName].validate((valid) => {
                        if (valid) {
                            request.post("/user",this.form).then(res=>{
                                if (res.code=="0"){
                                    this.$message({
                                        message: '操作成功o(*￣▽￣*)ブ',
                                        type: 'success'
                                    });
                                    this.dialogVisible = false;
                                    this.load();
                                }else {
                                    this.$message({
                                        message: '操作失败,,ԾㅂԾ,,',
                                        type: 'error'
                                    });
                                    this.dialogVisible = false;
                                }
                            });
                        } else {
                            this.$message({
                                message: '请确认表单(ˉ▽ˉ；)...',
                                type: 'warning'
                            });
                            return false;
                        }
                    });
                },
                //删除数据按钮
                deleteUser(userId) {
                    request.delete("user/"+userId,).then(res =>{
                        if (res.code=="0"){
                            this.$message({
                                message: '操作成功o(*￣▽￣*)ブ',
                                type: 'success'
                            });
                            this.load();
                        }else {
                            this.$message({
                                message: '操作失败,,ԾㅂԾ,,',
                                type: 'error'
                            });
                        }
                    });
                },
                //编辑信息按钮
                updateUser(row) {
                    this.form = JSON.parse(JSON.stringify(row));
                    this.dialogVisible = true;
                    this.load();
                },
                //导出数据按钮
                exportUser() {
                    this.load();
                },
                //导入数据按钮
                importUser() {
    
                },
                // 改变当前每页数据个数触发
                handleSizeChange() {
                    this.load();
                },
                // 改变当前页码触发
                handleCurrentChange() {
                    this.load();
                }
            }
        }
    </script>

 五、一些技术点
1.前端解决跨域访问问题 vue.config.js文件
// 解决跨域访问
// 跨域配置
module.exports = {
    devServer: {                //记住，别写错了devServer//设置本地默认端口  选填
        port: 8089,
        proxy: {                 //设置代理，必须填
            '/api': {              //设置拦截器  拦截器格式   斜杠+拦截器名字，名字可以自己定
                target: 'http://localhost:8090',     //代理的目标地址
                changeOrigin: true,              //是否设置同源，输入是的
                pathRewrite: {                   //路径重写
                    '/api': ''                     //选择忽略拦截器里面的内容
                }
            }
        }
    }
}

2.使用 element-puls  icon图标


引入icon：

//{Location, Grid, Document, Setting,User}为所使用的icon图标名称

import {Location, Grid, Document, Setting,User} from '@element-plus/icons-vue'
export default {
    name: "Aside",
    components: {
        Location,
        Grid,
        Document,
        Setting,
        User
    }
}
使用：

    <el-icon>
        <user/>
    </el-icon>
3.点击导航栏选项跳转视图页面
在导航栏属性加入 router

选项的 index值为配置好的路由
![7](/images/StudyVue/7.png)


4.前端登录状态拦截 utils/reques.js 文件
![8](/images/StudyVue/8.png)

六、后端工程
1.SpringBoot工程结构
![9](/images/StudyVue/9.png)

2.Maven文件/依赖
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.6.7</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
        <groupId>com.example</groupId>
        <artifactId>demo</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>springbootdemo</name>
        <description>Demo project for Spring Boot</description>
        <properties>
            <java.version>11</java.version>
        </properties>
        <dependencies>
            <!--        web-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
            <!--mysql-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <scope>runtime</scope>
            </dependency>
            <!--        lombok-->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <optional>true</optional>
            </dependency>
            <!--        mybatis-->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.2</version>
            </dependency>
            <!--        MybatisPlus依赖-->
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.4.3.1</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
        </dependencies>
    
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <excludes>
                            <exclude>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </exclude>
                        </excludes>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    
    </project>

3.application.yml配置文件
server:
  port: 8090
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springboot-vue
    username: root
    password: 123456
4.实体类User
package com.example.demo.pojo;
 
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;
 
/**
 * 用户实体类
 */
@TableName("user")//与数据库表明对应
@Data//lombok 的Data注解,不用写get/set方法
public class User {
    @TableId(type = IdType.AUTO)//主键id自动生成
    private Integer id;
    private String username;
    private String password;
    private String nickname;
    private Integer age;
    private String sex;
    private String address;
}

5.封装统一返回对象Result
package com.example.demo.pojo;
 
/**封装的统一返回对象
 * @param <T>
 */
public class Result<T> {
    private String code;
    private String msg;
    private T data;
 
    public String getCode() {
        return code;
    }
 
    public void setCode(String code) {
        this.code = code;
    }
 
    public String getMsg() {
        return msg;
    }
 
    public void setMsg(String msg) {
        this.msg = msg;
    }
 
    public T getData() {
        return data;
    }
 
    public void setData(T data) {
        this.data = data;
    }
 
    public Result() {
    }
 
    public Result(T data) {
        this.data = data;
    }
 
    public static Result success() {
        Result result = new Result<>();
        result.setCode("0");
        result.setMsg("成功");
        return result;
    }
 
    public static <T> Result<T> success(T data) {
        Result<T> result = new Result<>(data);
        result.setCode("0");
        result.setMsg("成功");
        return result;
    }
 
    public static Result error(String code, String msg) {
        Result result = new Result();
        result.setCode(code);
        result.setMsg(msg);
        return result;
    }
    public static Result error() {
        Result result = new Result<>();
        result.setCode("1");
        result.setMsg("失败");
        return result;
    }
}

6.UserMapper层
package com.example.demo.mapper;
 
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.demo.pojo.User;
import org.springframework.stereotype.Controller;
 
/**
 * User的持久层
 */
@Controller
//继承BaseMapper接口
public interface UserMapper extends BaseMapper<User> {
 
}
7.UserController控制层
package com.example.demo.controller;
 
import com.baomidou.mybatisplus.core.toolkit.Wrappers;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.example.demo.mapper.UserMapper;
import com.example.demo.pojo.Result;
import com.example.demo.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.bind.annotation.*;
 
 
 
/**
 * User控制器
 */
@RestController
@RequestMapping("/user")
public class UserController {
 
    @Autowired
    private UserMapper userMapper;
 
 
    /**
     * 登陆方法
     * @param user
     * @return
     */
    @PostMapping("/login")
    public Result login(@RequestBody User user){
        //在数据库中查询与传来的用户名和密码系统的数据
        User res = userMapper.selectOne(Wrappers.<User>lambdaQuery().eq(User::getUsername,user.getUsername()).eq(User::getPassword,user.getPassword()));
        if (res==null){
            return Result.error("-1","用户名或密码错误");
        }else {
            return Result.success(res);
        }
    }
 
    /**
     * 注册方法
     * @param user
     * @return
     */
    @PostMapping("/register")
    public Result Register(@RequestBody User user){
        //在数据库中查询与传来的用户名和密码系统的数据
        User res = userMapper.selectOne(Wrappers.<User>lambdaQuery().eq(User::getUsername,user.getUsername()));
        if (res!=null){
            return Result.error("-1","用户名重复");
        }else {
            userMapper.insert(user);
            return Result.success(res);
        }
    }
 
 
    /**
     * 新增数据操作
     *
     * @param user
     * @return
     */
    @Transactional
    @PostMapping
    //@RequestBody注解，将传入的json数据转换为对象
    public Result saveUser(@RequestBody User user) {
        if (user.getId() != null) {
            userMapper.updateById(user);
        } else {
            userMapper.insert(user);
        }
        return Result.success();
    }
 
    /**
     * 根据ID删除数据
     * @param
     * @return
     */
    @Transactional
    @DeleteMapping("/{userId}")
    public Result deleteUser(@PathVariable Integer userId){
        userMapper.deleteById(userId);
        return Result.success();
    }
 
    /**
     * 分页查询User表
     * 根据前端传来的搜索值进行模糊擦查询
     *
     * @param pageNum
     * @param pageSize
     * @param search
     * @return
     */
    @GetMapping
    public Result findPage(@RequestParam(defaultValue = "1") Integer pageNum,
                           @RequestParam(defaultValue = "10") Integer pageSize,
                           @RequestParam(defaultValue = "") String search) {
        // Wrappers.<User>lambdaQuery().like(User::getNickname,search) 模糊查询，根据前端传来的search
        Page<User> page = userMapper.selectPage(new Page<>(pageNum, pageSize), Wrappers.<User>lambdaQuery().like(User::getNickname, search));
        return Result.success(page);
    }
 
}

七、后端技术点
1.MybatisPlus分页插件配置
package com.example.demo.common.config;
 
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
 
/**
 *  mybatis-plus 分页插件
 */
@Configuration
//扫描mapper文件包，加入ioc容器，不用在写@Mapper
@MapperScan("com.example.demo.mapper")
public class MybatisPlusConfig {
 
    /**
     * 分页插件
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
 
}
