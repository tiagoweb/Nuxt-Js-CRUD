<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Relatório - {{relatorio.name}}</h5>
          <hr />

          <div class="overflow-auto">
            <vue-good-table
              :columns="headers"
              :rows="dados"
              :pagination-options="{
                enabled: true,
                mode: 'records',
                perPage: 10,
                nextLabel: 'Próxima',
                prevLabel: 'Anterior',
                rowsPerPageLabel: 'Linhas por página',
                ofLabel: 'de',
                pageLabel: 'página', // for 'pages' mode
                allLabel: 'Todos',
                //position: 'top',

              }"
              :sort-options="{
                enabled: false,
                initialSortBy: [
                {field: 'region', type: 'asc'},
              ]
              }"
              @on-row-click="onRowClick"
            >
              <template slot="table-row" slot-scope="props">

                <span v-if="props.column.field === 'total'">
                  {{ props.row.total * 8  | currency }}
                </span>

                <span v-else>
                  {{ props.formattedRow[props.column.field] }}
                </span>
              </template>
            </vue-good-table>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
// import { mapGetters } from "vuex";
// import the styles
import "vue-good-table/dist/vue-good-table.css";
import { VueGoodTable } from "vue-good-table";
import Vue2Filters from 'vue2-filters'


function mesAtual() {
  var dateObj = new Date();
  var month = dateObj.getUTCMonth() + 1; //months from 1-12
  var year = dateObj.getUTCFullYear();

  month = month < 10 ? "0" + month : month;
  const newdate = month + "/" + year;
  return newdate;
}

export default {
  name: "listAllNew",
  data: () => ({
    qtd: 100,
    relatorio: {
      name: "Qtd de inscritos por Comunidade e Programa",
    },
    search: "",
    dados: [],
    datas: [],
  }),
  components: {
    VueGoodTable,
  },
  computed: {
    // a computed getter
    // publishedBooksMessage() {
    //   // `this` points to the component instance
    //   return this.qtd;
    // },
    // ...mapGetters({
    //   user: "auth/user",
    // }),
    headers() {
      return [
        {
          label: "Região",
          field: "region",
          width: "200px",
          filterOptions: {
            // styleClass: "class1", // class to be added to the parent th element
            enabled: true, // enable filter for this column
            placeholder: "Digite região", // placeholder for filter input
            // filterValue: "", // initial populated value for this filter
            // filterDropdownItems: ["SA", "Diretor", "Tutor"], // dropdown (with selected values) instead of text input
            // filterFn: this.filterFnTipo, //custom filter function that
            trigger: "enter", //only trigger on enter not on keyup
          },
        },
        {
          label: "Comunidade",
          field: "community",
          width: "200px",
          filterOptions: {
          // styleClass: "class1", // class to be added to the parent th element
          enabled: true, // enable filter for this column
          placeholder: "Digite comunidade", // placeholder for filter input
          // filterValue: "", // initial populated value for this filter
          // filterDropdownItems: [], // dropdown (with selected values) instead of text input
          //filterFn: this.filterFnRegiao, //custom filter function that
          // trigger: "keyup", //only trigger on enter not on keyup
          },
        },
        {
          label: "Programa",
          field: "program",
          width: "200px",
          filterOptions: {
          // styleClass: "class1", // class to be added to the parent th element
          enabled: true, // enable filter for this column
          placeholder: "Digite comunidade", // placeholder for filter input
          // filterValue: "", // initial populated value for this filter
          // filterDropdownItems: [], // dropdown (with selected values) instead of text input
          //filterFn: this.filterFnRegiao, //custom filter function that
          // trigger: "keyup", //only trigger on enter not on keyup
          },
        },
        {
          label: "QTD",
          field: "count",
          width: "100px",
          // filterOptions: {
          // styleClass: "class1", // class to be added to the parent th element
          // enabled: true, // enable filter for this column
          // placeholder: "Filtrar", // placeholder for filter input
          // filterValue: "", // initial populated value for this filter
          // filterDropdownItems: [], // dropdown (with selected values) instead of text input
          // filterFn: this.filterFnRegiao, //custom filter function that
          // trigger: "keyup", //only trigger on enter not on keyup
          // },
        },

      ];
    },
  },

  mounted() {
    //this.id = this.$route.params.id

    this.$axios
      .get("/qtdInscritosPorComunidadePorPrograma")
      .then((response) => {
        //assign response ke state "posts"
        this.dados = response.data;
        //this.rows = this.post.length
        console.log(this);
        console.log(1);
      })
      .catch((error) => {
        console.log(error.response.data);
      });

    // console.log(this.$http.getBaseURL() + '/upload')
  },

  methods: {
    list(e) {
      //this.$emit('save-localstorage')
      this.$router.push("/turma/detalhes/" + e);
    },

    filterFn: function (data, filterString) {
      let analise = new RegExp(filterString, "i");
      return analise.test(data);
    },

    filterFnAnalise: function (data, filterString) {
      const data2 = {
        0: "Não analisado",
        1: "Aprovado",
        2: "Rejeitado",
        3: "Reanalisar",
      };

      let analise = new RegExp(filterString, "i");
      return analise.test(data2[data]);
    },

    filterFnNota: function (data, filterString) {
      const data2 = {
        0: "Não",
        1: "Sim",
      };

      let analise = new RegExp(filterString, "i");
      return analise.test(data2[data]);
    },

    filterFnPago: function (data, filterString) {
      const data2 = {
        0: "Não",
        1: "Sim",
      };

      let analise = new RegExp(filterString, "i");
      return analise.test(data2[data]);
    },

    onRowClick(params) {
      this.$router.push("/turma/detalhes/" + params.row.hash);
    },

    onColumnFilter(params) {
      console.log(params.columnFilters);
      sessionStorage.setItem("listAll-tipo", params.columnFilters.tipo);
      sessionStorage.setItem("listAll-regiao", params.columnFilters.regiao);
      sessionStorage.setItem("listAll-nota", params.columnFilters.pdf);
      sessionStorage.setItem("listAll-turma", params.columnFilters.turma);
      sessionStorage.setItem("listAll-mes", params.columnFilters.mes);
      sessionStorage.setItem("listAll-analise", params.columnFilters.aprovado);
      sessionStorage.setItem("listAll-pago", params.columnFilters.pago);
      //this.updateParams(params)
      //this.loadItems()
    },

    async searchDates() {
      const response = await this.$http.$get(`/searchDates`);
      if (response.retorno) {
        console.log(response.retorno);
        this.datas = response.retorno;
      }
    },
  },
};
</script>

<style>
</style>
