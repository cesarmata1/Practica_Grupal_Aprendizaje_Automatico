# Predicción de nivel de ingresos — Adult / Census Income

Práctica de **Aprendizaje Automático**

## Autores

- César Mata Rodríguez — cesar.mata@cunef.edu
- Ángel Piédrola García — angel.piedrola@cunef.edu
- Alberto de Mingo Martín — alberto.demingo@cunef.edu
- Roberto Pascua Fernández — roberto.pascua@cunef.edu

**Repositorio:** https://github.com/cesarmata1/Practica_Grupal_Aprendizaje_Automatico

---

## Problema de negocio

Muchas empresas, como bancos o aseguradoras, necesitan saber si un cliente tiene ingresos altos, pero es un dato que la gente no suele dar o que cuesta conseguir (hay que pedir nóminas o declaraciones). Sin embargo, otros datos como la edad, los estudios o el trabajo son mucho más fáciles de obtener.

Queremos **predecir si una persona gana más de 50.000 dólares al año** a partir de esos datos.

---

## Datos

Usamos el dataset **Adult** del UCI Machine Learning Repository: https://archive.ics.uci.edu/dataset/2/adult

Sale del **censo de EE. UU. de 1994** y tiene **32.561 personas** con 14 variables (edad, tipo de empleo, estudios, estado civil, ocupación, raza, sexo, horas trabajadas, país de origen, etc.) y la variable que queremos predecir, `ingresos`, que indica si la persona gana más o menos de 50.000 dólares.

### Ventajas

- Es público, gratuito y está bien documentado.
- Tiene un tamaño manejable.
- Mezcla variables numéricas y de texto.
- Tiene pocos datos faltantes (menos de un 6 % en las columnas afectadas).

### Desventajas encontradas

- **Es de 1994**, así que los sueldos y el mercado laboral han cambiado mucho.
- **Solo incluye EE. UU.**
- **Las clases están desbalanceadas:** un 76 % gana 50.000 dólares o menos y solo un 24 % gana más.
- **El fichero viene "sucio":** no tiene cabecera, los textos llevan un espacio delante y los datos faltantes aparecen como `?`.
- **Hay valores tope** puestos por el censo (ganancias de 99.999 y edades de 90).
- **Algunos países están mal escritos** (`Columbia`, `Trinadad&Tobago`, `Hong`).
- **Hay columnas que no sirven:** `educacion` repite lo mismo que `educacion_num`, y `peso_censo` es un dato interno del censo.
- **Tiene variables delicadas** (sexo y raza) con diferencias muy grandes que el modelo podría aprender.

---

## Planteamiento

Es un problema de **aprendizaje supervisado** de **clasificación binaria**: para cada persona queremos predecir si gana más de 50.000 dólares (**1**) o no (**0**), usando el resto de sus datos.

- **Variable objetivo (y):** `ingresos`.
- **Variables de entrada (X):** `edad`, `tipo_empleo`, `educacion_num`, `estado_civil`, `ocupacion`, `relacion`, `raza`, `sexo`, `ganancia_capital`, `perdida_capital`, `horas_semana` y `pais_origen`.

Como las clases están desbalanceadas, al evaluar los modelos no nos fiaremos solo de la accuracy, sino que usaremos métricas que tengan en cuenta ese desbalanceo.

---

## Resumen del EDA

El análisis completo está en `notebooks/00_EDA_adult.ipynb`.

- Eliminamos `educacion` (repite `educacion_num`) y `peso_censo` (no describe a la persona).
- No borramos duplicados: son personas distintas con un perfil muy común.
- Rellenamos los datos faltantes con `Desconocido`, porque la gente sin ocupación apuntada gana menos y no queríamos perder esa información.
- No hay valores imposibles, y los atípicos son reales, así que no borramos ninguno.
- Agrupamos los países en `United-States` y `Otro`.
- Las variables que más se relacionan con los ingresos son el estado civil, la relación en el hogar, la ocupación, los estudios, la edad, las horas trabajadas y las ganancias de capital.
