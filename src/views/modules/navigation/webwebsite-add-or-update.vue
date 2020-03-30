<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="名称" prop="name">
      <el-input v-model="dataForm.name" placeholder="名称"></el-input>
    </el-form-item>
    <el-form-item label="url地址" prop="url">
      <el-input v-model="dataForm.url" placeholder="url地址"></el-input>
    </el-form-item>
    <el-form-item label="协议" prop="protocol">
      <el-input v-model="dataForm.protocol" placeholder="协议"></el-input>
    </el-form-item>
    <el-form-item label="主地址" prop="host">
      <el-input v-model="dataForm.host" placeholder="主地址"></el-input>
    </el-form-item>
    <el-form-item label="路径" prop="path">
      <el-input v-model="dataForm.path" placeholder="路径"></el-input>
    </el-form-item>
    <el-form-item label="端口" prop="port">
      <el-input v-model="dataForm.port" placeholder="端口"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          name: '',
          url: '',
          protocol: '',
          host: '',
          path: '',
          port: ''
        },
        dataRule: {
          name: [
            { required: true, message: '名称不能为空', trigger: 'blur' }
          ],
          url: [
            { required: true, message: 'url地址不能为空', trigger: 'blur' }
          ],
          protocol: [
            { required: true, message: '协议不能为空', trigger: 'blur' }
          ],
          host: [
            { required: true, message: '主地址不能为空', trigger: 'blur' }
          ],
          path: [
            { required: true, message: '路径不能为空', trigger: 'blur' }
          ],
          port: [
            { required: true, message: '端口不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/navigation/webwebsite/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.webWebsite.name
                this.dataForm.url = data.webWebsite.url
                this.dataForm.protocol = data.webWebsite.protocol
                this.dataForm.host = data.webWebsite.host
                this.dataForm.path = data.webWebsite.path
                this.dataForm.port = data.webWebsite.port
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/navigation/webwebsite/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'url': this.dataForm.url,
                'protocol': this.dataForm.protocol,
                'host': this.dataForm.host,
                'path': this.dataForm.path,
                'port': this.dataForm.port
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
