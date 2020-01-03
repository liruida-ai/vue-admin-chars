<template>
    <div>
        <center>产品管理</center><br/>
        <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">批量删除</el-button>

        <el-table :data="productmanagements">

            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column fixed="left" prop="name" label="产品名称"></el-table-column>
            <el-table-column prop="price" label="价格"></el-table-column>
            <el-table-column prop="photo" label="照片">
                
            </el-table-column>
            <el-table-column prop="description" label="描述"></el-table-column>
            <el-table-column prop="categoryId" width="200" label="所属产品"></el-table-column>


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
            <el-form-item label="产品名称">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="价格">
                <el-input v-model="form.price"></el-input>
            </el-form-item>
            <el-form-item label="描述">
                <el-input v-model="form.description"></el-input>
            </el-form-item>
            <el-form-item label="所属产品">
                <el-select v-model="value" placeholder="请选择">
                    <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                    </el-option>
                </el-select>

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
             let url = "http://localhost:6677/product/findAll"
             request.get(url).then((response)=>{
                 this.productmanagements = response.data;
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
            let url = "http://localhost:6677/product/saveOrUpdate"
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
            productmanagements:[],
            form:{
                type:"product"
            },
            
            options: [{
                value: '选项1',
                label: '9139'
                }, {
                value: '选项2',
                label: '9202'
                }, {
                value: '选项3',
                label: '9357'
                }, {
                value: '选项4',
                label: '9358'
                }, {
                value: '选项5',
                label: '9392'
                }, {
                value: '选项6',
                label: '9411'
                }, {
                value: '选项7',
                label: '9451'
                }, {
                value: '选项8',
                label: '9476'
                }, {
                value: '选项9',
                label: '9477'
                }, {
                value: '选项10',
                label: '9478'
                }, {
                value: '选项11',
                label: '9479'
                }, {
                value: '选项12',
                label: '9480'
                }, {
                value: '选项13',
                label: '9481'
                }
                ],
                value: ''
            }
    }
}
</script>
<style scoped>

</style>