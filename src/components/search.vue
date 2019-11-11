<template>
  <div class="search-area">
    <div class="search">
      <input v-model="input" type="text" placeholder="请输入搜索内容">
    <button @click="handleSearchResult(input)">搜索</button>
    </div>
    <div class="content">
      <span>搜索历史</span>
      <span class="cancel" @click="clearHistory">清空</span>
    </div>
    <div class="history">
      <span class="norecord" v-if="historyList.length == 0">暂无搜索记录</span>
      <span
        class="record"
        v-else
        v-for="(historyItem, index) in historyList"
        :key="index"
        @click="handleSearchResult(historyItem)"
      >
        {{historyItem}}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'search',
  data () {
    return {
      input: '',
      historyList: []
    }
  },
  mounted () {
    if (localStorage.getItem('localHistory') !== null) {
      this.historyList = localStorage.getItem('localHistory').split('|')
    }
  },
  methods: {
    /**
     * 执行搜索
     */
    handleSearchResult (val) {
      if (val === '') {
        alert('请输入搜索内容！')
        return
      }
      this.setlocalHistory(val) // 将搜索值加入本地localStorage
      this.historyList = localStorage.getItem('localHistory').split('|') // 从本地localStorage取出搜索历史并展示
      this.input = '' // 清空输入框
      // alert(`跳转至 ${val} 搜索结果页`) // 跳转至搜索结果页
    },
    /**
     * 加入历史搜索记录
     */
    setlocalHistory (val) {
      val = val.trim()
      let localHistory = localStorage.getItem('localHistory')
      if (localHistory === null) {
        localStorage.setItem('localHistory', val) // 若未设置过则直接设置本地存储
      }else {
        let localHistoryArray = localHistory.split('|').filter(item => item != val) // 删除搜索历史中与本次输出重复项
        if (localHistoryArray.length > 0) {
          localHistory = val + '|' + localHistoryArray.join('|') // 新增记录
        }
        if (localHistory.split('|').length > 10) { // 最大历史搜索记录10条
          localHistoryArray = localHistory.split('|')
          localHistoryArray.pop() // 删除最旧一项
          localHistory = localHistoryArray.join('|')
        }
        localStorage.setItem('localHistory', localHistory) // 存入本地
      }
    },
    /**
     * 清空历史搜索记录
     */
    clearHistory () {
      localStorage.removeItem('localHistory') // 清空搜索历史
      this.historyList = []
    }
  }
}
</script>

<style>
.search-area {
  width: 230px;
  height: 120px;
  border: 1px solid #ccc;
  padding: 20px;
}
.content {
  width: 225px;
  font-size: 12px;
  margin-top: 10px;
}
.content .cancel {
  float: right;
  color: #1F8CEB;
  cursor: pointer;
}
.search button {
  margin-left: 10px;
}
.history {
  margin-top: 10px;
}
.history .norecord {
  font-size: 12px;
  color: #aaa;
  margin-left: 80px;
}
.history .record {
  display: inline-block;
  padding: 3px 10px;
  border-radius: 5px;
  background-color: #f6f6f6;
  font-size: 12px;
  color: #333;
  margin-right: 10px;
  margin-top: 5px;
  cursor: pointer;
}
</style>
