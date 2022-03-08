<template>
  <div id="main_hmk">
    <div class="b1"><button type="button" id="b01" @click="create_data($event)">手机号码</button></div>
    <div class="b1">
        <button  type="button" id="b02" @click="create_data($event)">身份证ID</button>
        <label>
          <input class="input_style" type="text" name="card_id" id="id_num" placeholder="请输入个数" v-model="num1">
        </label>
    </div>
    <div class="b1"><button type="button" id="b03" @click="create_data($event)">人名</button>
      <input class="input_style" type="text" name="name" id="name_num" placeholder="请输入个数" v-model="num2">
    </div>
    <div class="b1"><button type="button" id="b07">清空输出</button></div>

    <div><textarea class="textera" id="result" v-model="info"></textarea></div>
  </div>

</template>

<script>
  import axios from 'axios'
  import qs from 'qs'
  axios.defaults.withCredentials = true  //允许跨域携带cookie信息，必须加上
  export default {
    name: "main_page",
    data(){
      return{
        num1: null,
        num2: null,
        info: null,
      }
    },
    methods: { 
      token() {
        axios.get("http://localhost:8000/testreq/get_csrf_token").then(res => {
          let token = res.data.token
          console.log("**********"+token)
          sessionStorage.setItem("csrf_token", token) //有些博主说把生成的token放到缓存中，然后发请求时拿这个缓存放到请求头中，我试了下这个其实并没有用，可以注释掉，因为我们不拿这个token
        })
      },
      create_data(event) {
        console.log('点击元素的id='+event.target.id) //打印看下结果

        this.token()  //调用create_data()函数时，先调用token()函数，请求后台生成csrftoken
        console.log('cookie='+document.cookie)  //打印浏览器cookie
        let cookie = document.cookie  //提取cookie
        let csrf_token = cookie.split("=")[1]  //提取cookie中的csrftoken
        console.log('cookie_csrf_token='+cookie.split("=")[1])
        
        if (event.target.id === "b01") {  //通过event.target.id，获取浏览器监听到的点击事件，并查看点击元素的id，通过比对id值判断触发哪个请求
          axios({
            url: "http://localhost:8000/testreq/phone"  //如果不指定method，默认发送get请求
            }).then(res => {
              this.info = res.data
              // console.log(res)
          })
        } 
        else if (event.target.id === "b02") {
          // get请求
          // let payload = {
          //   num: this.num1
          // }
          // console.log(payload)
          // axios({
          //   method: "get",
          //   params: payload,  //发送get请求，使用params关键字接收请求参数
          //   url: "http://localhost:8000/testreq/id"
          // }).then(res => {
          //   this.info = res.data
          //   console.log(res)
          // })

          //post 请求
          let payload1 = {
            num: this.num1,
          }
          // #region
          // axios({
          //   method: "post",
          //   // xsrfHeaderName:'X-XSRF-TOKEN',
          //   headers: {
          //     'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8',
          //     'X-CSRFToken': csrf_token,
          //   },
          //   data: qs.stringify(payload1),  //发送post请求，使用data关键字接收请求参数
          //   url: "http://localhost:8000/testreq/id"
          // }).then(res => {
          //   this.info = res.data
          //   console.log(res)
          // })
          // #endregion
          axios({
            method: "post",
            // xsrfHeaderName:'X-XSRF-TOKEN',
            headers: {
              'Content-Type':'application/json',
              'X-CSRFToken': csrf_token,
            },
            data: payload1,  //也可以使用 JSON.stringify(payload1)
            url: "http://localhost:8000/testreq/id"
          }).then(res => {
            this.info = res.data
            console.log(res)
          })
        } 
        else if (event.target.id === "b03") {
          let payload = {
            num: this.num2
          }
          console.log(payload)
          axios({
            method: "get",
            params: payload,  //发送get请求，使用params关键字接收请求参数
            url: "http://localhost:8000/testreq/name"
          }).then(res => {
            this.info = res.data
            console.log(res)
          })
        }
      }
    },
  }
</script>

<style scoped>
  .b1 {
    float: left;

    margin-right: 20px;
    margin-left: 50px;
    margin-top: 20px;
  }

  .textera {
    position:absolute;
    left: 60px;
    top: 80px;
    resize: both;  /*用户可调整元素的高度和宽度*/
    height: 244px;
    width: 823px;
  }

  .input_style {
    margin-left: 8px
  }
</style>