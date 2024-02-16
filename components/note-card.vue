<template>
  <ScDialog>
    <ScDialogTrigger
      class="relative flex flex-col gap-3 overflow-hidden rounded-md bg-slate-800 p-5 text-left outline-none hover:ring-2 hover:ring-slate-600 focus-visible:ring-2 focus-visible:ring-lime-400"
    >
      <span class="text-sm font-medium text-slate-300">
        {{
          formatDistanceToNow(props.note.date, {
            locale: ptBR,
            addSuffix: true,
          })
        }}
      </span>

      <p class="break-all text-sm leading-6 text-slate-400">
        {{ props.note.content }}
      </p>

      <div
        class="pointer-events-none absolute bottom-0 left-0 right-0 h-1/2 bg-gradient-to-t from-black/60 to-black/0"
      />
    </ScDialogTrigger>
    <ScDialogContent
      class="flex h-[60vh] w-full max-w-[640px] flex-col overflow-hidden rounded-md border-transparent bg-slate-700 p-0 outline-none"
    >
      <ScDialogHeader class="relative p-5">
        <ScDialogTitle>
          <span class="text-sm font-medium text-slate-300">
            {{
              formatDistanceToNow(props.note.date, {
                locale: ptBR,
                addSuffix: true,
              })
            }}
          </span>
        </ScDialogTitle>
        <ScDialogDescription
          class="h-[450px] overflow-y-auto scrollbar-thin scrollbar-track-transparent scrollbar-thumb-slate-300/20"
        >
          <p
            class="break-all pr-2 text-justify text-sm leading-6 text-slate-400"
          >
            {{ props.note.content }}
          </p>
        </ScDialogDescription>
      </ScDialogHeader>
      <ScDialogFooter class="absolute bottom-0 w-full">
        <button
          type="button"
          class="group w-full bg-slate-800 py-4 text-center text-sm font-medium text-slate-300 outline-none"
          @click="emit('onNoteDeleted', props.note.id)"
        >
          Deseja
          <span class="text-red-400 group-hover:underline">
            apagar essa nota?
          </span>
        </button>
      </ScDialogFooter>
    </ScDialogContent>
  </ScDialog>
</template>

<script setup lang="ts">
import { formatDistanceToNow } from "date-fns";
import { ptBR } from "date-fns/locale";

interface NoteCardProps {
  note: {
    id: string;
    date: Date;
    content: string;
  };
}

const props = defineProps<NoteCardProps>();

const emit = defineEmits<{
  onNoteDeleted: [id: string];
}>();
</script>
