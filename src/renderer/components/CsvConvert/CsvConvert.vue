<template>
  <div>
      <div class="toolbar">
        <button @click="selectFile">选择CSV文件</button>
        <span>{{curFilePath}}</span>
      </div>
      <div>
        <table>
          <tbody>
            <tr v-for="(row, rowIndex) in csvData" :key="rowIndex">
              <td v-for="(value, valIndex) in row" :key="valIndex">{{value}}</td>
            </tr>
          </tbody>
        </table>
      </div>
  </div>
</template>

<script>
var csv = require('fast-csv')

export default {
  data () {
    return {
      name: 'csv',
      curFilePath: '',
      csvData: []
    }
  },
  methods: {
    selectFile () {
      this.$electron.remote.dialog.showOpenDialog({
        title: '选择CSV文件',
        properties: ['openFile'],
        filters: [
          {name: 'CSV', extensions: ['csv']}
        ]
      }, (filenames) => {
        this.curFilePath = filenames[0]
        csv.fromPath(filenames[0])
          .on('data', (data) => {
            console.log(data)
            this.csvData.push(data)
          })
          .on('end', () => {
            console.log('done')
          })
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.toolbar {
  margin: 10px 0;

  span {
    color: $brand-primary;
    margin-left: 10px;
  }
}

table {
  border-collapse: collapse;

  tr:first-child {
    background-color: #7d7a7a;
    color: #fff;
  }

  td {
    border: 1px solid #999;
    padding: 5px 10px;
  }
}

</style>
