<template>
  <b-card class="bg-light">
    <b-row>
      <b-col cols="4">ID</b-col>
      <b-col>{{ monster.ID }}</b-col>
    </b-row>
    <b-row>
      <b-col cols="4">名前</b-col>
      <b-col>{{ monster.name }}</b-col>
    </b-row>
    <b-row>
      <b-col cols="4">タイプ</b-col>
      <b-col>
        <span
          class="type left"
          :class="typeJaEn[type]"
          v-for="type in monster.types"
          :key="type"
        >
          {{ type }}
        </span>
      </b-col>
    </b-row>
    <b-row>
      <b-col cols="4">わざ</b-col>
      <b-col>
        <span class="mr-2 move" v-for="move in monster.moves" :key="move">
          {{ move }}
        </span>
      </b-col>
    </b-row>
    <b-row>
      <b-col cols="4">持ち物</b-col>
      <b-col>{{ monster.item }}</b-col>
    </b-row>
    <b-row>
      <b-col cols="4">性格</b-col>
      <b-col>{{ monster.nature }}</b-col>
    </b-row>
    <b-row>
      <b-col cols="4">努力値</b-col>
      <b-col>
        <span class="mr-2" v-for="val in monster.effort_value" :key="val">
          {{ val }}
        </span>
      </b-col>
    </b-row>
    <b-row>
      <b-col cols="4">ステータス</b-col>
      <b-col>{{ status }}</b-col>
    </b-row>
  </b-card>
</template>

<script>
import nature from "@/assets/nature.json";

export default {
  props: { monster: Object, individualValue: Number },
  data() {
    return {
      period: 1,
      nature: nature,
      typeJaEn: {
        ノーマル: "normal",
        ほのお: "fire",
        みず: "water",
        でんき: "electric",
        くさ: "grass",
        こおり: "ice",
        じめん: "ground",
        ひこう: "flying",
        ゴースト: "ghost",
        いわ: "rock",
        かくとう: "fighting",
        どく: "poison",
        エスパー: "psychic",
        むし: "bug",
        あく: "dark",
        はがね: "steel",
        ドラゴン: "dragon"
      }
    };
  },
  methods: {
    calcHP(base, ind, ef) {
      return base * 2 + ind + Number(ef / 4) + 110;
    },
    calcVal(base, ind, ef, corr) {
      return Math.floor((base * 2 + ind + Number(ef / 4) + 5) * corr);
    }
  },
  computed: {
    status() {
      let ret = "";

      let effort = 252;
      if (this.monster.effort_value.length == 3) {
        effort = 170;
      }

      let H;
      if (this.monster.name === "ヌケニン") {
        H = 1;
      } else if (this.monster.effort_value.includes("HP")) {
        H = this.calcHP(
          this.monster.base_stats.H,
          this.individualValue,
          effort
        );
      } else {
        H = this.calcHP(this.monster.base_stats.H, this.individualValue, 0);
      }

      ret = ret + H;

      const arr = ["A", "B", "C", "D", "S"];
      const statusEn = {
        A: "攻撃",
        B: "防御",
        C: "特攻",
        D: "特防",
        S: "素早さ"
      };

      for (const item of arr) {
        const base = this.monster.base_stats[item];
        const ind = this.individualValue;
        const ef = this.monster.effort_value.includes(statusEn[item])
          ? effort
          : 0;
        const corr = nature[this.monster.nature][item];
        ret += "-" + this.calcVal(base, ind, ef, corr);
      }

      return ret;
    }
  }
};
</script>

<style>
@import "../assets/css/types.css";

.move {
  display: inline-block;
}
</style>
