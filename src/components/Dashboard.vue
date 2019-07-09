<template>
<div>
  <el-row type="flex" justify="space-around" class="row">
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>濕度</span>
        </div>
        <div>
          <span>{{humidity}}</span>
        </div>
      </el-card>
    </el-col>
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>溫度</span>
        </div>
        <div>
          <span>{{temperture}}</span>

        </div>
      </el-card>
    </el-col>
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>土壤濕度1</span>
        </div>
        <div>
          <span>{{soild1humid1}}</span>
        </div>
      </el-card>
    </el-col>
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>土壤濕度2</span>
        </div>
        <div>
          <span>{{soild1humid2}}</span>
        </div>
      </el-card>
    </el-col>
  </el-row>
    <el-row type="flex" justify="space-around" class="row">
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>Ph值</span>
        </div>
        <div>
          <span>{{ph}}</span>
        </div>
      </el-card>
    </el-col>
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>水箱1高度</span>
        </div>
        <div>
          <span>{{waterLevelTank1}}</span>
        </div>
      </el-card>
    </el-col>
    <el-col :span="5">
      <el-card>
        <div slot="header" class="clearfix">
          <span>水箱2高度</span>
        </div>
        <div>
          <span>{{waterLevelTank2}}</span>
        </div>
      </el-card>
    </el-col>
  </el-row>
</div>
</template>

<script>
import axios from "axios";
import { Row, Col, Card } from "element-ui";
export default {
  name: "dashboard",
  data: function() {
    return {
      humidity: "22",
      temperture: "33",
      soild1humid1: "12",
      soild1humid2: "43",
      ph: "8.02",
      waterLevelTank1: "2",
      waterLevelTank2: "3"
    };
  },
  created: function() {
    this.da = "loading";
    let self = this;
    self.getData();
    setInterval(() => {
      self.getData();
    }, 10000);
  },
  methods: {
    getData: function() {
      var self = this;
      axios
        .get("/api/monitor/data")
        .then(resp => {
          self.humidity = resp.data.humidity;
          self.temperture = resp.data.temperture;
          self.soild1humid1 = resp.data.soild1humid1;
          self.soild1humid2 = resp.data.soild1humid1;
          self.ph = resp.data.ph;
          self.waterLevelTank1 = resp.data.waterLevelTank1;
          self.waterLevelTank2 = resp.data.waterLevelTank2;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  components: {
    "el-row": Row,
    "el-col": Col,
    "el-card": Card
  }
};
</script>
<style scoped>
@import url("element-ui/lib/theme-chalk/index.css");
.row {
  padding: 15px;
}
</style>

