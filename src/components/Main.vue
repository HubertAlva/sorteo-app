<script setup lang="ts">
import {
  TagsInput,
  TagsInputInput,
  TagsInputItem,
  TagsInputItemDelete,
  TagsInputItemText,
} from '@/components/ui/tags-input'
import { Label } from '@/components/ui/label'
import { Alert, AlertDescription } from '@/components/ui/alert'
import {
  NumberField,
  NumberFieldContent,
  NumberFieldDecrement,
  NumberFieldInput,
  NumberFieldIncrement,
} from '@/components/ui/number-field'
import { Button } from '@/components/ui/button'
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['winners-announced', 'clear'])

const participants = ref<string[]>([])

const winnersLimit = ref(1)

const winners = ref<string[]>([])

const handlePaste = (event: ClipboardEvent) => {
  const pasteData = event.clipboardData?.getData('text')
  if (pasteData) {
    const newTags = pasteData
      .split(',')
      .map((tag) => tag.trim())
      .filter((tag) => tag)
    newTags.forEach((tag) => {
      if (!participants.value.includes(tag)) {
        participants.value.push(tag)
      }
    })
    event.preventDefault()
  }
}

const getWinners = () => {
  if (participants.value.length === 0) {
    alert('No hay participantes')
    return
  } else if (participants.value.length < winnersLimit.value) {
    alert('No hay suficientes participantes')
    return
  }
  const shuffledParticipants = [...participants.value].sort(() => Math.random() - 0.5)
  winners.value = shuffledParticipants.slice(0, winnersLimit.value)
  emit('winners-announced')
}

const clear = () => {
  participants.value = []
  winnersLimit.value = 1
  winners.value = []
  emit('clear')
}
</script>

<template>
  <section class="space-y-8">
    <div class="space-y-3">
      <div class="space-y-2">
        <Label class="text-lg md:text-xl">👥 Participantes</Label>
        <TagsInput v-model="participants">
          <TagsInputItem v-for="item in participants" :key="item" :value="item">
            <TagsInputItemText />
            <TagsInputItemDelete />
          </TagsInputItem>

          <TagsInputInput placeholder="Nick,Juanito,Gusgus..." @paste="handlePaste" />
        </TagsInput>
      </div>

      <Alert class="bg-amber-500/15 border-amber-600">
        <AlertDescription>
          Para agregar participantes, puedes escribir sus nombres separados por comas o pegar una
          lista de nombres separados por comas.
        </AlertDescription>
      </Alert>
    </div>

    <div>
      <NumberField :min="1" :max="10" :default-value="1" v-model="winnersLimit">
        <Label class="text-lg md:text-xl">🏆 Número de ganadores</Label>
        <NumberFieldContent>
          <NumberFieldDecrement />
          <NumberFieldInput />
          <NumberFieldIncrement />
        </NumberFieldContent>
      </NumberField>
    </div>

    <Button class="w-full" @click="getWinners()"> Sortear </Button>
  </section>

  <section v-if="winners.length" class="space-y-4">
    <h2 class="text-lg md:text-xl font-bold">🎉 Ganadores</h2>
    <div class="border p-4 rounded flex flex-col gap-2 justify-start items-center">
      <template v-for="winner in winners" :key="winner">
        <h3 class="font-medium text-lg text-center">
          {{ winner }}
        </h3>
      </template>
    </div>
  </section>

  <div class="flex justify-end">
    <Button variant="secondary" @click="clear()">Limpiar</Button>
  </div>
</template>
