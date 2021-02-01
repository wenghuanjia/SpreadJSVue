<template>
  
  <div class="componentContainer" >
    <h3>打印</h3>
    <div>
      <div>
        <input type="button" @click="print" value="打印">
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
import '@grapecity/spread-sheets/styles/gc.spread.sheets.excel2016colorful.css'
// import GC,{ Spread } from '@grapecity/spread-sheets'
import GC from '@grapecity/spread-sheets'
import '@grapecity/spread-sheets-vue'
import data from './demo.json'
export default {
   name: 'Demo',
  data() {
    return {
      spread: {},
      tbData0: [
        {
          'a': '5',
          'b': '93422.12',
          'c': '12144.88'
        },
        {
          'a': '4',
          'b': '93422.12',
          'c': '12144.88'
        },
        {
          'a': '3',
          'b': '14134.00',
          'c': '0.s00'
        },
        {
          'a': '2',
          'b': '0.00',
          'c': '340.00'
        },
        {
          'a': '1',
          'b': '134.00',
          'c': '35200'
        }
      ]
    }
  },
  mounted () {

  },
  methods: {
     spreadInitHandle: function (spread) {
       this.spread = spread
      this.spread.fromJSON(data)
    this.setTableData()
    },
    setTableData() {
      let sheet2 = this.spread.getSheetFromName('附表二')
      let dataSource2 = {
        'part1': this.tbData0
      }

      let tables = sheet2.tables.all()

      var colname1 = new GC.Spread.Sheets.Tables.TableColumn()
      colname1.name('项目')
      var colname2 = new GC.Spread.Sheets.Tables.TableColumn()
      colname2.name('栏次')
      var tableColumn1 = new GC.Spread.Sheets.Tables.TableColumn()
      tableColumn1.dataField('a')
      tableColumn1.name('份数')
      var tableColumn2 = new GC.Spread.Sheets.Tables.TableColumn()
      tableColumn2.dataField('b')
      tableColumn2.name('金额')
      var tableColumn3 = new GC.Spread.Sheets.Tables.TableColumn()
      tableColumn3.dataField('c')
      tableColumn3.name('税额')

      console.log('tables[0]:', tables[0])
      tables[0].bindColumns([colname1, colname2, tableColumn1, tableColumn2, tableColumn3])

      tables[0].bindingPath('part1')
      tables[0].autoGenerateColumns(false)

      sheet2.autoGenerateColumns = false
      dataSource2 = new GC.Spread.Sheets.Bindings.CellBindingSource(dataSource2)
      sheet2.setDataSource(dataSource2)
      // sheet2.setFormula(2,2,'=SUM(C4,C5)')
      sheet2.options.sheetAreaOffset = {left: 2, top: 2}

      this.spread.resumePaint()
    }
  }
}
</script>
<style>
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
