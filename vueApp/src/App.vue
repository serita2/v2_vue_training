<template>
  <div class="box_con">
    <h2>○簡易フォーム</h2>
    <form class="form-group" action="">
      <table class="formTable">
        <tr>
          <th><MyLabel>名前 <small style="color: red;">必須</small></MyLabel></th>
          <td>
            <MyInput
              v-model="sampleForm.text"
              placeholder="山本 太郎"
              name="sample-input"
              type="text"
            ></MyInput>
          </td>
        </tr>
        <tr>
          <th><MyLabel>部屋タイプ <small style="color: red;">必須</small></MyLabel></th>
          <td>
            <MySelect
              v-model="sampleForm.select"
              name="sample-select"
              :roomType="roomType"
              :style="str"
            ></MySelect>
          </td>
        </tr>
        <tr>
          <th><MyLabel>性別 <small style="color: red;">必須</small></MyLabel></th>
          <td>
            <div class="box_br">
              <MyRadio
                v-model="sampleForm.radio"
                name="sample-radio"
                :sex="sex"
                :style="str"
              ></MyRadio>
            </div>
          </td>
        </tr>
        <tr>
          <th><MyLabel>食事 <small style="color: #c0c0c0;">任意</small></MyLabel></th>
          <td>
            <MyCheckbox
              v-model="sampleForm.checkbox"
              name="sample-checkbox"
              :meal="meal"
              :style="str"
            ></MyCheckbox>
          </td>
        </tr>
        <tr>
          <th><MyLabel>備考 <small style="color: #c0c0c0;">任意</small></MyLabel><br /></th>
          <td>
            <MyTextarea
              v-model="sampleForm.textarea"
              placeholder="ご要望がございましたら入力してください。"
              name="sample-textarea"
              :rows="5"
              :cols="35"
            ></MyTextarea>
          </td>
        </tr>
      </table>
        <p class="btn"><span><MyBtn @click="sendForm" :activateSubmit="activateSubmit">送信</MyBtn></span></p>
        <p class="msg">{{ msg }}</p>
    </form>
    <h2>○予約リスト</h2>
    <table class="table" border="1">
      <thead>
        <tr>
          <th>No</th>
          <th>名前</th>
          <th>性別</th>
          <th>部屋タイプ</th>
          <th>食事</th>
          <th>備考</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.sex }}</td>
          <td>{{ item.room }}</td>
          <td>
            <div v-for="meal in item.meal" :key="meal">{{ meal }}</div>
          </td>
          <td>{{ item.remarks }}</td>
        </tr>
      </tbody>
    </table>
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
      activateSubmit: true,
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
          label: "選択してください",
          value: ""
        },
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
      list: []
    };
  },
  methods: {
    sendForm() {
      if(this.sampleForm.text != "" && this.sampleForm.radio != "" && this.sampleForm.select != "") {
        axios
        .post('http://localhost:3000/reservation', {
            name: this.sampleForm.text,
            sex: this.sampleForm.radio,
            meal: this.sampleForm.checkbox,
            room: this.sampleForm.select,
            remarks: this.sampleForm.textarea
        })
      }
    }
  },
  computed: {
    msg() {
      if(this.sampleForm.text != "" && this.sampleForm.radio != "" && this.sampleForm.select != "") {
        this.activateSubmit = false;
      }else{
        this.activateSubmit = true;
        return "⚠必須項目を入力してください"
      }
    }
  },
  mounted(){
    axios
      .get('http://localhost:3000/reservation')
      .then(function(response){
            response.data.forEach(element => {
                        this.list.push({
                            id: element.id,
                            name: element.name,
                            sex: element.sex,
                            meal: element.meal,
                            room: element.room,
                            remarks: element.remarks
                        });
            });
            console.log(this.list);
        }.bind(this))
        .catch(function(error){
            console.log(error)
        })
  },
}
</script>

<style lang="scss">
  .button {
    text-align: center;
  }
  .msg {
    text-align: center;
    color: red;
    font-size: small;
  }
  table {
    border-collapse: collapse;
    border-spacing: 0;
    width: 100%;
  }
  caption,th {
    text-align: left;
  }
  /*デザインcss*/
  .box_con {
    max-width: 900px;
    margin: 0  auto;
  }
  .box_con form {
    width: 100%;
  }
  .box_con form table {
    width: 100%;
  }
  .box_con form table tr {
    position: relative;
  }
  .box_con form table tr:after {
    content: "";
    position: absolute;
    width: 100%;
    left: 0;
    bottom: 0;
    height: 1px;
    border-bottom: dotted #cdcdcd 1px;
  }
  .box_con form table tr th {
    width: 30%;
    font-weight: normal;
    padding: 1em .5em;
    box-sizing: border-box;
  }
  .box_con form table tr th span {
    background: #cd6f55;
    padding: 0 .3em;
    color: #fff;
    margin-left: .5em;
    box-sizing: border-box;
  }
  .box_con form table tr td {
    padding: 1em .5em;
    box-sizing: border-box;
  }

  .box_con form table tr .box_br {
    display: block;
  }
  .box_con form table tr select {
    border: 1px solid #97ae88;
  }
  .box_con form table tr select, .box_con form table tr textarea, .nameInput{
    width: 100%;
    height: 3em;
    padding: .5em;
    box-sizing: border-box;
  }
  .box_con form table tr textarea {
    height: 10em;
  }
  .btn {
    text-align: center;
    margin: 15px 0;
  }
  .submit-btn {
    display: inline-block;
    background: #eee;
    padding: .5em 4em;
    color: #000;
    text-decoration: none;
    cursor: pointer;
    border: none;
    transition: all 0.4s;
  }
  .submit-btn:hover {
    opacity: 0.7;
  }
</style>
