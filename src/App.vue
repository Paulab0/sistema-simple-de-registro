<template>
  <div>
    <h1>Registro de usuarios</h1>

    <input v-model="nombre" placeholder="Escribe tu nombre" />
    <input v-model="correo" placeholder="Escribe tu correo" />

    <button @click="agregarUsuario">Agregar</button>

    <ul>
      <li v-for="user in usuarios" :key="user.id">
        {{ user.nombre }} - {{ user.correo }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nombre: "",
      correo: "",
      usuarios: []
    }
  },

  methods: {
    async agregarUsuario() {
      if (this.nombre !== "" && this.correo !== "") {
        try {
          await fetch("http://localhost:3000/usuarios", {
            method: "POST",
            headers: {
              "Content-Type": "application/json"
            },
            body: JSON.stringify({
              nombre: this.nombre,
              correo: this.correo
            })
          })

          this.nombre = ""
          this.correo = ""

          this.obtenerUsuarios()
        } catch (error) {
          console.error("Error al guardar:", error)
        }
      }
    },

    async obtenerUsuarios() {
      try {
        const res = await fetch("http://localhost:3000/usuarios")
        const data = await res.json()
        this.usuarios = data
      } catch (error) {
        console.error("Error al obtener usuarios:", error)
      }
    }
  },

  mounted() {
    this.obtenerUsuarios()
  }
}
</script>

<style>
body {
  font-family: Arial, sans-serif;
}

input {
  margin-right: 10px;
  padding: 5px;
}

button {
  padding: 5px 10px;
}

ul {
  margin-top: 10px;
}
</style>