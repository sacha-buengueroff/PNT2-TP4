<template>

  <section class="src-components-http-client">
    <div class="jumbotron">
      <h2>HttpClient</h2>
      <hr>
      <hr>
      <br>
      
      <button class="btn btn-warning my-3 mr-3" @click="getUsuariosXHRPromise()">
        Pedir XHR (XMLHttpRequest) (Promise)
      </button>
      <button class="btn btn-success my-3 mr-3" @click="getUsuariosFetch()">
        Pedir Fetch
      </button>
      <button class="btn btn-success my-3 mr-3" @click="getUsuariosAxios()">
        Pedir Axios
      </button>
      <button class="btn btn-danger my-3 mr-3" @click="usuarios = []">
        Clear
      </button>
      
      <br>

      <div class="media alert alert-info" v-for="(usuario,index) in usuarios" v-bind:key="index">
          <div class="media-body">
              <br>
              <p>Nombre: {{ usuario.nombre }}</p>
              <p>Email: {{ usuario.email }}</p>
              <p>Numero de teléfono: {{ usuario.numeroTelefono }}</p>
          </div>
      </div>
    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-components-http-client',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: "https://633f79bd0dbc3309f3cb1986.mockapi.io/api/usuarios2",
        usuarios: [],
      }
    },
    methods: {
      xhrPromise() {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open("get", this.url);

        xhr.addEventListener("load", () => {
          // load se da cuando el servidor nos responde
          if (xhr.status == 200) {
            let res = JSON.parse(xhr.response);
            resolve(res);
          } else {
            reject(`Error http: ${xhr.status}`);
          }
        });
        xhr.send();
      });
    },
    async getUsuariosXHRPromise() {
      try {
        let res = await this.xhrPromise();
        this.usuarios = res;
      } catch (error) {
        console.error(error);
      }
    },
    async getUsuariosFetch() {
      try {
        let response = await fetch(this.url);
        let res = await response.json();
        this.usuarios = res;
      } catch (error) {
        console.log(error);
      }
    },
    async getUsuariosAxios() {
      try {
        let res = await this.axios(this.url);
        this.usuarios = res.data;
      } catch (error) {
        console.log(error);
      }
    },
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .jumbotron {
  background-color: teal;
  color: white;
}

hr {
  background-color: #bbb;
}
</style>
