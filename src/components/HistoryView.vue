<template>
  <div>
    <h1>{{ msg }}</h1>
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="時間"
        width="180">
      </el-table-column>
      <el-table-column
        prop="airhumid"
        label="空氣濕度">
      </el-table-column>
      <el-table-column
        prop="airtemp"
        label="溫度">
      </el-table-column>
      <el-table-column
        prop="ph"
        label="ph">
      </el-table-column>
      <el-table-column
        prop="soild1humid1"
        label="土壤濕度1">
      </el-table-column>
      <el-table-column
        prop="soild1humid2"
        label="土壤濕度2">
      </el-table-column>
      <el-table-column
        prop="waterLevelTank1"
        label="水箱高度">
      </el-table-column>
      <el-table-column
        prop="waterLevelTank2"
        label="液肥高度">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import firebase from "firebase";
export default {
  name: 'historyView',
  data () {
    return {
      msg: 'HistoryView',
      tableData:[]
    }
  },
  created: function() {
    this.da = "loading";
    let self = this;
    self.getData();
    setInterval(() => {
      self.getData();
    }, 100000);
  },
  methods: {
    getData: function(){
      let self = this;
      let dataRef = firebase.database().ref('/monitorData/');
      let retArr = [];
      dataRef.once('value').then(function(snapshot){
        let val = snapshot.val();
        let keys = Object.keys(val);
        for(let i = 0 ; i < keys.length ; i++){
          let item = {
            date:'',
            airhumid:'',
            airtemp:'',
            ph:'',
            soild1humid1:'',
            soild1humid2:'',
            waterLevelTank1:'',
            waterLevelTank2:''
          }
          let k = keys[i];
          item.date = k;
          item.airhumid = val[k].airhumid;
          item.airtemp = val[k].airtemp;
          item.ph = val[k].ph;
          item.soild1humid1 = val[k].soild1humid1;
          item.soild1humid2 = val[k].soild1humid2;
          item.waterLevelTank1 = val[k].waterLevelTank1;
          item.waterLevelTank2 = val[k].waterLevelTank2;
          retArr.push(item);
        }
      });
      self.tableData = retArr;
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
a {
  color: #42b983;
}
</style>