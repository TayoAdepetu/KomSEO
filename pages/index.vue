<template>
  <v-row class="primary" justify="center" align="center">
    <div>
      <div>
        <v-card-title class="headline">
          Create Search Engine Optimized Content and Images With GPT-4
        </v-card-title>
        <br />

        <div class="">
          <form>
            <v-combobox
              v-model="keywords"
              :search-input.sync="search"
              hide-selected
              hint="Maximum of 5 keywords"
              label="Add some keywords"
              multiple
              persistent-hint
              small-chips
              class="first"
            >
            </v-combobox>

            <br />

            <input
              style="
                border: solid white 2px;
                padding: 5px;
                min-width: 100%;
                display: block;
                color: white;
              "
              type="number"
              placeholder="Add number of words"
              v-model="words"
            />

            <br />

            <textarea
              style="
                border: solid white 2px;
                padding: 5px;
                min-width: 100%;
                display: block;
                margin-top: 10px;
                color: white;
              "
              v-model="title"
              placeholder="Add title of the article"
            ></textarea>

            <br />

            <v-select
              :items="tone"
              density="compact"
              label="Select writing tone"
            ></v-select>
          </form>
        </div>
        <v-card-text> </v-card-text>
        <v-card-actions>
          <v-btn color="primary" nuxt to="/writepilot"> Continue </v-btn>
        </v-card-actions>
      </div>
    </div>
  </v-row>
</template>

<script>
// import OpenAI from "openai";

export default {
  data() {
    return {
      items: [],
      search: null,
      words: null,
      keywords: [],
      title: null,
      tone: [
        "narrative",
        "authoritative",
        "sad",
        "emotional",
        "inspiring",
        "professional",
        "happy",
      ],
    };
  },

  methods: {
    async generateText() {
      const openai = new OpenAI({
        apiKey: "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",
      });

      const wordss = this.words;
      const keywordss = this.keywords.join(", ");
      const topic = this.title;

      const chatResponse = await openai.chat.completions.create({
        messages: [
          {
            role: "user",
            content: `You are an expert content creator, in a/an ${tone} tone, create a ${wordss} words SEO-optimized content about ${topic}, and include these keywords in the content: ${keywordss}.`,
          },
        ],

        model: "gpt-3.5-turbo",
      });

      console.log(chatResponse);
    },
  },

  watch: {
    keywords(val) {
      if (val.length > 5) {
        this.$nextTick(() => this.keywords.pop());
      }
    },
  },
};
</script>
<style>
.first:active {
  border: 2px solid, white;
}
</style>
