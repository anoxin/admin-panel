<script setup>
import SectionMain from "@/components/SectionMain.vue";
import LayoutAuthenticated from "@/layouts/LayoutAuthenticated.vue";
import SectionTitleLineWithButton from "@/components/SectionTitleLineWithButton.vue";
</script>

<template>
  <LayoutAuthenticated>
    <SectionMain>
      <SectionTitleLineWithButton :icon="mdiTableBorder" title="Задания" main>
        <button class="button" @click="showModal(true)">
          Добавить задание
        </button>
      </SectionTitleLineWithButton>
      <div class="task__table">
        <div class="task__item">
          <span class="task__text task__text_bold">№</span>
        </div>
        <div class="task__item">
          <span class="task__text task__text_bold">Название</span>
        </div>
        <div class="task__item">
          <span class="task__text task__text_bold">Описание</span>
        </div>
        <div class="task__item task__text_center">
          <span class="task__text task__text_bold">Действие</span>
        </div>
      </div>

      <div v-for="(task, index) in tasks" :key="index" class="task__table">
        <div class="task__item">
          <span class="task__text">{{ index + 1 }}</span>
        </div>
        <div class="task__item">
          <span class="task__text">{{ task.task }}</span>
        </div>
        <div class="task__item">
          <span class="task__text">{{ task.about }}</span>
        </div>
        <div class="task__item">
          <div class="table__buttons">
            <button class="button" @click="showModal(false, task, index)">
              Редактировать
            </button>
            <button class="button button_red" @click="deleteList(task, index)">
              Удалить
            </button>
          </div>
        </div>
      </div>

      <div v-show="show" class="popup">
        <div class="popup-dialog">
          <div class="popup-content">
            <button class="popup-close" @click="showModal()">&times;</button>
            <!-- Заголовок окна -->
            <h4 class="popup-header">Добавить задачу</h4>
            <div class="main-form popup-form">
              <div class="form">
                <label for="input-name">Введите название задачи</label>
                <input
                  v-model="taskText"
                  type="text"
                  name="name"
                  class="form-input"
                  placeholder="Название задачи"
                  required
                />
                <label for="input-text">Описание задачи</label>
                <textarea
                  v-model="taskAbout"
                  name="message"
                  class="form-input"
                  rows="5"
                  required
                ></textarea>
                <button
                  class="button form-btn"
                  @click="addList(taskText, taskAbout, newIndex)"
                >
                  Добавить
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </SectionMain>
  </LayoutAuthenticated>
</template>

<script>
export default {
  name: "CorrectTasks",
  data() {
    return {
      tasks: [{ task: "Задание №1", about: "Описание задания" }],
      show: false,
      newIndex: null,
    };
  },
  methods: {
    showModal(clear = false, task = null, index = false) {
      if (task) {
        this.taskText = task.task;
        this.taskAbout = task.about;
      }

      this.newIndex = index;

      if (clear) {
        this.taskText = "";
        this.taskAbout = "";
        this.newIndex = null;
      }
      this.show = !this.show;
    },
    addList(item, about, index = null) {
      console.log(index);
      if (index || index == 0) {
        this.tasks[index] = { task: item, about: about };
      } else {
        this.tasks.push({
          task: item,
          about: about,
        });
      }
      this.showModal();
    },
    deleteList(task, checkIndex) {
      this.tasks = this.tasks.filter((el, index) => {
        return index !== checkIndex;
      });
    },
  },
};
</script>

<style scoped>
.button {
  display: flex;
  align-items: center;
  padding: 15px;
  background-color: #55c97b;
  border-radius: 3px;
  color: #ffffff;
  cursor: pointer;
}
.button_red {
  background-color: red;
}

.task__table {
  display: grid;
  grid-template-columns: 1fr 3fr 4fr 3fr;
  gap: 20px;
  margin-bottom: 20px;
}

.task__text {
  color: #343434;
}

.task__text_bold {
  color: black;
  font-weight: 500;
}

.task__text_center {
  display: block;
  margin: 0 auto;
}

.table__buttons {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.popup {
  /* display: none; */
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  width: 100vw;
  height: 100vh;
  z-index: 999;
  background-color: rgba(0, 0, 0, 0.5);
  overflow: auto;
}
.popup-dialog {
  display: flex;
  justify-content: center;
  margin-top: 28px;
}
.popup-content {
  position: relative;
  height: min-content;
  background-color: #fff;
  padding: 4rem 6rem;
  border: 1px solid #fff;
}
.popup-form {
  padding: 0;
}
.popup-close {
  position: absolute;
  top: -0.8rem;
  right: 1.4rem;
  width: 2rem;
  text-align: center;
  border: none;
  background-color: transparent;
  font-size: 4rem;
  color: #343434;
}
.popup-content {
  text-align: center;
}
.popup-header {
  font-size: 30px;
  font-weight: 500;
  color: #343434;
  margin-bottom: 20px;
}

.form-input {
  background-color: #e9e9e9;
  display: block;
  width: 100%;
  border: 1px solid #000;
  border-radius: 5px;
  padding: 1.5rem;
  width: 25rem;
  margin-bottom: 20px;
}
.form label {
  padding: 0.5rem;
  margin-top: 5px;
  color: #343434;
}
.form-btn {
  width: 100%;
  justify-content: center;
}
</style>
