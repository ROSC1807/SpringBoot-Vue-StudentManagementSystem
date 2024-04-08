<template>
    <tr>
        <td v-show="!is_edit">{{ student.number }}</td>
        <td v-show="!is_edit">{{ student.name }}</td>
        <td v-show="!is_edit">{{ student.age }}</td>
        <td v-show="!is_edit">{{ student.place }}</td>
        <td v-show="!is_edit">{{ student.chi }}</td>
        <td v-show="!is_edit">{{ student.math }}</td>
        <td v-show="!is_edit">{{ student.eng }}</td>
        <td v-show="!is_edit">
            <el-button type="primary" @click="modify">修改</el-button>
        </td>
        <td v-show="!is_edit">
            <el-button type="danger" @click="deleteStudent">删除</el-button>
        </td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model="student.number"></td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model="student.name"></td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model.number="student.age"></td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model="student.place"></td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model.number="student.chi"></td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model.number="student.math"></td>
        <td v-show="is_edit"><input class="w-50" type="text" v-model.number="student.eng"></td>
        <td v-show="is_edit">
            <el-button type="primary" @click="save">保存</el-button>
        </td>
        <td v-show="is_edit">
            <el-button type="danger">删除</el-button>
        </td>
    </tr>
</template>

<script>
import axios from 'axios'
export default {
    props:["student"],
    data(){
        return{
            is_edit:false
        }
    },
    methods:{
        modify(){
            this.is_edit = true;
        },
        save(){
            axios({
                url: "http://localhost:8080/update",
                method: "post",
                data: this.student
            })
            this.is_edit = false;
        },
        deleteStudent(){
            axios({
                url: "http://localhost:8080/delete",
                method: "post",
                data: this.student
            })
            //location.reload();

        },
    }
}
</script>

<style>

</style>