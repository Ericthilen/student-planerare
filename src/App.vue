<script>
export default {
  data() {
    return {
      studentName: '',
      taskName: '',
      course: '',
      deadline: '',
      priority: "Medel"
    }
  }
}
</script>

<template>
  <div class="app-container">
    <header class="header">
      <h1>Studentplaneraren</h1>
      <p>Håll koll på dina skolluppgifter och deadliens.</p>
    </header>

    <main>
      <section class="student-section">
        <label for="studentName">Studentens namn:</label>

        <input
        id="studentName"
        v-model="studentName"
        type="text"
        placeholder="Ange studentens namn"
        />

        <p v-if="studentName" class="welcome-text">
          Hej {{ studentName }}! Här kan du planera dina skoluppgifter.
        </p>
      </section>

      <section class="form-section">
        <h2>Lägg till skoluppgift</h2>

        <form>
          <div class="form-grid">
            <div class="form-group">
              <label for="taskName">Uppgiftens namn:</label>
              <input
                id="taskName"
                v-model="taskName"
                type="text"
                placeholder="Ange uppgiftens namn"
              />
            </div>

            <div class="form-group">
              <label for="course">Kurs:</label>
              <input
                id="course"
                v-model="course"
                type="text"
                placeholder="Ange kurs"
              />
            </div>

            <div class="form-group">
              <label for="deadline">Deadline:</label>
              <input
                id="deadline"
                v-model="deadline"
                type="date"
              />
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

          <button type="button" class="add-button">
            Lägg till uppgiften
          </button>
        </form>
      </section>

      <section class="tasks-section">
        <div class="tasks-heading">
          <h2>Mina uppgifter</h2>
          <span>0 uppgifter</span>
        </div>

        <div class="empty-list">
          <p>Du har inga skolluppgifter ännu</p>
          <p>Lägg till din första uppgift ovan.</p>
        </div>
      </section>
    </main>
  </div>
</template>