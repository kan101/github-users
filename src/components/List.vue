<template>
  <section class="list">
    <div>
      <input
        type="text"
        v-model="searchTerm"
        @keyup="getUsers"
        class="search"
        placeholder="Search Users"
      />
    </div>
    <section class="results">
      <div v-for="(result, r) in results" :key="r" class="card">
        <article class="avatar-wrapper">
          <img :src="result.avatarUrl" class="avatar" alt="User Image" />
        </article>
        <article class="info">
          <div>
            <span>Name:</span>
            <span>{{ result.name }}</span>
          </div>
          <div>
            <span>login:</span>
            <span>{{ result.login }}</span>
          </div>
          <div>
            <span>Email:</span>
            <span>{{ result.email }}</span>
          </div>
          <div>
            <span>Company:</span>
            <span>{{ result.company }}</span>
          </div>
          <div>
            <span>Location:</span>
            <span>{{ result.location }}</span>
          </div>
        </article>
      </div>
    </section>
  </section>
</template>

<script lang="ts">
import axios from 'axios'
import debounce from 'lodash.debounce'

export default {
  name: 'List',
  data(): unknown {
    return {
      searchTerm: '',
      results: [],
    }
  },
  components: {},
  methods: {
    getUsers: debounce(function () {
      if (this.searchTerm !== '') {
        const headers = {
          'Content-Type': 'application/json',
          Authorization: 'Bearer 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',
        }

        axios
          .post(
            'https://api.github.com/graphql',
            {
              query: `query searchUsers {
                        search(query: "${this.searchTerm}", type: USER, first: 10) {
                            nodes {
                            ... on User {
                                email
                                login
                                location
                                name
                                company
                                avatarUrl
                            }
                            }
                        }
                    }`,
            },
            { headers: headers }
          )
          .then((response) => {
            console.log(response)
            this.results = response.data.data.search.nodes
            this.results.shift()
          })
          .catch(function (error) {
            console.log(error)
          })
      }
    }, 1000),
  },
}
</script>

<style lang="scss" scoped>
.search {
  height: 31px;
  border-radius: 5px;
  display: block;
  padding: 0.375rem 0.75rem;
  font-size: 16px;
  color: #495057;
  border: 1px solid #ced4da;
  margin-bottom: 30px;
}

.results {
  .card {
    display: flex;
    border: 1px solid rgba(0, 0, 0, 0.125);
    border-radius: 3px;
    margin-bottom: 30px;
    flex-wrap: wrap;
    padding: 5px;

    .avatar {
      max-width: 215px;
      margin-right: 10px;
      border-radius: 5px;
    }

    .info {
      padding-top: 42px;
      font-family: 'Nunito', sans-serif;

      div {
        margin-bottom: 10px;
        text-transform: capitalize;
        display: flex;

        span {
          &:first-child {
            font-weight: bold;
            min-width: 111px;
            margin-right: 10px;
            font-size: 15px;
            display: inline-block;
            color: #777;
          }
        }
      }
    }
  }
}
</style>
