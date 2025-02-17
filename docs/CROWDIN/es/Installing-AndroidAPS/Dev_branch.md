## Rama de desarrollo

<font color="#FF0000"><strong>Atención:</strong></font>
La rama de desarrollo es para el desarrollo adicional de AndroidAPS solamente. Se debe utilizar en un teléfono aparte para probar <font color="#FF0000"><strong>no para un lazo actual.</strong></font>

La versión más estable de AndroidAPS que se debe utilizar es la versión [Master](https://github.com/nightscout/AndroidAPS/tree/master). Se aconseja que permanezca en la rama maestra.

La versión desarrollo de AndroidAPS es sólo para desarrolladores y probadores que se sientan cómodos lidiando con los archivos de registro, buscando a través de archivos de registro y tal vez activando al depurador para producir informes de fallo que son útiles para los desarrolladores (en pocas palabras: gente que sabe lo que están haciendo sin ser asistidos!). Por lo tanto, muchas características inacabadas están inhabilitadas. To enable these features enter **Engineering Mode** by creating a file named `engineering_mode` in directory /AAPS/extra . Enabling the engineering mode might break the loop entirely.

Sin embargo, la rama de desarrollo es un buen lugar para ver qué características se están probando y para ayudar a resolver los errores y dar información sobre cómo funcionan las nuevas funciones en la práctica. A menudo, la gente probará la rama Dev en un teléfono viejo y la bomba hasta que estén seguros de que es estable - cualquier uso de la misma es a su propio riesgo. Al probar cualquier característica nueva, recuerde que está eligiendo probar una característica aún-en-desarrollo. Haga lo mismo a su propio riesgo & con la debida diligencia para mantenerte a salvo.

Si encuentra un error o cree que ha ocurrido algo incorrecto al utilizar la ramificación de desarrollo, visualice la [pestaña de problemas](https://github.com/nightscout/AndroidAPS/issues) para comprobar si alguien más lo ha encontrado o si lo ha añadido si no lo ha hecho. Cuanto más información pueda compartir aquí, mejor (no olvide que es posible que tenga que compartir sus [archivos de registro](../Usage/Accessing-logfiles.md). The new features can also be discussed on [discord](https://discord.gg/4fQUWHZ4Mw).

A dev version has an expiration date. This seems inconvenient when using it satisfactorily, but serves a purpose. When a single dev version doing the rounds, it is easier to keep track of bugs that people are reporting. The developers do not want to be in a position where there are three versions of dev in the wild where bugs are fixed in some and not others, and people continue to report the fixed ones.