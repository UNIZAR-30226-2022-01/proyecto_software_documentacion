# API de usuarios

## /registro
Registro registra un usuario en el sistema. 

**Tipo**: `POST`

**Entrada**: Formulario con campos `nombre` `email` y `password`

**Salida**: `Status 200` si ha habido éxito,  o `Status 500` si ha habido un error junto a su motivo en el cuerpo.

## /login
Login permite acceder a un usuario al sistema.

**Tipo**: `POST`

**Entrada**: Formulario de campos `nombre` y `password`

**Salida**: `Status 200` y una cookie de usuario si ha habido éxito, o `Status 500` si ha habido un error junto a su motivo en el cuerpo.



## /api/listarAmigos
Devuelve una lista con los nombres de los amigos del usuario que genera la solicitud 

**Tipo**: `GET`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**:  `[ string, string, ...]`

## /api/obtenerPerfil/{nombre}
Devuelve la información del perfil de un usuario, definido como parte de la URL.

**Tipo**: `GET`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**:  

        {
    	   "Email": string
    	   "Nombre": string
    	   "Biografia": string
     	   "PartidasGanadas": int
     	   "PartidasTotales": int
     	   "Puntos": int
     	   "ID_dado": int
     	   "ID_ficha": int
        }

## /api/obtenerUsuariosSimilares/{patron}
Devuelve una lista de nombres de usuario que coincidan con un patrón,   especificado en al parámetro "patron" de la URL, ordenados alfabéticamente. Los nombres de usuario coincidirán con dicho patrón o empezarán por él.

**Tipo**: `GET`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**:  `[ string, string, ...]`

## /api/obtenerSolicitudesPendientes
Devuelve una lista de nombres de usuario a los  que se les ha enviado una solicitud de amistad aún pendiente por aceptar o  rechazar.

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**:  `[ string, string, ...]`


## /api/aceptarSolicitudAmistad/{nombre}
Acepta una solicitud de amistad entre el usuario que genera  la solicitud y el indicado en el nombre. 

**Tipo**: `POST`

**Salida**: Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

## /api/rechazarSolicitudAmistad/{nombre}
Rechaza una solicitud de amistad entre el usuario que genera  la solicitud y el indicado en el nombre. 

**Tipo**: `POST`

**Salida**: Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

## /api/enviarSolicitudAmistad/{nombre}
Envía una solicitud de amistad entre el usuario que genera  la solicitud y el indicado en el nombre. 

**Tipo**: `POST`

**Salida**: Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

# API de gestión de partidas

## /api/crearPartida
Crea una nueva partida, para la que se definirá el número máximo de jugadores,  si es pública o privada, y la contraseña en caso de que fuera necesario  

**Tipo**: `POST`

**Entrada**: Formulario con campos `maxJugadores` (número máximo de jugadores),  `tipo` (si la partida es pública o privada con valores `"Publica"` o cualquier otro valor para privada),  y `password` (contraseña necesaria para el acceso a una partida privada)

**Salida**: Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

## /api/unirseAPartida
Permite al usuario unirse a una partida en caso de que no esté en otra, no esté completa la partida, sea pública, o tenga su contraseña si es privada.

**Tipo**: `POST`

**Entrada**: Formulario de campos `password` (contraseña de la partida si es privada) e `idPartida` (identificador de partida obtenido previamente)

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

## /api/abandonarLobby
Deja la partida en la que el usuario esté participando. 

**Tipo**: `POST`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

## /api/obtenerPartidas
Devuelve un listado de partidas codificado en JSON, con el siguiente orden:

 1. Partidas privadas, de más a menos amigos presentes  
 2. Partidas públicas, de más a menos amigos presentes  
 3. Partidas públicas sin amigos: de más a menos jugadores  
 4. Partidas privadas sin amigos: de más a menos jugadores

**Tipo**: `GET`
 
**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.


**JSON de respuesta**:

    [
        "IdPartida": 1,  
        "EsPublica": false,  
        "NumeroJugadores": 4,  
        "MaxNumeroJugadores": 6,  
        "AmigosPresentes": [  
          "amigo1",  
          "amigo2"  
        ],  
        "NumAmigosPresentes": 2  
      },  
      {  
        "IdPartida": 2,  
        "EsPublica": false,  
        "NumeroJugadores": 4,  
        "MaxNumeroJugadores": 6,  
        "AmigosPresentes": [  
          "amigo3"  
        ],  
        "NumAmigosPresentes": 1  
      },  
      {  
        "IdPartida": 3,  
        "EsPublica": true,  
        "NumeroJugadores": 3,  
        "MaxNumeroJugadores": 6,  
        "AmigosPresentes": null,  
        "NumAmigosPresentes": 0  
      }  
    ]

