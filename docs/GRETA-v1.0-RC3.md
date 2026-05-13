# GRETA
## Guía de Referencia de Equipos de Trabajo Afectivo

**Estado:** release candidate
**Versión:** 1.0-RC3
**Cambios respecto a RC2:** incorporación de la capa de impacto (sección 12) y los guardarraíles de la IA (sección 13)

---

## 1. Qué es GRETA

GRETA es un framework de diagnóstico y composición de equipos de ingeniería basado en cuatro dimensiones. Su propósito es dar a quien lidera una lectura estructurada de su equipo — no como una foto de competencias individuales, sino como un sistema vivo con gaps, fortalezas y dinámicas propias.

Normalmente se montan los equipos teniendo en cuenta una sola dimensión: la seniority. GRETA propone que un equipo bien compuesto necesita equilibrio en cuatro dimensiones simultáneas, y que ignorar cualquiera de ellas produce problemas que la seniority sola no puede resolver.

GRETA no es un cuestionario ni una herramienta de evaluación de personas. Es una guía de lectura para quien lidera.

**Las personas son la estrategia.** Todo lo demás — la tecnología, el producto, el modelo de negocio — depende de que las personas que lo construyen estén bien, se complementen y trabajen en las mejores condiciones posibles. GRETA existe para ayudar a que eso ocurra.

---

## 2. Principios éticos de uso

Estos principios no son una introducción que se lee una vez y se olvida. Son el núcleo de cómo se usa GRETA. Si no se respetan, el framework deja de ser útil y se convierte en algo dañino. Se repiten y referencian a lo largo de todo el documento porque su violación, aunque sea involuntaria, invalida el diagnóstico y rompe la confianza del equipo.

**GRETA lo gestiona quien lidera — con transparencia y criterio.**
GRETA es una herramienta de quien lidera, no un sistema de clasificación que el equipo juega ni un secreto que se oculta. Puede compartirse con transparencia. Lo que se gestiona con criterio es qué se comparte, cómo y con quién.

Lo más objetivo — cobertura de conocimiento, bus factor[^busfactor], composición de roles de contribución — puede compartirse con más libertad porque tiene base observable y verificable. Lo que requiere más cuidado son los niveles individuales por dimensión: no porque sean confidenciales, sino porque su publicación comparativa genera la dinámica equivocada. Si el equipo sabe que Marta está en Veteranus y Luis en Peritus en la dimensión de conocimiento, la conversación inevitable es "¿por qué ella y no yo?", "¿es porque le hace la pelota?", "¿qué tengo que hacer para subir?". Eso convierte el framework en un mecanismo de competición y comparación — exactamente lo contrario de lo que busca.

Si alguien pregunta en una O2O[^o2o] por qué tiene un nivel determinado o por qué otra persona tiene uno superior, GRETA proporciona las herramientas para responder con comportamientos concretos observados, proponer objetivos claros de mejora y dejar muy claro que los niveles son etiquetas de tendencia, no medallas. El objetivo no es competir por alcanzar el siguiente nivel — es mejorar, aportar de forma más equilibrada y orgánica, y hacerlo porque tiene sentido para la persona y para el equipo.

**El seguimiento frecuente no es opcional — es el método.**
Un diagnóstico construido sobre pocas observaciones es una señal falsa. Como cuando se convierte una señal analógica en digital: si se toman muestras con poca frecuencia, la señal digital no reproduce la realidad — solo instantes aislados. Cuanta más frecuencia de observación, más fiel es el mapa. GRETA no funciona con revisiones trimestrales. Funciona con observación continua y conversaciones regulares con las personas del equipo. No hace falta llamarlas O2Os ni formalizarlas como tales — lo importante es que ocurran con suficiente frecuencia para percibir las señales.

**En la O2O nunca se habla de niveles — se habla de comportamientos concretos.**
El nivel lo usa internamente quien lidera para orientar la conversación, nunca como diagnóstico que compartir. No "estás en Novicius". Sino "he notado que cuando el equipo tiene un conflicto, tiendes a salir de la conversación — ¿cómo lo ves tú?". La O2O habla de realidades observadas, de puntos de mejora concretos, de avances reales. Nunca de clasificaciones.

**Los niveles no son jerarquía ni clases sociales.**
Nadie es más que nadie. Los niveles describen lo que una persona aporta habitualmente en cada dimensión — no lo que vale como profesional ni como persona. Un nivel alto no otorga superioridad. Un nivel bajo no define un techo. Alguien en Novicius puede tener un momento Expertus. Alguien en Veteranus puede bloquearse ante algo que nunca ha visto. Los niveles son una lectura de tendencias, no un juicio permanente. Y cada persona puede estar en niveles muy distintos en cada dimensión — esa combinación única es lo que hace que los equipos se complementen.

**Cada persona puede estar en niveles distintos en cada dimensión.**
No existe "el nivel de una persona". Existen cuatro lecturas simultáneas e independientes. Alguien puede ser Veteranus en conocimiento, Novicius en rol de contribución, Peritus en seniority y Expertus en la dimensión emocional. El mapa completo es la suma de las cuatro, y esa combinación es única para cada persona.

**El diagnóstico caduca.**
Los roles de contribución evolucionan, el conocimiento se actualiza, la madurez crece, la dimensión emocional fluctúa. Un mapa que no se actualiza con regularidad es una foto vieja que puede llevar a decisiones equivocadas. GRETA es un sistema vivo, no un archivo.

**La subjetividad no se elimina — se gestiona.**
Quien lidera tiene tendencias naturales: se relaciona más con unas personas que con otras, hay personas más introvertidas o tímidas con las que la relación es naturalmente menos fluida, y eso introduce sesgo en el diagnóstico. No es un defecto de quien lidera — es una condición humana. Lo importante es ser consciente de ello y compensarlo: buscando el feedback de otros observadores, practicando la autocrítica y la reflexión regular, y prestando atención especial a las personas con las que la relación es más escasa o distante. La dimensión emocional en particular requiere este cuidado activo. Un liderazgo afectivo o humanista no garantiza la objetividad — pero crea las condiciones para aproximarse a ella.

---

## 3. Los tres escenarios operativos

GRETA sirve para tres situaciones que quien ejerce el rol de HoE[^hoe] puede vivir en paralelo dentro del mismo proyecto:

| Escenario | Descripción | Riesgo principal |
|-----------|-------------|-----------------|
| **Heredar** | Llegar con un equipo ya formado y leer lo que hay | Llegar con conclusiones previas antes de observar |
| **Formar** | Construir un equipo nuevo desde el mapa de gaps | Optimizar en papel y fallar en las personas |
| **Modificar** | Intervenir en un equipo que no está funcionando | Operar en un sistema vivo sin entender las dinámicas instaladas |

---

## 4. La escala de niveles

Las cuatro dimensiones comparten una escala común de siete niveles. Los niveles tienen asignado un color del espectro visible: tonos cálidos (rojo, naranja, amarillo) indican niveles iniciales; tonos fríos (azul, violeta) indican niveles avanzados. La mezcla de colores en el mapa de equipo indica diversidad saludable.

```
GRUPO 1 — Ejecuta con guía
  🔴 Tiro       →  empieza, necesita estructura y supervisión
                   (habitualmente alguien en prácticas o recién salido de universidad/FP/bootcamp)
  🟠 Novicius   →  gana soltura, empieza a tener autonomía parcial

GRUPO 2 — Ejecuta con autonomía
  🟡 Peritus    →  autónomo en lo conocido, resuelve sin ayuda
  🟢 Expertus   →  autónomo y criterioso, empieza a anticipar

GRUPO 3 — Decide y anticipa
  🔵 Veteranus  →  ha visto suficiente para prever, referente en su área
  🟣 Primus     →  multiplica a otros, guía sin necesitar jerarquía

GRUPO 4 — Transforma
  ⚪ Magister   →  nivel aspiracional, trasciende el equipo inmediato
```

