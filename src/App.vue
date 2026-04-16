<template>
  <div class="background">

    <div class="container">

      <!-- LOGIN / REGISTRO -->
      <div v-if="!usuario">

        <h1 class="title">
          {{ modo === 'login' ? 'Bienvenida' : 'Crear cuenta' }}
        </h1>

        <p class="subtitle">
          {{ modo === 'login' ? 'Ingresa a tu espacio ✨' : 'Empieza algo nuevo 💫' }}
        </p>

        <input
          v-if="modo==='registro'"
          v-model="nombre"
          placeholder="Nombre"
        />

        <input
          v-model="correo"
          placeholder="Correo"
        />

        <input
          v-model="password"
          type="password"
          placeholder="Contraseña"
        />

        <button @click="modo==='login' ? login() : register()">
          {{ modo === 'login' ? 'Entrar' : 'Registrarme' }}
        </button>

        <p class="switch" @click="cambiarModo">
          {{ modo === 'login' ? 'Crear cuenta' : 'Ya tengo cuenta' }}
        </p>

        <p v-if="error" class="error">{{ error }}</p>

      </div>

      <!-- DASHBOARD -->
      <div v-else class="dashboard">

        <h1 class="welcome">
          Hola {{ usuario.nombre }} 🌸
        </h1>

        <p class="subtitle">Qué bueno verte de nuevo</p>

        <button class="logout" @click="logout">
          Cerrar sesión
        </button>

      </div>

    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      nombre: "",
      correo: "",
      password: "",
      usuario: null,
      modo: "login",
      error: ""
    }
  },

  methods: {
    cambiarModo() {
      this.modo = this.modo === "login" ? "registro" : "login"
      this.error = ""
    },

    async register() {
      const res = await fetch("http://localhost:3000/register", {
        method: "POST",
        headers: {"Content-Type":"application/json"},
        body: JSON.stringify({
          nombre: this.nombre,
          correo: this.correo,
          password: this.password
        })
      })

      const data = await res.json()

      if (!res.ok) {
        this.error = data.error
        return
      }

      this.modo = "login"
      this.error = "Cuenta creada 💜"
    },

    async login() {
      const res = await fetch("http://localhost:3000/login", {
        method: "POST",
        headers: {"Content-Type":"application/json"},
        body: JSON.stringify({
          correo: this.correo,
          password: this.password
        })
      })

      const data = await res.json()

      if (!res.ok) {
        this.error = data.error
        return
      }

      this.usuario = data
      localStorage.setItem("usuario", JSON.stringify(data))
      this.error = ""
    },

    logout() {
      this.usuario = null
      localStorage.removeItem("usuario")
    }
  },

  mounted() {
    const user = localStorage.getItem("usuario")
    if (user) {
      this.usuario = JSON.parse(user)
    }
  }
}
</script>

<style>
/* 🌈 FONDO GENERAL */
.background {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;

  background: linear-gradient(
    135deg,
    #fdfbfb 0%,
    #ebedee 40%,
    #e0c3fc 70%,
    #8ec5fc 100%

    
  );
}

/* 🧊 TARJETA */
.container {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(10px);

  padding: 40px;
  border-radius: 20px;

  width: 320px;
  text-align: center;

  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

/* ✨ TITULOS */
.title {
  margin-bottom: 5px;
  color: #6c5ce7;
}

.subtitle {
  font-size: 14px;
  color: #888;
  margin-bottom: 20px;
}

/* 🎯 INPUTS */
input {
  width: 100%;
  margin: 8px 0;
  padding: 12px;

  border-radius: 12px;
  border: 1px solid #ddd;

  transition: 0.2s;
}

input:focus {
  outline: none;
  border-color: #a29bfe;
  box-shadow: 0 0 5px rgba(162,155,254,0.4);
}

/* 🎨 BOTÓN PRINCIPAL */
button {
  width: 100%;
  margin-top: 10px;
  padding: 12px;

  border: none;
  border-radius: 12px;

  background: linear-gradient(135deg, #a18cd1, #fbc2eb);
  color: white;
  font-weight: bold;

  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  transform: scale(1.03);
  opacity: 0.9;
}

/* 🔄 SWITCH */
.switch {
  margin-top: 15px;
  font-size: 13px;
  color: #6c5ce7;
  cursor: pointer;
}

/* ❌ ERROR */
.error {
  color: #ff7675;
  margin-top: 10px;
}

/* 🌸 DASHBOARD */
.welcome {
  color: #6c5ce7;
}

/* 🚪 LOGOUT */
.logout {
  margin-top: 20px;
  background: linear-gradient(135deg, #ff9a9e, #fecfef);
}
</style>