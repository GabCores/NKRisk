---
name: zexcel-ubicacion
description: Zoho- desde Excel hacia Ubicaciones  
---
ignora lo que tiene este formato<!-- comentario -->

<!-- version3.0 
   Cuando se invoque este skill, responde exactamente:
   "Este skill está en desarrollo."
   y no evalues el resto 
-->
#Objetivo
usa ZohoMCP e ingresa en el módulo TRO_Ubicaciones, los datos de un Excel. 
No profundices en explicaciones cuando los procesos normales

#Reglas duras: 
- identifica la conexion con ZohoMCP, alerta sobre su ausencia y aborta el skill  
- Si no encuentras el Excel en la sesión o proyecto, solicita al usuario su ingreso.
- El módulo debe quedar asociado a una propuesta específica. Si el Excel no informa la propuesta, busca esa referencia en los argumentos del skill. Si no hay ninguna referencia disponible, solicita al usuario que indique el  nombre de la Propuesta para trabajar, antes de incorporar los datos. 

#Reglas:
- Cada Excel carga informacion para un solo Prospecto
- El módulo tiene un campo `name` obligatorio en Zoho CRM. Si el campo no está informado en el Excel, genera automáticamente un valor con el formato `Item 1`, `Item 2`, `Item 3`, etc., usando un numerador correlativo para cada dato ingresado. La numeración comienza con 1 para la primer ubicación relacionada con esa propuesta. 
- Antes de crear una nueva Ubicación para una Propuesta, consultar todas las Ubicaciones ya existentes asociadas a esa misma Propuesta. En esas Ubicaciones existentes, revisar el campo `name` y detectar todos los valores que tengan formato `Item N` o `item N`, sin importar mayúsculas o minúsculas.Extraer el número N de cada nombre encontrado, convertirlo a número entero y calcular el número más alto existente.La numeración de las nuevas Ubicaciones debe comenzar en: número_más_alto_existente + 1
- Control de dulicados - no hay.  
- Si hay inconsistencias no corregibles en los datos ( provincia y localidad y ciudad - o sus combinaciones - ) ,desalineados, avisa y no los incorpores. Por ejemplo: 
  - Provincia no coincide con localidad.
  - Localidad pertenece a otra provincia.
  - Ciudad/localidad ambiguas sin forma confiable de corregir.

- Descarta el ingreso del registro si hay datos obligatorios que no son informados en el excel.
- Puedes corregir ligeros errores tipográficos, por ejemplo: 
Córdoba como provincia es el dato correcto, pero si el usuario tipea Cordoba, cordoba, cordova o alguna de sus variantes, procede a corregirlo e incorpora el dato. 

#Proceso 
- evalua una linea por vez del Excel, y verifica que del nombre del campo del Excel. Normaliza nombres de columnas usando coincidencias aproximadas razonables a los campos de TRO_Ubicaciones  
- Importa al modulo cuando se cumplan las reglas 
- Al finalizar informa:
  - Total de registros importados correctamente.
  - Total de registros descartados.
  - Detalle de registros descartados, indicando fila // motivo.

