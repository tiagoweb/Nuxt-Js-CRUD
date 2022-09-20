<template>
  <div>
    <div>
      <b-navbar
        toggleable="lg"
        type="dark"
        variant="dark"
        class="shadow-md nav"
        fixed="top"
      >
        <b-navbar-brand to="/"
          ><img src="img/logocc.png" class="logocc"
        /></b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <div>
              <b-dropdown id="dropdown-1" text="Relatórios" class="m-md-2">
                <b-dropdown-item to="/qtdInscritosPorComunidade"
                  >Qtd de inscritos por Comunidade</b-dropdown-item
                >

                <b-dropdown-item to="/qtdInscritosPorRegiao"
                  >Qtd de inscritos por Região</b-dropdown-item
                >

                <b-dropdown-item to="/qtdInscritosPorComunidadePorPrograma"
                  >Qtd de inscritos por Comunidade e Programa</b-dropdown-item
                >

                <b-dropdown-item to="/alunosInscritos"
                  >Alunos inscritos</b-dropdown-item
                >
              </b-dropdown>
            </div>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>

    <Nuxt />
  </div>
</template>

<style>
.nav {
  background-color: #092346!important;
}
body {
  background-color: #313943;
}
.mt-5 {
  margin-top: 120px !important;
}


.logocc {
  height: 80px;
}
</style>