# API de juego


## /api/obtenerEstadoPartida
Devuelve la lista de acciones transcurridas desde la última consulta del usuario hasta  el momento, que deberán ser procesadas en orden.

**Tipo**: `GET`
 
**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**: `[{acción}, {acción}]`. La lista de acciones y su formato en JSON están disponibles en el módulo de logica_juego, en acciones.go

## /api/reforzarTerritorio/{id}/{numTropas}
Refuerza un territorio con su identificador numérico `id` con un valor de tropas numérico  codificado en `numTropas`, ambos parámetros de la URL.

**Tipo**: `POST`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.

## /api/obtenerEstadoLobby/{id}
Devuelve el estado del lobby de una partida identificada por su id.

**Tipo**: `GET`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**:

     {  
      "EnCurso":bool,  
         "EsPublico":bool,  
      "Jugadores":int,  
      "MaxJugadores":int,  
      "NombresJugadores": [string, string, ...]  
     }  
    
## /api/cambiarCartas/{carta1}/{carta2}/{carta3}
Permite a un jugador cambiar un conjunto de 3 cartas por tropas. El número de tropas recibidas  dependerá del número de cambios totales realizados:  
 - En el primer cambio se recibirán 4 cartas  
  - Por cada cambio, se recibirán 2 cartas más que en el anterior  
  - En el sexto cambio se recibirán 15 cartas  
  - A partir del sexto cambio, se recibirán 5 cartas más que en el cambio anterior  

Los cambios válidos son los siguientes:  
   - 3 cartas del mismo tipo  
   - 2 cartas del mismo tipo más un comodín  
   - 3 cartas, una de cada tipo
   
Si el jugador cambia una carta en la que aparece un territorio ocupado por él, se añadirán dos tropas a ese territorio

**Tipo**: `GET`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500`. En cualquier otro caso, se devolverá `Status 200`.


## /api/consultarCartas
Permite al usuario consultar las cartas que tiene en la mano mientras juega una partida . Un usuario podrá consultar únicamente sus propias cartas.

**Tipo**: `GET`

**Salida**:  Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**:

         [  
             {  
                "IdCarta": 1,  
                "Tipo": 0,  
                "Region": 29,  
                "EsComodin": false  
             },  
             {  
                "IdCarta": 20,  
                "Tipo": 1,  
                "Region": 22,  
                "EsComodin": false  
             }, 
             
             ...
     ]

## /api/pasarDeFase
Permite al jugador actual cambiar de fase dentro de su propio turno, siendo estas fases Refuerzo,  ataque y fortificación. Cada fase tendrá unas condiciones especiales para el cambio de turno:  
- En el refuerzo, no podrá cambiar de fase si tiene más de 4 cartas o si le quedan tropas por asignar  
- En el ataque, no podrá cambiar de fase si tiene más de 4 cartas o si tiene que ocupar un territorio y aún no lo ha hecho.  
 - En la fortificación podrá cambiar de fase (dándole el turno a otro jugador) libremente  
 
**Tipo**: `GET`

**Salida**:  Si no es el turno del jugador, no está en una partida o no se cumplen las condiciones para el cambio de fase, devolverá un `Status 500`, en otro caso devolverá `Status 200`.


## /api/obtenerNotificaciones
Devuelve un listado codificado en JSON de notificaciones  a mostrar, relativas al usuario que lo solicita.

**Tipo**: `GET`

**Salida**: Si se produjera un error durante el procesado, se devolverá `Status 500` . En cualquier otro caso, se devolverá `Status 200` y un JSON de respuesta.

**JSON de respuesta**: `[notificacion1..., notificacion2...]`. La lista de notificaciones y su formato en JSON están disponibles en el módulo de logica_juego, en notificaciones.go


## /api/atacar_territorio/{id_territorio_origen}/{id_territorio_destino}/{n_dados_atacante}
Por implementar en la segunda versión.

## /api/ocupar_territorio/{num_tropas}
Por implementar en la segunda versión.
## /api/fortificar/{id_territorio_origen}/{id_territorio_destino}/{n_tropas}
Por implementar en la segunda versión.

