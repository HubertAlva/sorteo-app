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

const cleanParticipants = (participants: string[]) => participants.filter((p) => p.trim() !== '')
</script>

<template>
  <section class="space-y-8">
    <div class="space-y-3">
      <div class="space-y-2">
        <Label class="text-lg md:text-xl">👥 Participantes</Label>
        <TagsInput
          v-model="participants"
          addOnPaste
          @update:model-value="participants = cleanParticipants(participants)"
        >
          <TagsInputItem v-for="item in participants" :key="item" :value="item">
            <TagsInputItemText />
            <TagsInputItemDelete />
          </TagsInputItem>

          <TagsInputInput placeholder="Nick,Juanito,Gusgus..." :max-length="20" />
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
    <h2 class="text-lg font-bold md:text-xl">🎉 Ganadores</h2>
    <div class="flex flex-col items-center justify-start gap-2 p-4 border rounded">
      <template v-for="winner in winners" :key="winner">
        <h3 class="text-lg font-medium text-center">
          {{ winner }}
        </h3>
      </template>
    </div>
  </section>

  <div class="flex justify-end">
    <Button variant="secondary" @click="clear()">Limpiar</Button>
  </div>
</template>
