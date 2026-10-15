# Agente_Estadistica

Carpeta de conocimiento y configuración para un agente educativo de **Estadística Descriptiva y Análisis de Datos**, dirigido al programa de Ingeniería en Sistemas y Computación de la Universidad Tecnológica de Panamá (UTP).

## Estructura

```text
Agente_Estadistica/
├── config/
│   ├── system_prompt.md
│   └── agent_config.json
├── conocimientos/
│   ├── teoria/
│   │   └── Estadistica_Descriptiva_y_Momentos.md
│   └── software/
│       └── requerimientos_proyecto_software.md
├── prompts_ejemplo/
│   └── casos_de_uso.md
└── README.md
```

## Contenido
- `config/system_prompt.md`: rol, alcance pedagógico, reglas matemáticas y estilo de interacción.
- `config/agent_config.json`: configuración inicial independiente del proveedor. Ajustar nombre de modelo y parámetros a la API seleccionada.
- `conocimientos/teoria/Estadistica_Descriptiva_y_Momentos.md`: teoría, fórmulas y convenciones poblacionales.
- `conocimientos/software/requerimientos_proyecto_software.md`: alcance y criterios de aceptación para los ocho módulos.
- `prompts_ejemplo/casos_de_uso.md`: ejemplos de interacción y preguntas de validación.

## Guía de integración
1. Configura en la plataforma elegida el texto de `config/system_prompt.md` como prompt del sistema.
2. Proporciona como contexto los documentos bajo `conocimientos/`; los casos de uso pueden utilizarse como pruebas manuales de comportamiento.
3. Sustituye `configurable-by-deployment` por un modelo autorizado por el despliegue y adapta los nombres de parámetros a la API. `agent_config.json` es una plantilla conceptual, no una configuración universal ejecutable.
4. Verifica las respuestas con los casos de uso y comprueba que el agente distingue población de muestra, declara convenciones y guía sin resolver íntegramente las tareas evaluadas.
5. Mantén alineados las fórmulas de teoría, reglas del prompt y cálculos del software. Al cambiar convenciones, actualiza los tres.

## Convenciones estadísticas destacadas
El alcance predeterminado es poblacional: varianza con divisor `N`; Sturges como guía; cuantiles por defecto mediante posición `1+(N-1)p` e interpolación lineal; regla de Tukey de `1.5 RIC`; y reporte explícito de curtosis de Pearson `beta_2` y exceso `gamma_2=beta_2-3`. Los datos agrupados generan aproximaciones mediante marcas de clase. Toda desviación de estos valores predeterminados debe documentarse.

## Límites de uso
El agente apoya el aprendizaje; no sustituye el criterio docente, los requisitos oficiales del curso, ni la validación de datos sensibles. No se deben enviar credenciales o información personal identificable a un servicio externo. Las observaciones marcadas como atípicas no se eliminan automáticamente y todo análisis debe interpretarse con contexto.
