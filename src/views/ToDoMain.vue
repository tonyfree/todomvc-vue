<template>
  <el-table
    v-show="todos.length>0"
    :data="showTodos">
    <el-table-column width="80" align="left">
      <template slot-scope="scope">
        <el-checkbox :value="scope.row.completed" @change="toggle({todo:scope.row})"></el-checkbox>
      </template>
    </el-table-column>
    <el-table-column align="left">
      <template slot-scope="scope">
        <el-input
          v-show="scope.row.editing"
          v-model="scope.row.name"
          size="small"
          @blur="doneEdit({todo:scope.row})"
          @keyup.enter.native="doneEdit({todo:scope.row})">
        </el-input>
        <span
          v-show="!scope.row.editing"
          :class="{completed:scope.row.completed}">
          {{scope.row.name}}
        </span>
      </template>
    </el-table-column>
    <el-table-column width="100">
      <template slot-scope="scope">
        <el-button @click="edit(scope.$index)" type="text" size="small">编辑</el-button>
        <el-button @click="remove({id:scope.row.id})" type="text" size="small">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import { mapState, mapMutations, mapActions } from "vuex";
export default {
  computed: {
    ...mapState(["todos"]),
    showTodos() {
      let name = this.$route.name;
      if (name === "Completed") {
        return this.todos.filter(todo => todo.completed);
      }
      if (name === "Active") {
        return this.todos.filter(todo => !todo.completed);
      }
      return this.todos;
    }
  },
  methods: {
    ...mapMutations(["edit"]),
    ...mapActions(["remove", "toggle", "doneEdit"])
  }
};
</script>