**Sobre el Magister:** es un nivel aspiracional. Quizás se alcance en momentos puntuales porque nadie puede mantener esa regularidad de forma sostenida. No es un título que se otorga — es un estado que se reconoce. Se representa en blanco/plata porque trasciende el espectro ordinario.

**Sobre los tránsitos entre niveles:** alguien puede estar claramente entre dos niveles. Eso también es información útil — describe una dirección y un ritmo de evolución.

---

## 5. Las cuatro dimensiones

### Dimensión 1 — Rol de contribución

Describe cómo aporta cada persona al equipo. No qué sabe, sino cómo se comporta y qué función cumple dentro del grupo como sistema.

GRETA utiliza la taxonomía de roles de Belbin[^belbin] como referencia para esta dimensión. La taxonomía es una referencia interna del framework — no algo que el equipo necesita conocer.

**Los nueve roles de referencia:**

| Categoría | Rol | Contribución principal | Debilidad natural |
|-----------|-----|----------------------|------------------|
| Mental | Cerebro (PL[^pl]) | Ideas originales, creatividad disruptiva | Ignora detalles, no comunica bien |
| Mental | Monitor evaluador (ME[^me]) | Análisis frío, juicio estratégico | Frío, poco motivador |
| Mental | Especialista (SP[^sp]) | Conocimiento técnico profundo | Visión de túnel |
| Social | Coordinador (CO[^co]) | Clarifica objetivos, delega, integra | Puede parecer manipulador |
| Social | Investigador de recursos (RI[^ri]) | Extrovertido, busca oportunidades externas | Pierde energía rápido, no profundiza |
| Social | Cohesionador (TW[^tw]) | Mantiene la armonía, escucha, media | Indeciso en momentos críticos |
| Acción | Impulsor (SH[^sh]) | Presiona, supera obstáculos, urgencia | Irritable, hiere sensibilidades |
| Acción | Implementador (IMP[^imp]) | Transforma ideas en planes concretos | Inflexible, lento en adaptarse |
| Acción | Finalizador (CF[^cf]) | Perfeccionismo, cierra sin errores | Ansioso, delega mal |

**Puntuación de cobertura del equipo:**
- Rol primario de una persona: peso 1,0
- Rol secundario de una persona: peso 0,5
- Score del rol = suma ponderada de quienes lo ejercen

**Impacto de la IA[^ia] en esta dimensión:**
Con la irrupción de la IA, los roles sociales ganan peso relativo. El Especialista puro que aporta únicamente conocimiento técnico específico pierde parte de su valor diferencial porque ese conocimiento es parcialmente delegable a la IA.

**Patrones frecuentes en equipos técnicos:**
Los roles sobrerepresentados son ME, SP e IMP. Los crónicamente ausentes son TW, RI, CF y CO. Belbin llamó a esto Apollo Syndrome: máximo talento individual, mínima complementariedad, resultado por debajo del potencial.

*Recordatorio: en la O2O se habla de comportamientos concretos, nunca de niveles.*

**Comportamientos observables y repetidos por nivel:**

🔴 *Tiro* — actúa por instinto, sin consciencia de su patrón. Necesita indicaciones en cada situación de equipo.

🟠 *Novicius* — empieza a reconocer en qué situaciones se siente más cómodo. Identifica su rol cuando alguien se lo señala. Dificultad para ejercer roles distintos al suyo.

🟡 *Peritus* — conoce su rol y lo ejerce de forma consistente. Entiende cómo afecta al equipo en situaciones conocidas. Empieza a identificar los patrones de los demás.

🟢 *Expertus* — ejerce su rol primario con soltura y desarrolla un rol secundario. Adapta su comportamiento cuando detecta que el equipo necesita algo distinto.

🔵 *Veteranus* — reconoce qué falta en cada momento. Cubre huecos sin que nadie se lo pida. Describe con precisión los patrones de su equipo. Comparte su lectura espontáneamente.

🟣 *Primus* — referente en cómo ejercer los roles con consciencia. Ayuda a otros a identificar su rol sin imponerlo. Detecta cuando un rol genera fricción y actúa. Acompaña de forma deliberada.

⚪ *Magister* — ha trascendido su rol natural, puede ejercer cualquiera cuando el equipo lo necesita. Diseña equipos con la composición de roles como variable crítica. Mentoriza a otras personas con rol de liderazgo.

---

### Dimensión 2 — Conocimiento

Describe qué sabe el equipo colectivamente. El foco es el equipo como unidad: qué capas están cubiertas, cuáles tienen un único responsable y cuáles no existen.

**El cambio de paradigma con la IA:**
El conocimiento técnico específico ha perdido peso como diferenciador. Los fundamentos de ingeniería de software, la capacidad de razonamiento técnico y el criterio para evaluar lo que la IA produce son ahora el diferencial real.

**Áreas de referencia:**

| Grupo | Área |
|-------|------|
| Ingeniería | Fundamentos de SW[^sw] (patrones, SOLID[^solid], arquitecturas), Algoritmos y estructuras de datos, Testing[^testing] |
| Bases de datos | SQL[^sql], NoSQL[^nosql], Bases vectoriales |
| APIs[^api] y comunicación | REST[^rest], GraphQL[^graphql]/gRPC[^grpc], Mensajería |
| Frontend[^frontend] | Fundamentos web (HTML[^html]/CSS[^css]/JS[^js]), Componentes y frameworks UI[^ui], Accesibilidad |
| Operaciones | Cloud[^cloud]/Infra[^infra], CI/CD[^cicd], Observabilidad, Contenedores |
| Seguridad | OWASP[^owasp], autenticación/autorización, compliance[^compliance], GDPR[^gdpr] |
| Arquitectura | Diseño de sistemas, escalabilidad, trade-offs[^tradeoffs] |
| Calidad | QA[^qa], Performance testing |
| IA aplicada | Fundamentos de LLM[^llm], RAG[^rag], prompting[^prompting], evaluación de output |

**Bus factor por área:**

| Bus factor | Interpretación |
|-----------|---------------|
| 0 | Gap crítico — el conocimiento no existe |
| 1 | Crítico — si esa persona falta, el conocimiento se pierde |
| 2-3 | Aceptable |
| 4+ | Robusto |

**Nivel de dominio individual por área:**

| Grado | Descripción |
|-------|-------------|
| Básico | Conoce los conceptos y puede aplicarlos con guía |
| En progreso | Los aplica con autonomía en casos conocidos, aún con gaps |
| Sólido | Los aplica con criterio en casos nuevos, puede enseñarlos |

**Perfil de conocimiento individual:**

| Perfil | Áreas sólidas | Descripción |
|--------|--------------|-------------|
| I-shape | 1-2 | Especialista puro. Riesgo de silo. |
| T-shape | 3-5 | Especialidad + anchura. Perfil sano. |
| π-shape | 6-9 | Doble especialidad + anchura. Perfil senior sano. |
| Comb | 10+ | Visión sistémica completa. |

**Comportamientos observables y repetidos por nivel:**

🔴 *Tiro* — aplica soluciones vistas sin entender por qué funcionan. Necesita guía para interpretar el output de la IA. Conocimiento de capa superficial.

🟠 *Novicius* — empieza a entender el porqué. Usa la IA sin cuestionarla sistemáticamente. Reconoce sus gaps y los dice.

🟡 *Peritus* — fundamentos sólidos en su área principal. Detecta errores obvios en el output de la IA. Aprende de forma autónoma. Comparte cuando se le pregunta.

🟢 *Expertus* — aplica fundamentos para resolver lo nuevo. Usa la IA como multiplicador, sabe cuándo no usarla. Detecta alucinaciones y errores sutiles. Comparte con iniciativa.

🔵 *Veteranus* — su conocimiento está en saber qué preguntar. Detecta cuándo una solución técnicamente correcta es estratégicamente equivocada. Comparte y acompaña espontáneamente.

🟣 *Primus* — referente no por saber más sino por ayudar a pensar mejor. Define qué conocimiento necesita desarrollar el equipo. Su criterio sobre la IA orienta al resto.

