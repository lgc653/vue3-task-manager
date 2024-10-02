<template>
  <el-form
    ref="formRef"
    :model="form"
    :rules="rules"
  >
    <h2>添加任务</h2>
    <el-form-item
      label="任务名称"
      prop="newTask"
    >
      <el-input
        v-model="form.newTask"
        placeholder="输入任务名称"
      />
    </el-form-item>
    <el-form-item>
      <el-checkbox v-model="form.isCompleted">
        已完成
      </el-checkbox>
    </el-form-item>
    <el-form-item>
      <el-button
        type="primary"
        @click="submitTask"
      >
        添加
      </el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import { ref } from 'vue'
import { useStore } from 'vuex'

export default {
  setup() {
    const formRef = ref(null)
    const form = ref({
      newTask: '',
      isCompleted: false
    })

    const rules = {
      newTask: [
        { required: true, message: '任务名称不能为空', trigger: 'blur' }
      ]
    }

    const store = useStore()

    const submitTask = () => {
      formRef.value.validate((valid) => {
        if (valid) {
          store.dispatch('addTask', { name: form.value.newTask, completed: form.value.isCompleted })
          form.value.newTask = ''
          form.value.isCompleted = false
        } else {
          console.log('表单验证失败!')
          return false
        }
      })
    }

    return {
      form,
      rules,
      formRef,
      submitTask
    }
  }
}
</script>

<style scoped>
/* 添加样式 */
</style>

