<template>
  <div>
    <h1>Vocabulary Test</h1>
    <div v-if="currentWord" class="test-container">
      <div class="ui segment">
        <h3>Translate this word:</h3>
        <div class="ui large label">
          <i :class="questionIcon"></i> {{ questionLanguage }}: {{ questionWord }}
        </div>
        
        <div class="ui form">
          <div class="field">
            <label>Your answer in <i :class="answerIcon"></i> {{ answerLanguage }}:</label>
            <input 
              type="text" 
              v-model="userAnswer" 
              @keyup.enter="checkAnswer"
              ref="answerInput"
            />
          </div>
          <button class="ui primary button" @click="checkAnswer">Check Answer</button>
        </div>
        
        <div v-if="showResult" class="result-message">
          <div v-if="isCorrect" class="ui positive message">
            Correct! Well done!
          </div>
          <div v-else class="ui negative message">
            Incorrect. The correct answer is: {{ correctAnswer }}
          </div>
          <button class="ui button" @click="nextQuestion">Next Question</button>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Loading words...</p>
    </div>
  </div>
</template>

<script>
import { api } from '../helpers/helpers';

export default {
  name: 'test',
  data() {
    return {
      words: [],
      currentWord: null,
      questionLanguage: '',
      answerLanguage: '',
      questionWord: '',
      correctAnswer: '',
      userAnswer: '',
      showResult: false,
      isCorrect: false,
      languages: ['english', 'german', 'japanese'],
      languageIcons: {
        english: 'united kingdom flag',
        german: 'germany flag',
        japanese: 'japan flag'  
      }
    };
  },
  computed: {
    questionIcon() {
      return this.languageIcons[this.questionLanguage] || '';
    },
    answerIcon() {
      return this.languageIcons[this.answerLanguage] || '';  
    }
  },
  async mounted() {
    this.words = await api.getWords();
    if (this.words.length > 0) {
      this.setupQuestion();
    } else {
      this.flash('No words available for testing', 'error');
    }
  },
  methods: {
    setupQuestion() {
      this.currentWord = this.words[Math.floor(Math.random() * this.words.length)];
      
      const langIndex = Math.floor(Math.random() * this.languages.length);
      this.questionLanguage = this.languages[langIndex];
      
      let answerIndex;
      do {
        answerIndex = Math.floor(Math.random() * this.languages.length);
      } while (answerIndex === langIndex);
      this.answerLanguage = this.languages[answerIndex];
      
      this.questionWord = this.currentWord[this.questionLanguage];
      this.correctAnswer = this.currentWord[this.answerLanguage];
      
      this.userAnswer = '';
      this.showResult = false;
      this.isCorrect = false;
      
      this.$nextTick(() => {
        this.$refs.answerInput.focus();
      });
    },
    checkAnswer() {
      if (!this.userAnswer.trim()) {
        this.flash('Please enter an answer', 'error');
        return;
      }
      
      this.showResult = true;
      this.isCorrect = this.userAnswer.toLowerCase().trim() === this.correctAnswer.toLowerCase().trim();
      
      if (this.isCorrect) {
        this.flash('Correct answer!', 'success');
      } else {
        this.flash('Incorrect answer', 'error');
      }
    },
    nextQuestion() {
      this.setupQuestion();
    }
  }
};
</script>

<style scoped>
.test-container {
  max-width: 600px;
  margin: 0 auto;
}

.result-message {
  margin-top: 20px;
}

.ui.form .field {
  margin-bottom: 15px;
}

.ui.button {
  margin-top: 10px;
}
</style>