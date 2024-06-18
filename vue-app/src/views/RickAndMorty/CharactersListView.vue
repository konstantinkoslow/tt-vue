<template>
    <main class="ricknmorty">
      <h1>Rick and Morty characters</h1>
      <p v-if="error">Something went wrong...</p>
      <p v-if="loading">Loading...</p>
      <ul v-else>
        <li v-for="character in result.characters.results" :key="character.id">
          <router-link :to="'/ricknmorty/' + character.id">{{ character.name }}</router-link>
        </li>
      </ul>
    </main>
  </template>
  
  <script>
  import gql from 'graphql-tag'
  import { useQuery } from '@vue/apollo-composable'
  
  const CHARACTERS_QUERY = gql`
    query Characters {
      characters {
        results {
          id
          name
          image
        }
      }
    }
  `
  
  export default {
    name: 'CharactersListView',
    setup() {
      const { result, loading, error } = useQuery(CHARACTERS_QUERY);
  
      return {
        result,
        loading,
        error
      }
    }
  }
  </script>
  