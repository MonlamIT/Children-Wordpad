<template>
  <div>
    <div v-show="!profileModeOn" class="editor">
      <q-editor
        v-model="editor"
        :placeholder="$t('Type or paste your text here')"
      />
    </div>

    <div v-show="profileModeOn" class="profiledContent">
      <Word v-for="(word, idx) in contentWordsLevel" :key="idx" :word="word" />
    </div>
  </div>
</template>

<script>
import { mapGetters, mapState } from "vuex";

import Word from "components/Word";

export default {
  name: "Editor",

  components: {
    Word,
  },

  computed: {
    ...mapState("editor", ["content"]),
    ...mapState("profiler", ["contentWordsLevel", "profileModeOn"]),
    ...mapGetters("editor", ["contentInnerText"]),

    editor: {
      get() {
        return this.content;
      },
      set(value) {
        this.$store.dispatch("editor/updateContent", value);
      },
    },

  },

  created() {
    this.$store.dispatch("profiler/setupWordLists");
  },
};
</script>

<style lang="scss">

.editor {
  max-width: 1080px;
  margin: auto;
  font-size: 2rem;
  line-height: 1.7;
  overflow: auto;
  font-family: "Monlam Uni Ochan1";
}

.profiledContent {
  max-width: 1080px;
  margin-left: 100px;
  margin-right: 50px;
  padding: 20px;
  font-size: 2rem;
  font-family: "Monlam Uni Ochan1";
}

</style>
