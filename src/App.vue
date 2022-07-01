<template>
  <div class="content-box">
    <div class="header">一只小熊猫</div>
    <div class="group">
      <div class="todo-box">
        <h3>Todo</h3>
        <draggable :list="todoItems" :clone="clone" :group="{ name: 'people', pull: pullFunction }" @start="start"
          item-key="id">
          <template #item="{ element }">
            <div class="todo-item">
              {{ element.task }}
              <span @click="deleteTodoItem(element)" class="delete-icon">x</span>
            </div>
          </template>
        </draggable>
      </div>
      <div class="doing-box">
        <h3>Doing</h3>
        <draggable :list="doingItems" group="people" item-key="id">
          <template #item="{ element }">
            <div class="doing-item">
              {{ element.task }}
              <span class="delete-icon" @click="deleteDoingItem(element)">x</span>
            </div>
          </template>
        </draggable>
      </div>

      <div class="done-box">
        <h3>Done</h3>
        <draggable :list="doneItems" group="people" item-key="id">
          <template #item="{ element }">
            <div class="done-item">
              {{ element.task }}
              <span class="delete-icon" @click="deleteDoneItem(element)">x</span>
            </div>
          </template>
        </draggable>
      </div>
    </div>
    <div class="input-box">
      <input type="text" v-model="inputask" name="todo" id="input-todo" />
      <button @click="addTodoItem">确定</button>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";
import { ref } from "vue";
import draggable from "vuedraggable";
export default {
  components: {
    draggable,
  },
  setup() {
    let idcount = ref(10);

    let inputask = ref("");

    let todoItems = reactive([
      { task: "看电影", id: 1 },
      { task: "打游戏", id: 2 },
      { task: "吃饭", id: 3 },
    ]);

    let doingItems = reactive([
      { task: "看书", id: 4 },
      { task: "学vue", id: 5 },
      { task: "摸鱼", id: 6 },
    ]);
    let doneItems = reactive([
      { task: "跑步", id: 7 },
      { task: "睡觉", id: 8 },
      { task: "写代码", id: 9 },
    ]);

    let controlOnStart = true;

    function clone({ task }) {
      return { task, id: idcount.value++ };
    }
    function pullFunction() {
      return controlOnStart ? "clone" : true;
    }
    function start({ originalEvent }) {
      controlOnStart = originalEvent.ctrlKey;
    }

    //todo methods
    function deleteTodoItem(item) {
      const findItemIndex = todoItems.findIndex(
        (todoItem) => todoItem.id === item.id
      );
      todoItems.splice(findItemIndex, 1);
    }
    function addTodoItem() {
      console.log(inputask.value);
      todoItems.push({ task: inputask.value, id: idcount.value });
      idcount.value += 1;
      console.log(idcount);
      inputask.value = "";
      alert("任务添加成功了");
    }

    function deleteDoingItem(item) {
      const findItemIndex = doingItems.findIndex(
        (doingItem) => doingItem.id === item.id
      );
      doingItems.splice(findItemIndex, 1);
    }
    function deleteDoneItem(item) {
      const findItemIndex = doneItems.findIndex(
        (doneItem) => doneItem.id === item.id
      );
      doneItems.splice(findItemIndex, 1);
    }
    return {
      idcount,
      inputask,
      todoItems,
      doingItems,
      doneItems,
      doneItems,
      controlOnStart,
      clone,
      pullFunction,
      start,
      deleteTodoItem,
      addTodoItem,
      deleteDoingItem,
      deleteDoneItem,
    };
  },
};
</script>
<style scoped>
.content-box {
  height: 400px;
  width: 600px;
  background-color: rgb(141, 185, 170);
  margin: auto;
  border-radius: 15px;
}

.header {
  height: 30px;
  width: 100%;
  text-align: center;
}

.group {
  height: 320;
  width: 100%;
  /* background-color: rgb(158, 32, 15); */

  display: flex;
  justify-content: space-around;
}

.todo-box {
  height: 300px;
  width: 200px;
  display: inline-block;
  background-color: aliceblue;
  text-align: center;
}

.doing-box {
  height: 300px;
  width: 200px;
  display: inline-block;
  background-color: rgb(31, 126, 52);
  text-align: center;
}

.done-box {
  height: 300px;
  width: 200px;
  display: inline-block;
  background-color: rgb(92, 138, 74);
  text-align: center;
}

.todo-item {
  width: 90%;
  height: 30px;
  background-color: rgb(192, 68, 182);
  margin-top: 5px;
  display: inline-block;
  border-radius: 12px;
  line-height: 30px;
}

.doing-item {
  width: 90%;
  height: 30px;
  background-color: rgb(27, 156, 196);
  margin-top: 5px;
  display: inline-block;
  border-radius: 12px;
  line-height: 30px;
}

.done-item {
  width: 90%;
  height: 30px;
  background-color: rgb(119, 106, 30);
  margin-top: 5px;
  display: inline-block;
  border-radius: 12px;
  line-height: 30px;
}

.delete-icon {
  float: right;
  padding-right: 10px;
}

.delete-icon:hover {
  cursor: pointer;
}

.input-box {
  height: 30px;
  margin-top: 15px;
  text-align: center;
}

input {
  height: 20px;
  width: 30%;
  border-radius: 5px;
  outline-style: none;
  border: 1px solid rgb(11, 213, 228);
  margin-right: 3px;
}

input:focus {
  border-color: #daa700;
}

button {
  height: 25px;
  width: 45px;
  border-radius: 6px;
  border: 1px solid rgb(11, 213, 228);
}

button:hover {
  cursor: pointer;
}
</style>
