<template>
    <div v-if="character">
      <h1>{{ character.name }}</h1>
      <img :src="character.image" :alt="character.name">
      <p>Status: {{ character.status }}</p>
      <p>Species: {{ character.species }}</p>
      <p>Type: {{ character.type }}</p>
      <p>Gender: {{ character.gender }}</p>
      <p>Origin: {{ character.origin.name }}</p>
      <p>Last Known Location: {{ character.location.name }}</p>
    </div>
    <p v-else>No character selected</p>
  </template>
  
  <script>
  import gql from 'graphql-tag'
  import { useQuery } from '@vue/apollo-composable'
  import { computed } from 'vue'
  import { useRouter, useRoute } from 'vue-router'
  
  const CHARACTER_QUERY = gql`
    query Character($id: ID!) {
      character(id: $id) {
        id
        name
        status
        species
        type
        gender
        origin {
          name
        }
        location {
          name
        }
        image
        # Weitere benötigte Eigenschaften hier hinzufügen
      }
    }
  `
  
  export default {
    name: 'CharacterDetailView',
    setup() {
      const router = useRouter()
      const route = useRoute()
      const characterId = computed(() => route.params.id)
  
      const { result, loading, error } = useQuery(CHARACTER_QUERY, () => ({
        id: characterId.value
      }));
  
      const character = computed(() => result.value ? result.value.character : null)
  
      return {
        character,
        loading,
        error
      }
    }
  }
  </script>
  