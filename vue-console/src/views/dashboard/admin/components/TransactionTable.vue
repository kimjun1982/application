<template>
  <el-table v-loading="loading"  :data="list" border=true style="width: 100%;padding-top: 15px;">
    <el-table-column label="Number" width="120">
      <template slot-scope="scope">
        {{ scope.row.id | orderNoFilter }}
      </template>
    </el-table-column>
    <el-table-column label="Type" width="100" align="center">
      <template slot-scope="scope">
        {{ scope.row.type  }}
      </template>
    </el-table-column>
        <el-table-column label="Name" width="100" align="center">
      <template slot-scope="scope">
        {{ scope.row.name  }}
      </template>
    </el-table-column>
            <el-table-column label="Describe" width="200" align="center">
      <template slot-scope="scope">
        {{ scope.row.describe  }}
      </template>
    </el-table-column>
            <el-table-column label="CreateTime" min-width="150" align="center" >
      <template slot-scope="scope">
        {{scope.row.createTime | dateFormat}}
      </template>
    </el-table-column>
         <el-table-column label="StartTime" min-width="150" align="center" >
      <template slot-scope="scope">
        {{scope.row.startTime | dateFormat}}
      </template>
    </el-table-column>
       <el-table-column label="EndTime" min-width="150" align="center" >
      <template slot-scope="scope">
        {{scope.row.endTime | dateFormat}}
      </template>
    </el-table-column>
         <el-table-column label="Process" width="100" align="center">
      <template slot-scope="scope">
        {{ scope.row.processNum }}
      </template>
    </el-table-column>
      <el-table-column label="Alarm" width="100" align="center">
      <template slot-scope="scope">
           <el-tag :type="scope.row.alarm |alarmTagFilter">     {{ scope.row.alarm  |alarmFilter}}</el-tag>
    
      </template>
    </el-table-column>
    <el-table-column label="Status" width="100" align="center">
      <template slot-scope="scope">
        <el-tag :type="scope.row.status | statusFilter"> {{ scope.row.status |statusNameFilter}}</el-tag>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import store from '@/store'
import {showTransactionTableData} from '@/api/api'
import { formatTime } from '@/utils';

export default {
  filters: {
    dateFormat(stamp) {
      return formatTime(stamp)
    },
    alarmFilter(stamp){
        const map={
            0: '关闭',
            1: '启用',
        }
        return map[stamp]
    },
    statusFilter(status) {
      const statusMap = {
        0: 'info',
        1: 'success',
        2: 'danger',
        3: 'warning',
        4: 'error'
      }
      return statusMap[status]
    },
    alarmTagFilter(status){
      const map={
        0:'success',
        1:'error'
      }
      return map[status]
    },
    statusNameFilter(status){
      const map={
        0:'Stop',
        1:'Running',
        2:'Exception',
        3:'Error',
        4:'Finish'
      }
      return map[status]
    },
    orderNoFilter(str) {
      return str.substring(0, 32)
    }
  },
  data() {
    return {
      list: null
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      const param={
      id:store.getters.id,
      token:store.getters.token
      }
    showTransactionTableData(param).then(res=>{
      this.list=res.data
    });

    },
     
  }
}
</script>
