<!--
 * @Description: 左侧文件搜索组件
 -->
<template>
  <div class="search-container">
    <el-input placeholder="请输入内容" v-bind="$attrs" v-on="$listeners">
      <el-button slot="append" icon="el-icon-search" @click.native="onSearch()" />
    </el-input>
    <el-dropdown>
      <el-button type="primary" icon="el-icon-circle-plus-outline" circle />
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item @click.native="createFile()">新建笔记</el-dropdown-item>
        <el-dropdown-item divided @click.native="importFile()">导入文件</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
export default {
  name: 'FileSearch',
  methods: {
    // 新建笔记
    createFile() {
      this.$emit('create')
    },
    // 导入文件
    importFile() {
      this.$emit('import')
    },
    // 搜索文件
    onSearch() {
      this.$emit('search')
    }
  }
}
</script>

<style lang="less" scoped>
.search-container {
  display: flex;
  align-items: center;
  padding: 12px 10px;
  background: #daecfe;

  .el-dropdown {
    .el-button--small {
      margin-left: 10px;
      padding: 6px;
      font-size: 14px;
      border-radius: 30%;
    }
  }
}
</style>
