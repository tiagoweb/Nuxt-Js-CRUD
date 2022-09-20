<template>
  <b-row>
    <b-col md="12">
      <b-card class="shadow-md border-0 rounded-lg">
        <h5>Relatório - {{ relatorio.name }}</h5>
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
                  <b-form-checkbox value="community"
                    >Comunidade</b-form-checkbox
                  >
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
</template>
