<template>
    <div>
        <center>栏目管理</center><br/>
        <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">批量删除</el-button>

        <el-table :data="programmanagements">

            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column fixed="left" prop="name" label="栏目名称"></el-table-column>
            <el-table-column prop="num" label="序号"></el-table-column>
            <el-table-column prop="icon" width="200" label="图片"></el-table-column>
            <el-table-column prop="parentId" label="起源"></el-table-column>


            <el-table-column fixed="right" label="操作">
                <!--//slot接受当前行信息 -->
                <template v-slot="slot">

                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdataHandler(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>

        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>

        <el-dialog
        :title="title"
        :visible.sync="visible"
        width="60%"
        >

        ---{{form}}
        <el-form :model="form" label-width="80px">
            <el-form-item label="栏目名称">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="序号">
                <el-input v-model="form.num"></el-input>
            </el-form-item>
            <el-form-item label="图片">
                <el-input v-model="form.icon"></el-input>
            </el-form-item>
            <el-form-item label="起源">
                <el-input v-model="form.parentId"></el-input>
            </el-form-item>
        </el-form>

        <span slot="footer" class="dialog-footer">
            <el-button @click="closeMedalHandler" size="small">取 消</el-button>
            <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
        </span>
        </el-dialog>

    </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
        methods:{
            //重载
         loadData(){
             let url = "http://localhost:6677/category/findAll"
             request.get(url).then((response)=>{
                 this.programmanagements = response.data;
             });
        },

        toAddHandler(){
            this.title = "录入产品信息"
            this.visible = true;
        },
        toUpdataHandler(row){
            this.title = "修改产品信息"
            this.visible = true;
        },
        toDeleteHandler(id){
            //确认
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },   
        closeMedalHandler(){
            this.visible=false;
        },
        submitHandler(){
            let url = "http://localhost:6677/category/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Typpe":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeMedalHandler();
                    this.$message({
                    type:"success",
                    message:response.message
                })
                this.loadData();
            })
        }
    },
    created(){
        this.loadData();
    },

    //用于存放网页中显示的数据
    data(){
        return{
            visible:false,
            programmanagements:[],
            form:{
                type:"program"
            }
        }
    }
}
</script>
<style scoped>

</style>