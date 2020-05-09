<template>
  <div>
    <b-form inline class="mb-2">
      <b-form-input
        class="w-75 mr-3"
        v-model="query"
        @keydown.enter="search()"
      />
      <b-button variant="primary" @click="search()">検索</b-button>
    </b-form>
    <b-form inline class="mb-2">
      <label class="mr-2">周回数</label>
      <b-form-input class="w-25 mr-2" size="sm" disabled :value="numLaps" />
      <b-button class="mr-2" variant="light" @click="numLaps += 1">+</b-button>
      <b-button class="mr-2" variant="light" @click="numLaps -= 1">-</b-button>
    </b-form>
    <MonsterDetail
      class="mb-2"
      v-for="monster in monsterList"
      :key="monster.id"
      :monster="monster"
      :individual-value="individualValue"
    />
  </div>
</template>

<script>
import pokemons from "@/assets/pokemons.json";
import MonsterDetail from "@/components/MonsterDetail";

export default {
  components: {
    MonsterDetail
  },
  data() {
    return {
      query: "",
      monsterList: [],
      numLaps: 1
    };
  },
  methods: {
    search() {
      this.monsterList = pokemons.filter(monster => monster.name == this.query);
    }
  },
  computed: {
    individualValue() {
      if (this.numLaps < 8) {
        return (this.numLaps - 1) * 4;
      } else {
        return 31;
      }
    }
  },
  watch: {
    numLaps() {
      if (this.numLaps <= 0) {
        this.numLaps = 1;
      }
    }
  }
};
</script>
