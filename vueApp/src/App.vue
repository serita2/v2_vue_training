<template>
  <div id="wrap">
    <form action="">
      <MyLabel>名前</MyLabel>
      <MyInput
        v-model="sampleForm.text"
        placeholder="山本 太郎"
        name="sample-input"
        type="text"
      ></MyInput>
      <MyLabel>性別</MyLabel>
      <MyRadio
        v-model="sampleForm.radio"
        name="sample-radio"
        :sex="sex"
        :style="str"
      ></MyRadio>
      <MyLabel>食事</MyLabel>
      <MyCheckbox
        v-model="sampleForm.checkbox"
        name="sample-checkbox"
        :meal="meal"
        :style="str"
      ></MyCheckbox>
      <MyLabel>部屋タイプ</MyLabel>
      <MySelect
        v-model="sampleForm.select"
        name="sample-select"
        :roomType="roomType"
        :style="str"
      ></MySelect>
      <MyLabel>備考</MyLabel>
      <MyTextarea
        v-model="sampleForm.textarea"
        placeholder="ご要望がございましたら入力してください。"
        name="sample-textarea"
        :rows="5"
        :cols="35"
      ></MyTextarea>
      <MyBtn @click="sendForm">送信</MyBtn>
    </form>
    <p>{{ list.radio }}</p>
  </div>
</template>

<script>
import MyInput from "./components/MyInput.vue";
import MyTextarea from "./components/MyTextarea.vue";
import MyRadio from "./components/MyRadio.vue";
import MyCheckbox from "./components/MyCheckbox.vue";
import MySelect from "./components/MySelect.vue";
import MyBtn from "./components/MyBtn.vue";
import MyLabel from "./components/MyLabel.vue";
import axios from 'axios';

export default {
  name: "app",
  components: {
    MyTextarea,
    MyInput,
    MyRadio,
    MyCheckbox,
    MySelect,
    MyBtn,
    MyLabel
  },
  data() {
    return {
      str: "border: none;",
      sampleForm: {
        text: "",
        radio: "",
        select: "",
        textarea: "",
        checkbox: []
      },
      sex: [
        {
          label: "男性",
          value: "1"
        },
        {
          label: "女性",
          value: "2"
        },
        {
          label: "未選択",
          value: "3"
        }
      ],
      roomType: [   
        {
          label: "スタンダート",
          value: "1"
        },
        {
          label: "スーペリア",
          value: "2"
        },
        {
          label: "デラックス",
          value: "3"
        },
        {
          label: "スイート",
          value: "4"
        }
      ],
      meal: [
        {
          label: "朝食付",
          value: "morning"
        },
        {
          label: "夕食付",
          value: "dinner"
        }
      ],
      list: {}
    };
  },
  methods: {
    sendForm() {
      axios
        .post('http://localhost:3000/reservation', {
          params: {
            name: this.sampleForm.text,
            sex: this.sampleForm.radio,
            meal: this.sampleForm.checkbox,
            room: this.sampleForm.select,
            remarks: this.sampleForm.textarea
        }
      })
    }
  },
  mounted(){
    axios
      .get('http://localhost:3000/reservation')
      .then(function(response){
            this.list = response.data.sampleForm
            console.log(response.data);
        }.bind(this))
        .catch(function(error){
            console.log(error)
        })
  },
  
};
</script>

<style></style>
