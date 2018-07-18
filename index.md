title: Nadando en VueJS
author:
  name: Jorge Epuñan
  twitter: csslab
output: index.html
theme: juanbrujo/cleaver-beerjs

--

# Nadando en VueJS

## Pequeños trucos y módulos a prueba de balas 🎯

--

# BeerJS

## Santiago y Valdivia

--

![](https://i.imgur.com/fGEQN2Q.jpg)

--

![](https://www.beerjs.cl/santiago/assets/ediciones/201804-ubiome.jpg)

--

![](https://www.beerjs.cl/santiago/assets/ediciones/201801-elhonestomike.jpg)

--

### Jorge Epuñan

- UX Lead, Frontend Technical Leader, Frontend Architect, Director I+D, Jefe Desarrollo, Director Producción, Web Developer, Web Designer
- Cumplo, Mercado Libre, I2B, Global Interactive, Mayo Draft FCB, Cardumen, Taisa Chile, Virtualia.com (Copesa)
- catalizado.com, [buscandriu.cl](http://www.buscandriu.cl), designinsocial.com, [csslab.cl](http://www.csslab.cl), [devsChile](http://www.devschile.cl) admin, [Huemul](https://github.com/devschile/huemul) developer, [beerjs.cl](http://www.beerjs.cl) organizer, [open-source](http://github.com/juanbrujo/) advocate

--

# Nadando en VueJS

## Pequeños trucos y módulos a prueba de balas 🎯

--

# Autenticación

[vue-google-signin-button](https://github.com/phanan/vue-google-signin-button)

```html
<g-signin-button
  :params="googleSignInParams"
  @success="onSignInSuccess"
  @error="onSignInError">
  LogIn
</g-signin-button>
```

```javascript
import Vue from 'vue'
import GSignInButton from 'vue-google-signin-button'

data () {
  return {
    googleSignInParams: {
      client_id: process.env.GOOGLE_CLIENT_ID
    },
    authenticated: false,
    user: '',
    mail: '',
    ...
  }
},
```

--

```javascript
methods: {
  onSignInSuccess (googleUser) {
    const profile = googleUser.getBasicProfile()
    this.user = profile.ofa
    this.mail = profile.U3
    this.$router.push('rut')
  },
  onSignInError (error) {
    // Sentry
  },
  ...
}
```

--

## Ventanas Modales

[vue-js-modal](https://github.com/euvl/vue-js-modal)

![](https://camo.githubusercontent.com/31a7832740f85a5de68364ac4d292c29d89929f6/68747470733a2f2f6d656469612e67697068792e636f6d2f6d656469612f336f4b4950636f31654e78414131724434512f67697068792e676966)

--

```html
<modal name="modal-log" width="80%" height="auto" :scrollable="true" @before-close="emptyLogData()"></modal>
<button class="button" @click="openModalLog(row.changes)">Ver cambios</button>
```

```javascript
import VModal from 'vue-js-modal'

Vue.use(VModal)

data () {
  return {
    currentlog: null,
    ...
  }
},
methods: {
  openModalLog: function (data) {
    this.currentlog = data
    this.$modal.show('modal-log')
  },
}
```

--

## Alertas

[vue-sweetalert](https://github.com/limonte/sweetalert2)

![](https://camo.githubusercontent.com/cd434dae15a725e39b59626ec7b0391a1e237eb7/68747470733a2f2f7261772e6769746875622e636f6d2f6c696d6f6e74652f7377656574616c657274322f6d61737465722f6173736574732f7377656574616c657274322e676966)

--

```javascript
import swal from 'sweetalert2'

methods: {
  logout: function () {
    this.$swal({
      title: '¿Estás seguro?',
      text: 'Confirmando cerrarás tu sesión de usuario',
      type: 'warning',
      showCancelButton: true,
      confirmButtonText: 'Si',
      cancelButtonText: 'Cancelar'
    }).then(() => {
      localStorage.clear()
      this.$router.push('/')
    }).catch(this.$swal.noop)
  }
}
```

--

## Title & Meta tags

[vue-meta](https://github.com/declandewet/vue-meta)

```javascript
// router/index
import Meta from 'vue-meta'

Vue.use(Meta)
```

```javascript
metaInfo: {
  meta: [
    { charset: 'utf-8' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' }
  ],
  title: 'Inicio Sesión',
  titleTemplate: '%s | Empresa.mx',
  link: [
    { rel: 'stylesheet', href: '/css/inicio-sesion.css' },
  ],
  bodyAttrs: {
    class: 'inicio-sesion'
  }
}
```

```html
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Inicio Sesión | Empresa.mx</title>
  <link rel="stylesheet" href="/css/inicio-sesion.css">
</head>
<body class="inicio-sesion">
```

--

## Tablas

[vue-table-component](https://github.com/spatie/vue-table-component)

![](https://i.imgur.com/Mncfnwk.png)

--

```html
<table-component
  :data="log"
  filterPlaceholder="Filtrar datos..."
  filterNoResults="No se encontraron resultados."
  >
  <table-column show="date" label="Fecha" :formatter="formatDate"></table-column>
  <table-column show="description" label="Descripción"></table-column>
</table-component>
```

```javascript
data () {
  return {
    log: null,
    ...
  }
},
methods: {
  getLog: function () {
    this.log = []
    axios.get('...')
    .then(response => {
      if (response.data.status) {
        this.log = response.data.log
      }
    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  formatDate: function (date) {
    if (date) {
      return moment(date).format('DD/MM/YY HH:mm')
    } else {
      return ''
    }
  },
  ...
},
created: function () {
  this.getLog()
}
```

--

![](https://i.imgur.com/wzfh1Xd.png)

--

## Tabs

[vue-tabs-component](https://github.com/spatie/vue-tabs-component)

```html
<tabs class="box-tabs">
  <tab name="Ver Existente">
    ...
  </tab>
  ...
</tabs>
```

```javascript
import Tabs from 'vue-tabs-component'

Vue.use(Tabs)
```

--

## File Upload

[vue-upload-component](https://github.com/lian-yue/vue-upload-component)

![](https://i.imgur.com/V4jSmrv.png)

--

```html
<file-upload
  post-action="https://www.empresa.cl/upload"
  extensions="pdf"
  accept="application/pdf"
  v-model="files"
  ref="upload">
  Select files
</file-upload>
<button type="button" class="button" v-if="!$refs.upload || !$refs.upload.active" @click.prevent="$refs.upload.active = true">Subir archivo</button>
```

```javascript
import FileUpload from 'vue-upload-component'

Vue.use(FileUpload)

data () {
  return {
    files: [],
    ...
  }
},
...
}
```

--

### FIN Gracias México lindo y querido 😍

