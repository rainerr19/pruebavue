# pruebavue

El proyecto consiste en una app que genere eventos y que se pueda ingresar los tiquetes del mismo.

Este proyecto es el Front que se realizó con VueJS  y Framework de diseño BootstrapVue que implemanta estilos modernos de CCS.

Descripción detallada del desarrollo:
- Se pensó en diseñar una API sencilla Single page con 2 componentes (botones y tabla) con la que se puede obtener un CRUD sencillo.
- La comunicación entre el front y el Backend se hizo con librería de AXIOS que genera más facilidad a la hora de realizar las peticiones.

- Se creó el componente tabla primero ya que es el que se utiliza para mostrar datos y es la que lleva mayor tamaño. - Se le puso campos necesarios y uno adicional para las acciones como eliminar eventos y asignar. Dentro del mismo se generan funciones que actualicen la tabla cuando se hagan cualquier cambio. Los botones de la tabla se agregan sin ninguna funcionalidad. 
- Se genera el segundo componente que es el encargado de generar los datos para llenar la tabla. Se agrega el Botón junto a un Modal que funciona como formulario de los dos tipos de datos (clientes y boletas).
- Después de probar la funcionalidad de los botones se procede a agregar la funcionalidad de los botones de la tabla que serán encargado de eliminar las boletas y asignar boletas.







## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

