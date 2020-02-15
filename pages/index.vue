<template>
<div>

<div class="header">
  <h1>chat</h1>
</div>

<div class="split">
    <div class="split-item split-left">
        <div class="split-left__inner">
          <form>
            <ul　class="list_test-wrap">
              <li class="list_test" v-for="item in items" v-bind:key=item.id>
                <div class="says">
                  {{ item }}
                </div>
              </li>
            </ul>
          </form>
        </div><!--split-left__inner-->
    </div><!--split-item split-left-->


    <div class="split-item split-right">
        <div class="split-right__inner">
          <form @submit.prevent="submit" class="textlines" action="">
            <input type="text" id="input_message" placeholder="message">
            <input type="submit" value="送信">
          </form>
        </div><!--split-right__inner-->
    </div><!--split-item split-right-->
</div><!--split-->


</div>
</template>
 

<style>

.header {
  /* position: fixed; */
  position: fixed;
  width: 100%;
  z-index: 999;
  background: rgb(202, 197, 197);
  padding:10px 0px 5px 500px;   /*見出しの位置調整*/
  font-size:18px;       /*フォントのサイズ*/
}

.textlines {

}
.split{
    display: table;
    width: 100%;    
}
.split-item{
    display: table-cell;
    width: 80%;
    padding: 0px;
}
.split-left{
  /* overflow: scroll; */
  background: rgb(87, 85, 85);
  /* position: relative; */
}
.split-left__inner{
  padding:80px;
  overflow: scroll;
  height: 100vh
  /* height: 0px　0px 0px 0px; */
}
.split-right__inner{
  padding:650px 0px 0px 30px;
    /* height: 100%; */
    position: fixed;
    /* width: 50%; */
}


.list_test-wrap {
    list-style:  none;  /* デフォルトのアイコンを消す */
    margin:  0;         /* デフォルト指定上書き*/
    padding: 0px 0px 0px 0px;         /* デフォルト指定上書き */
}

.says {
  display: inline-block;
  position: relative; 
  margin: 9px 0 0 18px;
  padding: 17px 13px;
  border-radius: 12px;
  background: #55f19e;
}

.says:after {
  content: "";
  display: inline-block;
  position: absolute;
  top: 18px; 
  left: -24px;
  border: 12px solid transparent;
  border-right: 12px solid #55f19e;
}

.says p {
  margin: 0;
  padding: 0;
}

</style>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      items: []
    }
  },

  mounted() {
    var self = this
    axios
    .get('http://localhost:4000/chatlog')
    .then(function(response){
      var array = response.data;
      for (var key in array) {
        var text = array[key].text
      self.items.push(text)
      }
    })
  },
  methods:{
    
    submit () {
      var input_message = document.getElementById("input_message").value;
      var self = this
      const submitParams = { text: input_message };
    axios
    .post('http://localhost:4000/recv',submitParams)
    .then(console.log(input_message))
    // .then(location.href = 'http://localhost:3000/')
   }
  }
}
</script>