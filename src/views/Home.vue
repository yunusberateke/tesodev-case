<template>
  <div class="home">
    <div class="content">
      <header-logo />
      <div class="search-area">
        <div class="input-and-results">
          <search-input
            :searchQuery.sync="searchQuery"
            :error.sync="error"
            :errorMessage="errorMessage"
          />

          <div class="results" v-if="this.$route.path.startsWith('/search/')">
            <result-card
              v-for="(result, key) in filteredList"
              :key="key"
              :title="`${result[4]} - ${result[5]}`"
              :date="`${result[0]} - ${result[3].split('/')[2]}`"
              :email="result[2]"
            />

            <div class="more">
              <a @click="more"> Show more... </a>
            </div>
          </div>
        </div>
        <custom-button placeHolder="Search" @search="redirectSearch" />
      </div>
    </div>
  </div>
</template>

<script>
import HeaderLogo from "../components/HeaderLogo/HeaderLogo.vue";
import CustomButton from "../components/CustomButton/CustomButton.vue";
import SearchInput from "../components/SearchInput/SearchInput.vue";
import ResultCard from "../components/ResultCard/ResultCard.vue";
import mockData from "../assets/mockData.json";

export default {
  name: "Home",
  components: {
    HeaderLogo,
    CustomButton,
    SearchInput,
    ResultCard,
  },
  data() {
    return {
      error: false,
      errorMessage: null,
      searchQuery: this.$route.params.query,
      mockData: mockData.data,
      filteredList: [],
    };
  },

  methods: {
    redirectSearch() {
      if (this.searchQuery == null || this.searchQuery == "") {
        this.error = true;
        this.errorMessage = "Bu alan boş bırakılamaz.";

        return;
      }

      // Aynı aramayı yapmasın diye ufak önlem
      if (this.$route.path != `/search/${this.searchQuery}/`) {
        this.$router.push(`/search/${this.searchQuery}/`);
      }
    },

    more() {
      if (
        this.$route.path !=
        `/search/${this.searchQuery}/page/1/orderBy/name_asc`
      ) {
        this.$router.push(
          `/search/${this.searchQuery}/page/1/orderBy/name_asc`
        );
      }
    },

    findByNameAndSurname() {
      if(this.$route.path == "/")
        return
      var filteredList = this.mockData.filter((item) => {
        return item[0].toLowerCase().includes(this.searchQuery.toLowerCase());
      });

      if (filteredList.length > 3) {
        filteredList = filteredList.slice(0, 3);
      }

      this.filteredList = filteredList;
    },
  },

  created() {
    this.findByNameAndSurname();
  },

  watch: {
    "$route.path"() {
      this.findByNameAndSurname();
    },
  },
};
</script>

<style  scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 100vw;
  min-height: 100vh;
}

.home .content {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.home .content .search-area {
  display: flex;
  margin-top: 44px;
}

.home .content .search-area .input-and-results .results {
  margin-top: 18px;
  height: 258px;
  border: 1px solid #484848;
  border-radius: 4px;
  padding: 18px 32px;
  position: relative;
}

.home .content .search-area .input-and-results .results .more {
  position: absolute;
  bottom: 10px;
  left: 0;
  right: 0;
  text-align: center;
}

a {
  color: black;
  text-decoration: black;
  font-weight: bold;
  font-size: 12px;
  line-height: 14px;
}

a:hover {
  cursor: pointer;
}
</style>