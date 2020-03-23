<template>
  <div class="mod-oss">
    <el-form :inline="true">
      <el-form-item>
        <el-button type="primary" @click="uploadHandle()">上传文件</el-button>
        <el-button type="primary" @click="test()">测试</el-button>
      </el-form-item>
    </el-form>
    <nav-upload v-if="uploadVisible" ref="upload" @refreshDataList="getDataList"></nav-upload>
  </div>
</template>

<script>
    import NavUpload from './nav-upload'
export default {
      name: 'website',
      components: {NavUpload},
      data () {
        return {
          uploadVisible: false
        }
      },
      methods: {
        // 上传文件
        uploadHandle () {
          this.uploadVisible = true
          this.$nextTick(() => {
            this.$refs.upload.init()
          })
        },
        getDataList () {

        },
        test () {
          this.$http({
            url: this.$http.adornUrl(`/nav/website/test`),
            method: 'get'
          }).then(({data}) => {

          })
        }
      }
    }
</script>

<style scoped>

</style>
