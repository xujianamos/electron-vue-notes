<template>
  <div class="app-wrapper">
    <div class="sidebar-container">
      <file-search v-model="searchTitle" />
      <file-list :fileList="fileList" />
    </div>
    <div class="main-container">
      <file-edit
        v-model="fileItem.content"
        :title.sync="fileItem.title"
        :boxShadow="false"
        :subfield="false"
        :shortCut="false"
        @change="onSubmit"
      />
    </div>
  </div>
</template>

<script>
import FileSearch from '@/components/FileSearch'
import FileList from '@/components/FileList'
import FileEdit from '@/components/FileEdit'

export default {
  name: 'Home',
  data() {
    return {
      searchTitle: '',
      fileList: [
        { id: 1, title: '文件名 1', time: '2020-07-21' },
        { id: 2, title: '文件名 2', time: '2020-07-22' },
        { id: 3, title: '文件名 3', time: '2020-07-23' },
        { id: 4, title: '文件名 4', time: '2020-07-24' },
        { id: 5, title: '文件名 5', time: '2020-07-25' },
        { id: 6, title: '文件名 6', time: '2020-07-26' },
        { id: 7, title: '文件名 7', time: '2020-07-27' },
        { id: 8, title: '文件名 8', time: '2020-07-28' },
        { id: 9, title: '文件名 9', time: '2020-07-29' },
        { id: 10, title: '文件名 10', time: '2020-07-30' },
        { id: 11, title: '文件名 11', time: '2020-07-31' },
        { id: 12, title: '文件名 12', time: '2020-07-32' }
      ],
      fileItem: {
        title: '手摸手Electron + Vue实战教程（三）',
        content: ''
      }
    }
  },

  components: {
    FileSearch,
    FileList,
    FileEdit
  },

  computed: {},

  methods: {
    onSubmit(value) {
      console.log(value)
      console.log(this.fileItem)
    }
  }
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
