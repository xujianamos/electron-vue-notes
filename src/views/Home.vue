<template>
  <div class="app-wrapper">
    <div class="sidebar-container">
      <file-search v-model="searchTitle" />
      <file-list :fileList="fileList" />
    </div>
    <div class="main-container"></div>
  </div>
</template>

<script>
import FileSearch from '@/components/FileSearch'
import FileList from '@/components/FileList'

export default {
  name: 'Home',
  data() {
    return {
      searchTitle: '',
      fileList: [
        { id: 1, title: '文件名 1', time: '2020-06-21' },
        { id: 2, title: '文件名 2', time: '2020-06-21' },
        { id: 3, title: '文件名 3', time: '2020-06-21' },
        { id: 4, title: '文件名 4', time: '2020-06-21' },
        { id: 5, title: '文件名 5', time: '2020-06-21' },
        { id: 6, title: '文件名 6', time: '2020-06-21' },
        { id: 1, title: '文件名 1', time: '2020-06-21' },
        { id: 2, title: '文件名 2', time: '2020-06-21' },
        { id: 3, title: '文件名 3', time: '2020-06-21' },
        { id: 4, title: '文件名 4', time: '2020-06-21' },
        { id: 5, title: '文件名 5', time: '2020-06-21' },
        { id: 6, title: '文件名 6', time: '2020-06-21' }
      ]
    }
  },

  components: {
    FileSearch,
    FileList
  },

  computed: {},

  methods: {}
}
</script>
<style lang="less" scoped>
.app-wrapper {
  display: flex;
  > .sidebar-container {
    width: 300px;
    height: 100vh;
    border-right: 1px solid #eaeefb;
  }
  > .main-container {
    flex: 1;
  }
}
</style>