⚪ *Magister* — genera conocimiento nuevo. Mentoriza de forma deliberada. Su mayor satisfacción es que otros resuelvan gracias a su acompañamiento.

---

### Dimensión 3 — Seniority

Describe la madurez profesional — no el título organizativo. El criterio no son los años sino los contextos: proyectos, empresas, tipos de problemas, situaciones de crisis.

Un CV orientado a proyectos, retos afrontados y problemas resueltos es un input valioso para esta dimensión. La app puede extraer esta información con IA y sugerir preguntas dirigidas.

**Las tres capacidades que estructuran esta dimensión:**

*Anticipación* — ver el problema antes de que sea problema.
*Resiliencia ejecutiva* — enfrentarse a lo desconocido sin bloquearse.
*Criterio bajo incertidumbre* — decidir bien cuando no hay certeza.

**Pruebas de exploración de seniority:**

*Prueba de anticipación:* situación aparentemente completa. Se observa si identifica riesgos no mencionados e implicaciones no contempladas.

*Prueba de ambigüedad deliberada:* problema con información incompleta o contradictoria. Se observa si se lanza, se paraliza, pide clarificación o propone alternativas.

*Prueba de crisis simulada:* urgencia con tiempo limitado. Se observa cómo prioriza, comunica y decide bajo presión.

*Prueba de retrospectiva:* narrar un fracaso propio. Se observa si asume responsabilidad, extrae aprendizaje real y muestra qué cambió en ella o él.

*Nota:* son indicativas, no definitivas. Se contrastan con observación continuada.

**Comportamientos observables y repetidos por nivel:**

🔴 *Tiro* — ejecuta en contexto estable. Se paraliza o escala ante lo inesperado. No anticipa.

🟠 *Novicius* — empieza a entender el contexto más amplio. Resuelve lo conocido antes de escalar. Detecta algunos riesgos evidentes.

🟡 *Peritus* — autónomo en lo complejo conocido. Estructura el análisis antes de actuar. Anticipa en su área con regularidad.

🟢 *Expertus* — reconoce patrones de riesgo antes de que sean problemas. Decide con información incompleta. Anticipa en áreas adyacentes.

🔵 *Veteranus* — anticipación sistémica, ve lo que nadie mira. Referencia de calma en la crisis. Decide con convicción y revisa sin drama.

🟣 *Primus* — previene problemas que el equipo nunca verá. Su presencia cambia el estado del equipo en la crisis. Multiplica la resiliencia del equipo.

⚪ *Magister* — ha vivido ciclos completos. Modelo mental que funciona en lo desconocido. Forma a otros en la toma de decisiones bajo incertidumbre.

---

### Dimensión 4 — Emocional

Describe cómo está cada persona dentro del equipo. Tres subdimensiones: motivación, alineamiento y metaconsciencia.

*Motivación* — intrínseca vs extrínseca. No se juzga — se entiende para acompañar mejor.

*Alineamiento* — ¿comparte el propósito? Se lee entre líneas — en cómo habla del producto, en si usa "nosotros" o "ellos".

*Metaconsciencia* — ¿tiene consciencia de cómo afecta al sistema? Dos fallos opuestos: síndrome del impostor (infraestima) y síndrome de Narciso (sobreestima — superioridad por edad, sexo, experiencia, necesidad de adulación, intolerancia a la crítica).

**Advertencias críticas:**

*Requiere tiempo y relación.* El borrador inicial debe marcarse como provisional.

*Requiere múltiples observadores.* La percepción de una sola persona que lidera es limitada.

*El sesgo jerárquico es inevitable.* La persona filtra lo que muestra ante quien percibe como autoridad.

*Es la dimensión más dinámica.* La cadencia de revisión no puede estandarizarse.

**Comportamientos observables y repetidos por nivel:**

🔴 *Tiro* — motivación externa. Sin reflexión sobre por qué está aquí. Sin consciencia de su impacto en los demás. El síndrome del impostor, si existe, paraliza sin que sepa nombrarlo.

🟠 *Novicius* — identifica qué le da energía. Noción vaga del propósito. Percibe su impacto en situaciones evidentes. Síndrome del impostor frecuente — se retiene, no se siente con potestad para aportar.

🟡 *Peritus* — articula su motivación cuando se le pregunta. Conecta el propósito con su trabajo. Detecta la fricción que genera, aunque tarde. Gestiona el feedback sin drama.

🟢 *Expertus* — motivación mayoritariamente intrínseca. Habla en primera persona del plural. Ajusta su comportamiento cuando detecta su impacto. Riesgo de deriva: primeros síntomas del síndrome de Narciso.

🔵 *Veteranus* — conoce sus patrones emocionales. Detecta cuándo su motivación baja y lo gestiona. Imagen ajustada de sí misma o de sí mismo. Presencia estabilizadora sin necesitar protagonismo. Riesgo de deriva: síndrome de Narciso plenamente instalado — experiencia como argumento de autoridad, superioridad por atributos, intolerancia a la crítica.

🟣 *Primus* — su energía es una palanca consciente. Detecta el estado emocional de los demás y actúa sin invadir. Reconoce el síndrome del impostor en otros y crea espacio para que aporten. El síndrome de Narciso invalida este nivel.

⚪ *Magister* — relación madura con su motivación, sin necesitar validación externa. Lee el estado del equipo como sistema. Crea condiciones para que otros desarrollen su dimensión emocional. El síndrome de Narciso es estructuralmente incompatible.

---

## 6. Cómo se relacionan las cuatro dimensiones

- **Seniority → Rol de contribución:** la carrera técnica selecciona ciertos roles. Un equipo homogéneo en seniority tiende a ser homogéneo en roles de contribución.
- **Seniority → Conocimiento:** niveles bajos aportan conocimiento estrecho pero fresco. Niveles altos tienen conocimiento amplio pero pueden tener áreas desactualizadas.
- **Rol de contribución → Conocimiento:** el rol natural determina cómo alguien expande su mapa. Un RI aprende en anchura. Un SP en profundidad. Un IMP aprende lo justo para ejecutar.
- **Emocional → Todo lo demás:** es un multiplicador — positivo o negativo — de las otras tres. Una persona con alta motivación intrínseca y metaconsciencia desarrollada puede compensar gaps en las otras tres de formas que ningún framework predice.

Un equipo bien compuesto está equilibrado en las cuatro dimensiones. La mayoría de quienes lideran equipos técnicos solo gestionan la tercera.

---

## 7. Tamaño del equipo

**5-6 personas — tamaño de referencia:** Belbin identificó este rango como el más favorable para la cobertura natural de roles de contribución.

**Por encima de 6:** la superposición de roles se incrementa. Requiere gestión activa para que no genere conflicto ni redundancia.

**Por debajo de 5:** habrá roles de contribución sin cubrir. El diagnóstico debe incluir explícitamente cómo se están compensando.

---

## 8. Inputs para el diagnóstico

- **Observación directa:** el input principal y más valioso
- **Conversaciones regulares:** exploratorias, nunca clasificatorias. Si se transcriben, la app las procesa
- **CV y trayectoria:** orientado a proyectos, retos y problemas resueltos. La app extrae contexto con IA
- **Logros y contribuciones documentados:** procesables por la IA para refinar el diagnóstico
- **Cuestionario Belbin:** opcional, como hipótesis de punto de partida
- **Feedback de otras personas:** especialmente valioso para la dimensión emocional

---

## 9. Aplicación por escenario

### 9.1 Heredar un equipo

**Fase 1 — Observación (semanas 1-2)**
Observar en reuniones, código, comunicación, reacción ante problemas. Señales: ¿quién propone, cuestiona, ejecuta? ¿Quién calma o genera tensión? ¿Cómo hablan del producto — "nosotros" o "ellos"?

**Fase 2 — Conversaciones de diagnóstico (semanas 2-4)**
El framework guía las preguntas pero nunca se muestra. Una pregunta por dimensión:
- Rol: "¿Cuál es la parte del trabajo en equipo que más te gusta? ¿Y la que más te cuesta?"
- Conocimiento: "¿En qué áreas te sientes más sólida/sólido? ¿Dónde tienes más margen de crecer?"
- Seniority: "¿Cuál ha sido el momento más difícil en un proyecto? ¿Cómo lo gestionaste?"
- Emocional: "¿Qué te engancha más de este proyecto? ¿Hay algo que te quite energía?"

