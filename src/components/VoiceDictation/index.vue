<template>
  <div>
    <h1 class="h1">迅飞语音听写（流式版）WebAPI</h1>
    <hr />
    <section class="voice-box">
      <input type="search" name="voice" id="voice-txt" />
      <button id="start-btn">开始识别</button>
    </section>

    <section class="fixed-box" id="fixed-box">
      <div class="fixed-main">
        <button class="fixed-close" id="close-btn"></button>
        <div id="fixed-txt">Hello! 请说出你想说话。。。！</div>
        <div class="fixed-icon">
          <img src="./voice.png" alt />
        </div>
      </div>
    </section>
  </div>
</template>

<script>

import IatRecorder from './voice'

export default {
  name: "",
  data() {
    return {
    };
  },
  mounted() {
    const voiceTxt = document.querySelector("#voice-txt");
    const startBtn = document.querySelector("#start-btn");
    const fixedBox = document.querySelector("#fixed-box");
    const fixedTxt = document.querySelector("#fixed-txt");
    const closeBtn = document.querySelector("#close-btn");
    let fiveTell = null;

  

    // 实例化迅飞语音听写（流式版）WebAPI
    const iatRecorder = new IatRecorder({

        // 服务接口认证信息 注：apiKey 和 apiSecret 的长度都差不多，请要填错哦！
        appId: '5ec244d5',
        apiKey: '78b6c006f1f3df5e24d315e3dff09212',
        apiSecret: '37912e3e3f205e2a6201ec290452470a',
        // 注：要获取以上3个参数，请到迅飞开放平台：https://www.xfyun.cn/services/voicedictation

        onWillStatusChange: function (oldStatus, newStatus) {
            //可以在这里进行页面中一些交互逻辑处理：注：倒计时（语音听写只有60s）,录音的动画，按钮交互等！
            fixedBox.style.display = 'block';
        },
        onTextChange: function (text) {
            //监听识别结果的变化
            voiceTxt.value = text;
            fixedTxt.innerText = text;

            // 3秒钟内没有说话，就自动关闭
            if (text) {
                clearTimeout(fiveTell);
                fiveTell = setTimeout(function () {
                    iatRecorder.stop();
                    fixedBox.style.display = 'none';
                }, 3000);
            };
        }
    });

    // 开始识别
    startBtn['onclick'] = function () {
        iatRecorder.start();
    };

    // 关闭识别
    closeBtn['onclick'] = function () {
        iatRecorder.stop();
        fixedBox.style.display = 'none';
    };
  },

  methods: {
    name() {},
  },
};
</script>

<style scoped>
.h1 {
  text-align: center;
}

.voice-box {
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  margin: 50px auto;
  padding: 50px;
  width: 60%;
  border: 1px solid gray;
  border-radius: 3px;
}

.voice-box input {
  box-sizing: border-box;
  padding: 10px;
  width: 70%;
  height: 50px;
  font-size: 18px;
  color: #187cff;
  border: 1px solid #187cff;
  border-radius: 3px 0px 0px 3px;
}

.voice-box button {
  width: 30%;
  height: 50px;
  line-height: 50px;
  font-size: 18px;
  color: white;
  background: #187cff;
  border: none;
  border-radius: 0px 3px 3px 0px;
  cursor: pointer;
}

.voice-box button::before {
  content: "";
  display: inline-block;
  width: 23px;
  height: 26px;
  vertical-align: middle;
  background: url(./voice.png) no-repeat;
  background-size: contain;
}

.fixed-box {
  box-sizing: border-box;
  display: none;
  position: fixed;
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
  border: 0.04rem solid #e0e7ff;
  background: rgba(16, 22, 62, 0.2);
}

.fixed-close {
  position: absolute;
  top: 0px;
  right: 0px;
  padding: 24px;
  width: 20px;
  height: 20px;
  border: none;
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAeCAYAAAA7MK6iAAABeklEQVRIibXWsWoVQRQA0LObgGCTFFr6env9h/yETSz0tclrBEUJWhjzAqkV/A9/QrC2CtgIYqk2DwvdMExm991ZJrdaZoY53Bnm3u0Wy80BPuI21jjRPjqc4il+4FG3WG4ucS9ZtMaqMfoej5OxLz3uZAuPcXaDKNzt/TuCPFrgYyi87fEK543xKfQFLvoEaYVvQ19DnwxO4euWaA5P4UcBPIzCbmHRcYLleDqfox9wGEG5nnGKRzOvRilnnOIDluPD/BT6HG/GNp+CI/j+HDQCR/BqlPE7LuGlO5+F1sARPIzWwh32Jub3K/YKw1PFYYgV3rWEI2iKh2r7NnhbGZzdWKaeU03tzZ/W8ARH/2TGMq5BZ7XUElzVZebiOTzU3ho0RUqts4in8Kwuk8Uqig/wti4TQSP41XgfQMNlMIBf9fPd/x8t0RTn+h/LEX52i+XmN241RtM4K+Dfe3y7QZTysV/u7D18+RkP8AfPxPpubXzCL9zHVzz5C8ecbD1n3xuUAAAAAElFTkSuQmCC)
    no-repeat center center;
  background-size: 20px;
  transition: all 0.5s;
  cursor: pointer;
  outline: none;
}

.fixed-close:hover {
  transform: rotate(270deg);
  -webkit-transform: rotate(270deg);
}

.fixed-main {
  box-sizing: border-box;
  position: absolute;
  left: 20%;
  bottom: 0px;
  padding: 30px;
  width: 60%;
  height: 240px;
  font-size: 18px;
  background: white;
  border: 2px solid #e0e7ff;
  border-radius: 5px 5px 0px 0px;
  overflow-y: auto;
}

.fixed-icon {
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -50px;
  margin-left: -50px;
  width: 100px;
  height: 100px;
  background: linear-gradient(115deg, #56d8e4 5%, #9f01ea 95%);
  border: 1px solid #e0e7ff;
  border-radius: 50%;
}

.fixed-icon::before {
  content: "";
  position: absolute;
  display: inline-block;
  width: 100px;
  height: 100px;
  border: 1px solid #9f01ea;
  box-shadow: 0px 0px 6px 0px #9f01ea;
  border-radius: 50%;
  animation: move 1.5s infinite;
}

.fixed-icon::after {
  content: "";
  position: absolute;
  display: inline-block;
  width: 100px;
  height: 100px;
  border: 1px solid #56d8e4;
  box-shadow: 0px 0px 6px 0px #56d8e4;
  border-radius: 50%;
  animation: move 1.5s 0.5s infinite;
}

.fixed-icon img {
  width: 30px;
  height: 50px;
}

@keyframes move {
  0% {
    opacity: 1;
    transform: scale(1);
  }

  100% {
    opacity: 0;
    transform: scale(2);
  }
}

::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  background-color: #f9f9f9;
}

::-webkit-scrollbar-thumb {
  border-radius: 6px;
  background-color: #187cff;
}
</style>