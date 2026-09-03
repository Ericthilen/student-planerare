<script>
export default {
  data() {
    return {
      studentName: '',
      taskName: '',
      course: '',
      deadline: '',
      filter: 'Alla',
      sortOrder: 'Tidigast',
      priority: 'Medel',
      tasks: []
    }
  },

  computed: {
    filteredTasks() {
      let result = this.tasks

      if (this.filter === 'Klara') {
        result = result.filter(task => task.completed)
      }

      if (this.filter === 'Ej klara') {
        result = result.filter(task => !task.completed)
      }

      return [...result].sort((a, b) => {
        if (this.sortOrder === 'Tidigast') {
          return new Date(a.deadline) - new Date(b.deadline)
        }

        return new Date(b.deadline) - new Date(a.deadline)
      })
    }
  },

  methods: {
    addTask() {
      if (
        this.taskName === '' ||
        this.course === '' ||
        this.deadline === ''
      ) {
        return
      }
      const newTask = {
        id: Date.now(),
        name: this.taskName,
        course: this.course,
        deadline: this.deadline,
        priority: this.priority,
        completed: false
      }

      this.tasks.push(newTask)
      this.taskName = ''
      this.course = ''
      this.deadline = ''
      this.priority = 'Medel'
    },

    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    },

    toggleTask(id) {
      const task = this.tasks.find(task => task.id === id)

      if (task) {
        task.completed = !task.completed
      }
    }
  }
}
</script>

<template>
  <div class="app-container">
    <header class="header">
      <h1>Studentplaneraren</h1>
      <p>Håll koll på dina skoluppgifter och deadlines.</p>
    </header>

    <main>
      <section class="student-section">
        <label for="studentName">Studentens namn:</label>

        <input id="studentName" v-model="studentName" type="text" placeholder="Ange studentens namn" />

        <p v-if="studentName" class="welcome-text">
          Hej {{ studentName }}! Här kan du planera dina skoluppgifter.
        </p>
      </section>

      <section class="form-section">
        <h2>Lägg till skoluppgift</h2>

        <form @submit.prevent="addTask">
          <div class="form-grid">
            <div class="form-group">
              <label for="taskName">Uppgiftens namn:</label>
              <input id="taskName" v-model="taskName" type="text" placeholder="Ange uppgiftens namn" />
            </div>

            <div class="form-group">
              <label for="course">Kurs:</label>
              <input id="course" v-model="course" type="text" placeholder="Ange kurs" />
            </div>

            <div class="form-group">
              <label for="deadline">Deadline:</label>
              <input id="deadline" v-model="deadline" type="date" />
            </div>

            <div class="form-group">
              <label for="priority">Prioritet:</label>
              <select id="priority" v-model="priority">
                <option value="Hög">Hög</option>
                <option value="Medel">Medel</option>
                <option value="Låg">Låg</option>
              </select>
            </div>
          </div>

          <button type="submit" class="add-button">
            Lägg till uppgiften
          </button>
        </form>
      </section>

      <section class="tasks-section">
        <div class="tasks-heading">
          <h2>Mina uppgifter</h2>
          <span>{{ tasks.length }} uppgifter</span>
        </div>

        <div class="filter-buttons">
          <button type="button" class="filter-button" :class="{ active: filter === 'Alla' }" @click="filter = 'Alla'">
            Alla
          </button>

          <button type="button" class="filter-button" :class="{ active: filter === 'Ej klara' }"
            @click="filter = 'Ej klara'">
            Ej klara
          </button>

          <button type="button" class="filter-button" :class="{ active: filter === 'Klara' }" @click="filter = 'Klara'">
            Klara
          </button>
        </div>

        <div class="sort-section">
          <label for="sortOrder">Sortera efter deadline:</label>

          <select id="sortOrder" v-model="sortOrder">
            <option value="Tidigast">Tidigast</option>
            <option value="Senast">Senast</option>
          </select>
        </div>

        <div v-if="filteredTasks.length === 0" class="empty-list">
          <p>Inga uppgifter att visa.</p>
          <p>Byt filter eller lägg till en ny uppgift.</p>
        </div>

        <div v-else class="tasks-list">
          <div v-for="task in filteredTasks" :key="task.id" class="task-card" :class="{
            completed: task.completed,
            'priority-high': task.priority === 'Hög',
            'priority-medium': task.priority === 'Medel',
            'priority-low': task.priority === 'Låg'
          }">
            <h3>{{ task.name }}</h3>
            <p>
              <strong>Kurs:</strong> {{ task.course }}
            </p>

            <p>
              <strong>Deadline:</strong> {{ task.deadline }}
            </p>

            <p>
              <strong>Prioritet:</strong> {{ task.priority }}
            </p>

            <button type="button" class="complete-button" @click="toggleTask(task.id)">
              {{ task.completed ? 'Markera som ej klar' : 'Markera som klar' }}

            </button>

            <button type="button" class="delete-button" @click="removeTask(task.id)">
              Ta bort uppgiften
            </button>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>