**Fase 3 — Mapa completo (mes 1-2)**
Construir el mapa de las cuatro dimensiones. La dimensión emocional se marca como borrador provisional.

### 9.2 Formar un equipo

1. Partir del mapa de gaps
2. Identificar el perfil que cubre más gaps simultáneamente en las cuatro dimensiones
3. Diseñar la entrevista para validar ese perfil — no solo competencias técnicas

**Cómo detectar el rol de contribución en entrevista:**

| Para detectar... | Preguntar sobre... |
|-----------------|-------------------|
| Cerebro (PL) | Una solución no convencional que propuso y nadie esperaba |
| Monitor evaluador (ME) | Una decisión que ralentizó para evitar un error que nadie más vio |
| Cohesionador (TW) | Una situación de conflicto en el equipo y cómo actuó |
| Impulsor (SH) | Un momento en que el equipo estaba parado y cómo lo desbloqueó |
| Finalizador (CF) | Cómo gestiona el cierre de proyectos y los detalles finales |
| Investigador (RI) | Cómo se mantiene al día y qué conexiones externas tiene |

**Cómo explorar la dimensión emocional en entrevista:**

| Para explorar... | Preguntar sobre... |
|-----------------|-------------------|
| Motivación | "¿Qué te hizo salir de tu último proyecto? ¿Qué buscas en el siguiente?" |
| Alineamiento | "¿Qué sabes de lo que estamos construyendo y qué te parece?" |
| Metaconsciencia | "¿Cuál fue el momento en que más afectaste al equipo, positiva o negativamente?" |

### 9.3 Modificar un equipo

**Principios:** diagnosticar antes de actuar, identificar qué funciona antes de tocar nada, una modificación a la vez, revisar la dimensión emocional antes de cualquier cambio estructural.

**Palancas:**

| Palanca | Cuándo usarla |
|---------|--------------|
| Contratación | Gap estructural que no puede cubrirse internamente |
| Redistribución de responsabilidades | Rol presente en alguien que no lo está ejerciendo |
| Formación técnica | Bus factor crítico resoluble internamente |
| Acompañamiento emocional | Dimensión emocional comprometida con potencial real |
| Cambio de rol o equipo | Perfil muy mal ubicado en su rol natural |
| Baja o salida | Último recurso |

---

## 10. Outputs del diagnóstico

1. **Mapa del equipo** — cuatro dimensiones con colores. Una foto del sistema, no un juicio.
2. **Lista de gaps por prioridad** — críticos, medios, bajos.
3. **Recomendaciones de acción** — palanca, orden, objetivo concreto.
4. **Score de salud** — tres primeras dimensiones cuantificables; dimensión emocional cualitativa.

---

## 11. Límites de GRETA

- **No mide rendimiento individual.** Dos equipos con el mismo mapa pueden rendir muy diferente.
- **No sustituye la relación.** Quien aplica el framework sin la relación produce diagnósticos correctos y decisiones frías.
- **Depende de la calidad de la observación.** Un observador sesgado o poco frecuente produce un mapa falso.
- **Es una foto, no una película.** Sin frecuencia, caduca.

**Las personas son la estrategia.** Un equipo bien compuesto, que colabora con confianza y trabaja en las mejores condiciones posibles, no es un medio para alcanzar el producto — es el producto en sí.

---

## 12. La capa de impacto

### Output, outcome e impacto — tres conceptos que no son lo mismo

Antes de describir cómo funciona esta capa, es necesario distinguir tres conceptos que se confunden con frecuencia y cuya confusión es precisamente el problema que GRETA quiere evitar.

**Output** — lo que produce el equipo. Features entregadas, tickets cerrados, código escrito, documentación generada. Es una medida de actividad. Describe lo que hace el equipo, no el valor que genera. Los outputs son necesarios, pero medir outputs como proxy de impacto es uno de los errores más comunes y más dañinos en la gestión de equipos de ingeniería.

**Outcome** — el cambio que genera ese output en el mundo. Lo que mejora para el usuario, el negocio o el equipo como consecuencia de lo que se ha producido. Un outcome bien definido describe algo que cambia, no algo que se hace.

**Impacto** — el efecto acumulado y sostenido de los outcomes en el tiempo. Más difícil de medir directamente, pero los outcomes bien definidos son su mejor aproximación.

La pregunta que separa un output de un outcome es siempre la misma:

> *¿Esto describe lo que hace el equipo, o lo que cambia gracias a lo que hace el equipo?*

Si la respuesta es "lo que hace" — es un output. Si es "lo que cambia" — es un outcome. Y solo los outcomes miden impacto real.

Ejemplos de la confusión más frecuente:

| Parece un outcome | En realidad es | El outcome real sería |
|-------------------|---------------|----------------------|
| "Entregar 10 features al mes" | Output de actividad | "Que los usuarios completen el onboarding sin soporte" |
| "Mejorar la cobertura de tests al 80%" | Output de actividad | "Cero incidentes críticos en producción este trimestre" |
| "Documentar todos los procesos" | Output de actividad | "Que cualquier persona del equipo pueda resolver un incidente sin ayuda externa" |
| "Hacer 5 reuniones de arquitectura" | Output de actividad | "Que las decisiones de arquitectura se tomen en la reunión, no después" |

Los guardarraíles de la IA en GRETA tienen como función principal detectar esta confusión y devolverla a quien lidera antes de que quede registrada como un outcome válido.

### Por qué no es una quinta dimensión

Las cuatro dimensiones de GRETA describen lo que tiene y cómo es cada persona dentro del equipo. La capa de impacto describe lo que genera — el cambio real que produce en el equipo y en el negocio.

Son cosas distintas. Alguien puede tener niveles altos en las cuatro dimensiones y generar impacto bajo porque el equipo no tiene quien coordine sus aportaciones. O puede tener niveles iniciales y generar impacto alto porque está en un equipo que la multiplica.

**El impacto individual no es una propiedad de la persona — es una propiedad de la persona en ese equipo, en ese contexto, en ese momento.** La misma persona en otro equipo puede tener un impacto radicalmente diferente ante los mismos inputs.

Por eso la capa de impacto vive encima de las cuatro dimensiones, no dentro de ellas. Es la capa que conecta la composición del equipo con los outcomes reales que importan.

### Los tres niveles

```
NIVEL 3 — Outcomes del negocio
  ¿Qué está generando el equipo que importa de verdad?
        ↑
NIVEL 2 — Contribución del equipo
  ¿Cómo aporta este equipo a esos outcomes?
        ↑
NIVEL 1 — Contribución individual
  ¿Cómo aporta esta persona a ese equipo?
  (en función de sus 4 dimensiones + contexto del equipo)
```

La lectura se hace en ambas direcciones: de abajo arriba para entender cómo se construye el impacto, y de arriba abajo para diagnosticar dónde está el gap cuando un outcome no se alcanza.

### El reto específico del equipo

Antes de definir outcomes, es necesario nombrar el reto concreto que el equipo tiene delante. No en abstracto — en el contexto específico del momento: qué está fallando, qué necesita mejorar, qué oportunidad hay que capturar.

El reto específico hace que los outcomes sean contextuales y útiles, no genéricos e intercambiables. "Mejorar la calidad del software" es demasiado amplio para guiar nada. "El equipo tiene un bus factor crítico en la capa de infraestructura y un despliegue que bloquea a otros equipos dos veces por semana" es un reto concreto que genera outcomes accionables.

Definir el reto primero también protege contra la trampa de los outcomes de catálogo — los que se copian de un OKR genérico y no reflejan la realidad del equipo. Cada reto tiene sus outcomes propios, y la IA de GRETA guarda ese contexto para que todo lo que se defina después — outcomes, señales tempranas, contribuciones — esté anclado a él.

