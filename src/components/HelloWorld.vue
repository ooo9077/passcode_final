<template>
  <div>
    <div style="text-align: left;"> 
        <span style="position:relative;display: inline-block; left:5%; width:65%;">
        <p style="color:white;font-size:150%">姓名：{{full_name}}</p>
        <p style="color:white;font-size:150%">部门：{{org_name}}</p>
    </span> 
    <img src="../assets/fuda-logo-blue.png" style="width:30%;height:30%;padding:0px;position:relative;top:20px;"/>
    </div>
    
    <div class="qrcanvas">
        <br>
        <img id="barcode" style="width:95%"/>
        <br>
        <h3 style="color:green;">{{currentdate}}</h3>
        <h3 style="color:green;font-size:300%;">{{currentsec}}</h3>
        <h3 style="color:green;">{{currenthour}}</h3><br>
        <div style="display: inline-block;margin: 0 10px;text-align: center;"  id="qrcode" ref="qrcode"></div>
        <p style="text-align: center; color:white;">{{currentTime}}</p>
        <marquee><span style="font-weight: bolder;font-size: 40px;color: green;">{{large_word}}</span></marquee>
        <p style="position:relative;width:95%;color:green;text-align:left;left:5%;font-size:100%;">未见异常，允许通行，请主动出示，配合检测，并做好自我防护，出行前请确认。</p>
        <div style="position: relative; left:-25%">
            <img src="../assets/yikatong-logo.jpg" style="width:5%;height:5%;padding:0px;"/>
            <p style="display: inline-block;text-align:left">一卡通<br>适用于所有场景</p>
        </div>
    </div>
    <br>
    <div style="position: relative; left:25%;border: 1px solid #FFF; border-radius: 5px; width:40%;color:white;font-size:120%" v-on:click="inputBoxHideShow()">每日健康填报</div>
    <br>
    <input v-model="full_name" placeholder="姓名" v-if="full_name_show">
    <input v-model="org_name" placeholder="部门" v-if="org_name_show">
    <input v-model="large_word" placeholder="滚动文字" v-if="large_word_show">
  </div>
</template>

<script>
import QRCode from 'qrcodejs2';
import JsBarcode from 'jsbarcode';
export default {
  
  name: 'HelloWorld',
  components:{
    QRCode
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      timer: "",
      currentTime: new Date(), // 当前时间 
      downloadButton:false,
      configs:{
          value:"helloworld!",
          imagepath:"passqrcode/passqrcode/static/123455.png",
          filter:"color"
      },
      full_name:'',
      org_name:'',
      large_word:'',
      full_name_show:false,
      org_name_show:false,
      large_word_show:false,
    }
  },

  created() {
            var vm = this;
            vm.timer = setInterval(() => {
                    var y = new Date().getFullYear();
                    var m = vm.appendZero(new Date().getMonth() + 1);
                    var d = vm.appendZero(new Date().getDate());
                    var ho = vm.appendZero(new Date().getHours());
                    var mi = vm.appendZero(new Date().getMinutes());
                    var sc = vm.appendZero(new Date().getSeconds());
                //修改数据date
                vm.currentTime = y + "/" + m + '/' + d + ' ' + ho + ':' + mi + ':' + sc;
                vm.currentdate=m+'月'+d+'日';
                vm.currenthour=ho+':'+mi;
                vm.currentsec=sc;
            }, 1000);
            
        },
    methods: {
            getRandomRagne(max,min){
                return Math.random() * (max-min) + min + '';
            },
            randomString(len) {
            　　len = len || 32;
            　　var $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';    /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
            　　var maxPos = $chars.length;
            　　var pwd = '';
            　　for (var i = 0; i < len; i++) {
            　　　　pwd += $chars.charAt(Math.floor(Math.random() * maxPos));
            　　}
            　　return pwd;
            },
                        //时间过滤加0
            appendZero(obj) {
                if (obj < 10) {
                    return "0" + obj;
                } else {
                    return obj;
                }
            },
            qrcode(){
                var qrtext = this.getRandomRagne(90000,40000);
                let qrcode = new QRCode("qrcode",{
                    text:qrtext
                });
            },
            barcode(){
                var bartext = this.randomString(9);
                let barcode=new JsBarcode('#barcode', bartext,{
                    format:'CODE39',
                    lineColor:'#000',
                    width:3,
                    displayValue:false,
                }); 
            },
            inputBoxHideShow(){
              this.full_name_show=!(this.full_name_show);
              this.org_name_show=!(this.org_name_show);
              this.large_word_show=!(this.large_word_show);
            }    
        },
    mounted() {
    this.qrcode();
    this.barcode();
    },
    beforeDestroy() {
                if (this.timer) {
                    clearInterval(this.timer); // 在Vue实例销毁前，清除我们的定时器
                }
            }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
h3{
    display: inline-block;
}
a {
  color: #42b983;
}
.back-color{
    background: #0984BD;
}

.qrcanvas{
    background: white;
    position: relative;
    left: 5%;
    width: 90%;
    border-top-right-radius:10px;
    border-top-left-radius:10px;
    border-bottom-left-radius:10px;
    border-bottom-right-radius:10px;
}
</style>
