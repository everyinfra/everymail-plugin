# EveryMail — Correo transaccional con aprobación de destinatarios y envío

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/hero-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/hero-light.svg">
    <img alt="Un correo de restablecimiento de contraseña para tres destinatarios espera la aprobación; después se pone en cola, se envía y se entrega, y cada evento queda registrado." src="assets/hero-light.svg" width="100%">
  </picture>
</p>

<p align="center">
  <a href="https://cursor.com/en/install-mcp?name=everyinfra&config=eyJ1cmwiOiJodHRwczovL2FwaS5ldmVyeWluZnJhLmNvbS9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgPEFQSSBLZXk%2BIn19"><img src="https://img.shields.io/badge/Instalar_en_Cursor-2F363D?style=for-the-badge&logo=cursor&logoColor=white" alt="Instalar en Cursor"></a>
  <a href="https://vscode.dev/redirect/mcp/install?name=everyinfra&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.everyinfra.com%2Fmcp%22%2C%22headers%22%3A%7B%22Authorization%22%3A%22Bearer%20%3CAPI%20Key%3E%22%7D%7D"><img src="https://img.shields.io/badge/Instalar_en_VS_Code-0065A9?style=for-the-badge&logo=githubcopilot&logoColor=white" alt="Instalar en VS Code"></a>
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/blob/main/i18n/README.es.md#claude-code"><img src="https://img.shields.io/badge/Claude_Code-un_comando-B4532A?style=for-the-badge&logo=claude&logoColor=white" alt="Claude Code: un comando"></a>
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/blob/main/i18n/README.es.md#codex"><img src="https://img.shields.io/badge/Codex-un_comando-2F363D?style=for-the-badge" alt="Codex: un comando"></a>
</p>

<p align="center">
  <a href="https://github.com/everyinfra/everyinfra-agent-plugins/stargazers"><img src="https://img.shields.io/github/stars/everyinfra/everyinfra-agent-plugins?style=social" alt="GitHub stars of everyinfra-agent-plugins"></a>
  &nbsp;<b>Dale una estrella al repo principal y recibe $1 de saldo.</b> <a href="https://everyinfra.com/es/docs?utm_source=github&utm_medium=readme&utm_campaign=everymail-plugin&utm_content=star_line#quickstart">Cómo reclamarlo →</a>
</p>

<p align="center"><a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <b>Español</b></p>

> [!TIP]
> **Este repositorio contiene una sola skill: EveryMail.** El plugin completo de EveryInfra (10 skills, el servidor MCP remoto en `https://api.everyinfra.com/mcp` y la configuración para Claude Code, Codex, Cursor y VS Code) está en **[everyinfra/everyinfra-agent-plugins](https://github.com/everyinfra/everyinfra-agent-plugins)**. Los botones de arriba conectan el servidor MCP; la instalación del plugin en cada entorno está en el repo principal.

> [!NOTE]
> **El envío de EveryMail no está disponible por ahora.** La skill sigue leyendo primero el catálogo en vivo e indica si el envío está configurado antes de preparar nada.

[Configuración](docs/setup.md) · [Flujo de trabajo](docs/workflow.md) · [Prompts](examples/prompts.md) · [Referencia de capacidades](docs/reference.md) · [Documentación de la API](https://api.everyinfra.com/docs) <sub>(en inglés)</sub>

EveryMail es una skill independiente de EveryInfra para correo transaccional a través de REST. Comprueba que el remitente está listo, prepara los campos admitidos y el número de destinatarios, y exige aprobar los destinatarios y el contenido exactos antes de entregar nada.

Úsala para preparar y enviar, con aprobación, mensajes como códigos de verificación, avisos o enlaces de restablecimiento. Que EveryAI ayude a redactar un texto no da permiso para enviarlo.

## Qué puedes hacer

- Consultar el catálogo de envío en vivo antes de preparar una integración.
- Revisar los destinatarios en to, cc y bcc como un único recuento y un único límite de permiso.
- Preparar un mensaje transaccional, individual o por lotes, sin enviarlo hasta que se aprueben destinatarios y contenido.

## Instalación

Es un paquete independiente con una sola skill y un flujo de trabajo **REST**; no es un servicio nuevo. Necesita un entorno de agente compatible y el acceso configurado que describe la [guía de configuración](docs/setup.md). Validar el paquete en local no demuestra que la API esté disponible.

Revisa el contenido del repositorio y luego instálalo en Codex. Estos comandos añaden un catálogo respaldado por GitHub, no un aval de ningún marketplace oficial:

```bash
codex plugin marketplace add everyinfra/everymail-plugin
codex plugin add everymail@everymail-plugin
```

Para una copia local, cambia la fuente del primer comando por `.`. La [guía de configuración](docs/setup.md) también cubre Claude Code y la conexión al servicio, que va aparte.

Con el acceso configurado, prueba:

> Consulta el catálogo de EveryMail y dime si el envío está configurado. No envíes ningún correo de prueba.

Este primer prompt solo inspecciona o prepara. Revisa cualquier operación de pago o efecto externo antes de seguir.

## Cómo funciona

1. Lee `GET /api/v1/email/catalog` y revisa la disponibilidad de envío y los campos admitidos.
2. Usa `GET /api/v1/email/usage` con autenticación cuando haga falta información de uso de la cuenta.
3. Prepara el contenido y cuenta todos los destinatarios en to, cc y bcc. Usa solo los campos de remitente y mensaje admitidos.
4. Obtén aprobación explícita de los destinatarios y el contenido exactos antes de `POST /api/v1/email/send` o `/api/v1/email/batch`.
5. Informa del estado de entrega devuelto por separado de la preparación; trata cada mensaje programado de forma individual cuando la cancelación y los reembolsos necesiten un límite claro.

**Un buen resultado:** un informe de disponibilidad o un envío aprobado, con el alcance exacto de destinatarios y el estado devuelto. Que la API acepte un mensaje no prueba que llegue a la bandeja de entrada ni que alguien lo lea.

## Límites y seguridad

- Solo REST en este paquete; no se anuncia ninguna herramienta MCP de correo.
- No se admiten remitentes arbitrarios, cabeceras personalizadas ni adjuntos fuera del contrato vigente.
- Los cambios de dominio y de webhook necesitan su propio alcance; no se deducen de una petición para redactar un correo.

El paquete trae una sola skill: no concede permisos ni registra una segunda conexión MCP. No pongas credenciales en prompts, archivos del repositorio, capturas ni registros compartidos. Descubrir, ejecutar, cobrar y obtener un resultado externo final son estados distintos. Consulta la [política de seguridad](SECURITY.md).

## Preguntas frecuentes y validación

Las preguntas frecuentes, la comprobación sin conexión (`python3 scripts/validate.py`) y la guía para contribuir están en el [README en inglés](README.md#frequently-asked-questions).

Mantenido por [EveryInfra](https://everyinfra.com/es?utm_source=github&utm_medium=readme&utm_campaign=everymail-plugin&utm_content=footer). Licencia [Apache-2.0](LICENSE).
