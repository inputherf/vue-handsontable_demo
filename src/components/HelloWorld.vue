<template>
<section>
  <div class="overf">
    <div id="example-container" class="wrapper">
      <HotTable :root='test' ref='textHot' :settings="hotSettings"></HotTable>
    </div>
  </div>
  <div>
    <button  @click='submit'>提交</button>
    <button disabled="disabled" >重置</button>
  </div>
</section>
</template>
<script>
import HotTable from "vue-handsontable-official";
import 'handsontable/languages/zh-CN';
import Vue from "vue";

export default {
  data: function() {
    return {
      test: "test-hot",
      hotSettings: {
        data:[], // 数据在这个里面,由数据填充表
        startRows: 10, //初始行列数
        startCols: 35,
        minRows: 1, //最小行列
        minCols: 1,
        rowHeaders: true, //行表头
        colHeaders: [
          "所属机构",
          "检测时间",
          "工号",
          "姓名",
          "手机号",
          "性别",
          "出生日期"
        ], //自定义列表头or 布尔值
        minSpareCols: 1, //列留白
        // minSpareRows: 2, //行留白
        className: "htCenter",
        currentRowClassName: "currentRow", //为选中行添加类名，可以更改样式
        currentColClassName: "currentCol", //为选中列添加类名
        autoWrapRow: true, //自动换行
        language:'zh-CN',
        contextMenu: [
           "row_above",
           "row_below",
           "col_left",
           "col_right",
           "---------",
           "remove_row",
           "remove_col",
           "---------",
           "alignment",
           'make_read_only',
           "borders",
           "copy",
           "cut"
        ],
        //右键效果
        fillHandle: true, //选中拖拽复制 possible values: true, false, "horizontal", "vertical"
        fixedColumnsLeft: 0, //固定左边列数
        fixedRowsTop: 1, //固定上边行数
        columns: [
          //添加每一列的数据类型和一些配置
          {data: 'subordindateUnit'}, // data后面跟的这个字段是上传对应的字段
          {
            data: "excelDetectTime",
            type: "time",
            timeFormat: "h:mm:ss a",
            correctFormat: true
          },
          {data: 'jobNumber'},
          {data: 'name'},
          {
            data: 'phone',
            type: "numeric"
          }, //数值
          {
            data: 'excelGender',
            type: "dropdown",
            source: ["男", "女"],
            strict: false
          },
          {
            data: 'excelBirthday',
            type: "date", //时间格式
            dateFormat: "YYYY-MM-DD",
            correctFormat: true,
            defaultDate: "1960-01-01"
          },
        ],
        afterChange: function (changes, source) {
          this.updatePlayerList = this.getSourceData()
          if(changes){
            changes.forEach(([row,prop,oldValue,newValue]) => {
              // 正则判断
              let phoneReg = /^1(3|4|5|7|8)\d{9}$/
              if(prop == 'phone'){ //phone是我需要判断的字段
                if( phoneReg.test(newValue)){
                  console.log(true);
                } else{
                  console.log(false);
                }
              }
          })
          }
        },
        updatePlayerList: null
      }
    };
  },
  name: "SampleApp",
  components: {
    HotTable
  },
  methods:{
    getSourceData: function () {
      return this.$refs.textHot.table.getSourceData()
    },
    submit: function () {

      console.log(this.getSourceData());

    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
section{
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.overf{
  width: 50%;
  height: 100px;
  /* height: 100%; */
  overflow: hidden;
}
#example-container{
  overflow: auto;
  width: 100%;
  height: 100%;
}
h3{
  width: 100%;
  background-color: burlywood;
}
button {

  margin: 20px 20px;
}
.handsontable .currentRow {
  background-color: #b6bada;
}

.handsontable .currentCol {
  background-color: #d1dfd7;
}
</style>
