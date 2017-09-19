---
title: Comparación con otros frameworks
type: guide
order: 801
---

Esta es la página más difícil en la guía para escribir, pero sentimos que es importante. Existe la posibilidad de que hayas tenido problemas, los cuales trataste de resolver y para ello utilizaste otra biblioteca. Estás aquí porque quieres saber si Vue puede resolver tus problemas de una mejor manera. Eso es lo que esperamos responder.

Nos esforzamos mucho para evitar prejuicios. Siendo el equipo principal, obviamente nos gusta mucho Vue. Hay algunos problemas que creemos resuelve mejor que cualquier otra cosa. Si no lo creyeramos, no estaríamos trabajando en esto. Sin embargo, queremos ser justos y precisos. Cuando otras bibliotecas ofrecen ventajas significativas, como el vasto ecosistema de procesadores alternativos de React o el soporte de navegación de Knockout en IE6, intentamos también mencionarlas.

¡También nos gustaría **tu** ayuda para mantener este documento actualizado porque el mundo de JavaScript se mueve rápido! Si notas una inexactitud o algo que no esté correcto, por favor, háznoslo saber [abriendo un issue (en Inglés)](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide).

## React

React y Vue comparten muchas similitudes. Ambos:

- utilizan un DOM virtual
- proporcionan componentes de vista reactivos y componibles
- mantienen el enfoque en el núcleo de la biblioteca, preocupandose por temas como el enrutamiento y la administración del estado global a través de bibliotecas complementarias

Siendo muy similares en su alcance, hemos puesto más tiempo en afinar esta comparación que cualquiera. Queremos garantizar no sólo la precisión técnica, sino también el equilibrio. Señalamos donde React supera Vue, por ejemplo en la riqueza de su ecosistema y la abundancia de sus renderizadores personalizados.

Con eso dicho, es inevitable, para algunos usuarios de React, que la comparación pareciera sesgada hacia Vue, pues muchos de los temas explorados son hasta cierto punto subjetivos. Reconocemos la existencia de diversos gustos técnicos, y esta comparación tiene como objetivo principal esbozar las razones por las que Vue podría ser una mejor elección si tus preferencias coinciden con las nuestras.

