<template>
  <div class="home">
    <form id="sumComp">
        <input type="text" class="form-control" v-model="state.num1"> +
        <input type="text" class="form-control" v-model="state.num2">
        <button @click="addNumbers" type="button" class="btn btn-light"> Add me! </button>
        <div v-if="clicked">Сумма: {{state.sum}}</div>
    </form>
    <child :msg='msg' />
  </div>
</template>

<script lang="ts">
import { ref, reactive, computed, provide, defineComponent, onMounted, onUnmounted } from 'vue'
import child from '../components/Child.vue'
// import { onUnmounted } from '@vue/composition-api'

export default defineComponent({
  name: 'Home',
  components: {
    child
  },
  setup () {
    const x = ref(0)
    const y = ref(0)
    const msg = 'сообщение от родителя'
    let clicked = true

    const state = reactive({
      num1: 0,
      num2: 0,
      sum: 0
    })

    provide('username', computed(() => state.sum))

    function addNumbers () {
      // sum.value = Number(num1.value) + Number(num2.value)
      state.sum = Number(state.num1) + Number(state.num2)

      clicked = false
    }
    onMounted(() => {
      window.addEventListener('mousemove', updateCoords)
      console.log('onMounted!')
    })

    onUnmounted(() => {
      console.log('onUnmounted!')
    })

    function updateCoords (e:MouseEvent) {
      x.value = e.clientX
      y.value = e.clientY
      console.log(x.value)
      console.log(y.value)
    }
    return { state, x, y, msg, clicked, addNumbers, updateCoords }
  }
})
</script>
<style lang="stylus">
.home
  width: 100vw;
  height: 100vh;
.form-control
  margin: 20px
  padding: 5px
  border-radius: 5px
.btn
  padding: 5px
  background: black
  border-radius: 5px
  color white
  padding: 5px 20px
</style>
