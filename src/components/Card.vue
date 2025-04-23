<template>
  <div
    class="flex justify-center items-center w-[80px] h-[110px] sm:w-[96px] sm:h-[128px] md:w-24 md:h-32 perspective-1000 cursor-pointer"
    @click="$emit('click')"
  >
    <div class="card-inner w-full h-full relative" :class="{ flipped, matched }">
      <!-- ด้านหน้า -->
      <div class="card-face card-front bg-green-200">
        <img :src="icon" alt="icon" class="w-10 h-10 sm:w-12 sm:h-12 md:w-14 md:h-14" />
      </div>

      <!-- ด้านหลัง -->
      <div class="card-face bg-gradient-to-br from-yellow-100 to-orange-400">
        <!-- พื้นหลังคว่ำ -->
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  icon: string
  flipped: boolean
  matched: boolean
}>()
defineEmits<{
  (e: 'click'): void
}>()
</script>

<style lang="scss" scoped src="../styles/animations.scss"></style>

<style lang="scss" scoped>
@mixin center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-face {
  @include center;
  width: 100%;
  height: 100%;
  border-radius: 0.5rem;
  border: 1px solid #b2b2b2;
  position: absolute;
  top: 0;
  left: 0;
  backface-visibility: hidden;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);

  &.card-front {
    transform: rotateY(180deg);
  }
}
</style>
