<template>
  <section class="src-componentes-formulario">
    <div class="jumbotron">
      <h2>Ingreso de datos</h2>
      <hr />

      <vue-form
        id="inputform"
        :state="formstate"
        method="POST"
        @submit.prevent="postAxios"
      >
        <!-- Campo Nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            v-model="formData.nombre"
            required
            name="nombre"
            autocomplete="off"
            class="form-control"
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
          />

          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              La cantidad minima de caracteres es de {{ nombreMinLength }}.
            </div>
            <div slot="maxlength" class="alert alert-danger mt-1">
              La cantidad maxima de caracteres es de {{ nombreMaxLength }} años.
            </div>
          </field-messages>
        </validate>

        <br />

        <!-- Campo edad -->
        <validate tag="div">
          <label for="edad">Edad</label>
          <input
            type="number"
            id="edad"
            name="edad"
            class="form-control"
            v-model.number="formData.edad"
            autocomplete="off"
            required
            :min="edadMin"
            :max="edadMax"
          />

          <!-- mensaje de validación para el campo edad -->
          <field-messages name="edad" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido.
            </div>
            <div slot="min" class="alert alert-danger mt-1">
              La edad mínima permitida es de {{ edadMin }} años.
            </div>
            <div slot="max" class="alert alert-danger mt-1">
              La edad máxima permitida es de {{ edadMax }} años.
            </div>
          </field-messages>
        </validate>
        <br />

        <!-- Campo Email -->

        <validate tag="div">
          <label for="email">email</label>
          <input
            type="email"
            id="email"
            v-model.number="formData.email"
            required
            name="email"
            autocomplete="off"
            class="form-control"
          />

          <!-- mensaje de validación para el campo Mail -->

          <field-messages name="email" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Este campo es obligatorio
            </div>
            <div slot="email" class="alert alert-danger mt-1">
              Email no válido.
            </div>
          </field-messages>
        </validate>

        <br />

        <button
          class="btn btn-success mt-3"
          :disabled="formstate.$invalid"
          type="submit"
        >
          Enviar
        </button>
      </vue-form>

      <br />
      <hr />
      <!-- Tabla -->

      <h2>Datos ingresados</h2>
      <br />
      <div v-if="datos.length" class="table-responsive">
        <br />
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Email</th>
          </tr>
          <tr v-for="(dato, index) in datos" :key="index">
            <td>{{ dato.nombre }}</td>
            <td>{{ dato.edad }}</td>
            <td>{{ dato.email }}</td>
          </tr>
        </table>
      </div>
    </div>
  </section>
</template>

<script>
const axios = require("axios").default;
const serializer = require("form-serialize");
const JSON_URL = "https://61893787d0821900178d7880.mockapi.io/api/usuarios";


export default {
  name: "src-componentes-formulario",
  props: [],
  mounted() {},
  data() {
    return {
      formstate: {},
      nombreMinLength: 3,
      nombreMaxLength: 10,
      edadMin: 18,
      edadMax: 120,
      formData: this.getInitialData(),
      datos: [],
    };
  },
  methods: {
    postAxios(event) {
      event.preventDefault();

      let form = document.querySelector("#inputform");
      let obj = serializer(form, { hash: true });

      axios
        .post(JSON_URL, obj)
        .then((response) => {
          console.log(response);
          console.log(response.data);
        })
        .then((response) => {
          this.getAxios();
          console.log(response);
        })
        .catch((error) => error);

        this.enviar()
    },
    getInitialData() {
      return {
        nombre: null,
        edad: null,
        mail: null,
      };
    },

    enviar() {
      console.log(this.formData);

      this.datos.push(this.formData);
      this.formData = this.getInitialData();
      this.formstate._reset();
    },
  },
  computed: {},
};
</script>

<style scoped lang="css">
.src-componentes-formulario {
}
</style>
