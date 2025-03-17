# Laboratorio 0: "Aplicación cliente" - FaMAF (2025)

## **Links empleados para testear [`hget.py`](./hget.py)**
- http://httpforever.com/
- http://www.google.com/
- http://example.com/
- http://www.textfiles.com/
- http://www.example.org/
- http://info.cern.ch/
- http://www.webpagesthatsuck.com/
- http://pagina-web.com/
- http://httpbin.org/
- http://www.edu4java.com
- http://web.archive.org/
- http://www.hipertexto.info
- http://www.milliondollarhomepage.com/

## **Punto estrella**
### ¿Qué mecanismos permiten funcionar a nombres de dominio como `http://中文.tw/` o `https://💩.la`?
#### IDNA: Internationalized Domain Name in Applications
El funcionamiento de dominios con emojis o letras de otros idiomas es posible gracias al estándar **IDNA** que predomina actualmente en Internet.  

En principio, los nombres de dominio tradicionales solo permiten caracteres ASCII (es decir, A-Z, 0-9 y "-"). No obstante, a medida que el Internet se volvía 
un fenómeno más masivo y popular a lo largo del mundo, fue necesario permitir que otros países cuyos idiomas no contienen estos caracteres latinos (por ejemplo la cultura japonesa, rusa, árabe, etc.) también pudieran formar sus propios dominios.

Es por ello que en 2003 se propone el estándar **IDNA** (siglas de *Internationalized Domain Name in Applications* o **nombre de dominio internacionalizado en aplicaciones** en español), el cual permite usar caracteres no ASCII en los nombres de dominio.
<hr>

#### ¿Cómo IDNA lidia con aquellos nombres de dominios que contienen caracteres no ASCII?
El susodicho estándar trabaja bajo tres algoritmos a destacar: *Nameprep*, *ToASCII* y *ToUnicode*.
  - **Nameprep** es un algoritmo de normalización de nombres de dominio Unicode que se encarga de:
    - convertir todas las etiquetas del dominio a minúsculas.
    - convertir caracteres compatibles en sus formas canónicas equivalentes.
    - eliminación de caracteres prohibidos como control characters o espacios no estándar.
    - manejo de caracteres bidimensionales (Bidi), para evitar problemas con idiomas que escriben de derecha a izquierda como el árabe o el hebreo.

  - **ToASCII** es aplicado únicamente si la etiqueta no es utilizable en DNS (caso contrario, esta etiqueta ya estaría en formato ASCII y no requeriría cambio alguno). En principio utilizará el algoritmo *nameprep* para normalizar el nombre del dominio y se traducirá el resultado a ASCII usando **Punycode** antes de anteponer la cadena de cuatro caracteres `xn--`. Esta cadena de cuatro caracteres es conocida como prefijo `ACE` (por sus siglas *ASCII Compatible Encoding* o **codificación compatible con ASCII**) y se utiliza para distinguir las etiquetas codificadas en Punycode de las etiquetas ASCII ordinarias.

  - **ToUnicode** revierte la acción de *ToASCII*, quitando el prefijo `ACE` y aplicando el algoritmo de decodificación *Punycode*. No revierte el procesado de *Nameprep* ya que este último es una normalización, y por lo tanto, es irreversible.
<hr>

#### ¿Entonces cuál sería la codificación de los dos dominios puestos como ejemplo?
Existe una herramienta online llamada [**Punycoder**](https://www.punycoder.com/), muy útil a la hora de realizar conversiones de *Unicode* a *ASCII/Punycode* y viceversa. Si le pasamos los dominios de ejemplo, se obtienen los siguientes resultados:

| Texto | Punycode |
| :---: | :---: |
| http://中文.tw/ | http://xn--fiq228c.tw/ |
| https://💩.la | https://xn--ls8h.la/ |

Puede comprobarse que ambos enlaces redirigen hacia la misma página, demostrando la eficacia del estándar *IDNA*.
<hr>
