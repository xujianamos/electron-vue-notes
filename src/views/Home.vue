<template>
  <div class="app-wrapper">
    <div class="sidebar-container">
      <file-search
        v-model.trim="searchTitle"
        @clear="getFileList"
        @change="handleChange"
        @create="fileCreate"
        @search="handleSearch"
        @keyup.enter.native="handleSearch"
        clearable
      />
      <file-list :fileList="fileList" :active.sync="activeIndex" :selectedFile.sync="selectedFile">
        <template v-slot:menu>
          <li @click="handleFileTop()">{{ selectedFile.isTop ? '取消置顶' : '置顶' }}</li>
          <li @click="fileDelete()">删除</li>
        </template>
      </file-list>
    </div>
    <div class="main-container">
      <div class="placeholder" v-if="fileList.length === 0">暂无笔记</div>
      <file-edit
        v-else
        v-model="fileItem.content"
        :title.sync="fileItem.title"
        :boxShadow="false"
        :subfield="false"
        :shortCut="false"
        @titleBlur="updateTitle"
        @change="updateContent"
      />
    </div>
  </div>
</template>

<script>
import FileSearch from '@/components/FileSearch'
import FileList from '@/components/FileList'
import FileEdit from '@/components/FileEdit'
import dayjs from 'dayjs'

export default {
  name: 'Home',
  data() {
    return {
      searchTitle: '',
      fileList: [],
      fileItem: {
        _id: '',
        title: '',
        content: ''
      },
      activeIndex: 0,
      // 定时器id
      timerId: 0,
      // 右键菜单选中的文件数据
      selectedFile: {}
    }
  },
  mounted() {
    this.init()
  },
  components: {
    FileSearch,
    FileList,
    FileEdit
  },

  computed: {},

  methods: {
    // 初始化
    async init() {
      await this.getFileList()
      if (this.fileList.length === 0) return
      const [firstFileItem] = this.fileList
      this.fileItem = firstFileItem
      this.activeIndex = 0
    },
    // 获取笔记列表
    async getFileList(query = {}) {
      const list = await this.$db.markdown.find(query).sort({ isTop: -1, updatedAt: -1 })
      // 格式化时间，添加内容备份字段
      // for (const item of list) {
      //   item.createdAt = dayjs(item.createdAt).format('YY-MM-DD HH:mm:ss')
      //   item.updatedAt = dayjs(item.updatedAt).format('YY-MM-DD HH:mm:ss')
      // }
      this.fileList = list.map(item => {
        item.originalContent = item.content
        item.createdAt = dayjs(item.createdAt).format('YYYY-MM-DD HH:mm:ss')
        item.updatedAt = dayjs(item.updatedAt).format('YYYY-MM-DD HH:mm:ss')
        return item
      })
    },
    // 新增笔记
    fileCreate() {
      console.log('123')
      // markdown文件只需要存储标题和内容就够了
      // isTop表示是否置顶
      const defaultFile = { title: '无标题笔记', content: '', isTop: false }
      console.log(this.$db.markdown)
      this.$db.markdown.insert(defaultFile).then(async () => {
        await this.getFileList()
        const [firstFileItem] = this.fileList
        this.fileItem = firstFileItem
        this.activeIndex = 0
        this.$message.success('新增笔记成功')
      })
    },
    // 搜索笔记
    handleSearch() {
      if (!this.searchTitle) return
      // 动态生成正则表达式
      const reg = new RegExp(`${this.searchTitle}`, 'i')
      const query = { title: reg }
      this.getFileList(query)
    },
    // 监听输入框的变化
    handleChange(val) {
      if (!val) {
        // 输入框为空就重新获取列表
        this.getFileList()
      }
    },
    // 实时监听标题内容并存储
    updateTitle(title) {
      const { id } = this.fileItem
      console.log(id)
      this.$db.markdown.update({ _id, title: { $ne: title } }, { $set: { title } }).then(() => {
        this.refreshList()
      })
    },
    // 实时监听编辑内容并保存
    updateContent(content) {
      const { _id, originalContent } = this.fileItem
      console.log(_id)
      if (originalContent === content) return
      if (this.timerId) clearTimeout(this.timerId)
      this.timerId = setTimeout(() => {
        this.$db.markdown.update({ _id, content: { $ne: content } }, { $set: { content } }).then(() => {
          this.refreshList()
        })
      }, 1000)
    },
    // 刷新列表，并高亮第一个笔记
    async refreshList() {
      if (this.activeIndex === 0) return
      await this.getFileList()
      const [firstFileItem] = this.fileList
      this.fileItem = firstFileItem
      this.activeIndex = 0
      this.$message.success('保存成功')
    },
    // 右键删除操作
    fileDelete() {
      this.$confirm('此操作将永久删除该笔记, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          const { _id } = this.selectedFile
          this.$db.markdown
            .remove({ _id })
            .then(num => {
              this.$message.success(`删除了${num}个项目`)
              this.init()
            })
            .catch(() => {
              this.$message.error('删除失败')
            })
        })
        .catch(() => {})
    },
    // 右键置顶操作
    handleFileTop() {
      const { _id, isTop } = this.selectedFile
      this.$db.markdown.update({ _id }, { $set: { isTop: !isTop } }).then(() => {
        this.init()
      })
    }
  },
  watch: {
    activeIndex(newValue) {
      this.fileItem = this.fileList[newValue]
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