### Cómo se definen los outcomes

El marco de referencia es *Impact Driven Growth* de Carlos Iglesias: primero el outcome que importa, luego las iniciativas que lo generan — nunca al revés.

Un outcome de GRETA debe cumplir tres criterios:

**Es un cambio real, no una actividad.**
Describe algo que mejora en el mundo — para el usuario, el equipo o el negocio. No describe lo que hace el equipo.

- ✗ Actividad: "entregar más features al mes"
- ✓ Outcome: "que los usuarios completen el onboarding sin necesitar soporte"

- ✗ Actividad: "mejorar la cobertura de tests"
- ✓ Outcome: "que los incidentes críticos en producción sean cero en el próximo trimestre"

**Está dentro del control del equipo.**
El equipo puede influirlo directamente, sin depender de factores externos como marketing, precio o competencia.

- ✗ Fuera de control: "aumentar el NPS en 20 puntos"
- ✓ Dentro de control: "que el equipo pueda desplegar de forma autónoma sin dependencias externas"

**Es observable.**
Quien lidera puede saber cuándo se está alcanzando sin necesitar un informe elaborado.

- ✗ No observable: "mejorar la cultura de calidad"
- ✓ Observable: "ningún bug crítico llega a producción sin haber pasado por revisión"

Quien lidera define 3-5 outcomes por equipo, idealmente acordados con el negocio y revisados periódicamente.

### Señales tempranas por outcome

Un outcome bien definido dice adónde se quiere llegar. Una señal temprana dice si se está yendo en la dirección correcta antes de llegar.

La diferencia es crítica: si solo mides el outcome final, puedes llegar al final del trimestre y descubrir que no se alcanzó sin haber tenido ninguna señal de alerta en el camino. Las señales tempranas son los indicadores previos, observables semana a semana, que preceden al outcome.

Cada outcome en GRETA debe tener 1-2 señales tempranas asociadas:

| Outcome | Señal temprana |
|---------|---------------|
| "Cero incidentes críticos en producción este trimestre" | "Número de bugs detectados en revisión antes de llegar a producción — sube semana a semana" |
| "El equipo puede desplegar sin dependencias externas" | "Número de deploys realizados sin intervención del equipo de plataforma" |
| "Las decisiones de arquitectura se toman en la reunión, no después" | "Porcentaje de reuniones de arquitectura que terminan con una decisión documentada ese mismo día" |

Si la señal temprana no se mueve, el outcome tampoco lo hará. Y si la señal se mueve pero el outcome no, hay algo más que está bloqueando — y GRETA puede ayudar a diagnosticar qué.

La IA de GRETA puede sugerir señales tempranas para cada outcome basándose en el reto específico del equipo y en las cuatro dimensiones del diagnóstico.

### Cómo se definen las contribuciones individuales — como hipótesis

Cada persona tiene 1-3 contribuciones esperadas a los outcomes del equipo. Se acuerdan en la O2O, no se imponen. Se revisan periódicamente.

Una contribución esperada no es una tarea ni un objetivo SMART — es una hipótesis de impacto. Una proposición verificable que conecta la aportación natural de la persona con el outcome del equipo y que puede confirmarse o refutarse con evidencias en el tiempo.

Formularlas como hipótesis tiene tres ventajas: hace la causalidad explícita y honesta, convierte la revisión en aprendizaje en lugar de en juicio, y responsabiliza a quien lidera tanto como a la persona si la hipótesis no se cumple.

**Formato de hipótesis de contribución:**

> *"Creemos que si [persona] [hace/genera/consigue], contribuirá a [outcome del equipo], porque [razón conectada con sus dimensiones GRETA]."*

Ejemplos:

- *"Creemos que si Ana consigue que al menos dos personas más dominen el proceso completo de despliegue, contribuirá al outcome de deploys autónomos, porque actualmente es la única con ese conocimiento y su ausencia bloquea al equipo."* *(bus factor crítico en conocimiento)*

- *"Creemos que si Luis lidera las reuniones de arquitectura con un orden del día claro y cierra cada una con una decisión documentada, contribuirá al outcome de decisiones en reunión, porque su rol natural de Coordinador está infrautilizado en el equipo actual."* *(rol de contribución no ejercido)*

- *"Creemos que si Marta genera cobertura de testing sólida en el módulo de pagos, contribuirá al outcome de cero incidentes críticos, porque el 70% de los incidentes del último trimestre vinieron de ese módulo."* *(bus factor y seniority)*

Cuando la hipótesis no se cumple, la primera pregunta no es "¿por qué no lo ha hecho esta persona?" sino "¿teníamos razón en la hipótesis? ¿Las condiciones del equipo le han permitido hacerlo?"

### Cómo se registran las evidencias

El impacto no se mide con un número — se registra con evidencias. Situaciones concretas, observadas y repetidas, donde la contribución de esa persona se vio o no se vio.

Una evidencia válida es una observación concreta, no una valoración:

- ✗ Valoración: "es muy proactiva"
- ✓ Evidencia: "detectó el problema de rendimiento en el módulo de pagos tres días antes de que llegara a producción y lo resolvió sin que nadie se lo pidiera"

- ✗ Valoración: "no está comprometida"
- ✓ Evidencia: "en los últimos dos sprints no ha documentado ninguno de los cambios que acordamos documentar, y cuando se lo mencioné en la O2O no dio una razón concreta"

Las evidencias se registran en la app igual que los assessments de las cuatro dimensiones. La IA puede procesar conversaciones y documentos para sugerir evidencias relevantes.

### Cómo se usa para alineamiento

La capa de impacto es una herramienta de alineamiento — no de evaluación. La diferencia no está en los datos sino en cómo se usan y cómo se comunican.

**Evaluación:** "tu impacto ha sido bajo este trimestre."

**Alineamiento:** "acordamos que tu contribución esperada era que el equipo tuviera cobertura de testing en las áreas críticas. Revisemos juntos si las condiciones del equipo te han permitido hacerlo — y si no, qué cambiamos."

La segunda conversación responsabiliza a quien lidera tanto como a la persona. Y conecta directamente con las palancas de modificación de GRETA: si el impacto individual es bajo, ¿es un problema de la persona o de la composición del equipo que la rodea?

**El ciclo de la capa de impacto:**

1. Quien lidera nombra el reto específico del equipo en este momento
2. Quien lidera y el negocio definen 3-5 outcomes que importan para ese reto, con sus señales tempranas
3. Quien lidera y cada persona formulan 1-3 hipótesis de contribución esperada a esos outcomes
4. Cuando la intervención es incierta, se diseña un experimento acotado antes de comprometer el cambio completo
5. La observación continua registra evidencias de impacto (o de su ausencia)
6. En la O2O se revisan las evidencias y se ajustan las hipótesis si el contexto ha cambiado
7. Si un outcome no se alcanza, la matriz de valor/riesgo ayuda a priorizar qué gap resolver primero, y GRETA ayuda a diagnosticar en qué dimensión está el origen

### Experimentos de equipo acotados

Cuando una intervención es incierta — un cambio de composición, una redistribución de responsabilidades, un nuevo rol que alguien nunca ha ejercido — no hay que comprometer el cambio completo antes de saber si funcionará. Se diseña un experimento acotado primero.

Un experimento de equipo en GRETA tiene cuatro elementos:

- **Qué se prueba:** la intervención concreta y limitada en el tiempo
- **Qué se observa:** la señal temprana que indica si va en la dirección correcta
- **Cuánto tiempo:** el periodo mínimo para que la señal sea significativa
- **Qué se decide:** si la señal se mueve, se consolida el cambio; si no, se revisa la hipótesis

Ejemplo: "Probamos que Luis lidere las reuniones de arquitectura durante tres sprints. Si al final de ese periodo el 80% de las reuniones terminan con una decisión documentada ese mismo día, consolidamos el rol. Si no, revisamos si el problema era la facilitación o algo más profundo en la dinámica del equipo."

