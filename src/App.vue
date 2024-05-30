<template>
  <section class="container">
    <h1>The Rick and Morty</h1>
    <form class="filters" @submit.prevent="searchCharacters">
      <input class="filters__input" type="text" name="name" placeholder="enter a name">
      <input class="filters__input" type="text" name="status" placeholder="enter a status">
      <button class="filters__button" type="submit">Search</button>
    </form>
    <div class="cards">
      <transition-group name="bounce">
        <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
      </transition-group>
      <span v-if="errorState">There are no results for this query</span>
    </div>
    <div class="pagination">
      <button class="pagination__left" type="button"
       @click="getCharacters(prevPage)"
       :disabled="leftPaginationDisabledState">prev</button>
      <button class="pagination__right" type="button"
       @click="getCharacters(nextPage)"
       :disabled="rightPaginationDisabledState">next</button>
    </div>
  </section>
</template>

<script>
import CharacterCard from '@/components/CharacterCard.vue'

export default {
  name: 'App',
  components: {
    CharacterCard
  },
  data: () => ({
    url: 'https://rickandmortyapi.com/api/character',
    nextPage: null,
    prevPage: null,
    characters: [],
    errorState: false 
  }),
  methods: {
    changeData(nextPage, prevPage, characters) {
      this.nextPage = nextPage
      this.prevPage = prevPage
      this.characters = characters
    },
    getCharacters(url) {
      fetch(url)
      .then(response => {
        if (response.ok) {
          this.errorState = false
          return response.json()
        }
        throw Error
      })
      .then(json => {
        this.changeData(json.info.next, json.info.prev, json.results)
      })
      .catch(() => {
        this.errorState = true
        this.changeData(null, null, [])
      })
    },
    searchCharacters({target}) {
      const data = new FormData(target)
      const name = data.get('name')
      const status = data.get('status')
      this.getCharacters(this.url + `/?name=${name}&status=${status}`)
    }
  },
  computed: {
    leftPaginationDisabledState() {
      if (this.prevPage === null) {
        return true
      }
      return false
    },
    rightPaginationDisabledState() {
      if (this.nextPage === null) {
        return true
      }
      return false
    }
  },
  created() {
    this.getCharacters(this.url)
  }
}
</script>

<style>
*,
::after,
::before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: "Roboto", Arial, sans-serif;
  font-weight: 500;
  font-size: 16px;
  color: #ffffff;
  background-color: #202329;
  min-height: 100vh;
}

.bounce-enter-active {
  animation: bounce-in 0.4s;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}

.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 80px 120px;

  @media (max-width: 1440px) {
    width: fit-content;
    padding: 60px 80px;
  }

  @media (max-width: 768px) {
    width: 320px;
    padding: 40px 20px;
  }
}

h1 {  
  font-weight: 900;
  font-size: 62px;
  text-align: center;
  margin-bottom: 60px;

  @media (max-width: 1440px) {
    font-size: 52px;
    margin-bottom: 40px;
  }

  @media (max-width: 768px) {
    font-size: 36px;
  }
}

.cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 40px;

  @media (max-width: 1440px) {
    display: flex;
    flex-direction: column;
  }
}

.pagination {
  display: flex;
  justify-content: center;
  gap: 30px;

  @media (max-width: 768px) {
    gap: 15px;
  }
}

.pagination button {
  padding: 10px;
  border: none;
  cursor: pointer;
  color: inherit;
  font-family: inherit;
  font-size: 20px;
  background-color: rgb(60, 62, 68);
  border-radius: 10px;
  display: flex;
  gap: 10px;
  align-items: center;

  @media (max-width: 768px) {
    font-size: 15px;
  }
}

.pagination button:hover {
  background-color: rgb(46, 48, 53);
}

.pagination button[disabled]:hover {
  background-color: rgb(60, 62, 68);
}

.pagination button[disabled] {
  opacity: 0.2;
  cursor: default;
}

.pagination__left::before {
  content: '';
  display: block;
  width: 0;
  height: 0;
  border-top: 10px solid transparent;
  border-right: 20px solid #ffffff;
  border-bottom: 10px solid transparent;

  @media (max-width: 768px) {
    border-top: 7px solid transparent;
    border-right: 14px solid #ffffff;
    border-bottom: 7px solid transparent;
  }
}

.pagination__right::after {
  content: '';
  display: block;
  width: 0;
  height: 0;
  border-top: 10px solid transparent;
  border-left: 20px solid #ffffff;
  border-bottom: 10px solid transparent;

  @media (max-width: 768px) {
    border-top: 7px solid transparent;
    border-right: 14px solid #ffffff;
    border-bottom: 7px solid transparent;
  }
}

.filters {
  display: flex;
  margin-bottom: 30px;
  width: 600px;
  gap: 20px;

  @media (max-width: 768px) {
    width: 100%;
    flex-direction: column;
    gap: 10px;
  }
}

.filters__input {
  display: block;
  padding: 10px 15px;
  background-color: rgb(60, 62, 68);
  border-radius: 10px;
  border: none;
  color: #ffffff;
  font-family: inherit;
  font-size: 16px;

  @media (max-width: 768px) {
    width: 100%;
    font-size: 14px;
  }
}

.filters__input:hover {
  background-color: rgb(46, 48, 53);
}

.filters__input:focus {
  background-color: rgb(46, 48, 53);
  outline: none;
}

.filters__button {
  color: #202329;
  border: none;
  padding: 10px 20px;
  background-color: rgb(170, 170, 170);
  border-radius: 10px;
  font-family: inherit;
  font-size: 16px;
  cursor: pointer;
  
  @media (max-width: 768px) {
    font-size: 14px;
  }
}

.filters__button:hover {
  background-color: rgb(60, 62, 68);
  color: #b8b8b8;
}
</style>
