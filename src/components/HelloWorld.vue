<template>
  <div class="container">
    <div class="row">
      <h3 class="mx-auto">Github Repo Compare</h3>
    </div>

    <div class="row">
      <div class="col">
        <input
          @keyup.enter="searchRepo"
          v-model="form.reponame"
          type="text"
          class="form-control"
          placeholder="Enter name of repository to compare"
        >

        <ul class="list-group">
          <li v-for="repo in searchResults" :key="repo.id" class="list-group-item">
            {{repo.full_name}} ({{repo.stargazers_count}})
            <button
              v-if="reposToCompare.map(repoToCompare => repoToCompare.id).includes(repo.id)"
              @click="remove(repo)"
              class="btn btn-sm btn-outline-danger"
            >Remove</button>

            <button
              v-else
              @click="addToCompare(repo)"
              class="btn btn-sm btn-outline-success"
            >Add to compare</button>
          </li>
        </ul>
      </div>

      <!-- results -->
      <div class="col">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Name</th>
              <th>Stars</th>
              <th>Watch</th>
              <th>Forks</th>
              <th>Activity</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="repo in reposToCompare" :key="repo.id">
              <td>{{repo.full_name}}</td>
              <td>{{repo.stargazers_count}}</td>
              <td>{{repo.watchers_count}}</td>
              <td>{{repo.forks_count}}</td>
              <td>{{repo.updated_at | fromnow}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Moment from 'moment'
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  filters:{
    fromnow(date) {
      return (new Moment(date)).fromNow();
    }
  },

  data() {
    return {
      form: {
        reponame: ""
      },
      searchResults: [],
      reposToCompare: []
    };
  },

  methods: {
    searchRepo() {
      let apiUrl = "https://api.github.com/search/repositories?q=";

      apiUrl = apiUrl + this.form.reponame;

      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          this.searchResults = data.items;
        });
    },

    addToCompare(repo) {
      this.reposToCompare.push(repo);
    },

    remove(repo) {
      const indexOfRepoToRemove = this.reposToCompare.indexOf(repo);

      this.reposToCompare.splice(indexOfRepoToRemove, 1);
    }
  }
};
</script>