Los experimentos de equipo no son pilotos eternos — tienen una duración definida y un criterio de decisión claro desde el inicio. Eso evita tanto el abandono prematuro ("no funcionó en la primera semana") como el alargamiento indefinido por inercia.

### Priorización de intervenciones por valor y riesgo

Cuando GRETA identifica múltiples gaps simultáneos — y casi siempre hay más de uno — la pregunta es cuál resolver primero. La respuesta no es siempre "el más crítico estructuralmente". Es "el que más limita los outcomes que importan ahora, con el menor riesgo de disrupción".

Cada intervención posible se puede evaluar en dos dimensiones:

**Valor:** ¿cuánto limita este gap el outcome prioritario del equipo? Un gap puede ser crítico en abstracto pero de bajo impacto en el outcome que más importa en este momento. Y al revés: un gap aparentemente menor puede ser el cuello de botella real.

**Riesgo:** ¿cuán disruptiva es la intervención? Contratar implica tiempo y coste. Redistribuir responsabilidades puede generar fricción. Formar internamente es menos disruptivo pero más lento. Cambiar de rol a alguien es reversible; una baja, no.

La matriz de priorización es simple:

| | Riesgo bajo | Riesgo alto |
|---|---|---|
| **Valor alto** | Actuar primero | Actuar con experimento acotado |
| **Valor bajo** | Monitorizar | Posponer o descartar |

La IA de GRETA puede ayudar a construir esta matriz cruzando el diagnóstico de las cuatro dimensiones con los outcomes definidos y sus señales tempranas.

### Conexión con las cuatro dimensiones

La capa de impacto no reemplaza el diagnóstico de las cuatro dimensiones — lo completa. Cuando un outcome no se alcanza, GRETA puede ayudar a entender por qué:

- ¿Falta el rol de contribución que coordinaría las aportaciones individuales? *(dimensión 1)*
- ¿El bus factor del área de conocimiento crítica es 1? *(dimensión 2)*
- ¿El equipo tiene suficiente seniority para anticipar los problemas que bloquean el outcome? *(dimensión 3)*
- ¿La dimensión emocional de alguna persona clave está comprometida y reduce su multiplicador? *(dimensión 4)*

---

## 13. Los guardarraíles de la IA

La IA en GRETA tiene dos funciones principales: procesar información para sugerir diagnósticos, y proteger el espíritu del framework cuando el uso humano tiende a desviarse.

Esta segunda función — los guardarraíles — es tan importante como la primera. La tendencia humana a simplificar, a buscar métricas cómodas y a convertir herramientas de alineamiento en herramientas de evaluación es inevitable. No porque la gente sea mala, sino porque los indicadores de actividad son más fáciles de medir, más cómodos de defender y generan menos conversaciones difíciles.

Los guardarraíles de la IA no bloquean nunca. Aconsejan, contextualizan, proponen alternativas y, cuando es importante, dejan advertencia registrada. Quien lidera siempre tiene la última palabra. El tono es siempre el mismo: un colega que conoce bien el framework, hace preguntas honestas y nunca juzga — solo ayuda a pensar mejor.

**Guardarraíl 1 — Al definir outcomes del equipo**
La IA evalúa cada outcome contra tres criterios: ¿es un cambio real o una actividad?, ¿está dentro del control del equipo?, ¿es observable? Si detecta un desvío, señala qué tipo es y propone 1-2 alternativas concretas reformuladas. El outcome puede guardarse igualmente, pero con la advertencia visible y registrada.

**Guardarraíl 2 — Al definir contribuciones individuales**
El mismo criterio que para los outcomes del equipo, aplicado a nivel de persona. "Cerrar 5 tickets a la semana" es actividad. "Que el equipo tenga cobertura de testing en las áreas donde eres la única que domina" es contribución de impacto. La IA distingue y aconseja.

**Guardarraíl 3 — Al registrar evidencias de impacto**
La IA detecta si lo que se registra es una observación concreta o una valoración subjetiva. Si es una valoración, pide que se concrete con una situación real. "Es muy proactiva" → "¿puedes describir una situación concreta reciente donde lo hayas visto?"

**Guardarraíl 4 — Al procesar transcripciones de conversaciones**
La IA detecta si en la conversación aparecen patrones que contradicen los principios de GRETA: si se habló de niveles explícitamente, si la conversación sonó más a evaluación que a alineamiento, si no se habló de comportamientos concretos sino de percepciones generales. Lo señala y sugiere cómo reconducirlo en la siguiente conversación.

**Guardarraíl 5 — Al asignar niveles manualmente sin suficiente base**
Si quien lidera asigna un nivel sin haber registrado suficientes observaciones o conversaciones previas con esa persona, la IA advierte que el diagnóstico puede ser poco fiable por falta de muestras — el principio de la señal analógica. Propone qué observaciones o conversaciones ayudarían a mejorar la fiabilidad.

**Guardarraíl 6 — Al crear vistas compartidas**
Cuando se genera una vista para compartir con terceros, la IA revisa el contenido y advierte si podría revelar niveles individuales comparativos de forma que genere competición o malinterpretación en quien la recibe.

**Guardarraíl 7 — Al leer bajadas de nivel en la evolución temporal**
Si la evolución de una persona muestra bajadas de nivel, la IA contextualiza automáticamente: no es necesariamente negativo. Puede ser una reasignación de roles, un cambio de equipo, un momento de transición. Evita que quien lidera lea la bajada como un problema sin explorar el contexto primero.

**Guardarraíl 8 — Al detectar sesgos de atención**
Si hay personas del equipo que llevan mucho tiempo sin conversaciones registradas, sin evidencias actualizadas o con mapa marcado como provisional, la IA lo señala como posible sesgo de atención. Puede ser que la relación es menos fluida, que la persona es más introvertida, o simplemente que se ha descuidado su seguimiento. Lo nombra sin juzgar y sugiere una acción concreta.

**Guardarraíl 9 — Al interpretar el mapa de equipo**
Si quien lidera usa el mapa para comparar personas entre sí de forma explícita ("¿por qué esta persona está en Veteranus y esta otra en Peritus en la misma dimensión?"), la IA recuerda que los niveles son tendencias individuales en contexto — no escalas competitivas — y redirige hacia la pregunta útil: "¿qué necesita cada una para evolucionar en esa dimensión?"

---

## 14. GRETA en red — automedición y liderazgo multinivel

### Para qué sirve esta sección

Las secciones anteriores describen cómo quien lidera usa GRETA para leer a su equipo. Esta sección describe cómo GRETA puede usarse en la otra dirección — para que quien lidera se lea a sí mismo — y cómo puede extenderse a una red de liderazgo donde TLs[^tl], EMs y CTOs[^cto] usan GRETA en su contexto y comparten, de forma voluntaria, una visión del sistema completo.

El propósito es siempre el mismo: no medir ni buscar culpables. Conseguir que las personas estén bien, se encuentren motivadas y puedan generar impacto. Eso requiere que el liderazgo también se examine a sí mismo con el mismo rigor con que examina a los demás.

---

### GRETA para quien lidera — automedición

Si GRETA sirve para leer un equipo, y quien lidera es parte del sistema del equipo, entonces quien lidera también tiene un perfil en las cuatro dimensiones. No medirse a sí mismo con el mismo rigor con que se mide a los demás crea un punto ciego enorme en el diagnóstico.

**El problema del observador.** Cuando se mide al equipo, se pueden observar comportamientos desde fuera. Cuando uno se mide a sí mismo, es simultáneamente el observador y el observado. Eso introduce tres distorsiones frecuentes:

*Sobreestimación en las dimensiones donde uno tiene más confianza.* Quien lidera tiende a sobrevalorar su rol de contribución (se ve como Coordinador porque cree que coordina bien, sin ver que a veces es Impulsor sin consciencia) y su seniority.

*Subestimación en la dimensión emocional.* El síndrome de Narciso casi nunca se detecta desde dentro. El síndrome del impostor, a veces sí — pero no siempre.

*El mapa que se quiere tener versus el mapa real.* Uno tiende a describirse como aspira a ser, no como realmente se comporta de forma repetida y observable.

