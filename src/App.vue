<template>
    <v-container>
      <h1>Text Tags Component Demo</h1>
      <TextList :tags="tags" alignment="left" />
      <TextList :tags="tags" alignment="justify" />
      <div style="display: flex; width: 100%;">
        <TextList :tags="tags" alignment="left" style="width: 50%;" />
        <TextList :tags="tags" alignment="justify" style="width: 50%;" />
      </div>
    </v-container>
</template>

<script>
import TextList from './components/TextList';
import {VContainer} from "vuetify/lib/components";

export default {
  name: 'App',
  components: {
    TextList, VContainer
  },
  data() {
    return {
      tags: [
        { text: 'текст1' },
        { text: 'текст2', icon: 'mdi-star' },
        { text: 'текст3' },
        { text: 'текст4', icon: 'mdi-heart' }
      ]
    };
  }
};
</script>
