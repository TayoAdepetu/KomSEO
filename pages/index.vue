<template>
  <v-row class="primary" justify="center" align="center">
    <div>
      <div>
        <v-card-title class="headline">
          Create Search Engine Optimized Content and Images With GPT-4
        </v-card-title>
        <br />

        <div class="">
          <form @submit.prevent="generateText">
            <v-combobox
              v-model="keywords"
              :search-input.sync="search"
              hide-selected
              hint="Maximum of 5 keywords"
              label="Add some keywords. Press Enter after each keyword to add to the keyword array."
              multiple
              persistent-hint
              small-chips
              style="
                border: solid white 2px;
                padding: 10spx;
                min-width: 100%;
                display: block;
                background-color: #353839;
              "
            >
            </v-combobox>

            <br />

            <input
              style="
                border: solid white 2px;
                padding: 5px;
                min-width: 100%;
                display: block;
                background-color: white;
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
                background-color: white;
              "
              v-model="title"
              placeholder="Add title of the article"
            ></textarea>

            <br />

            <v-select
              style="
                border: solid white 2px;
                padding: 10spx;
                min-width: 100%;
                display: block;
                background-color: #353839;
              "
              :items="tone_options"
              v-model="tone"
              density="compact"
              label="Select writing tone"
            ></v-select>
            <v-btn
              color=""
              style="
                background-color: white;
                color: black;
                width: 100%;
                margin-top: 15px;
              "
              type="submit"
              block
            >
              Click To Generate SEO Rich Text
            </v-btn>
          </form>
        </div>
        <v-card-text> </v-card-text>
      </div>
    </div>
  </v-row>
</template>

<script>
import OpenAI from "openai";

export default {
  data() {
    return {
      openaiAPIKey: process.env.VUE_APP_OPENAI_API_KEY,
      items: [],
      search: null,
      words: null,
      keywords: [],
      title: null,
      tone: null,
      tone_options: [
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
        apiKey: this.openaiAPIKey,
        dangerouslyAllowBrowser: true,
      });

      const wordss = this.words;
      const keywordss = this.keywords.join(", ");
      const topic = this.title;
      const tonee = this.tone; // Fix: Change `tone` to `tonee`

      const chatResponse = await openai.chat.completions.create({
        messages: [
          {
            role: "user",
            content: `You are an expert content creator, in a/an ${tonee} tone, create a ${wordss} words SEO-optimized content about ${topic}, and include these keywords in the content: ${keywordss}.`,
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
