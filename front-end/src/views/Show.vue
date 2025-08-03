<template>
  <div>
    <h1>Show Word</h1>
    <div class="ui labeled input fluid">
      <div class="ui label">
        <i class="germany flag"></i> German
      </div>
      <input type="text" readonly v-model="word.german" />
    </div>

    <div class="ui labeled input fluid">
      <div class="ui label">
        <i class="united kingdom flag"></i> English
      </div>
      <input type="text" readonly v-model="word.english" />
    </div>

        <div class="ui labeled input fluid">
      <div class="ui label">
        <i class="japan flag"></i> Japanese
      </div>
      <input type="text" readonly v-model="word.japanese" />
    </div>

    <router-link :to="{ name: 'edit', params: { id: word._id }}" class="ui button">
      Edit word
    </router-link>
  </div>
</template>

<script>
import { api } from '../helpers/helpers';

export default {
  name: 'show',
  data: function() {
    return {
      word: {}
    };
  },
  async mounted() {
    this.word = await api.getWord(this.$route.params.id);
  }
};
</script>