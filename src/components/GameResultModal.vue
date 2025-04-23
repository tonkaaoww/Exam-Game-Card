<template>
    <div class="fixed inset-0  bg-opacity-0 flex justify-center items-center">
      <div class="bg-white rounded-2xl shadow-2xl w-[500px] h-[400px] text-center border-1 flex flex-col justify-center items-center">
        <h2 class="text-2xl font-bold mb-4">🎉 เกมจบแล้ว! 🎉</h2>
        <p>⏱️ เวลาที่ใช้: {{ elapsedTime }} วินาที</p>
        <p>🂡 เปิดไพ่ไปแล้ว: {{ flipCount }} ครั้ง</p>
        <button @click="$emit('restart')" class="mt-4 bg-blue-500 hover:bg-blue-600 text-white py-2 px-4 rounded">
          Restart
        </button>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  defineProps<{ elapsedTime: number; flipCount: number }>()
  defineEmits<{ (e: 'restart'): void }>()
  </script>
  