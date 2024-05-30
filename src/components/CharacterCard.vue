<template>
  <div :class="card.card">
    <img :class="card.img" :src="$props.character.image" width="220" height="230">
    <div :class="card.content">
      <div :class="card.wrapper">
        <h2 :class="card.name">{{ $props.character.name }}</h2>
        <span :class="[card.status,
         $props.character.status === 'Dead' ? card.dead : '',
         $props.character.status === 'Alive' ? card.alive : '']">
          {{ $props.character.status }} - {{ $props.character.species }}
        </span>
      </div>
      <div :class="card.wrapper">
        <span :class="card.title">Last known location:</span>
        <span :class="card.description">{{ $props.character.location.name }}</span>
      </div>
      <div :class="card.wrapper">
        <span :class="card.title">First seen in:</span>
        <span :class="card.description">{{ firstSeen }}</span>
      </div>
    </div>
  </div> 
</template>

<script>
export default {
  name: 'character-card',
  data: () => ({
    firstSeen: ''
  }),
  props: {
    character: {
      type: Object,
      required: true
    }
  },
  created() {
      fetch(this.$props.character.episode[0])
      .then(response => response.json())
      .then(json => this.firstSeen = json.name)
  }
}
</script>

<style module="card">
.card {
  display: flex;
  width: 600px;
  min-height: 220px;
  background-color: rgb(60, 62, 68);
  border-radius: 10px;
}
.img {
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
}

.content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
}

.name {
  font-weight: 900;
  font-size: 27px;
}

.status,
.description,
.title {
  display: block;
  font-size: 18px;
  font-weight: 500;
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
}
</style>
