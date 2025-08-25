<script setup>
import { ref } from "vue"
import Sidebar from './components/Sidebar.vue'

const notes = ref([]); //array of notes
const active_note = ref(null);
const input_title = ref('');
const input_content = ref('');

// Create Note Function
function create_note() {
  // random id every call
  const id = Math.random().toString(36).substring(2, 9);
  // create new note
  notes.value.push({
    id,
    title: 'untitled',
    content: ''
  })
  //set active note id
  set_active_note(id);
};

// Set Active Note Function
function set_active_note(id) {
  active_note.value = id;
  let note = notes.value.find(notes => note.id === id);
  input_title.value = note.title;
  input_content.value = note.content;

  setTimeout(() => {
    document.querySelector('input[type="text"]').focus();
  }, 0);
};

// Delete Note Function
function delete_note() {
  evt.stopPropagation();
  
  let note = notes.value.findIndex(note => note.id === id);
  notes.value.splice(note, 1);

  active_note.value = null;
  input_title.value = '';
  input_content.value = '';

};

// Update Note Function
function update_note() {
  let note = notes.value.findIndex(note => note.id === active_note.value);

  notes.value[note].title = input_title.value;
  notes.value[note].content = input_content.value;
};

</script>

<template>
  <div class="bg-gray-700 text-white min-h-screen flex items-stretch justify-start">
    <!-- SIDEBAR -->
    <Sidebar
      :active_note="active_note"
      :notes="notes"
      @new-note="create_note"
      @set-active-note="set_active_note"
      @delete-note="delete_note"
    />
    <!-- MAIN CONTENT -->
    <main class="flex-1">
      <div v-if="active_note" class="px-4 py-8 flex flex-col h-full">
        <input
          v-model="input_title"
          @input="update_note"
          type="text"
          class="block w-full text-3xl pb-2 font-bold border-b-2 border-gray-500
          focus:border-white outline-none transition-colors duration-200"
        />
        <textarea
          v-model="input_content"
          @input="update_note"
          class="block w-full h-full mt-4 text-lg outline-none flex-1"
        ></textarea>
      </div>
    </main>
  </div>
</template>


