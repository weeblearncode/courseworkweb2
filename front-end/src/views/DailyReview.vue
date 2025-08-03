<template>
  <div class="daily-review">
    <h1>Daily Review</h1>
    
    <div v-if="currentWord" class="review-card ui card">
      <div class="content">
        <div class="header">
          <i class="united kingdom flag"></i> English: {{ currentWord.english }}
        </div>
        <div class="meta">
          <i class="germany flag"></i> German: {{ showTranslation ? currentWord.german : '???' }}
        </div>
        <div class="meta">
          <i class="japan flag"></i> Japanese: {{ showTranslation ? currentWord.japanese : '???' }}
        </div>
      </div>
    </div>
    
    <div class="controls">
      <button 
        class="ui primary button" 
        @click="toggleTranslation"
      >
        {{ showTranslation ? 'Hide Translation' : 'Show Translation' }}
      </button>
      <button 
        class="ui positive button" 
        @click="getRandomWord"
      >
        Next Word
      </button>
    </div>
    
    <div v-if="words.length === 0" class="ui warning message">
      No words available. Add some words first.
    </div>
  </div>
</template>

<script>
import { api } from '../helpers/helpers';

export default {
  name: 'DailyReview',
  data() {
    return {
      words: [],
      currentWord: null,
      showTranslation: false
    };
  },
  async mounted() {
    this.words = await api.getWords();
    if (this.words.length > 0) {
      this.getRandomWord();
    }
  },
  methods: {
    getRandomWord() {
      this.showTranslation = false;
      this.currentWord = this.words[Math.floor(Math.random() * this.words.length)];
    },
    toggleTranslation() {
      this.showTranslation = !this.showTranslation;
    }
  }
};
</script>

<style scoped>
.daily-review {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.review-card {
  width: 100%;
  margin: 20px auto;
  padding: 20px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 20px;
}

.ui.card .content .header {
  font-size: 1.3em;
  margin-bottom: 15px;
}

.ui.card .content .meta {
  font-size: 1.1em;
  margin-bottom: 10px;
}
</style>