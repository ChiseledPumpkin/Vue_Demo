<template>
  <div id="app">
    <div class="logButton" @click="showCoverLog">点击显示遮罩层</div>
    <!-- 遮罩层 -->
    <div class="coverLog" v-if="isShowCoverLog" @click="hideCoverLog">
        <!-- 开发日志 -->
        <div class="devlog">
            <div class="logContainer">
                <div class="logTitle">标题</div>
                <!-- <div class="logContent">{{logDetail.detail}}</div> -->
                <div class="logContent">
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                  测试文本<br />
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
// 从外部引入文本js文件，将文本包含在对象中，对外暴露
// 这里直接用标签内文本代替
// import logDetail from '../assets/DevelopmentLogs'
export default {
    name: 'App',
    data () {
        return {
          // 是否显示遮罩层
          isShowCoverLog: false,
          // 接收文本 
          // logDetail: logDetail.detail,
        }
    },
    methods: {
        // 点击显示开发日志
        showCoverLog(){
            this.isShowCoverLog = true;
            //停止页面滚动
            let m = function(e){e.preventDefault();};
            document.body.style.overflow='hidden'; // 隐藏滚动条
            window.addEventListener("touchmove",m,{ passive:false });
        },
        // 点击隐藏开发日志
        hideCoverLog(){
            this.isShowCoverLog = false;
            //开启页面滚动
            let m =function(e){e.preventDefault();};
            document.body.style.overflow=''; // 出现滚动条
            window.removeEventListener("touchmove",m,{ passive:true });
        },
    },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: lightblue;
  box-sizing: border-box;
}

/* 显示开发日志按钮 */
.logButton {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 100px;
  border-radius: 5px;
  background-color: cyan;
  text-align: center;
  line-height: 100px;
  cursor: pointer;
}

/* 遮罩层 */
.coverLog {
    position: fixed;
    top: 0%;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.6);
    z-index: 999;
}
/* 开发日志 */
.devlog {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 600px;
    height: 700px;
    padding: 20px;
    border-radius: 30px;
    border-top: 1px solid rgba(255, 255, 255, 0.4);
    box-shadow: 0px 15px 15px rgba(0, 0, 0, 0.6);
    background-color: rgba(255, 255, 255, 0.2);
    backdrop-filter: blur(2px);
}

.devlog .logContainer {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 550px;
    height: 650px;
    border-radius: 20px;
    background-color: rgb(204, 205, 177);
    border: 1px solid rgba(255, 255, 255, 0.6);
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.9) inset;
    color: #646b8a;
    text-align: center;
}

.devlog .logContainer .logTitle {
    margin-top: 15px;
    font-size: 24px;
}

.devlog .logContainer .logContent {
    margin-top: 15px;
    margin-left: 20px;
    margin-right: 20px;
    height: 90%;
    overflow: auto;
    white-space: pre;
    /* text-align: left; */
    font-size: 14px;
    /* background-color: #fff; */
}
</style>