La comunidad de React [ha sido fundamental](https://github.com/vuejs/vuejs.org/issues/364) para ayudarnos a lograr este equilibrio, queremos agradecer especialmente a Dan Abramov del equipo de React. Él fue extremadamente generoso con su tiempo y experiencia para ayudarnos a refinar este documento hasta que [ambos estuvimos contentos](https://github.com/vuejs/vuejs.org/issues/364#issuecomment-244575740) con el resultado final.

### Desempeño

Tanto React como Vue ofrecen un desempeño comparable en los casos de uso más comunes, con Vue por lo general ligeramente por delante debido a su aplicación de DOM virtual de peso más ligero. Si estás interesado en los números, puedes comprobar esta [referencia de terceros](https://rawgit.com/krausest/js-framework-benchmark/master/webdriver-ts/table.html) que se centra en la representación bruta de desempeño/actualización. Ten en cuenta que esto no toma en cuenta estructuras de componentes complejas, por lo que sólo debe considerarse como una referencia en lugar de un veredicto.

#### Esfuerzos de optimización

En React, cuando el estado de un componente cambia, activa la representación de todo el sub-árbol de componentes, comenzando en ese componente como root. Para evitar re-renders innecesarios de componentes secundarios, es necesario utilizar `PureComponent` o implementar `shouldComponentUpdate` siempre que se pueda. Es posible que también necesite utilizar estructuras de datos inmutables para hacer que sus cambios de estado sean más fáciles de optimizar. Sin embargo, en ciertos casos puede que no aw pueda confiar en tales optimizaciones porque `PureComponent/shouldComponentUpdate` asume que la salida del renderizado del subárbol completo es determinada por los accesorios del componente actual. Si ese no es el caso, entonces tales optimizaciones pueden conducir a un estado del DOM inconsistente.

En Vue, las dependencias de un componente se rastrean automáticamente durante su procesamiento, por lo que el sistema sabe exactamente qué componentes  necesitan volver a ser renderizados cuando cambia el estado. Se puede decir que cada componente tiene `shouldComponentUpdate` implementado automáticamente, sin las limitaciones de componente anidadas.

En general, esto elimina la necesidad de optimizaciones de desempeño de parte del desarrollador, y permite centrarse más en la construcción de la aplicación en sí misma a medida que se escala.

### HTML y CSS

En React, todo es JavaScript. No sólo las estructuras HTML se expresan a través de JSX, también la administración del CSS va dentro del JavaScript. Este enfoque tiene sus propios beneficios, pero también viene con varias desventajas que no parecen valer la pena para algunos desarrolladores.

Vue adopta las tecnologías web clásicas y se construye en base a ellas. Para demostrar lo que esto significa, veamos algunos ejemplos.

#### JSX vs plantillas

En React, todos los componentes expresan su interfaz de usuario dentro de las funciones de render utilizando JSX, una sintaxis declarativa similar a XML que funciona dentro de JavaScript.

Las funciones de renderizado con JSX tienen algunas ventajas:

- Se puede aprovechar el poder de un lenguaje de programación completo (JavaScript) para crear la vista. Esto incluye variables temporales, controles de flujo y referencia directa a los valores de JavaScript en el scope.

- El soporte de herramientas (por ejemplo, linting, comprobación de tipo, autocompletado del editor) para JSX es, en algunos aspectos, más avanzado que lo que actualmente está disponible para las plantillas de Vue.

En Vue, también tenemos [funciones de render](render-function.html) que pueden ser [compatibles con JSX](render-function.html#JSX), porque a veces se necesita ese poder. Sin embargo, como experiencia predeterminada ofrecemos plantillas como una alternativa más sencilla. Cualquier HTML válido es también una plantilla para Vue válida, lo cual tiene algunas ventajas:

- Para muchos desarrolladores que han estado trabajando con HTML, las plantillas simplemente se sienten naturales para leer y escribir. La preferencia en sí puede ser algo subjetiva, pero si hace que el desarrollador sea más productivo, entonces el beneficio es objetivo.

- Las plantillas basadas en HTML facilitan la migración progresiva de aplicaciones ya existentes para aprovechar las características de reactividad de Vue.

- También hace mucho más fácil para los diseñadores y los desarrolladores menos experimentados analizar y contribuir a la base de código.

- Puedes utilizar preprocesadores como Pug (anteriormente conocido como Jade) para crear plantillas para Vue.

Algunos argumentan que necesitarías aprender un lenguaje específico de dominio (DSL por sus siglas en inglés) adicional para poder escribir plantillas - creemos que esta diferencia es superficial en el mejor de los casos. En primer lugar, JSX no significa que el usuario no necesita aprender nada más - es una sintaxis adicional sobre el JavaScript, por lo que es fácil para cualquiera familiarizado con JavaScript pueda aprenderlo, pero decir que es igual al JS es engañoso. Del mismo modo, una plantilla es simplemente sintaxis adicional sobre HTML simple y por lo tanto tiene un costo de aprendizaje muy bajo para aquellos que ya están familiarizados con HTML. Con el DSL también podemos ayudar al usuario a hacer más con menos código (por ejemplo, modificadores `v-on`). La misma tarea puede implicar mucho más código al usar funciones JSX o render.

A un alto nivel, podemos dividir los componentes en dos categorías: presentacionales y lógicos. Recomendamos utilizar plantillas para componentes de presentación y funciones de renderizado/JSX para los componentes lógicos. El porcentaje de estos componentes depende del tipo de aplicación que estás construyendo, pero en general encontramos que los presentacionales son mucho más comunes.

#### Componentes de CSS con alcance

A menos que se extiendan los componentes a través de múltiples archivos (por ejemplo, con [Módulos CSS](https://github.com/gajus/react-css-modules)), el CSS en React se realiza a través de soluciones CSS dentro de JS (por ejemplo, [styled-components](https://github.com/styled-components/styled-components), [glamorous](https://github.com/paypal/glamorous) y [emotion](https://github.com/emotion-js/emotion)). Esto introduce un nuevo paradigma de estilo orientado a componentes que es diferente del proceso normal de creación de CSS. Además, aunque hay soporte para la extracción de CSS en una sola hoja de estilos durante el tiempo de construcción, todavía es común que se necesite incluir un tiempo de ejecución en el paquete para que el estilo funcione correctamente. Mientras obtienes acceso al dinamismo de JavaScript mientras construyes tus estilos, tienes la desventaja de aumentar el tamaño del paquete y el coste de ejecución.

Si eres un fan del CSS dentro de JS, muchas de las bibliotecas populares de CSS dentro de JS soportan Vue (por ejemplo, [styleled-components-vue](https://github.com/styled-components/vue-styled-components) y [vue-emotion](https://github.com/egoist/vue-emotion)). La principal diferencia entre React y Vue es que el método por defecto de estilo en Vue es a través de la etiqueta `style` en [componentes de un solo archivo](single-file-components.html).

[Componentes de un solo archivo](single-file-components.html) te brinda acceso completo al CSS en el mismo archivo como al resto del código de tu componente.

``` html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```
El atributo opcional `scoped` le da automáticamente este CSS a su componente añadiendo un atributo único (como` data-v-21e5b78`) a los elementos y compilando `.list-container: hover` a algo como` .list-container [ data-v-21e5b78]: hover`.

Por último, dar estilo a un componente de Vue en un solo archivo es muy flexible. A través de [vue-loader](https://github.com/vuejs/vue-loader), puedes utilizar cualquier preprocesador, posprocesador e incluso una profunda integración con [Modulos CSS](https://vue-loader.vuejs.org/en/features/css-modules.html) - todo dentro del elemento `<style>`.

### Escalamiento

#### Escalando hacia arriba

Para aplicaciones grandes, Vue y React ofrecen soluciones de enrutamiento robustas. La comunidad React también ha sido muy innovadora en términos de soluciones de gestión de estado (por ejemplo, Flux / Redux). Estos patrones de administración de estado e [incluso el propio Redux](https://github.com/egoist/revue) se pueden integrar fácilmente en las aplicaciones de Vue. De hecho, Vue ha tomado este modelo un paso más allá con [Vuex](https://github.com/vuejs/vuex), una solución de gestión de estado inspirada en Elm que se integra profundamente en Vue, la cual creemos ofrece una experiencia de desarrollo superior.

Otra diferencia importante es que las bibliotecas complementarias de Vue para la administración de estados y enrutamientos (entre [otras preocupaciones](https://github.com/vuejs)) están oficialmente soportadas y se mantienen al día con la biblioteca principal. React en su lugar elige dejar estas preocupaciones a la comunidad, creando un ecosistema más fragmentado. Sin embargo, siendo más popular, el ecosistema de React es considerablemente más rico que el de Vue.

Por último, Vue ofrece un [generador de proyecto CLI](https://github.com/vuejs/vue-cli) que hace que sea sencillo iniciar un nuevo proyecto utilizando su sistema de compilación, incluido [webpack](https://github.com/videjs-templates/webpack), [Browserify](https://github.com/vuejs-templates/browserify), o incluso [sin sistema de compilación](https://github.com/vuejs-templates/simple). React también está avanzando en esta área con [create-react-app](https://github.com/facebookincubator/create-react-app), pero actualmente tiene algunas limitaciones:

- No permite ninguna configuración durante la generación del proyecto, mientras que las plantillas de proyecto de Vue lo permiten al estilo de [Yeoman](http://yeoman.io/).
- Sólo ofrece una plantilla única, asumiendo que estás construyendo una aplicación de una sola página, mientras que Vue ofrece una amplia variedad de plantillas para diversos propósitos y sistemas de compilación.
- No puede generar proyectos a partir de plantillas construidas por el usuario, lo que puede ser especialmente útil para entornos empresariales con convenciones preestablecidas.

Es importante tener en cuenta que muchas de estas limitaciones son decisiones de diseño intencional hechas por el equipo de create-react-app y tienen sus ventajas. Por ejemplo, siempre y cuando las necesidades del proyecto sean muy simples y nunca necesite "expulsar" para personalizar su proceso de compilación, podrá actualizarlo como una dependencia. Puede leer más acerca de [la filosofía de diferenciación aquí](https://github.com/facebookincubator/create-react-app#philosophy).

#### Escalando hacia abajo

React es conocido por su curva de aprendizaje. Antes de que realmente puedas empezar, necesitas saber sobre JSX y probablemente ES2015+, ya que muchos ejemplos usan la sintaxis de clase de React. También tienes que aprender sobre los sistemas de compilación, porque aunque técnicamente puedes usar Babel Standalone para compilar en tiempo real tu código en el navegador, no es absolutamente adecuado para producción.

Mientras Vue escala igual o mejor que React, también se escala hacia abajo igual que jQuery. Asi es, todo lo que tienes que hacer es escribir una sola etiqueta de script en la página:

``` html
<script src="https://unpkg.com/vue"></script>
```

Asi puedes comenzar a escribir código en Vue e incluso enviar la versión minificada a producción sin sentirte culpable o tener que preocuparte por problemas de desempeño.

Dado que no necesitas saber acerca de JSX, ES2015 ni construir sistemas para comenzar con Vue, a los desarrolladores les suele tomar menos de un día leer [la guía](./) para aprender lo suficiente como para construir aplicaciones no tan triviales.

### Rederizado Nativo

React Native te permite escribir aplicaciones de renderizado nativo para iOS y Android utilizando el mismo modelo de componentes de React. Esto es genial ya que como desarrollador puedes aplicar tus conocimientos del framework a través de múltiples plataformas. En este frente, Vue tiene una colaboración oficial con [Weex](https://alibaba.github.io/weex/), un framework de interfaz de usuario multiplataforma desarrollado por Alibaba Group, que utiliza Vue como su framework Javascript para tiempo de ejecución. Esto significa que con Weex, puedes utilizar la misma sintaxis de componentes de Vue para crear componentes que no sólo se pueden representar en el navegador, sino también de forma nativa en iOS y Android.

En este momento, Weex sigue en desarrollo activo y no es tan maduro y probado como React Native, pero su desarrollo está impulsado por las necesidades de producción del mayor negocio de comercio electrónico en el mundo, y el equipo de Vue también participará activamente con el equipo de Weex para asegurar una experiencia de calidad para los desarrolladores de Vue.

Otra opción que los desarrolladores de Vue pronto tendrán es [NativeScript](https://www.nativescript.org/), a través de un [complemento hecho por la comunidad](https://github.com/rigor789/nativescript-vue).

### Con MobX

MobX se ha vuelto muy popular en la comunidad de React ya que utiliza un sistema de reactividad casi idéntico al de Vue. En cierta medida, el flujo de trabajo React + MobX se puede considerar como un Vue más detallado, por lo que si estás utilizando esa combinación y estás disfrutandola, saltar a Vue es probablemente el siguiente paso lógico.

## AngularJS (Angular 1)

Parte de la sintaxis de Vue se ve muy similar a la de AngularJS (por ejemplo `v-if` vs ` ng-if`). Esto es porque había un montón de cosas que AngularJS tiene bien definidas y estas fueron una inspiración para Vue muy temprano en su desarrollo. Sin embargo, también hay muchos dolores que vienen con AngularJS, donde Vue ha intentado ofrecer una mejora significativa.

### Complejidad

Vue es mucho más simple que AngularJS, tanto en términos de API cómo de diseño. Aprender suficiente como para construir aplicaciones no triviales suele tardar menos de un día, lo que no es igual para AngularJS.

### Flexibilidad y modularidad

AngularJS tiene opiniones fuertes sobre cómo las aplicaciones deben ser estructuradas, mientras que Vue es una solución más flexible y modular. Si bien esto hace que Vue sea más adaptable a una amplia variedad de proyectos, también reconocemos que a veces es útil tener algunas decisiones tomadas por ti, de modo que te puedes enfocar sólo en empezar a codificar.

Es por eso que ofrecemos una [plantilla para webpack](https://github.com/vuejs-templates/webpack) que puedes configurar en cuestión de minutos, al mismo tiempo que brindamos acceso a funciones avanzadas, como la recarga de módulos en caliente, linting, la extracción de CSS, y mucho más.

### Vinculación de datos

AngularJS utiliza vínculos bidireccionales entre los scopes, mientras que Vue impone un flujo de datos unidireccional entre los componentes. Esto hace que el flujo de datos sea más fácil de razonar en aplicaciones no triviales.

### Directivas vs componentes

Vue tiene una separación más clara entre las directivas y los componentes. Las directivas están diseñadas para encapsular manipulaciones DOM únicamente, mientras que los componentes son unidades independientes que tienen su propia visión y lógica de datos. En AngularJS, hay mucha confusión entre los dos.

### Desempeño

Vue tiene un mejor desempeño y es mucho, mucho más fácil de optimizar porque no utiliza el control sucio. AngularJS se vuelve lento cuando hay un montón de observadores, porque cada vez que algo en el ámbito cambia, todos estos observadores necesitan ser reevaluados de nuevo. Además, el ciclo de digestión puede ejecutarse varias veces para "estabilizar" si algún observador activa otra actualización. Los usuarios de AngularJS a menudo tienen que recurrir a técnicas esotéricas para evitar el ciclo de digestión, y en algunas situaciones, simplemente no hay manera de optimizar un ámbito con muchos observadores.

Vue no sufre de esto en absoluto porque utiliza un sistema de observación de seguimiento de dependencia transparente con cola asíncrona - todos los cambios se activan independientemente a menos que tengan relaciones de dependencia explícitas.

Curiosamente, hay bastantes similitudes en cómo Angular y Vue están abordando estos problemas de AngularJS.

## Angular (Anteriormente conocido como Angular 2)

Tenemos una sección separada para el nuevo Angular porque realmente es un framework completamente diferente al de AngularJS. Por ejemplo, cuenta con un sistema de componentes de primera clase, muchos detalles de implementación han sido completamente reescritos, y la API también ha cambiado drásticamente.

### TypeScript

Angular requiere el uso de TypeScript, dado que casi toda su documentación y recursos de aprendizaje están basados ​​en TypeScript. TypeScript tiene sus beneficios obvios: la comprobación de tipo estático puede ser muy útil para aplicaciones a gran escala y puede brindar un gran aumento de productividad para desarrolladores que han trabajado en Java y C#.

Sin embargo, no todo el mundo quiere utilizar TypeScript. En casos de uso de menor escala, la introducción de un sistema de tipo puede resultar en más trabajo que en aumento de productividad. En esos casos sería mejor ir con Vue, ya que el uso de Angular sin TypeScript puede ser un reto.

Por último, aunque no está tan profundamente integrado con TypeScript como Angular, Vue también ofrece [tipados oficiales](https://github.com/vuejs/vue/tree/dev/types) y un [decorador oficial](https://github.com/vuejs/vue-class-component) para aquellos que desean usar TypeScript con Vue. También estamos colaborando activamente con los equipos TypeScript y VSCode de Microsoft para mejorar la experiencia de TS/IDE para los usuarios de Vue + TS.

### Tamaño y desempeño

En términos de desempeño, ambos frameworks son excepcionalmente rápidos y no hay suficientes datos de casos de uso del mundo real para hacer un veredicto. Sin embargo, si estás determinado a ver algunos números, Vue 2.0 parece estar por delante de Angular de acuerdo con este [punto de referencia de terceros](http://stefankrause.net/js-frameworks-benchmark4/webdriver-ts/table.html).

Las versiones recientes de Angular, con compilación AOT y agitación de árboles, han sido capaces de bajar considerablemente su tamaño. Sin embargo, un proyecto Vue 2 con Vuex + Vue Router incluido (~ 30kb comprimido con gzip) es todavía mucho más liviano que una aplicación compilada AOT generada por `angular-cli` (~ 130kb comprimido con gzip) .

### Flexibilidad

Vue es mucho menos dogmático que Angular, ofreciendo soporte oficial para una variedad de sistemas de compilación, sin restricciones sobre cómo estructurar tu aplicación. Muchos desarrolladores disfrutan de esta libertad, mientras que algunos prefieren tener sólo una forma correcta de construir cualquier aplicación.

### Curva de aprendizaje

Para comenzar con Vue, todo lo que necesitas es estar familiarizado con HTML y JavaScript de ES5 (es decir, JavaScript normal). Con estas habilidades básicas, puedes empezar a construir aplicaciones no triviales en menos de un día leyendo [la guía](./).

La curva de aprendizaje de Angular es mucho más pronunciada. La superficie de la API del framework es enorme y como usuario tendrás que familiarizarte con muchos más conceptos antes de ser productivo. Obviamente, la complejidad de Angular se debe en gran parte a su objetivo de diseño de apuntar únicamente a aplicaciones grandes y complejas, pero eso hace que el framework sea mucho más difícil para los desarrolladores menos experimentados.

## Ember

Ember es un framework completo que está diseñado para ser altamente dogmático. Proporciona una gran cantidad de convenciones establecidas y una vez que se está lo suficientemente familiarizado con ellos, se puede hacer que sea muy productivo. Sin embargo, también significa que la curva de aprendizaje es alta y la flexibilidad se ve afectada. Es una desventaja cuando se trata de elegir entre un framework dogmático y una biblioteca con un conjunto de herramientas sueltas que trabajan juntas. Este último te da más libertad, pero también requiere que tomes más decisiones de arquitectura.

Dicho esto, probablemente una mejor comparación sería entre el núcleo de Vue y las capas para [plantillas](https://guides.emberjs.com/v2.10.0/templates/handlebars-basics/) y [modelo de objetos](https://guides.emberjs.com/v2.10.0/object-model/) de Ember:

- Vue proporciona reactividad discreta en objetos JavaScript sencillos y propiedades computarizadas totalmente automáticas. En Ember, es necesario envolver todo en objetos Ember y declarar manualmente las dependencias de las propiedades calculadas.

- La sintaxis para plantillas de Vue aprovecha toda la potencia de las expresiones JavaScript, mientras que la expresión y la sintaxis del ayudante de Handlebars son intencionalmente bastante limitadas en comparación.

- En cuanto a desempeño, Vue supera a Ember [por un buen margen](https://rawgit.com/krausest/js-framework-benchmark/master/webdriver-ts/table.html), incluso después de la última actualización del motor Glimmer en Ember 2.x. Vue procesa automáticamente las actualizaciones, mientras que en Ember necesita administrar manualmente bucles de ejecución en situaciones críticas de desempeño.

## Knockout

Knockout fue un pionero en el MVVM y en espacios de seguimiento de dependencias, su sistema de reactividad es muy similar al de Vue. Su [soporte de navegador](http://knockoutjs.com/documentation/browser-support.html) también es muy impresionante teniendo en cuenta todo lo que hace, ¡con soporte desde IE6! Vue por otro lado sólo soporta desde IE9+.

Con el tiempo, el desarrollo de Knockout se ha ralentizado y ha comenzado a mostrar su edad un poco. Por ejemplo, su sistema de componentes carece de un conjunto completo de ganchos de ciclo de vida y aunque es un caso de uso muy común, la interfaz para pasar los hijos a un componente se siente un poco torpe comparado con [Vue](components.html#Content-Distribution-with-Slots).

También parece haber diferencias filosóficas en el diseño del API que si eres curioso, se puede demostrar por la forma en que cada uno maneja la creación de una [lista de tareas sencillas](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). Definitivamente es algo subjetivo, pero muchos consideran que el API de Vue es menos complejo y está mejor estructurado.

## Polymer

Polymer es otro proyecto patrocinado por Google y de hecho fue una fuente de inspiración para Vue también. Los componentes de Vue pueden ser comparados con los elementos personalizados de Polymer y ambos proporcionan un estilo de desarrollo muy similar. La mayor diferencia es que Polymer se basa en las características más recientes de Web Components y requiere que los polyfills no triviales funcionen (con un desempeño degradado) en los navegadores que no admiten esas funciones de forma nativa. En contraste, Vue trabaja sin ninguna dependencia o polyfills desde IE9.

En Polymer 1.0, el equipo también ha hecho que su sistema de vinculación de datos sea muy limitado para compensar el desempeño. Por ejemplo, las únicas expresiones soportadas en las plantillas Polymer son la negación booleana y las llamadas de método único. Su implementación de la propiedad calculada no es muy flexible.

Los elementos personalizados de Polymer se crean en archivos HTML, lo que limita el uso de JavaScript/CSS (y características de lenguaje compatibles con los navegadores actuales). En comparación, los componentes de archivo único de Vue permiten utilizar fácilmente ES2015 + y cualquier preprocesador CSS que desees.

Cuando se implementa en producción, Polymer recomienda cargar todo sobre la marcha con Importaciones HTML, asumiendo que los navegadores implementan la especificación y el soporte HTTP/2 en el servidor y el cliente. Esto puede o no ser factible dependiendo del público objetivo y del entorno de despliegue. En los casos en que esto no es deseable, tendrá que utilizar una herramienta especial llamada Vulcanizer para agrupar los elementos Polymer. En este frente, Vue puede combinar su característica de componente asíncrono con la función de división de código de webpack para dividir fácilmente partes del paquete de aplicaciones para que cargen por demanda. Esto garantiza la compatibilidad con los navegadores más antiguos, al tiempo que mantiene un gran desempeño de carga de la aplicación.

También es totalmente factible ofrecer una integración más profunda entre Vue con las especificaciones de Web Component tales como Custom Elements y el encapsulado de estilos Shadow DOM. Sin embargo, en este momento seguimos esperando a que las especificaciones maduren y se implementen ampliamente en todos los navegadores principales antes de comprometernos seriamente.

## Riot

Riot 2.0 proporciona un modelo de desarrollo basado en componentes similar (llamado "tag" en Riot), con una API mínima y bellamente diseñada. Riot y Vue probablemente comparten mucho en sus filosofías de diseño. Sin embargo, a pesar de ser un poco más pesado que Riot, Vue ofrece algunas ventajas significativas:

- [Sistema de efectos de transición](transitions.html). Riot no tiene ninguno.
- Un enrutador mucho más potente. El API de enrutamiento de Riot es extremadamente mínimo.
- Mejor desempeño. Riot [atraviesa un árbol DOM](http://riotjs.com/compare/#virtual-dom-vs-expressions-binding) en lugar de utilizar un DOM virtual, por lo que sufre los mismos problemas de rendimiento que AngularJS.
- Soporte de herramientas más maduro. Vue proporciona soporte oficial para [webpack](https://github.com/vuejs/vue-loader) y [Browserify](https://github.com/vuejs/vueify), mientras que Riot se basa en el soporte de la comunidad para el sistema de integración.
