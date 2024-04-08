<template>
  <div id="app">
    <div class="col-8 offset-2">
<table class="table caption-top table-hover table-striped">
  <caption class="text-center">
    <h1>学生管理系统</h1>
    <el-button type="primary" @click="getStudents">获取学生信息</el-button>
    <el-button type="warning" @click="dialogVisible = true">添加学生</el-button>
    <el-input type="text" placeholder="请输入姓名"  class="w-25" v-model="search_name"/><el-button type="success" @click="search">搜索</el-button>

<el-dialog
  title="提示"
  :visible.sync="dialogVisible"
  width="30%"
  :before-close="handleClose">
  <div>添加学生信息</div>
  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="addStudent">添 加</el-button>
  </span>
  <span>学号</span><input v-model="newStudent.number"/> <br/>
  <span>姓名</span><input v-model="newStudent.name"/> <br/>
  <span>年龄</span><input v-model="newStudent.age"/> <br/>
  <span>地区</span><input v-model="newStudent.place"/> <br/>
  <span>语文</span><input v-model="newStudent.chi"/> <br/>
  <span>数学</span><input v-model="newStudent.math"/> <br/>
  <span>英语</span><input v-model="newStudent.eng"/> <br/>
</el-dialog>
<el-button type="info" @click="dialogVisiblelogin = true">登录</el-button>

<el-dialog
  title="登录"
  :visible.sync="dialogVisiblelogin"
  width="30%"
  :before-close="handleClose">
  <span>用户名</span>
  <input type="text" v-model="user_for_login.username"/>
  <br/>
  <span>密码</span>
  <input type="password" v-model="user_for_login.password"/>

  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisiblelogin = false">取 消</el-button>
    <el-button type="primary" @click="login">登 录</el-button>
  </span>


</el-dialog>
<el-button type="danger" @click="dialogVisibleregister = true">注册</el-button>

<el-dialog
  title="提示"
  :visible.sync="dialogVisibleregister"
  width="30%"
  :before-close="handleClose">
  <span>用户名</span>
  <input type="text" v-model="user_for_register.username"/>
  <br/>
  <span>密码</span>
  <input type="password" v-model="user_for_register.password"/>
  <br/>
  <span>确认密码</span>
  <input type="password" v-model="user_for_register.confirmpassword"/>

  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisibleregister = false">取 消</el-button>
    <el-button type="primary" @click="register">注 册</el-button>
  </span>
</el-dialog>
  </caption>
  <thead>
    <tr>
      <th scope="col">学号</th>
      <th scope="col">姓名</th>
      <th scope="col">年龄</th>
      <th scope="col">地区</th>
      <th scope="col">语文</th>
      <th scope="col">数学</th>
      <th scope="col">英语</th>
      <th scope="col">操作</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <Student v-for="stu in students_for_page" :key="stu.id" :student="stu"></Student>
  </tbody>
</table>
  <div class="text-center">
    <el-button-group>
      <el-button type="primary" icon="el-icon-arrow-left" @click = "last_page">上一页</el-button>
      <el-button type="primary" @click = "next_page">下一页<i class="el-icon-arrow-right el-icon--right"></i></el-button>
    </el-button-group>
  </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Student from './components/Student'

export default {
  name: 'App',
  components: {
    Student
  },
  data(){
    return{
      search_name:"",
      user_for_login:{
        username:"",
        password:""
      },
      user_for_register:{
        username:"",
        password:"",
        confirmpassword:""
      },
      page:1,
      students:[],
      dialogVisible: false,
      dialogVisiblelogin: false,
      dialogVisibleregister: false,
      newStudent:{
        number:"",
        name:"",
        age:"",
        place:"",
        chi:"",
        math:"",
        eng:"",
      }
    } 
  },

  methods: {
    getStudents() {
      if(sessionStorage.getItem("isLogined")=="true"){
        axios({
        url:"http://localhost:8080/students",
        method:"GET",
      }).then(res=>{
        console.log(res.data);
        this.students=res.data;
      })
      }
      else{
        this.$alert("请先登录");
      }
    },
    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },
    addStudent(){
      axios({
        url:"http://localhost:8080/add",
        method:"POST",
        data:this.newStudent
      })
      this.dialogVisible = false
    },
    next_page(){
      this.page++;
    },
    last_page(){
      this.page--;
    },
    login(){
      axios({
        url:"http://localhost:8080/login",
        method:"POST",
        data:this.user_for_login,
      }).then(res=>{
        console.log(res.data);
        if(res.data=="1"){
          sessionStorage.setItem("isLogined","true");
        }
        else{
          this.$alert("用户名或密码错误");
        }
        
      })
      this.dialogVisiblelogin = false
    },
    register(){
      axios({
        url:"http://localhost:8080/register",
        method:"POST",
        data:this.user_for_register,
      })
      this.dialogVisibleregister = false
      this.$alert("注册成功")
    },
    search(){
      axios({
        url:"http://localhost:8080/search",
        method:"POST",
        data:{
          name:this.search_name,
        }
        }).then(res=>{
          this.students=res.data;
      })
      console.log(res.data);
    },

    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    }
  },
  computed:{
    students_for_page(){
      return this.students.slice(this.page*5-5,this.page*5)
    }
  }
}
</script>

<style>

</style>
