<template>
  <div class="selectDiv">
    <select :id="name" v-model="langSelected" :aria-label="$t('accessibility.selectLang')">
      <option
        :lang="lang.abbr"
        :value="lang.abbr"
        v-for="lang in langs"
        :key="lang.abbr"
      >{{ lang.text }}</option>
    </select>
  </div>
</template>

<script>
import { mapActions, mapGetters, mapMutations } from "vuex";

export default {
  props: ["name"],
  static() {
    return {
      langs: [
        {
          abbr: "fr",
          text: "Français"
        } /* ,
        {
          abbr: "it",
          text: "Italiano"
        },
        {
          abbr: "rm",
          text: "Rumansch"
        },
        {
          abbr: "de",
          text: "Deutsch"
        } */
      ]
    };
  },
  computed: {
    langSelected: {
      get() {
        if (Object.values(this.langs).indexOf(this.$store.state.lang) > -1) {
          return this.$store.state.lang;
        } else {
          return this.langs[0].abbr;
        }
      },
      set(value) {
        this.setLang(value).then(() => {
          this.loadImages();
          this.RESET_GAME();
          this.SET_GAME_STATE();
          this.RESET_ROUND();
          this.RESET_ROUND_DONE();
          this.INIT_CHRONO();
        });
      }
    }
  },
  methods: {
    ...mapActions(["setLang", "loadImages"]),
    ...mapMutations([
      "RESET_GAME",
      "SET_GAME_STATE",
      "RESET_ROUND",
      "RESET_ROUND_DONE",
      "INIT_CHRONO"
    ])
  }
};
</script>

<style lang="postcss" scoped>
select {
  @apply text-white;
  background: none;
  border: none;
  appearance: none;
  box-sizing: border-box;
  line-height: 1.3;
  padding-right: 1rem;
}
.selectDiv {
  position: relative;
}

.selectDiv::after {
  position: absolute;
  content: "";
  width: 1.4rem;
  height: 0.7rem;
  background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23FFF%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E");
  background-repeat: no-repeat;
  background-position: right 0px top 80%;
  background-size: 0.6rem auto;
  right: 0;
  top: 0.4rem;
  pointer-events: none;
}

select::-ms-expand {
  display: none;
}

select option {
  background-color: rgba(255, 0, 90, 1);
}
</style>
