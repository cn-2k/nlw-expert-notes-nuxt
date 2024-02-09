<template>
  <ScDialog>
    <ScDialogTrigger
      class="flex flex-col gap-3 rounded-md bg-slate-700 p-5 text-left outline-none hover:ring-2 hover:ring-slate-600 focus-visible:ring-2 focus-visible:ring-lime-400"
    >
      <span class="text-sm font-medium text-slate-200"> Adicionar nota </span>
      <p class="text-sm leading-6 text-slate-400">
        Grave uma nota em áudio que será convertida para texto automaticamente.
      </p>
    </ScDialogTrigger>
    <ScDialogContent
      class="flex h-[60vh] w-full max-w-[640px] flex-col rounded-md bg-slate-700 outline-none"
    >
      <form class="flex flex-1 flex-col" @submit.prevent="handleSaveNote">
        <div class="flex flex-1 flex-col gap-3 p-5">
          <span class="text-sm font-medium text-slate-300">
            Adicionar nota
          </span>

          <p
            v-if="shouldShowOnboarding"
            class="text-sm leading-6 text-slate-400"
          >
            Comece
            <button class="font-medium text-lime-400 hover:underline">
              gravando uma nota
            </button>
            em áudio ou se preferir
            <button
              class="font-medium text-lime-400 hover:underline"
              @click="handleStartEditor"
            >
              utilize apenas texto
            </button>
          </p>

          <textarea
            v-if="!shouldShowOnboarding"
            v-model="content"
            autofocus
            class="flex-1 resize-none bg-transparent text-sm leading-6 text-slate-400 outline-none"
            @change="handleContentChanged"
          />
        </div>

        <button
          type="submit"
          class="w-full bg-lime-400 py-4 text-center text-sm font-medium text-lime-950 outline-none hover:bg-lime-500"
        >
          Salvar nota
        </button>
      </form>
    </ScDialogContent>
  </ScDialog>
</template>

<script setup lang="ts">
import { toast } from "vue-sonner";

const shouldShowOnboarding = ref<boolean>(true);
const content = ref<string>("");

function handleStartEditor() {
  shouldShowOnboarding.value = false;
}

function handleContentChanged() {
  console.log(content.value);
  if (content.value === "") {
    shouldShowOnboarding.value = true;
  }
}

function handleSaveNote() {
  console.log(content.value);
  toast.success("Nota criada com sucesso!");
}
</script>
