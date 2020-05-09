# Manejo de Git para descargas y entregas

En esta pÃ¡gina se explica el mecanismo de entrega para los labs y TPs de la materia. Los puntos clave son:

1.  a cada estudiante y a cada grupo se les asigna **un repositorio privado
    donde realizar el trabajo y subir el cÃ³digo;**

2.  una vez completado el trabajo, **la entrega se realiza desde ese mismo
    repositorio mediante un _pull request;_**

3.  para cada lab o TP, hay un repositorio pÃºblico donde **se proporciona un
    esqueleto o cÃ³digo inicial sobre el que basar la implementaciÃ³n.**

El mÃ©todo aquÃ­ descrito es el Ãºnico mecanismo de entregas vÃ¡lido, y es obligatorio seguir todos los pasos para garantizar que la entrega sea aceptada.
{:.alert .alert-primary}


## Ãndice
{:.no_toc}
* TOC
{:toc .sidetoc}


## Repositorio privado
{: #repos}

Durante la cursada, se proporciona a cada estudiante un repositorio privado en la organizaciÃ³n [fiubatps] de GitHub. Este repositorio suele tener la forma _sisop_aÃ±o_apellido_, por ejemplo: `github.com:fiubatps/sisop_2020a_mendez`.

Para los trabajos prÃ¡cticos en grupo se proporciona un segundo repositorio privado siguiendo el esquema _sisop_aÃ±o_numgrupo_apellidos_, por ejemplo: `github.com:fiubatps/sisop_2020a_g8_mendez_simo`.

### Descarga inicial
{:#clone}

Cada estudiante deberÃ¡ clonar sus repositorios privados en su computadora personal, bien vÃ­a _http_ (con contraseÃ±a), o _ssh_ (con clave privada):

```
# Por HTTP
$ git clone https://github.com/fiubatps/sisop_2020a_mendez

# Por SSH
$ git clone git@github.com:fiubatps/sisop_2020a_g8_mendez_simo
```

Se le puede agregar un segundo parÃ¡metro a _git clone_ para usar un nombre de directorio mÃ¡s corto, por ejemplo â€œlabsâ€ o â€œtpsâ€, respectivamente:

```
$ git clone git@github.com:fiubatps/sisop_2020a_simo labs
$ git clone git@github.com:fiubatps/sisop_2020a_g8_mendez_simo tps
```

Cada operaciÃ³n _git clone_ resulta en un **repositorio local** que es copia del repositorio remoto alojado en GitHub. Para poder conectarse en futuras operaciones, **Git se guarda la direcciÃ³n del repositorio remoto bajo el alias _origin_.** Pueden estar presentes otros repositorios remotos bajo otros alias.
{:.alert .alert-info}

[fiubatps]: https://github.com/fiubatps/


### ElecciÃ³n de rama local
{:#branching}

En la materia se realizan dos tipos de trabajos distintos:

  - los labs individuales, que son relativamente independientes entre sÃ­
  - los trabajos prÃ¡cticos grupales, que suelen implementarse cada uno sobre el
    anterior

AsÃ­, para cada uno de los repositorios privados (individual y grupal) se trabaja de manera distinta en cuanto a ramas se refiere:

  - **para los trabajos prÃ¡cticos, la implementaciÃ³n se realiza directamente
    sobre la rama principal** (normalmente llamada _master)._ Esto quiere decir
    que tras realizar _git clone_, ya se estÃ¡ en la rama adecuada para integrar
    el esqueleto.

  - en cambio, para los labs no es deseable ver la soluciÃ³n de uno de ellos
    mezclada en el pull request de otro (pues son independientes); siendo asÃ­,
    **la implementaciÃ³n de cada lab se realizarÃ¡ en una rama local distinta,
    que deberÃ¡ ser creada con _git checkout -b_**.

Las instrucciones exactas para crear las ramas se proporcionan mÃ¡s adelante, en la secciÃ³n [IntegraciÃ³n del cÃ³digo](#skel-merge){:.alert-link}.
{:.alert .alert-success}

Por otra parte, se recomienda subir de manera periÃ³dica el cÃ³digo a GitHub para que el repositorio remoto actÃºe como copia de seguridad del trabajo:

```
$ git push
```

**AdemÃ¡s, para realizar consultas sobre el cÃ³digo, se debe subir siempre la Ãºltima versiÃ³n al repositorio privado.** De esta manera los docentes puedan consultarlo sin que sea necesario enviarlo por correo.


### AutenticaciÃ³n sin contraseÃ±a
{:#passwordless}

En la configuraciÃ³n estÃ¡ndar, Git pedirÃ¡ una contraseÃ±a cada vez que se comunique con el repositorio remoto en GitHub (ya sea para _push_, _pull_, o _clone_). Hay dos maneras de evitar esto:

  - usar el protocolo _ssh_ en combinaciÃ³n con la herramienta `ssh-agent` del
    sistema, tal y como se explica en la documentaciÃ³n oficial de GitHub, [Connecting to GitHub with
    SSH][gh-ssh-en] (o, en castellano: [Conectar a GitHub con SSH][gh-ssh-es]).

  - utilizar el protocolo _https_ y usar el â€œayudante de credencialesâ€ del
    sistema operativo para almacenar la contraseÃ±a: [Caching your GitHub
    password in Git][gh-https-en] (o, en castellano: [Guardar en cachÃ© tu
    contraseÃ±a de GitHub en Git][gh-https-es]).

[gh-ssh-en]: https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[gh-ssh-es]: https://help.github.com/es/github/authenticating-to-github/connecting-to-github-with-ssh
[gh-https-en]: https://help.github.com/en/github/using-git/caching-your-github-password-in-git
[gh-https-es]: https://help.github.com/es/github/using-git/caching-your-github-password-in-git


## Esqueleto del TP
{:#skel}


### UbicaciÃ³n pÃºblica
{:#skel-repo}

Para cada lab y trabajo prÃ¡ctico se proporcionarÃ¡ un repositorio pÃºblico con un esqueleto sobre el que _obligatoriamente_ realizar la implementaciÃ³n. En cada enunciado, se proporcionarÃ¡ la siguiente informaciÃ³n:

  - la direcciÃ³n del _repositorio_ donde se aloja el esqueleto
  - la _rama_ exacta que contiene el esqueleto a usar para un lab o TP
    en particular
  - si el esqueleto debe integrarse sobre un trabajo anterior ya completado, o
    si se parte de Ã©l desde cero

<div class="alert alert-secondary" markdown="1">
Como ejemplo ilustrativo, podrÃ­a suceder que dos labs hipotÃ©ticos llamados _athena_ y _apollo_ alojasen su cÃ³digo inicial en repositorios distintos:

  - lab _athena:_ repositorio `github.com/fisop/athena`, rama **_master_**
  - lab _apollo:_ repositorio `github.com/fisop/apollo`, rama **_master_**

pero que otros dos labs, _themis_ y _prometheus_, tuvieran sus esqueletos en ramas distintas de un mismo repositorio:

  - lab _themis:_ repositorio `github.com/fisop/titans`, rama **_themis_**
  - lab _prometheus:_ repositorio `github.com/fisop/titans`, rama **_prometheus_**
</div>


### Agregar _remote_
{:#skel-remote}

El paso previo a la descarga del esqueleto es agregar su repositorio pÃºblico como un â€œremoteâ€ cuyo alias haga referencia al lab o TP. AsÃ­, para agregar los repositorios de ejemplo descritos en la secciÃ³n anterior, se deberÃ­a hacer:

```
$ cd labs
$ git remote add esqueleto_athena https://github.com/fisop/athena
$ git remote add esqueleto_apollo https://github.com/fisop/apollo
$ git remote add esqueleto_titans https://github.com/fisop/titans
```

_Nota al margen:_ los alias de estos remotes (_esqueleto_athena_, _esqueleto_apollo_, etc.) son arbitrarios; podrÃ­a haberse usado cualquier otro nombre sea _skel_athena_, _catedra_apollo_ o, meramente, algo bien breve como _titans_.
{:.small}


### IntegraciÃ³n del cÃ³digo
{:#skel-merge}

Para realmente tener el cÃ³digo inicial en el repositorio local, se debe descargar mediante _git fetch_, e **integrar en la rama local adecuada** para que aparezca en el directorio de trabajo. Los pasos siempre son:

1.  pararse en el repositorio privado (individual o grupal) **usando _cd_**
2.  realizar la integraciÃ³n del esqueleto en _master_. Para ello:
    - bajarse los Ãºltimos cambios del esqueleto con **_git fetch_**
    - pararse en la rama _master_ con **_git checkout_**
    - integrar el esqueleto allÃ­ con **_git merge_**
    - crear una rama de referencia a esta integraciÃ³n con **_git branch_**
3.  _(solamente para labs independientes)_{:.text-danger} crear, a partir del
    esqueleto, una rama de trabajo separada con **_git checkoutÂ -b_**
4.  enviar las nuevas ramas al repositorio remoto con **_git push_**

Por ejemplo, si _athena_ y _apollo_ son labs que se implementan de manera independiente en las semanas 2 y 6, se harÃ­a:

```
(Semana 1: clone)
$ git clone git@github.com:fiubatps/sisop_2020a_simo labs

(Semana 2: Lab Athena, en su propia rama)
$ cd labs

$ git fetch -v esqueleto_athena
$ git checkout master
$ git merge --no-ff -m "Integrar esqueleto del lab athena" esqueleto_athena/master
$ git branch base_athena

$ git checkout -b lab_athena esqueleto_athena/master

$ git push -u origin --all

(Semana 6: Lab Apollo, en su propia rama)
$ cd labs

$ git fetch -v esqueleto_apollo
$ git checkout master
$ git merge --no-ff -m "Integrar esqueleto del lab apollo" esqueleto_apollo/master
$ git branch base_apollo

$ git checkout -b lab_apollo esqueleto_apollo/master

$ git push -u origin --all
```

Si, hipotÃ©ticamente, los labs _themis_ y _prometheus_ se implementaran uno sobre el otro, el procedimiento serÃ­a el mismo excepto que se eliminarÃ­a el paso `git checkoutÂ -b`.
{:.small}

Es obligatorio hacer _git merge_ del esqueleto en el repositorio privado (no es suficiente meramente copiar los archivos). **No se corregirÃ¡n trabajos que no compartan el historial de Git con el repositorio pÃºblico.**
{:.alert .alert-danger}


### Directorio de trabajo
{:#workdir}

Una consecuencia de usar ramas indepedientes para los labs es que no es posible trabajar, en un mismo directorio, en mÃ¡s de un lab a la vez. Esto, en general, no constituye un problema, pues no se suele trabajar en mÃ¡s de un lab al mismo tiempo; pero puede resultar molesto en caso de sÃ­ necesitar realizar cambios en dos labs de manera concurrente.

La manera estÃ¡ndar de trabajar con ramas independientes serÃ­a usar _git checkout_ para alternar entre ellas. AsÃ­, si se estuviera trabajando en el lab _apollo_ y se desease realizar algÃºn cambio en el lab anterior _(athena)_, el procedimiento serÃ­a:

```
$ git checkout lab_athena
# realizar los cambios...
$ git commit
$ git checkout lab_apollo
```

Para que esto funcione, el directorio de trabajo debe estar previamente â€œlimpioâ€ (esto es, que _git status_ no reporte cambios). AdemÃ¡s, los archivos del lab _apollo_ desaparecerÃ­an temporalmente del directorio de trabajo hasta que se volviese a la rama original, lo cual puede desorientar al editor o IDE.
{:.small}

*[IDE]: Integrated development environment

**Una alternativa es asignar un directorio distinto a cada lab, pero sin hacer _git clone_ de nuevo.** Git ofrece esta funcionalidad mediante el concepto de _worktree._ A travÃ©s de ellos es posible tener varias â€œcopias de trabajoâ€ de un mismo repositorio, siempre que a cada copia se le asigne una rama distinta.

Para usar _worktrees_ con las ramas de los labs, solamente serÃ­a necesario sustituir la orden _git checkoutÂ -b_ explicada arriba por:

```
(Semana 2.)
...
$ git worktree add -b lab_athena ../athena esqueleto_athena/master

(Semana 6.)
...
$ git worktree add -b lab_apollo ../apollo esqueleto_apollo/master
```

donde `../athena` y `../apollo` representan las rutas donde se alojarÃ¡n las copias de trabajo adicionales (una por cada lab).

AsÃ­, en caso de usar _worktrees_ el directorio principal del repositorio quedarÃ­a siempre en la rama _master_ y, siguiendo el ejemplo de arriba, se crearÃ­an sendos directorios adicionales al mismo nivel que el repositorio principal:

```
$ ls
athena   apollo   labs
```


## Entrega vÃ­a _pull request_
{:#entregas}

Una vez terminado el lab o TP, se debe crear un _pull request_ en Github, bien visitando de manera directa `https://github.com/fiubatps/sisop_<aÃ±o>_<apellido>/compare`; bien con la opciÃ³n **_New pull request_** en la pestaÃ±a _Pull requests_ del repositorio.[^prinfo]

[^prinfo]: Para mÃ¡s informaciÃ³n sobre _pull requests_, consultar la documentaciÃ³n de GitHub, [About pull requests][gh-pull-en], o en castellano: [Acerca de las solicitudes de extracciÃ³n][gh-pull-es].

[gh-pull-en]: https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests
[gh-pull-es]: https://help.github.com/es/github/collaborating-with-issues-and-pull-requests/about-pull-requests

AllÃ­ aparecerÃ¡ una interfaz de creaciÃ³n de _pull request_ en la que se deberÃ¡ hacer tres cosas:

1.  elegir la rama _base_ (a la izquierda)
2.  elegir la rama _compare_ (a la derecha)
3.  una vez elegidas, hacer click en _Create pull request_ y completar los
    siguientes campos: TÃ­tulo, _Reviewers_ y _Assignees_.


### Ramas _base_ y _compare_
{:#prcompare}

Tanto para labs como para TPs, **la rama _base_ es siempre la rama que fue creada en el momento de integraciÃ³n del esqueleto**, concretamente en el paso: â€œcrear una rama de referencia a esta integraciÃ³n con _git branch_â€. Por ejemplo _base_athena_, _base_themis_ o, llegado el caso, _base_tp1_.

En cambio, la rama _compare_ varÃ­a entre labs y TPs:

  - **para los labs, la rama _compare_ es la rama creada con _git checkout
    -b_**, por ejemplo _lab_athena_ o _lab_apollo_.

  - **para los TPs, la rama _compare_ se crea a mano en el momento:**

        $ git push origin master:refs/heads/entrega_themis

    O bien:

        $ git push origin master:refs/heads/entrega_tp2

    AsÃ­, en estos ejemplos la rama _compare_ serÃ­a _entrega_themis_ o
    _entrega_tp2_. **No se debe usar _master_ para este propÃ³sito.**


### Campos a completar
{:#prfields}

Antes de finalizar la creaciÃ³n del _pull request_, se deben completar los siguientes campos:

 - _tÃ­tulo:_
   - para entregas individuales, el nombre del lab mÃ¡s el apellido, siguiendo
     el formato:

         [sisop] lab shell â€“ MÃ©ndez
         [sisop] lab virt â€“ SimÃ³

   - para entregas grupales, se agrega el nÃºmero de grupo a los apellidos,
     siguiendo el formato:

         [sisop] jos tp3 â€“ g8 (MÃ©ndez/SimÃ³)
         [sisop] xv6 alloc â€“ g9 (Fresia/Raik)

  - _assignees:_ dado que los _pull requests_ se crean desde una sola cuenta de
    GitHub, si el trabajo es grupal se debe incluir en este campo al resto de
    integrantes del grupo, a fin de que les lleguen las notificaciones sobre la
    correcciÃ³n.

  - _reviewers:_ en caso de ya contar con un docente asignado para las
    correcciones, se le deberÃ¡ incluir en este campo. En caso contrario, se
    deberÃ¡ seleccionar _fiubatps/sisop-adm_.

<!-- TODO: make format, make test, make entrega. -->

### Mecanismo de correcciÃ³n
{:#codereview}

La correcciÃ³n se realizarÃ¡ a travÃ©s del _pull request_ creado en el paso anterior. AsÃ­, el docente asignado revisarÃ¡ el cÃ³digo y realizarÃ¡ las correcciones y comentarios oportunos. Estos comentarios se recibirÃ¡n por correo electrÃ³nico, y se podrÃ¡n consultar tambiÃ©n a travÃ©s de las interfaces web de GitHub y (en su caso) Reviewable. **Se recomienda la lectura de la correcciÃ³n a travÃ©s de las interfaces web, pues:**

  - aparecerÃ¡n los comentarios junto con el cÃ³digo a que estos se refieren
    (en otras palabras, los comentarios aparecerÃ¡n con el contexto exacto
    en que se realizaron)

  - se podrÃ¡ contestar a los comentarios de manera directa desde la
    interfaz web, lo cual permite saber con exactitud a quÃ© comentario o
    correcciÃ³n se refiere cada respuesta (cosa que no ocurre con igual
    facilidad por correo)

Junto con los comentarios, la correcciÃ³n recibida indicarÃ¡ claramente:

  - si el TP estÃ¡ aprobado o no;
  - en caso de no estar aprobado, quÃ© cambios es imprescindible realizar
    para poder aprobarlo;
  - en caso de estar aprobado, si el corrector aceptarÃ­a cambios
    adicionales para mejorar la nota, y cuÃ¡les son estos cambios.

En caso de no estar claro quÃ© es lo que se estÃ¡ pidiendo, se puede pedir una aclaraciÃ³n con el mecanismo de respuestas en el propio _pull request_, descrito arriba.

#### Reviewable

Algunos docentes utilizan [Reviewable](https://docs.reviewable.io) para revisar el cÃ³digo, en lugar de la interfaz de GitHub. En ese caso, se recomienda leer las correcciones en el sitio web de Reviewable, y no a travÃ©s de GitHub (pues no aparecerÃ­an los comentarios con todo el contexto necesario). Para acceder a Reviewable, no es necesario crear una cuenta; es suficiente con usar la opciÃ³n _Sign in with GitHub_.

Asimismo, los comentarios de Reviewable tienen un concepto de â€œprioridadâ€ (o _disposition_, en inglÃ©s). En general, el significado en las correcciones de la materia es:

  - _blocking_ (marcados con el Ã­cono de prohibiciÃ³n <span class="fa
    fa-minus-circle"></span>): si el TP no estÃ¡ aprobado, se debe corregir
    este Ã­tem para poder aprobar; si estÃ¡ aprobado, para subir la nota se
    deben corregir TODOS los Ã­tems marcados de esta manera.

  - _discussing_ (marcados con el cÃ­rculo vacÃ­o <span class="far
    far-circle"></span>): el Ã­tem merece la pena ser corregido, pero no
    es obligatorio hacerlo.

  - _informing_ (marcados con el Ã­cono de informaciÃ³n <span class="fa
    fa-info-circle"></span>): correcciÃ³n menor o informativa.


### Reentregas y mejoras
{:#reentregas}

En caso de realizarse cambios (para aprobar o para subir nota), estos deben enviarse vÃ­a el mismo pull request que fue creado para la entrega, y no a a travÃ©s de uno nuevo. Esto se consigue haciendo _git push_ a la rama _compare_ del pull request:

```
$ git checkout entrega_tp2
# ...
$ git commit ...
$ git push
```

O bien, si se usan _worktrees:_

```
$ cd shell
# ... realizar cambios
$ git commit ...
$ git push
```


Una vez enviados los cambios con _git push_, se deben hacer dos cosas:

1.  Revisar la lista completa de comentarios realizados por el docente, y
    responder a los mismos indicando si se realizaron los cambios solicitados
    (en los casos mÃ¡s triviales es suficiente con responder _Done_ o _Hecho_;
    Reviewable proporciona un botÃ³n para esto); si corresponde, responder
    tambiÃ©n a cualquier pregunta que hubiera hecho el docente, e indicar quÃ©
    decisiones se tomaron en la (re-)implementaciÃ³n.[^commreply]

    {:.small}
    - En el caso de Reviewable, es importante que no queden â€œpending
      conversationsâ€, esto es, que al enviar los comentarios, Reviewable
      no diga â€œwaiting on ... nombre del alumneâ€.

2.  Una vez respondidos los comentarios, hacer explÃ­cito en el _pull request_
    que Ã©ste estÃ¡ listo para ser revisado de nuevo. Esto se puede hacer al
    tiempo que se envÃ­an los comentarios, incluyendo el acrÃ³nimo PTAL en la
    respuesta (estÃ¡ndar en la industria anglosajona con significado _Please
    take another look_).

    Como alternativa, este paso tambiÃ©n se puede realizar vÃ­a la interfaz
    de GitHub, con el Ã­cono de solicitud de revisiÃ³n (dos flechas en
    cÃ­rculo <span class="fa fa-sync-alt"></span>) que aparece al lado del
    campo _Reviewer_.

[^commreply]: Esta prÃ¡ctica es estÃ¡ndar en la industria, de manera que se le haga fÃ¡cil a la persona que revisa la nueva versiÃ³n saber quÃ© se hizo y quÃ© no, esto es, con quÃ© se va a encontrar.

**Como comentario final**, es especialmente importante realizar las correcciones con gran atenciÃ³n al detalle, para que no suceda que una reentrega empeora la situaciÃ³n (esto es, que en una reentrega dejen de funcionar aspectos que sÃ­ funcionaban antes).


<!--
## En papel
{: #papel}

Las entregas en papel deben seguir los siguientes lineamientos:

  - todas las lÃ­neas de cÃ³digo deben imprimirse, siempre, en tipografÃ­a de paso fijo `(monoespaciada)`.

  - bien visible en la primera hoja (sea carÃ¡tula o no): su nombre y nÃºmero de legajo, nombre de la entrega y fecha en que fue entregada.

  - todas las hojas abrochadas juntas y sin folio.[^1]

[^1]: HabrÃ¡ en el aula una abrochadora a disposiciÃ³n de los alumnos que la necesiten.

Cuando la entrega incluya preguntas a responder **en prosa, se pide respuestas concisas** de dos o tres oraciones simples por pÃ¡rrafo. Ejemplo:

> _Â¿QuÃ© hace la BIOS una vez verificado el hardware? Â¿QuÃ© debe ocurrir a continuaciÃ³n?_
>
> Tras la inicializaciÃ³n del hardware, la BIOS determina el dispositivo de arranque y copia su primer sector (512 bytes) en la direcciÃ³n de memoria fÃ­sica `0x7c00`. A continuaciÃ³n, la ejecuciÃ³n salta a esa posiciÃ³n.
>
> Estos 512 bytes de cÃ³digo, denominados el â€œboot loaderâ€ del kernel, deben ocuparse de inicializar la CPU al estado deseado, y cargar en memoria el resto de instrucciones del kernel.
>
> La incializaciÃ³n de la CPU consiste, normalmente, en el paso de modo real (16-bit) a modo protegido (32-bit). La carga del kernel se suele realizar desde disco.
>
> La Ãºltima instrucciÃ³n del boot loader consiste en saltar al punto de entrada del kernel cargado, que toma asÃ­ el control de la mÃ¡quina.

-->

{% include footnotes.html %}

{::options parse_block_html="true" /}
