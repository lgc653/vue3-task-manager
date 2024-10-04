<template>
  <div>
    <h2>任务列表</h2>
    <div>
      <el-select v-model="filter" @change="filterTasks">
        <el-option label="所有任务" value="all" />
        <el-option label="已完成" value="completed" />
        <el-option label="未完成" value="incomplete" />
      </el-select>
    </div>
    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index">
        <el-checkbox v-model="task.completed" @change="updateTaskStatus(task)">
          {{ task.name }}
        </el-checkbox>
        <el-button type="danger" @click="removeTask(index)"> 删除 </el-button>
        <el-button @click="speekTask(index)">播放</el-button>
      </li>
    </ul>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  data() {
    return {
      filter: 'all',
      filteredTasks: [],
      speech: {
        allVoices: [],
        synth: window.speechSynthesis
      }
    }
  },
  computed: {
    ...mapGetters(['allTasks'])
  },
  mounted() {
    this.filteredTasks = this.allTasks
  },

  created() {
    this.speech.allVoices = this.speech.synth.getVoices()
    console.log('this.speech==', this.speech)
  },

  methods: {
    ...mapActions(['removeTask']),
    filterTasks() {
      if (this.filter === 'completed') {
        this.filteredTasks = this.allTasks.filter(task => task.completed)
      } else if (this.filter === 'incomplete') {
        this.filteredTasks = this.allTasks.filter(task => !task.completed)
      } else {
        this.filteredTasks = this.allTasks
      }
    },
    updateTaskStatus(task) {
      // 这里可以添加逻辑来更新任务状态
    },
    speekTask(index) {
      if (typeof SpeechSynthesisUtterance !== 'undefined') {
        if (this.speech.allVoices.length > 0) {
          const voiceUtt = new SpeechSynthesisUtterance(this.allTasks[index].name)
          voiceUtt.voice = this.speech.allVoices.filter(
            item => item.lang === 'zh-CN' && item.name.includes('(Natural)')
          )[0]
          this.speech.synth.speak(voiceUtt)
        } else {
          console.log('this.speech:', this.speech)
        }
      }
    }
  }
}
</script>

<style scoped>
/* 添加样式 */
</style>
