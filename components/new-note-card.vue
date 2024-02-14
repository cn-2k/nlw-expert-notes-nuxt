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
      trap-focus
      class="flex h-[60vh] w-full max-w-[640px] flex-col overflow-hidden rounded-md border-transparent bg-slate-700 p-0 outline-none"
      @close-auto-focus="handleClose"
      @interact-outside="(e) => e.preventDefault()"
    >
      <ScDialogHeader class="px-5 pt-5">
        <ScDialogTitle>
          <span class="text-sm font-medium text-slate-300">
            Adicionar nota
          </span></ScDialogTitle
        >
        <ScDialogDescription>
          <p class="text-sm leading-6 text-slate-400">
            Grave uma nota em áudio que será convertida para texto
            automaticamente.
          </p>
        </ScDialogDescription>
      </ScDialogHeader>
      <form class="flex flex-1 flex-col" @submit.prevent="handleSaveNote">
        <div class="flex flex-1 flex-col gap-3 p-5">
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
              utilize apenas texto.
            </button>
          </p>

          <textarea
            v-if="!shouldShowOnboarding"
            v-model="content"
            v-focus
            class="flex-1 resize-none bg-transparent text-sm leading-6 text-slate-400 outline-none"
            @input="handleContentChanged"
          />
        </div>

        <button
          type="submit"
          :disabled="!content"
          class="w-full bg-lime-400 py-4 text-center text-sm font-medium text-lime-950 outline-none hover:bg-lime-500 disabled:cursor-not-allowed disabled:bg-lime-700"
        >
          Salvar nota
        </button>
      </form>
    </ScDialogContent>
  </ScDialog>
</template>

<script setup lang="ts">
import { toast } from "vue-sonner";

const vFocus = {
  mounted: (el: HTMLElement) => el.focus(),
};

const shouldShowOnboarding = ref<boolean>(true);
const content = ref<string>("");

function handleStartEditor() {
  shouldShowOnboarding.value = false;
}

function handleContentChanged() {
  if (content.value === "") {
    shouldShowOnboarding.value = true;
  }
}

function handleSaveNote() {
  toast.success("Nota criada com sucesso!");
}

function handleClose() {
  shouldShowOnboarding.value = true;
  content.value = "";
}
</script>
