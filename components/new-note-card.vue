<template>
  <ScDialog :open="isDialogOpen" @update:open="isDialogOpen = !isDialogOpen">
    <ScDialogTrigger
      class="flex flex-col gap-3 overflow-hidden rounded-md bg-slate-700 p-5 text-left outline-none hover:ring-2 hover:ring-slate-600 focus-visible:ring-2 focus-visible:ring-lime-400"
    >
      <span class="text-sm font-medium text-slate-200"> Adicionar nota </span>
      <p class="break-all text-sm leading-6 text-slate-400">
        Grave uma nota em áudio que será convertida para texto automaticamente.
      </p>
    </ScDialogTrigger>
    <ScDialogContent
      class="flex h-[60vh] min-h-[400px] w-full max-w-[640px] flex-col overflow-hidden rounded-md border-transparent bg-slate-700 p-0 outline-none"
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
      <div class="flex flex-1 flex-col">
        <div class="flex flex-1 flex-col gap-3 p-5">
          <p
            v-if="shouldShowOnboarding"
            class="text-sm leading-6 text-slate-400"
          >
            Comece
            <button
              class="font-medium text-lime-400 hover:underline"
              @click="handleStartRecording"
            >
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
            class="w-full flex-1 resize-none bg-transparent pr-2 text-sm leading-6 text-slate-400 outline-none scrollbar-thin scrollbar-track-transparent scrollbar-thumb-slate-300/20"
            placeholder="Comece a escrever sua nota..."
            @input="handleContentChanged"
          />
        </div>
      </div>
      <ScDialogFooter class="w-full">
        <button
          v-if="isRecording"
          type="button"
          class="flex w-full items-center justify-center gap-2 bg-slate-900 py-4 text-center text-sm font-medium text-slate-300 outline-none hover:text-slate-100"
          @click="handleStopRecording"
        >
          <div class="size-3 animate-pulse rounded-full bg-red-500" />
          Gravando! (clique p/ interromper)
        </button>

        <button
          v-if="!isRecording"
          type="button"
          :disabled="!content"
          class="w-full bg-lime-400 py-4 text-center text-sm font-medium text-lime-950 outline-none hover:bg-lime-500 disabled:cursor-not-allowed disabled:bg-lime-700"
          @click="handleSaveNote"
        >
          Salvar nota
        </button>
      </ScDialogFooter>
    </ScDialogContent>
  </ScDialog>
</template>

<script setup lang="ts">
import { toast } from "vue-sonner";

const emit = defineEmits<{
  onNoteCreated: [content: string];
}>();

const vFocus = {
  mounted: (el: HTMLElement) => el.focus(),
};

const shouldShowOnboarding = ref<boolean>(true);
const isDialogOpen = ref<boolean>(false);
const content = ref<string>("");
const isRecording = ref<boolean>(false);
const SpeechRecognitionAPI =
  window.SpeechRecognition || window.webkitSpeechRecognition;
const speechRecognition = new SpeechRecognitionAPI();

function handleStartEditor() {
  shouldShowOnboarding.value = false;
}

function handleContentChanged() {
  if (content.value === "") {
    shouldShowOnboarding.value = true;
  }
}

function handleSaveNote() {
  if (content.value === "") {
    return;
  }

  emit("onNoteCreated", content.value);

  content.value = "";
  shouldShowOnboarding.value = true;
  toast.success("Nota criada com sucesso!");
  isDialogOpen.value = false;
}

function handleStartRecording() {
  const isSpeechRecognitionAPIAvailable =
    "SpeechRecognition" in window || "webkitSpeechRecognition" in window;

  if (!isSpeechRecognitionAPIAvailable) {
    alert(
      "Infelizmente seu navegador não suporta a API de reconhecimento de fala.",
    );
    return;
  }

  isRecording.value = true;
  shouldShowOnboarding.value = false;

  speechRecognition.lang = "pt-BR";
  speechRecognition.continuous = true;
  speechRecognition.maxAlternatives = 1;
  speechRecognition.interimResults = true;

  speechRecognition.onresult = (event) => {
    const transcription = Array.from(event.results).reduce((text, result) => {
      return text.concat(result[0].transcript);
    }, "");

    content.value = transcription;
  };

  speechRecognition.onerror = (event) => {
    console.error(event);
  };

  speechRecognition.start();
}

function handleStopRecording() {
  isRecording.value = false;

  if (speechRecognition !== null) {
    speechRecognition.stop();
  }
}

function handleClose() {
  shouldShowOnboarding.value = true;
  content.value = "";
  handleStopRecording();
}
</script>
