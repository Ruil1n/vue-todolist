<template>
  <div id="todolist">
        <el-container>
          <el-header><h1 v-text="msg"></h1></el-header>
        </el-container>
    <el-main>
        <el-row :gutter="20"> 
            <el-col :span="12" :offset="6">
                <div class="grid-content bg-purple">
                    <el-input v-model="newItems" @keyup.enter.native="addNew"></el-input>
                </div>
            </el-col>
        </el-row>
<el-row :gutter="20"> 
    <el-col :span="12" :offset="6"><div class="grid-content bg-purple">
    <el-table :data="items">
        <el-table-column type="index">
    </el-table-column>
            <el-table-column prop="task" label="任务" type="normal">
            </el-table-column>
             <el-table-column prop="status" label="状态" 
             :filters="[{ text: 'Todo', value: false }, { text: 'Finish', value: true }]" 
             :filter-method="filterTag" filter-placement="bottom-end">
                 <template slot-scope="scope">
                 <el-tag :type="iFinish(scope.row)">{{changeWord(scope.row)}}</el-tag>
                 </template>
            </el-table-column>
             <el-table-column prop="do" label="操作">
                 <template slot-scope="scope">
            <el-button size="mini" icon="el-icon-success" :type="iFinish(scope.row)"
                       @click="toggleFinish(scope.row)"></el-button>
            <el-button size="mini" icon="el-icon-delete"
                       @click="delTask(scope.$index)"></el-button>
        </template>
            </el-table-column>
    </el-table>
    </div></el-col>
</el-row>
    </el-main>
  </div>
</template>

<script>
import Storage from "./storage";

export default {
  name: "TodoList",
  data() {
    return {
      msg: "TodoList",
      items: Storage.fetch(),
      newItems: ""
    };
  },
  watch: {
    items: {
      handler(items) {
        Storage.save(items);
      },
      deep: true //深度监视，只要 items 有一点改变就会触发回调函数handler
    }
  },
  methods: {
    filterTag(value, row) {
      return row.status === value;
    },
    iFinish(item) {
      if (item.status == true) {
        return "success";
      }
      return "";
    },
    changeWord(item) {
      if (item.status == true) {
        return "Finish";
      }
      return "Todo";
    },
    toggleFinish(item) {
      item.status = !item.status;
    },
    addNew() {
      this.items.push({
        task: this.newItems,
        status: false
      });
      this.newItems = "";
    },
    delTask(index) {
      this.items.splice(index, 1);
    }
  }
};
</script>

<style scopeds>
.el-header{
  text-align: center;
}
</style>