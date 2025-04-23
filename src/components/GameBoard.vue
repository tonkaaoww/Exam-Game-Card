<template>
  <div
    class="flex flex-col items-center rounded-2xl bg-white mx-4 my-4 sm:mx-18 sm:my-10 shadow-2xl p-4 sm:p-12"
  >
    <!-- ส่วนข้อมูลด้านบน -->
    <div
      class="w-full flex flex-col sm:flex-row justify-between items-center text-gray-600 text-base sm:text-xl mb-4 gap-2 sm:gap-0"
    >
      <p>⏱️ เวลา: {{ elapsedTime }} วินาที</p>
      <p>🂡 เปิด Card ไปแล้ว: {{ flipCount }} ครั้ง</p>
    </div>

    <!-- ส่วนการ์ด -->
    <div class="flex flex-wrap justify-center gap-4 sm:gap-6">
      <Card
        v-for="card in cards"
        :key="card.id"
        :icon="card.icon"
        :flipped="card.flipped"
        :matched="card.matched"
        @click="handleFlipCard(card)"
      />
    </div>
  </div>

  <GameResultModal
    v-if="isGameOver"
    :elapsedTime="elapsedTime"
    :flipCount="flipCount"
    @restart="restartGame"
  />
</template>



<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import Card from "./Card.vue";
import type { CardType } from "@/types/card.type";
import GameResultModal from "./GameResultModal.vue";

const isGameOver = ref(false) //` สถานะเกมจบหรือไม่

//  ดึงไอคอน svg จาก assets/cards/
const images = import.meta.glob("../assets/card/*.svg", { eager: true });
const icons: string[] = Object.values(images).map((mod: any) => mod.default);

//  สร้าง state สำหรับการ์ด
const cards = ref<CardType[]>([]);
const flippedCards = ref<CardType[]>([]);

//จับเวลา
const elapsedTime = ref(0);
let timer: number | null = null;

// นับจำนวนครั้งการเปิดไพ่
const flipCount = ref(0);


onMounted(() => {
  initGame(); // เริ่มเกมเมื่อ component ถูก mount
  startTimer(); // เริ่มจับเวลา
});

// //  แบ่งเป็นแถวละ 5 ใบ
// const cardRows = computed(() => {
//   const rows = [];
//   for (let i = 0; i < cards.value.length; i += 5) {
//     rows.push(cards.value.slice(i, i + 5));
//   }
//   return rows;
// });

watch(
  () => cards.value.filter((card) => !card.matched).length,// <- ฟังก์ชันนี้ return ค่าที่จะ watch
  (remaining) => { // <- Vue เอาค่าที่ return มาใส่ให้ตรงนี้
    if (remaining === 0 && timer !== null) {
      clearInterval(timer);
      timer = null;
      isGameOver.value = true; // เกมจบแล้ว
    }
  }
);

function handleFlipCard(card: any) {
  if (card.flipped || card.matched || flippedCards.value.length === 2) return; // ถ้าการ์ดคว่ำอยู่หรือจับคู่แล้ว
  card.flipped = true; // หงายการ์ด
  flippedCards.value.push(card); // เพิ่มการ์ดที่หงายแล้วลงในอาเรย์
  flipCount.value++; //  นับการเปิดไพ่

  if (flippedCards.value.length === 2) {
    // ถ้ามีการ์ดหงาย 2 ใบ
    const [firstCard, secondCard] = flippedCards.value;

    if (firstCard.icon === secondCard.icon) {
      // ถ้าการ์ดทั้งสองใบมีไอคอนเหมือนกัน
      firstCard.matched = true;
      secondCard.matched = true;
      flippedCards.value = []; // เคลียร์การ์ดที่หงายแล้ว
    } else {
      // ถ้าไม่เหมือนกัน
      setTimeout(() => {
        firstCard.flipped = false; // คว่ำการ์ดแรก
        secondCard.flipped = false; // คว่ำการ์ดที่สอง
        flippedCards.value = []; // เคลียร์การ์ดที่หงายแล้ว
      }, 1000);
    }
  }
}

function restartGame() {
  isGameOver.value = false
  elapsedTime.value = 0
  flipCount.value = 0
  cards.value = []
  flippedCards.value = []
  // รีเซ็ตสถานะทั้งหมด
  initGame();
  startTimer();
}

function initGame() {
  const pairedIcons = [...icons, ...icons]; // สร้างคู่ของไอคอน
  const shuffled = pairedIcons.sort(() => 0.5 - Math.random());

  cards.value = shuffled.map((icon, index) => ({
    id: index + 1,
    icon,
    flipped: false,
    matched: false
  }));
}

function startTimer(){
    // เริ่มจับเวลา
    timer = setInterval(() => {
    elapsedTime.value++;
  }, 1000);
};
</script>
