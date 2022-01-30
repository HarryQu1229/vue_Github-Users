<template>
  <div class="wrap">
    <div class="search">
      <input
        type="text"
        class="searchTerm"
        placeholder="Who are you looking for?"
        v-model="search"
        @keyup.enter="getUsers"
      />
      <button type="submit" class="searchButton" @click="getUsers">
        <div class="search icon"></div>
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      search: "",
    };
  },
  methods: {
    getUsers() {
      this.$bus.$emit("updateData", {
        isWelcome: false,
        isLoading: true,
        users: [],
        errorMsg: "",
      });
      axios.get(`https://api.github.com/search/users?q=${this.search}`).then(
        (response) => {
          // if a resolved promise is returned by axios
          this.$bus.$emit("updateData", {
            isLoading: false,
            users: response.data.items,
            errorMsg: "",
          });
        },
        // if a rejected promise is returned by axios
        (error) => {
          this.$bus.$emit("updateData", {
            isLoading: false,
            users: [],
            errorMsg: error,
          });
        }
      );
    },
  },
};
</script>

<style scoped>
.wrap {
  width: 30%;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.search {
  width: 100%;
  position: relative;
  display: flex;
}

.searchTerm {
  width: 100%;
  border: 3px solid #068884;
  border-right: none;
  padding: 5px 15px;
  height: 40px;
  border-radius: 5px 0 0 5px;
  outline: none;
  font-size: 20px;
  color: #9dbfaf;
}

.searchTerm:focus {
  color: #008a85;
}

.searchButton {
  width: 90px;
  border: 3px solid #068884;
  border-left: none;
  background: #07c3bd;
  text-align: center;
  color: #fff;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-size: 24px;
}

.search.icon {
  color: white;
  position: absolute;
  margin-top: -9px;
  margin-left: 19px;
  width: 13px;
  height: 13px;
  border: solid 1px currentColor;
  border-radius: 100%;
  transform: rotate(-45deg);
}

.search.icon:before {
  content: "";
  position: absolute;
  top: 12px;
  left: 5px;
  height: 6px;
  width: 1px;
  background-color: currentColor;
}
</style>
