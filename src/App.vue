<template>
  <div class="app">
    <MainHeader title="The Anime Quoter" />

    <QuoteItem :quote="quote" v-if="quote.content" />
    <div v-else-if="error" class="other">{{ error }}</div>
    <div v-else class="other">Loading...</div>

    <div class="button-container">
      <button :disabled="error" type="button" @click="getQuote()">Randome Quote</button>
    </div>

    <QuotesList :quotes="quotes" />
  </div>
</template>

<script>
  import MainHeader from '@/components/MainHeader.vue'
  import QuoteItem from '@/components/QuoteItem.vue'
  import QuotesList from '@/components/QuotesList.vue'

  export default {
    name: 'App',
    components: {
      MainHeader,
      QuoteItem,
      QuotesList
    },
    data() {
      return {
        quote: {},
        quotes: [],
        error: ''
      }
    },
    methods: {
      async getQuote() {
        if (this.quote.content) {
          this.quotes.push(this.quote)
        }

        await fetch('https://animechan.vercel.app/api/random')
          .then(response => response.json())
          .then(data => {
            this.quote = {
              content: data.quote,
              character: data.character,
              anime: data.anime
            }
          })
          .catch(err => {
            this.error = 'Something went wrong, please try again later!'
          })
      }
    },
    created() {
      this.getQuote()
    }
  }
</script>

<style lang="scss">
:root {
  --primary: #D81E5B;
  --secondary: #8A4FFF;
  --tertiary: #32CBFF;
  --dark: #131A26;
  --light: #EEE;
  --grey: #848484;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}

.button-container {
  display: flex;
  justify-content: center;
  padding: 0px 32px;
  margin: 64px auto;

  button {
    border: none;
    outline: none;
    background-color: var(--primary);
    padding: 16px 32px;
    border-radius: 99px;
    color: var(--light);
    font-size: 1.5em;
    font-weight: 700;
    text-transform: uppercase;
    cursor: pointer;
    transition: 0.4s;

    &:hover {
      background-color: var(--secondary);
    }

    &:disabled {
      opacity: 0.6;
      cursor: not-allowed;
    }

    &:disabled:hover {
      background-color: var(--primary);
    }
  }
}

.other {
    text-align: center;
    font-size: 2rem;
    margin-top: 60px;
}
</style>