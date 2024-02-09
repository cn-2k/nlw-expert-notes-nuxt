<template>
  <div class="mx-auto my-12 max-w-6xl space-y-6">
    <img src="~/assets/logo-nlw-expert.svg" alt="NLW Expert" />
    <form class="w-full">
      <input
        type="text"
        placeholder="Busque em suas notas..."
        class="w-full bg-transparent text-3xl font-semibold tracking-tight outline-none placeholder:text-slate-500"
      />
    </form>

    <div class="h-px bg-slate-700"></div>

    <div
      class="grid auto-rows-[250px] grid-cols-1 gap-6 p-6 lg:grid-cols-3 lg:p-0"
    >
      <NewNoteCard />
      <NoteCard :note="note" />
      <NoteCard :note="note" />
    </div>
  </div>
</template>

<script setup lang="ts">
const note = {
  date: new Date(),
  content: "Hello World",
};
</script>
