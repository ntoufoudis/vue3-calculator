<script setup lang="ts">
import { ref } from 'vue'

const result = ref('')
const calculated = ref(false)

function handleClick(value: any) {
  if (calculated.value) {
    result.value = value
    calculated.value = false
  } else {
    result.value += value
  }
}

function handleClear() {
  result.value = ''
  calculated.value = false
}

function handleClearEntry() {
  if (result.value && result.value.length > 0) {
    result.value = result.value.slice(0, -1)

    if (result.value.length === 0 ) {
      handleClear()
    }
  } else {
    handleClear()
  }
}

function handleOperatorClick(operator: any) {
  if (/[*/-]$/.test(result.value)) {
    result.value = result.value.slice(0, -1) + operator
  } else {
    result.value += operator
  }
  calculated.value = false
}

function calculate() {
  try {
    let evaluatedResult = eval(result.value.replace(/(^|[^0-9])0+(\d+)/g, '$1$2'))

    if (evaluatedResult === Infinity || evaluatedResult === -Infinity) {
      throw new Error('Divide by zero error')
    }
    result.value = Number.isFinite(evaluatedResult)
      ? evaluatedResult : 'Error'
    calculated.value = true
  } catch (error) {
    if (error instanceof Error) {
      if (error.message === 'Divide by zero error') {
        result.value = 'Error: Divide by zero';
      } else {
        result.value = 'Error';
      }
    }
  }
}
</script>

<template>
  <div class="text-center font-sans">
    <h1 class="text-gray-700 text-3xl font-bold my-5">Calculator App</h1>
    <div class="w-80 mx-auto my-0 p-5 border-solid border border-gray-300 rounded-md">
      <input
        type="text"
        class="w-full text-right p-2.5 text-xl mb-2.5 border border-solid rounded border-black"
        :value="result"
        readonly
      />
      <div class="grid gap-2.5 grid-cols-4">
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('7')"
        >
          7
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('8')"
        >
          8
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('9')"
        >
          9
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleOperatorClick('/')"
        >
          /
        </button>

        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('4')"
        >
          4
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('5')"
        >
          5
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('6')"
        >
          6
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleOperatorClick('*')"
        >
          *
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('1')"
        >
          1
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('2')"
        >
          2
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('3')"
        >
          3
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleOperatorClick('-')"
        >
          -
        </button>

        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('0')"
        >
          0
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('.')"
        >
          .
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleClick('00')"
        >
          00
        </button>

        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-gray-100 hover:bg-gray-200"
          @click="handleOperatorClick('+')"
        >
          +
        </button>

        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-red-400 hover:bg-red-500 text-white"
          @click="handleClear"
        >
          C
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-red-400 hover:bg-red-500 text-white"
          @click="handleClearEntry"
        >
          CE
        </button>
        <button
          class="p-4 text-lg cursor-pointer border-none outline-none bg-red-400 hover:bg-red-500 text-white col-span-2"
          @click="calculate()"
        >
          =
        </button>
      </div>
    </div>
  </div>
</template>