**Cómo resolverlo:**

*Feedback estructurado del equipo* — no "¿qué te parece cómo lidero?" sino preguntas específicas sobre comportamientos observables. Este mecanismo se describe en detalle en la subsección siguiente.

*Revisión con alguien externo* — un mentor, un peer de otro equipo, alguien que pueda leer el autodiagnóstico y decir "esto que describes como Veteranus en seniority, yo lo recuerdo de otra manera en aquella situación de crisis".

**La conexión con la capa de impacto.** Quien lidera también tiene contribuciones esperadas a los outcomes del equipo. Las más importantes no suelen ser técnicas — son las de alineamiento, facilitación y protección del espíritu del framework. Si los outcomes no se alcanzan, una de las hipótesis a revisar siempre debería ser: ¿es el estilo de liderazgo parte del problema? Esa pregunta solo se puede responder honestamente si quien lidera tiene su propio mapa en GRETA y lo revisa con el mismo rigor que el de su equipo.

---

### La evaluación del equipo a quien lidera

**Qué es y qué no es.** No es una métrica de ego para ver lo bueno que es quien lidera ni un mecanismo de autoflagelación por lo mal que lo está haciendo. Es una herramienta de escucha estructurada — para obtener percepción, identificar puntos de mejora, entender qué comportamientos merecen repetirse y qué necesidades del equipo no se están cubriendo. El objetivo es que las personas puedan hacer su trabajo en las mejores condiciones y generar impacto.

**Dos modalidades:**

*Proactiva libre* — cualquier persona del equipo puede enviar feedback en cualquier momento, sin que nadie se lo pida. La app lo recibe, lo agrega y lo guarda hasta que haya suficientes respuestas para mostrarlo con anonimato real.

*Periódica invitada* — quien lidera activa una ronda de feedback cada cierto tiempo. El equipo recibe una invitación y responde quien quiere, sin obligación. La app no registra quién ha respondido y quién no — solo el agregado.

**El anonimato estructural — cuatro capas:**

Una — umbral mínimo de respuestas antes de mostrar cualquier resultado. Si responden menos personas de ese umbral, los resultados no se muestran. El umbral es configurable según el tamaño del equipo.

Dos — los resultados se muestran siempre agregados. Nunca respuestas individuales, nunca frases literales si el equipo es pequeño.

Tres — la app no registra quién respondió ni cuándo. Solo que llegó una respuesta, no de quién.

Cuatro — quien lidera no puede cruzar los resultados con otras variables del sistema para intentar identificar quién respondió qué.

**Preguntas ancladas a comportamientos observables por dimensión:**

Las preguntas no son valoraciones genéricas — son situaciones concretas organizadas por dimensión para que el resultado alimente el autodiagnóstico de GRETA.

*Sobre el rol de contribución:*
"¿Hay situaciones recientes donde hayas sentido que quien lidera ocupó un espacio que debería haber sido tuyo?"
"¿Sientes que quien lidera cubre los huecos del equipo cuando los hay?"

*Sobre el conocimiento y criterio:*
"¿Cuando tienes una duda técnica compleja, sientes que quien lidera puede ayudarte a pensar?"
"¿Las decisiones técnicas que toma quien lidera te parecen bien razonadas aunque no siempre las compartas?"

*Sobre seniority — anticipación y resiliencia:*
"¿Recuerdas alguna situación de presión reciente donde quien lidera te transmitiera calma y criterio?"
"¿Hay problemas que el equipo descubrió tarde y que en retrospectiva crees que deberían haberse anticipado?"

*Sobre la dimensión emocional:*
"¿Sientes que quien lidera entiende lo que necesitas para hacer bien tu trabajo?"
"¿Hay situaciones donde hayas sentido que quien lidera no era consciente del impacto que tuvo en ti o en el equipo?"
"¿Sientes que el propósito que transmite quien lidera es real, o que es solo discurso?"

*Sobre la capa de impacto:*
"¿Las condiciones en las que trabajas te permiten generar el impacto que se espera de ti?"
"¿Sientes que quien lidera elimina obstáculos para que puedas hacer tu trabajo?"

**El resultado: un mapa de percepción, no un nivel.** El sistema muestra a quien lidera las áreas donde la percepción del equipo coincide con su autodiagnóstico y las áreas donde diverge. La divergencia es la información más valiosa — no para juzgar quién tiene razón, sino para abrir una conversación. La app puede sugerir preguntas para la siguiente O2O basándose en las divergencias detectadas.

---

### Evaluación en tres direcciones

Quien lidera no solo recibe feedback del equipo — también puede dar y recibir feedback en otras direcciones:

| Dirección | La activa | La recibe | La puede compartir |
|-----------|-----------|-----------|-------------------|
| Equipo → quien lidera | Quien lidera (periódica) o equipo (libre) | Quien lidera | Nadie por defecto — decisión activa de quien la recibe |
| Quien lidera → su líder (CTO, VP) | Quien lidera | Quien lidera (para su propio mapa) | El líder evaluado, solo si quien lidera decide compartirlo |
| Equipo → TL | Quien lidera (EM) | Quien lidera (EM) | El TL, si el EM decide compartirlo para su desarrollo |

El principio es siempre el mismo: **el resultado lo recibe quien puede hacer algo con él**, no quien tiene más poder jerárquico. Y quien lidera decide qué comparte — nunca es automático.

**La evaluación hacia arriba — cuando quien lidera evalúa a su líder.** Aquí el anonimato no aplica de la misma forma porque hay un solo evaluador. El mecanismo de honestidad es la estructura de la pregunta: no "¿es un buen CTO?" sino "¿las decisiones que toma el CTO te permiten hacer tu trabajo en las mejores condiciones?" Eso convierte la evaluación en información accionable, no en un juicio personal.

---

### GRETA en red — el modelo multinivel

**La estructura:**

```
CTO / VP Engineering
        ↕  ↕  ↕
  EM1  EM2  EM3      ← cada uno usa GRETA en su contexto
   ↕    ↕    ↕
 TL1  TL2  TL3
   ↕    ↕    ↕
Eq.1 Eq.2 Eq.3
```

Cada nivel usa GRETA de forma autónoma y privada. Pero hay patrones que solo son visibles cuando se mira el conjunto — y que ningún nodo puede ver solo desde su perspectiva.

**La diferencia con la jerarquía tradicional.** En una jerarquía de control, los datos fluyen hacia arriba para que quien tiene más poder tome decisiones sobre quien tiene menos. Eso genera las dinámicas que GRETA quiere evitar — disimulo, actuación para la métrica, competición.

En una red de liderazgo afectivo, los datos fluyen para que **cada nodo entienda mejor el sistema del que forma parte** — no para controlar a los demás sino para colaborar mejor. Todos están simultáneamente liderando y siendo liderados. El TL lidera hacia su equipo y hacia arriba hacia el EM. El EM lidera hacia sus equipos y hacia arriba hacia el CTO. Nadie solo da — todos dan y reciben.

**Nada fluye automáticamente.** La red existe porque cada nodo decide conectarse — no porque el sistema los conecta por defecto. Todo lo que se comparte entre niveles es una decisión activa de quien lo comparte. Esta es la regla más importante del modelo en red.

**Los tres mecanismos que hacen posible la transparencia voluntaria:**

*Quien está arriba comparte primero.* Si el CTO comparte su mapa con los EMs antes de pedirles el suyo, la dinámica cambia completamente. No es "muéstrame tu equipo para evaluarte" — es "te muestro mi mapa para que entiendas cómo me veo yo y qué necesito de ti". Eso invierte la asimetría de poder y crea reciprocidad real.

*La app hace visible el porcentaje de participación, no quién participa.* Si el CTO ve que el 40% de los equipos han optado por compartir su mapa y el 60% no, esa es información útil en sí misma — algo en la cultura o en la confianza no está funcionando. No necesita saber quiénes son el 60%.

