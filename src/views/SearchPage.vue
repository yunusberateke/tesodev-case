<template>
  <div class="containerSearch">
    <div class="header">
      <img src="../assets/tesodev_logo.png" width="200" />
      <search-input
        :searchQuery.sync="searchQuery"
        :error.sync="error"
        :errorMessage="errorMessage"
      ></search-input>
      <custom-button placeHolder="Search" @search="search"></custom-button>
    </div>
    <div class="results" v-if="this.$route.path.startsWith('/search/')">
      <div class="orderByRow">
        <div class="orderBy">
          <order-icon /> <span>Order By</span>
          <div class="dropdown-content">
            <p @click="orderBy = 'name_asc'">Name ascending</p>
            <p @click="orderBy = 'name_des'">Name descending</p>
            <p @click="orderBy = 'year_asc'">Year ascending</p>
            <p @click="orderBy = 'year_des'">Year descending</p>
          </div>
        </div>
      </div>
      <result-card
        v-for="(result, key) in showList"
        :key="key"
        :title="`${result[4]} - ${result[5]}`"
        :date="`${result[0]} - ${result[3].split('/')[2]}`"
        :email="result[2]"
      />

      <paginate
        :currentPaginate.sync="currentPaginate"
        :paginateCount="paginateCount"
      />
    </div>
  </div>
</template>

<script>
import CustomButton from "../components/CustomButton/CustomButton.vue";
import ResultCard from "../components/ResultCard/ResultCard.vue";
import SearchInput from "../components/SearchInput/SearchInput.vue";
import OrderIcon from "../components/OrderIcon.vue";
import Paginate from "../components/Paginate/Paginate.vue";
import mockData from "../assets/mockData.json";

export default {
  components: { SearchInput, CustomButton, ResultCard, OrderIcon, Paginate },
  computed: {
    paginateCount() {
      return Math.ceil(this.filteredList.length / 6);
    },
    showList() {
      return this.orderList();
    },
  },
  data() {
    return {
      searchQuery: this.$route.params.query,
      error: false,
      errorMessage: null,
      filteredList: [],
      mockData: mockData.data,
      currentPaginate: 1,
      orderBy: "name_asc",
    };
  },
  methods: {
    search() {
      if (this.searchQuery == null || this.searchQuery == "") {
        this.error = true;
        this.errorMessage = "Bu alan boş bırakılamaz.";

        return;
      }

      // Aynı aramayı yapmasın diye ufak önlem
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
      var filteredList = this.mockData.filter((item) => {
        return item[0].toLowerCase().includes(this.searchQuery.toLowerCase());
      });

      this.filteredList = filteredList;
    },

    orderList() {
      var output;
      switch (this.orderBy) {
        case "name_asc":
          output = this.filteredList
            .sort((a,b) => (a[0] > b[0]) ? 1 : ((b[0] > a[0]) ? -1 : 0))
            .slice((this.currentPaginate - 1) * 6, 6 * this.currentPaginate);
          return output;
        case "name_des":
          output = this.filteredList
             .sort((a,b) => (b[0] > a[0]) ? 1 : ((a[0] > b[0]) ? -1 : 0))
            .slice((this.currentPaginate - 1) * 6, 6 * this.currentPaginate);
          return output;
        case "year_asc":
          output = this.filteredList
            .sort((a, b) => {
              return a[3].split("/")[2] - b[3].split("/")[2];
            })
            .slice((this.currentPaginate - 1) * 6, 6 * this.currentPaginate);
          return output;

        case "year_des":
          output = this.filteredList
            .sort((a, b) => {
              return b[3].split("/")[2] - a[3].split("/")[2];
            })
            .slice((this.currentPaginate - 1) * 6, 6 * this.currentPaginate);
          return output;
      }

      this.filteredList = this.filteredList.sort((a, b) => {
        return a[0] - b[0];
      });
    },
  },

  created() {
    this.findByNameAndSurname();
  },

  watch: {
    "$route.path"() {
      this.findByNameAndSurname();
    },
    currentPaginate() {
      this.$route.params.page = this.currentPaginate;
    },
  },
};
</script>

<style scoped>
.orderByRow {
  width: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding-bottom: 40px;
}
.orderBy {
  position: relative;
  display: inline-block;
}

.orderBy span {
  font-weight: bold;
  font-size: 18px;
  line-height: 21px;
}

.orderBy .dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  padding: 12px 16px;
  z-index: 1;
}

.orderBy .dropdown-content p {
  padding: 10px;
  font-weight: bold;
  font-size: 14px;
  line-height: 16px;
}

.orderBy .dropdown-content p:hover {
  background-color: grey;
  color: white;
}
.orderBy:hover {
  cursor: pointer;
}
.orderBy:hover .dropdown-content {
  display: block;
}

.containerSearch {
  padding: 27px;
}

.containerSearch .header {
  display: flex;
  align-items: center;
}

.containerSearch .header img {
  margin-right: 35px;
}

.containerSearch .results {
  margin-top: 18px;
  height: 258px;
  width: 80%;
  margin: auto;
  border-radius: 4px;
  padding: 18px 32px;
  position: relative;
}
</style>