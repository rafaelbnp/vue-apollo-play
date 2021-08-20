<template>
  <div v-if="loading">Loading...</div>

  <div v-else-if="error">Error: {{ error.message }}</div>

  <ul v-if="characters">
    <li v-for="{ id, name } of characters" :key="id">
      {{ name }}
    </li>
  </ul>
</template>

<script>
import { useQuery } from '@vue/apollo-composable';
import { computed } from 'vue';
import gql from 'graphql-tag';

export default {
  setup() {
    const { result, loading, error } = useQuery(gql`
      query getCharacters {
        characters(page: 1, filter: { name: "rick" }) {
          results {
            id
            name
          }
        }
      }
    `);

    const characters = computed(() => result.value?.characters.results);

    return { characters, loading, error };
  }
};
</script>

<style></style>
