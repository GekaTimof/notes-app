<template>
  <div class="p-4 max-w-3xl mx-auto border-4 border-black rounded-lg">
    <h1 class="text-3xl font-bold mb-4 text-center text-blue-600 border-b-2 border-blue-400 pb-2">Notes Manager</h1>

    <div class="p-4 border-4 border-black rounded-lg mb-6 text-center">
      <!-- Form for adding notes -->
      <form @submit.prevent="addNote" class="shadow-md p-4 rounded bg-gray-50 border border-gray-300">
        <div class="mb-4">
          <label for="title" class="block text-lg font-semibold mb-1">Title</label>
          <input
            v-model="newNote.title"
            id="title"
            type="text"
            class="w-full p-2 border rounded focus:outline-none focus:ring focus:border-blue-400"
            placeholder="Enter a title"
          />
        </div>
        <div class="mb-4">
          <label for="content" class="block text-lg font-semibold mb-1">Content</label>
          <textarea
            v-model="newNote.content"
            id="content"
            class="w-full p-2 border rounded focus:outline-none focus:ring focus:border-blue-400"
            placeholder="Enter the content"
          ></textarea>
        </div>
        <div class="mb-4">
          <label class="block text-lg font-semibold mb-1">Priority</label>
          <select v-model="newNote.priority" class="w-full p-2 border rounded focus:outline-none focus:ring focus:border-blue-400">
            <option value="low">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
          </select>
        </div>
        <button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded shadow hover:bg-blue-600">
          Add Note
        </button>
      </form>
    </div>

    <!-- Notes list -->
    <div
      v-for="(note, index) in notes"
      :key="index"
      class="p-4 mb-4 border-2 rounded bg-gray-100 shadow border-gray-400 text-center"
      :class="{ 'bg-green-100 border-green-400': note.completed }"
    >
      <div class="flex items-center justify-between mb-2">
        <input type="checkbox" v-model="note.completed" @change="toggleCompletion(note)" class="mr-2" />
        <h2 class="text-xl font-bold text-blue-600 border-b border-blue-300 pb-1 flex-1 text-left" :class="{ 'line-through text-gray-500': note.completed }">
          {{ note.title }}
        </h2>
      </div>
      <p class="mb-2" :class="{ 'line-through text-gray-500': note.completed }">{{ note.content }}</p>
      <p class="text-sm text-gray-600" :class="{ 'line-through text-gray-500': note.completed }">Priority: {{ note.priority }}</p>
      <button @click="deleteNote(index)" class="bg-red-500 text-white py-1 px-3 rounded shadow hover:bg-red-600">
        Delete
      </button>
    </div>

    <!-- Reset button -->
    <button @click="resetNotes" class="bg-gray-500 text-white py-2 px-4 rounded shadow hover:bg-gray-600 mt-4 border border-gray-400">
      Clear All Notes
    </button>
  </div>
</template>





<script>
export default {
  data() {
    return {
      notes: (JSON.parse(localStorage.getItem("notes")) || []).map(note => ({
        ...note,
        completed: note.completed || false, // Добавляем свойство completed, если его нет
      })),
      newNote: {
        title: "",
        content: "",
        priority: "low",
        completed: false,
      },
    };
  },
  methods: {
    addNote() {
      if (this.newNote.title && this.newNote.content) {
        this.notes.push({ ...this.newNote });
        this.saveNotes();
        this.newNote = { title: "", content: "", priority: "low", completed: false };
      }
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
      this.saveNotes();
    },
    resetNotes() {
      this.notes = [];
      this.saveNotes();
    },
    saveNotes() {
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },
  },
};
</script>




<style scoped>
h1 {
  font-family: 'Arial', sans-serif;
}

form {
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.line-through {
  text-decoration: line-through;
}

div.p-4.border-4.border-black {
  border: 4px solid black;
  border-radius: 10px;
  padding: 16px;
}

div.p-4.mb-4.border-2 {
  border: 2px solid gray;
  border-radius: 8px;
  padding: 12px;
  transition: background-color 0.3s, border-color 0.3s;
}

div.p-4.mb-4.border-2.bg-green-100 {
  border-color: green; 
}
</style>
