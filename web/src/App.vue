<template>
  <div class="chat">

    <div class="chat__header">
      <div class="chat__header__title">
        ЖИЗНЬ ШУШПАНЧИКОВ
      </div>
      <div class="chat__header__subtitle">
        Все, что вы хотели о ней узнать, но боялись спросить
      </div>    
    </div>
   
    <div 
      class="chat__body"
      ref="chatBody">
        <dialog-item
          v-for="(item, k) in dialogs"
          :key="k"
          :dialog="item"/>   
    </div>

    <input
      class="chat__input"
      :disabked="isInputDisabled"
      placeholder="Введите вопрос"
      v-model="currentQuestion"
      @change="handleChange" 
      autofocus/>
    
  </div>
</template>

<script>
import axios from 'axios'
import DialogItem from './components/DialogItem'

export default {
  name: 'app',
  components: {
    DialogItem
  },
  data () {
    return {
      isInputDisabled: false,
      currentQuestion: '',
      dialogs: [],
    }
  },
  updated() {
    this.$refs.chatBody.scrollTop = this.$refs.chatBody.scrollHeight
  },
  methods: {
    handleChange() {
      this.getAnswer(this.currentQuestion)
      this.currentQuestion = ''
    },
    getAnswer(str) { 
      const   options = {
        method: 'POST',
        url: `http://${location.host}/api/get-answer`,
        data: `${encodeURIComponent('q')}=${encodeURIComponent(str)}`,
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
            'X-Requested-With': 'XMLHttpRequest'
        }
      }
      const dateQ = Date.now()
      this.isInputDisabled = true
      axios(options)
        .then((resp) => {
          if( resp.data.ok) {
            console.log(resp.data.a)
            this.dialogs.push({
              q: { 
                content: str,
                date: dateQ
              },
              a: {
                content: resp.data.a,
                date: Date.now()
              }
            })
          } else {

          }
          this.isInputDisabled = false
        })
        .catch((err) => {
          //TODO: Workout error
          this.isInputDisabled = false
        })
    }
  }
}
</script>

<style>
.chat {
  font-family: sans-serif;
  color: #555;
  background-color: #fff;
  width: 600px;
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
  display: flex !important;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.chat__header {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center; 
}
.chat__header__title {
  font-size: 26px;
  font-weight: bold;
  color: #066;
}
.chat__header__subtitle {
  margin-top: 5px;
  margin-bottom: 10px;
  font-size: 16px;
  font-style: italic;
  font-weight: lighter;
  color: #066;
} 
.chat__body {
  width: 100%;
  height: 400px;
  padding-top: 10px;
  padding-bottom: 10px;
  border-radius: 15px;
  background-color: #eee;
  background-size: 100% 100%;
  overflow-y: auto;
}
.chat__input {
  width: 95%;
  height: 34px;
  padding-left: 15px;
  padding-right: 15px;
  margin-top: 10px;
  border: 1px solid #eee;
  border-radius: 17px;
  background-color: #eee;
  outline: none;
  font-size: 14px;
  color: #555;
}
.chat__input:focus {
  background-color: #fff;
  cursor: pointer;
  border: 1px solid #999;
}
</style>
