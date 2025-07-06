  <script setup lang="ts">
  defineProps<{
    image: string;
    status: "closed" | "opened" | "matched",
    disabled?: boolean
  }>()
</script>

  <template>
    <div class="card" :class="{
      flipped: status === 'opened' || status === 'matched',
      matched: status === 'matched',
      disabled
    }">
      <div class="card-inner">
        <div class="card-face card-back"></div>
        <div class="card-face card-front">
          <img :src="image" />
        </div>
      </div>
    </div>
  </template>

<style>
.card {
  height: 120px;
  perspective: 1000px;
  cursor: pointer;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.6s;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.card-back {
  background-color: #757575;
  border: 2px solid #f0f9ff;
}

.card-front {
  background-color: #fff;
  border: 2px solid #e0e0e0;
  transform: rotateY(180deg);
}

.card-front img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.card.matched .card-front {
  background-color: #89ff897f;
  border-color: #00f500;
}

.card.disabled {
  pointer-events: none;
}
</style>