*La vista del conjunto solo aparece con masa crítica suficiente.* Si no hay suficientes nodos compartiendo, la vista agregada no se muestra. Eso crea un incentivo colectivo para participar sin convertirlo en una obligación individual. El umbral es configurable.

**La preocupación legítima del sesgo.** Si solo algunos comparten, la visión del conjunto está distorsionada. La solución no es forzar la visibilidad — es entender que compartir es una responsabilidad de liderazgo. Un líder que no comparte su mapa no está protegiendo su privacidad — está limitando la capacidad del sistema de mejorar. Eso no es una regla que impone la app — es una consecuencia del propósito compartido que cada persona que lidera debe interiorizar.

**Los tres tipos de visibilidad compartida:**

*Lateral — entre pares.* Dos EMs pueden decidir compartir sus mapas de equipo para contrastar patrones. No "tu equipo está mejor que el mío" sino "los dos tenemos el mismo gap en roles sociales — ¿compartimos lo que estamos haciendo al respecto?"

*Hacia arriba — voluntaria y agregada.* Un EM puede compartir con el CTO un mapa agregado de sus equipos — no los niveles individuales de nadie, sino los patrones de gaps, la evolución de la capa de impacto, los outcomes que se alcanzan y los que no. El CTO ve el sistema, no las personas.

*Del todo — para quien coordina.* Con suficiente participación, quien coordina varios equipos puede tener una vista que responde a tres preguntas:
- ¿Dónde están los patrones comunes de gap? Si todos los equipos tienen el mismo gap, no es un problema de cada equipo — es un problema sistémico que requiere una intervención sistémica.
- ¿Dónde está la dimensión emocional comprometida de forma generalizada? Si la motivación o el alineamiento están bajos en varios equipos a la vez, la pregunta es "¿qué está generando el sistema que afecta a todos por igual?"
- ¿Dónde está el impacto generándose y dónde no? ¿Qué tienen en común los equipos que alcanzan sus outcomes? ¿Y los que no?

**Lo que nunca aparece en la vista del todo:**
- Niveles individuales de ninguna persona
- Nombres
- Datos de la dimensión emocional desagregados
- Evaluaciones de liderazgo individuales
- Nada que permita identificar a una persona o señalar a un equipo concreto

La vista del todo muestra patrones, no personas. Tendencias, no juicios.

**La pregunta de fondo que articula todo GRETA en red:**

> *¿Qué necesita cambiar en el sistema para que las personas estén bien y puedan generar impacto?*

---

## Glosario de términos y acrónimos

[^o2o]: **O2O** — One to One. Conversación individual entre quien lidera y un miembro del equipo. No necesariamente formal ni periódica — lo importante es que sea regular y genere confianza.

[^hoe]: **HoE** — Head of Engineering. Rol de liderazgo técnico que gestiona equipos de ingeniería.

[^tl]: **TL** — Tech Lead. Rol técnico de referencia dentro de un equipo. Puede liderar técnicamente sin tener responsabilidad directa de gestión de personas.

[^cto]: **CTO** — Chief Technology Officer. Máximo responsable técnico de una organización. Coordina a los EMs y define la dirección tecnológica.

[^belbin]: **Belbin** — R. Meredith Belbin, investigador británico que identificó los nueve roles de equipo. Su libro *Management Teams: Why They Succeed or Fail* (1981) es la referencia original.

[^pl]: **PL** — Plant (Cerebro). Rol Belbin Mental. Ideas originales y creatividad disruptiva.

[^me]: **ME** — Monitor Evaluador. Rol Belbin Mental. Análisis frío y juicio estratégico.

[^sp]: **SP** — Specialist (Especialista). Rol Belbin Mental. Conocimiento técnico profundo.

[^co]: **CO** — Coordinator (Coordinador). Rol Belbin Social. Clarifica, delega e integra.

[^ri]: **RI** — Resource Investigator (Investigador de Recursos). Rol Belbin Social. Busca oportunidades externas.

[^tw]: **TW** — Team Worker (Cohesionador). Rol Belbin Social. Mantiene la armonía y escucha.

[^sh]: **SH** — Shaper (Impulsor). Rol Belbin Acción. Presiona y supera obstáculos.

[^imp]: **IMP** — Implementer (Implementador). Rol Belbin Acción. Transforma ideas en planes ejecutables.

[^cf]: **CF** — Completer Finisher (Finalizador). Rol Belbin Acción. Garantiza el cierre sin errores.

[^ia]: **IA** — Inteligencia Artificial. Modelos de lenguaje y herramientas de generación de código que están transformando el trabajo de los equipos de ingeniería.

[^framework]: **Framework** — Conjunto de herramientas, librerías y convenciones para el desarrollo de aplicaciones. Ejemplos: React, Django, Spring Boot.

[^sw]: **SW** — Software.

[^solid]: **SOLID** — Cinco principios de diseño orientado a objetos: Single responsibility, Open/closed, Liskov substitution, Interface segregation, Dependency inversion.

[^testing]: **Testing** — Proceso de verificación de que el software funciona. Incluye tests unitarios, de integración y e2e (end to end: flujo completo de usuario).

[^e2e]: **e2e** — End to End. Test que verifica el flujo completo desde el punto de vista del usuario.

[^sql]: **SQL** — Structured Query Language. Lenguaje para gestionar bases de datos relacionales.

[^nosql]: **NoSQL** — Not Only SQL. Bases de datos no relacionales: documentos, clave-valor, grafos, columnas.

[^api]: **API** — Application Programming Interface. Interfaz que permite a dos sistemas comunicarse.

[^rest]: **REST** — Representational State Transfer. Estilo arquitectónico para diseñar APIs web.

[^graphql]: **GraphQL** — Lenguaje de consulta para APIs que permite al cliente especificar qué datos necesita.

[^grpc]: **gRPC** — Google Remote Procedure Call. Framework de comunicación entre servicios de alto rendimiento.

[^frontend]: **Frontend** — Parte de la aplicación que se ejecuta en el navegador o dispositivo del usuario.

[^html]: **HTML** — HyperText Markup Language. Lenguaje que estructura el contenido web.

[^css]: **CSS** — Cascading Style Sheets. Lenguaje que define la presentación visual web.

[^js]: **JS** — JavaScript. Lenguaje de programación principal del desarrollo web.

[^ui]: **UI** — User Interface. Interfaz de usuario; la parte visual e interactiva de una aplicación.

[^cloud]: **Cloud** — Computación en la nube. Infraestructura ofrecida por proveedores como GCP, AWS o Azure.

[^infra]: **Infra** — Infraestructura. Recursos tecnológicos que soportan los sistemas de software.

[^cicd]: **CI/CD** — Continuous Integration / Continuous Delivery. Automatización del proceso de integración, prueba y despliegue.

[^owasp]: **OWASP** — Open Web Application Security Project. Organización que publica estándares de seguridad web. Su OWASP Top 10 recoge los riesgos más críticos.

[^compliance]: **Compliance** — Cumplimiento normativo. Adherencia a leyes y estándares aplicables al software.

[^gdpr]: **GDPR** — General Data Protection Regulation. Reglamento europeo de protección de datos personales.

[^tradeoffs]: **Trade-offs** — Compromisos entre opciones con ventajas en un aspecto y desventajas en otro.

[^qa]: **QA** — Quality Assurance. Proceso de garantía de calidad del software.

[^llm]: **LLM** — Large Language Model. Modelo de lenguaje de gran escala. Ejemplos: GPT-4, Claude, Gemini.

[^rag]: **RAG** — Retrieval Augmented Generation. Técnica que combina un modelo de lenguaje con una base de conocimiento externa.

[^prompting]: **Prompting** — Comunicación con modelos de lenguaje mediante instrucciones textuales para obtener el output deseado.

[^busfactor]: **Bus factor** — Número de personas que deben quedar indisponibles para que un área crítica de conocimiento se pierda. Bus factor 1 = máximo riesgo.

---

*Documento vivo — versión 1.0-RC3*
*GRETA — Guía de Referencia de Equipos de Trabajo Afectivo*
*Referencia: Impact Driven Growth, Carlos Iglesias*
