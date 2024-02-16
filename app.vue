<template>
  <div
    class="mx-auto my-6 max-w-6xl space-y-4 px-6 lg:my-12 lg:space-y-6 lg:px-0"
  >
    <img src="~/assets/logo-nlw-expert.svg" alt="NLW Expert" />
    <form class="w-full">
      <input
        v-model="search"
        type="text"
        placeholder="Busque em suas notas..."
        class="w-full bg-transparent text-xl font-semibold tracking-tight outline-none placeholder:text-slate-500 lg:text-3xl"
      />
    </form>

    <div class="h-px bg-slate-700"></div>

    <div
      v-auto-animate
      class="grid h-[75vh] auto-rows-[250px] grid-cols-1 gap-6 overflow-y-auto overflow-x-hidden py-4 pl-2 pr-3 scrollbar scrollbar-track-transparent scrollbar-thumb-slate-300/20 lg:h-[700px] lg:grid-cols-3"
    >
      <NewNoteCard @on-note-created="onNoteCreated" />
      <NoteCard
        v-for="note in filteredNotes"
        :key="note.id"
        :note="note"
        @on-note-deleted="onNoteDeleted"
      />
    </div>
  </div>

  <Toaster rich-colors />
</template>

<script setup lang="ts">
import { Toaster, toast } from "vue-sonner";
import { vAutoAnimate } from "@formkit/auto-animate";

interface NoteProps {
  id: string;
  date: Date;
  content: string;
}

const search = ref<string>("");
const notes = ref<NoteProps[]>(loadNotesFromLocalStorage());

function onNoteCreated(content: string) {
  const newNote = {
    id: crypto.randomUUID(),
    date: new Date(),
    content,
  };

  const notesArray = [newNote, ...notes.value];
  notes.value = notesArray;
  localStorage.setItem("notes", JSON.stringify(notesArray));
}

function onNoteDeleted(id: string) {
  const notesArray = notes.value.filter((note) => {
    return note.id !== id;
  });

  notes.value = notesArray;

  localStorage.setItem("notes", JSON.stringify(notesArray));

  toast.success("Nota removida com sucesso!");
}

function loadNotesFromLocalStorage(): NoteProps[] {
  const savedNotes = localStorage.getItem("notes");
  return savedNotes ? JSON.parse(savedNotes) : [];
}

const filteredNotes = computed(() => {
  return search.value === ""
    ? notes.value
    : notes.value.filter((note) =>
        note.content.toLowerCase().includes(search.value.toLowerCase()),
      );
});
</script>
