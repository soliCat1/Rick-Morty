<template>
  <div :class="card.card">
    <img :class="card.img" :src="$props.character.image" width="220" height="230">
    <div :class="card.content">
      <div>
        <h2 :class="card.name">{{ $props.character.name }}</h2>
        <span :class="[card.status,
         $props.character.status === 'Dead' ? card.dead : '',
         $props.character.status === 'Alive' ? card.alive : '']">
          {{ $props.character.status }} - {{ $props.character.species }}
        </span>
      </div>
      <div>
        <span :class="card.title">Last known location:</span>
        <span :class="card.description">{{ $props.character.location.name }}</span>
      </div>
      <div>
        <span :class="card.title">First seen in:</span>
        <span :class="card.description">{{ firstSeen }}</span>
      </div>
    </div>
  </div> 
</template>

<script>
import {ref} from 'vue'
export default {
  name: 'character-card',
  props: {
    character: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const firstSeen = ref('')
    fetch(props.character.episode[0])
     .then(response => response.json())
     .then(json => firstSeen.value = json.name)
    return {firstSeen}
  }
}
</script>

<style module="card">
.card {
  display: flex;
  width: 100%;
  min-height: 220px;
  background-color: rgb(60, 62, 68);
  border-radius: 10px;

  @media (max-width: 768px) {
    flex-direction: column;
  }
}
.img {
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;

  @media (max-width: 768px) {
    display: block;
    border-bottom-left-radius: 0;
    border-top-right-radius: 10px;
    width: 100%;
    height: auto;
  }
}

.content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;

  @media (max-width: 768px) {
    gap: 10px;
  }
}

.name {
  font-weight: 900;
  font-size: 27px;

  @media (max-width: 768px) {
    font-size: 20px;
  }
}

.status,
.description,
.title {
  display: block;
  font-size: 18px;
  font-weight: 500;

  @media (max-width: 768px) {
    font-size: 14px;
  }
}

.status {
  display: flex;
  gap: 10px;
  align-items: center;
}

.status::before {
  content: '';
  display: block;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: rgb(190, 204, 68);
}

.dead::before {
  background-color: rgb(214, 61, 46);
}

.alive::before {
  background-color: rgb(85, 204, 68);
}

.title {
  color: rgb(158, 158, 158);
  margin-bottom: 10px;

  @media (max-width: 768px) {
    margin-bottom: 5px;
  }
}
</style>
