<template>
  <div class="app-container">
    医院设置添加
    <el-form label-width="120px">
      <el-form-item label="医院名称">
        <el-input v-model="hospitalSet.hosname"/>
      </el-form-item>
      <el-form-item label="医院编号">
        <el-input v-model="hospitalSet.hoscode"/>
      </el-form-item>
      <el-form-item label="api基础路径">
        <el-input v-model="hospitalSet.apiUrl"/>
      </el-form-item>
      <el-form-item label="联系人姓名">
        <el-input v-model="hospitalSet.contactsName"/>
      </el-form-item>
      <el-form-item label="联系人手机">
        <el-input v-model="hospitalSet.contactsPhone"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="saveOrUpdate">保存</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>
<script>
import hospset from "@/api/hospset"

export default {
  data(){
    return {
      hospitalSet:{}
    }
  },
  created() {//页面渲染之前执行
    //获取路由id值
    //调用接口得到医院设置信息
    if(this.$route.params && this.$route.params.id){
      const id = this.$route.params.id
      this.getHospSet(id)
    }
  },
  methods:{
    //根据id查询
    getHospSet(id){
      hospset.getHospSet(id)
        .then(response =>{
          this.hospitalSet = response.data
        })
    },
    update(){
      hospset.updateHospSet(this.hospitalSet)
        .then(response => {
          //提示
          this.$message({
            type: 'success',
            message: '修改成功!'
          })
          //清空表单数据
          this.hospitalSet = {}
          //跳转列表页面，使用路由跳转方式实现
          this.$router.push({path:'/hospset/list'})
        })
    },
    save(){
      hospset.saveHospSet(this.hospitalSet)
        .then(response => {
          //提示
          this.$message({
            type: 'success',
            message: '修改成功!'
          })
          //跳转列表页面，使用路由跳转方式实现
          this.$router.push({path:'/hospset/list'})
        })
    },
    //添加
    saveOrUpdate(){
      //判断添加还是修改
      if(!this.hospitalSet.id){//没有id，做添加
        this.save()
      }else{//修改
        this.update()
      }
    }
  }
}
</script>
