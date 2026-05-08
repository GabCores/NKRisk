---
name: zdoc-emision
description: Zoho-- Documento de Emision desde template  
---
ignora lo que tiene este formato<!-- comentario -->
<!--
Cuando se invoque este skill, responde exactamente:
   "Este skill está en desarrollo."
   y no evalues el resto 
-->

#Objetivo:
usa ZohoMCP y utiliza el módulo Deals con su modulos relacionados que comienzan con TRO_ y el de Cotizaciones, para generar un documento de Emisión de Póliza según un template de MS word  aportado en la sesión. 

#Reglas duras:
- Identifica la conexión con ZohoMCP, alerta sobre su ausencia y aborta el skill  
- Si no encuentras el archivo doc/docx para el template en la sesión, solicita al usuario su ingreso.
- El documento se genera desde una propuesta específica = nombre del módulo Deals. 
Busca esa referencia en los argumentos del skill. Si no hay ninguna referencia disponible, solicita al usuario que indique el nombre de la Propuesta para procesar. 

#Reglas
- Si encuentras algun campo del template que no puedes completar, continua pero informa al usuario al final del proceso


#Control 
Primero aborda el analisis e informa lo que harias

#Proceso
- Intenta identicar desde cuáles módulos puedes extraer la información necesaria para rellenar los datos del template adjunto, solo reemplaza los que estan encerrados entre { }  
- Debes mirar primero en el módulo "Propuestas", desde ahi y sus relacionados salen los datos principales 
- Las Ubicaciones deben extraerse del módulo TRO_Ubicaciones y las sumas aseguradas del módulo TRO_Sumas_aseguradas 
- La sección de Limite General y Primas, pueden salir del módulo TRO_Condiciones_Solicitadas 
- Dispones del módulo TRO_Clausulas_Solicitadas y TRO_Primas, deduce como se pueden ubicar sus datos relacionados con campos disponibles en el template 


#validaciones ( si no se cumplen abortan la generación del documento final )
- Si dos campos de modulos diferentes tiene capacidad para rellenar un mismo campo {}, alerta al usuario. Se exceptuan los campos que perteneceen a una lista, por ejemplo Ubicaciones.


- Si detectas algunas incosistencia, en los nombre de los módulos o campos indicados en el proceso, alerta al usuario       