<template>
  <el-row type="flex" justify="center">
    <el-col :span="12">
      <div class="block">
        <el-card :body-style="{ padding: '0px' } " class="webcam">
            <img v-bind:src="streamUrl" class="image">
            <div style="padding: 14px;">
                <span>實時監控</span>
                <div class="bottom clearfix">
                    <time class="time">{{ currentDate }}</time>
                </div>
            </div>
        </el-card>
      </div>
      <div>
        <el-card  class="block">
          <div slot="header">
            <span>機械臂控制</span>
          </div>
          <div>
            <span>x 軸</span>
            <el-slider v-model="xAxisStep" show-input :max=5000></el-slider>
          </div>
          <div>
            <span>y 軸</span>
            <el-slider v-model="yAxisStep" show-input :max=5000></el-slider>
          </div>
          <div>
            <span>z 軸</span>
            <el-slider v-model="zAxisStep" show-input :max=5000></el-slider>
          </div>
          <el-button class ="button" type="primary" v-on:click="goForward">前進</el-button>
          <el-button class ="button" type="primary" v-on:click="goBackward">後退</el-button>
        </el-card>
      </div>
      <div>
        <el-card  class="block">
          <div slot="header" class="clearfix">
            <span>水與肥料</span>
          </div>
          <div>
            <span>澆水</span>
            <el-slider v-model="waterpump" :step=1 show-stops :max=10></el-slider>
          </div>
          <el-button class ="button" type="primary" v-on:click="controlWaterPump">澆水</el-button>
          <div>
            <span>施肥</span>
            <el-slider v-model="fertilizerpump" :step=1 show-stops :max=10></el-slider>
          </div>
          <el-button class ="button" type="primary" v-on:click="controlFertilizerPump">施肥</el-button>
        </el-card>
      </div>
      <div>
        <el-card  class="block">
          <div slot="header" class="clearfix">
            <span>LED光色</span>
          </div>
          <el-color-picker v-model="LEDColor"  :predefine="predefineColors"></el-color-picker>
          <el-button class ="button" type="primary" v-on:click="ChangeLEDState">執行</el-button>
        </el-card>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import axios from "axios";
import { Row, Col, Card, Slider, Button, ColorPicker } from "element-ui";
export default {
  name: "dashboard",
  data: function() {
    return {
      streamUrl: "",
      xAxisStep: 0,
      yAxisStep: 0,
      zAxisStep: 0,
      LEDColor: '#409EFF',
      predefineColors: [
          '#ff4500',
          '#ff8c00',
          '#ffd700',
          '#90ee90',
          '#00ced1',
          '#1e90ff',
          '#c71585',
      ],
      waterpump: 0,
      fertilizerpump: 0,
      currentDate: ""
    };
  },
  created: function() {

  },
  methods: {
    axiosGetRequest: function(requestJob, config){
      let self = this;
      axios
        .post(`/api/control/${requestJob}`, config)
        .then(resp => {
          console.log(`job added:${requestUrl}`);
        })
        .catch(error => {
          console.log(error);
        });
    },
    goForward: function(){
      let self = this;
      self.axiosGetRequest('goPoint', {
        x: self.xAxisStep,
        y: self.yAxisStep,
        z: self.zAxisStep
      });
    },
    goBackward: function(){
      let self = this;
      self.axiosGetRequest('backPoint', {
        x: self.xAxisStep,
        y: self.yAxisStep,
        z: self.zAxisStep
      });
    },
    controlWaterPump: function(){
      let self = this;
      let waterSecond = 1000* self.waterpump;
      self.axiosGetRequest('controlWaterPump', {
        state: true,
        time: waterSecond
      });
    },
    controlFertilizerPump: function(){
      let self = this;
      let fertilizerSecond = 1000* self.fertilizerpump;
      self.axiosGetRequest('controlFertilizerPump', {
        state: true,
        time: fertilizerSecond
      });
    },
    ChangeLEDState: function(){
      let self = this;
      let LEDColorFormat = self.LEDColor.slice(1);
      self.axiosGetRequest('changeLedState', { color: LEDColorFormat });
    }
  },
  components: {
    "el-row": Row,
    "el-col": Col,
    "el-card": Card,
    "el-slider": Slider,
    "el-button": Button,
    "el-color-picker": ColorPicker
  }
};
</script>

<style scoped>
@import url("element-ui/lib/theme-chalk/index.css");
.webcam {
  margin: 0 auto; /* Added */
  float: none; /* Added */
  margin-bottom: 10px; /* Added */
  width: 600px;
}
.switch {
    padding: 10px;
}

.block {
  margin: 0 auto; /* Added */
  float: none; /* Added */
  margin-bottom: 10px; /* Added */
}
</style>

