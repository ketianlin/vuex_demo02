<template>
  <div id="app">
    <a-input
      @change="e => setInputValue(e.target.value)"
      :value="inputValue"
      placeholder="请输入任务"
      class="my_ipt"
    />
    <a-button @click="addItemToList" type="primary">添加事项</a-button>

    <a-list bordered :dataSource="infolist" class="dt_list">
      <a-list-item slot="renderItem" slot-scope="item">
        <!-- 复选框 -->
        <a-checkbox
          @change="e => cbStatusChanged(e, item.id)"
          :checked="item.done"
          >{{ item.info }}</a-checkbox
        >
        <!-- 删除链接 -->
        <a @click="removeItem(item.id)" slot="actions">删除</a>
      </a-list-item>

      <!-- footer区域 -->
      <div slot="footer" class="footer">
        <!-- 未完成的任务个数 -->
        <span>{{unDoneLength}}条剩余</span>
        <!-- 操作按钮 -->
        <a-button-group>
          <a-button :type="viewKey === 'all' ? 'primary' : ''" @click="changeViewKey('all')">全部</a-button>
          <a-button :type="viewKey === 'undone' ? 'primary' : ''" @click="changeViewKey('undone')">未完成</a-button>
          <a-button :type="viewKey === 'done' ? 'primary' : ''" @click="changeViewKey('done')">已完成</a-button>
        </a-button-group>
        <!-- 把已经完成的任务清空 -->
        <a @click="cleanDone">清除已完成</a>
      </div>
    </a-list>
  </div>
</template>

<script>
import { mapActions, mapState, mapMutations, mapGetters } from 'vuex'
export default {
  name: 'app',
  data() {
    return {}
  },
  methods: {
    ...mapActions(['getList']),
    ...mapMutations(['setInputValue', 'removeItem', 'cleanDone', 'changeViewKey']),
    addItemToList() {
      if (this.inputValue.trim().length <= 0) {
        return this.$message.warning('任务名称不能为空！')
      }
      this.$store.commit('addItem')
    },
    // 监听复选框选中状态变化的事件
    cbStatusChanged(e, id) {
      this.$store.commit('changeStatus', {
        id,
        status: e.target.checked
      })
    }
  },
  created() {
    this.getList()
  },
  computed: {
    ...mapState(['inputValue', 'viewKey']),
    ...mapGetters(['unDoneLength', 'infolist'])
  }
}
</script>

<style scoped>
#app {
  padding: 10px;
}

.my_ipt {
  width: 500px;
  margin-right: 10px;
}

.dt_list {
  width: 500px;
  margin-top: 10px;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
