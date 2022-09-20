<template>
  <b-container fluid="md" class="mt-5 mb-5">


    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Relatório</h5>
          <hr />

          <div class="overflow-auto">

            <b-row>

      <b-col lg="6" class="my-1">
        <b-form-group
          label="Buscar"
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
        >
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Digite"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''"
                >Limpar</b-button
              >
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>

      <b-col lg="6" class="my-1">
        <b-form-group
          v-model="sortDirection"
          label="Filtrar por"
          description="Desmarque todos para buscar em todos os registros"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
          v-slot="{ ariaDescribedby }"
        >
          <b-form-checkbox-group
            v-model="filterOn"
            :aria-describedby="ariaDescribedby"
            class="mt-1"
          >
            <b-form-checkbox value="region">Região</b-form-checkbox>
            <b-form-checkbox value="community">Comunidade</b-form-checkbox>

          </b-form-checkbox-group>
        </b-form-group>
      </b-col>


            </b-row>

            <b-table
              :filter="filter"
              @filtered="onFiltered"
              :filter-included-fields="filterOn"
              :sort-by.sync="sortBy"
              :sort-desc.sync="sortDesc"
              :sort-direction="sortDirection"
              stacked="md"
              show-empty
              striped
              bordered
              hover
              :items="posts"
              :fields="fields"
              id="my-table"
              :per-page="perPage"
              :current-page="currentPage"
              small
            >
            </b-table>

            <b-pagination
              v-model="currentPage"
              :total-rows="rows"
              :per-page="perPage"
              aria-controls="my-table"
            ></b-pagination>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      filter: null,
      totalRows: 1,
      currentPage: 1,
      perPage: 20,
      pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      //header table

      fields: [
        {
          key: "region",
          label: "Região",
          sortable: true,
          // Variant applies to the whole column, including the header and footer
          //variant: 'danger'
        },
        {
          key: "community",
          label: "Comunidade",
          sortable: true,
          // Variant applies to the whole column, including the header and footer
          //variant: 'danger'
        },
        {
          key: "program",
          label: "Programa",
          sortable: true,
          // Variant applies to the whole column, including the header and footer
          //variant: 'danger'
        },
        {
          key: "count",
          label: "QTD",
          sortable: true,
          // Variant applies to the whole column, including the header and footer
          //variant: 'danger'
        },
      ],

      //posts data
      posts: [],
    };
  },
  computed: {
    rows() {
      return this.posts.length;
    },
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter((f) => f.sortable)
        .map((f) => {
          return { text: f.label, value: f.key };
        });
    },
  },
  mounted() {
    this.totalRows = this.posts.length;
    //fething ke Rest API
    this.$axios
      .get("/qtdInscritosPorComunidadePorPrograma")
      .then((response) => {
        //assign response ke state "posts"
        this.posts = response.data;
        //this.rows = this.post.length
        //console.log(this);
      })
      .catch((error) => {
        console.log(error.response.data);
      });
  },

  methods: {
    async deletePost(row) {
      //delete data post by ID
      await this.$axios.delete(`/api/posts/${row.item.id}`).then(() => {
        //remove item array by index
        this.posts.splice(row.index, 1);
      });
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
  },
};
</script>

<style>
</style>
