<template>
  <div class="componentContainer" >
    <h3>导出</h3>
    <div>
      <div>
        <input type="button" @click="exportXlsx" value="导出Excel">
         <input style='width: 173px;' type="file" id="fileDemo" class="input">
          <input type="button" @click="importXlsx" value="导入">
          <input type="button" @click="savePdf" value="导出PDF">
      </div>
    </div>
    <div class="spreadContainer" >
      <gc-spread-sheets
        :hostClass='"spreadHost"'
        @workbookInitialized='spreadInitHandle($event)'>
      </gc-spread-sheets>
    </div>
  </div>
</template>

<script>
import ExcelIO from '@grapecity/spread-excelio'
import FaverSaver from 'file-saver'
import s from './e.json'
import * as GC from '@grapecity/spread-sheets';
import  "@grapecity/spread-sheets-print";
import  "@grapecity/spread-sheets-pdf";
import  "@grapecity/spread-sheets-charts";

export default {
  name: 'ExportXlsx',
  data () {
    return {
      spread: {}
    }
  },
  methods: {
    spreadInitHandle: function (spread) {
      this.spread = spread
      this.spread.fromJSON(s)
    },
    exportXlsx () {
      let ex = new ExcelIO.IO()
      let json = this.spread.toJSON()
      ex.save(json, function (blob) {
        FaverSaver.saveAs(blob, 'export.xlsx')
      }, function (e) {
        console.log(e)
      })
    },
    importXlsx(){
       let self = this;
        var excelIO = new ExcelIO.IO();
        console.log(excelIO);
        const excelFile = document.getElementById("fileDemo").files[0];
      excelIO.open(excelFile, function (json) {
          let workbookObj = json;
          self.spread.fromJSON(workbookObj);
        }, function (e) {
            alert(e.errorMessage);
        });
    },
    savePdf(){
         let self = this;
        let jsonString = JSON.stringify(self.spread.toJSON());
        let printSpread = new GC.Spread.Sheets.Workbook();
        printSpread.fromJSON(JSON.parse(jsonString));
    
        printSpread.savePDF(function(blob) {    
                // window.open(URL.createObjectURL(blob))	
                FaverSaver.saveAs(blob,  'Hello.pdf')
                }, function(error) {
                console.log(error);
                }, {
                title: 'Print',
            });  
    }
  }
}
</script>

<style scoped>
  .componentContainer {
    position: absolute;
    padding: 10px;
    left: 242px;
    top: 0;
    bottom: 20px;
    right: 0;
  }
  .spreadContainer {
    padding: 10px;
    box-shadow: 0 0 20px grey;
  }
  .spreadContainer{
    position: absolute;
    left: 0px;
    right: 30px;
    top: 100px;
    bottom: 10px;
  }
  .spreadHost{
    width: 100%;
    height: 100%;
  }
</style>
