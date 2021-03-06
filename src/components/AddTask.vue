<template>
  <form @submit.prevent="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input type="text" v-model="text" name="text" placeholder="Add Task" />
    </div>
    <div class="form-control">
      <label>Day and Time</label>
      <DatePicker v-model="date" mode="dateTime" :minute-increment="5" is24hr>
        <template v-slot="{ inputValue, inputEvents }">
          <input
            class="px-2 py-1 border rounded focus:outline-none focus:border-blue-300"
            :value="inputValue"
            v-on="inputEvents"
          />
        </template>
      </DatePicker>
    </div>
    <div class="form-control form-control-select">
      <label>Choose a Category</label>
      <select v-model="category">
        <option value disabled>please select</option>
        <option :key="category.id" v-for="category in categories">{{ category.name }}</option>
      </select>
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div>
    <input type="submit" value="Save Task" class="btn btn-warning" />
  </form>
</template>

<script lang = "ts">
import moment from "moment";
import { defineComponent, ref, reactive, onMounted } from "vue";
import { Category } from "./beans.vue";
export default defineComponent({
  name: "AddTask",
  setup(props, context) {
    let text = ref("");
    let date = ref(new Date());
    let reminder = ref(false);
    let categories = reactive<Category[]>([]);
    let category = ref("");

    const onSubmit = () => {
      if (!text.value) {
        alert("Please add a task");
        return;
      }
      const newTask = {
        id: Date.now(),
        text: text.value,
        date: moment(String(date.value)).format("YYYY-MM-DD HH:mm a"),
        reminder: reminder.value,
        category: category.value,
      };

      context.emit("add-task", newTask);
      // clear the things
      text.value = "";
      date.value = new Date();
      reminder.value = false;
    };

    const fetchCategories = async () => {
      const res = await fetch(process.env.VUE_APP_API_URL + "/categories");
      const data = await res.json();
      categories.push(...data);
    };

    onMounted(() => {
      fetchCategories();
    });

    return {
      text,
      date,
      reminder,
      categories,
      category,
      fetchCategories,
      onSubmit,
    };
  },
});
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
  border: solid 1px #ccc;
  border-radius: 5px;
}
.form-control-select {
  display: flex;
  justify-content: space-between;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
input[type="submit"] {
  font-weight: bold;
}
</style>
