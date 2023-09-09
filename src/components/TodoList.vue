<template>
  <div>
    <h3>todo list</h3>
    <ul>
      <li
        v-for="(item, index) in todos.list"
        style="display: flex; background-color: gray; color: #fff"
      >
        <span style="flex: 1; text-align: left">{{ item.text }} </span>
        <span style="width: 100px"> [{{ item.state }}] </span>
        <span style="width: 100px">
          <n-button type="error" @click="removeTodo(index)">remove</n-button>
        </span>
      </li>
    </ul>
    <button @click="openAddForm()">add</button>
    <div v-show="todos.adding">
      <div>
        Name:
        <n-input
          type="text"
          name="text"
          ref="textInputRef"
          v-model:value="todos.addingForm.text"
        ></n-input>
      </div>
      <div>
        State:
        <n-input type="text" v-model:value="todos.addingForm.state"></n-input>
      </div>
      <div>
        <n-button type="submit" @click="addTodo">提交</n-button>
        <n-button type="reset">重置</n-button>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref, nextTick, defineComponent } from 'vue';
import { NButton, NInput } from 'naive-ui';

export default defineComponent({
  componnets: { NButton, NInput },
  setup() {
    const textInputRef = ref();
    const todos = reactive({
      list: [{ text: '第一条数据', state: 'todo' }],
      adding: false,
      addingForm: {
        text: '',
        state: '',
      },
    });

    function openAddForm() {
      todos.adding = true;
      console.log('===nextTick before', textInputRef.value);
      nextTick(() => {
        console.log('===nextTick after', textInputRef.value);

        textInputRef.value.focus();
      });
    }

    function addTodo() {
      const item = {
        ...todos.addingForm,
      };
      todos.list.push(item);
      todos.addingForm = {
        text: '',
        state: '',
      };
      todos.adding = false;
    }

    function removeTodo(index) {
      todos.list.splice(index, 1);
    }

    return {
      textInputRef,
      todos,
      addTodo,
      removeTodo,
      openAddForm,
    };
  },
});
</script>
