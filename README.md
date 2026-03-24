# MCP_Claude - Servidor Pipedrive para Claude Desktop

Servidor Model Context Protocol (MCP) para integración de Pipedrive API con Claude Desktop.

## 📋 Descripción

Este repositorio contiene la implementación de un servidor MCP que permite a Claude Desktop interactuar directamente con tu cuenta de Pipedrive para:

- Consultar deals, personas, organizaciones y pipelines
- Buscar y filtrar datos según criterios específicos
- Obtener usuarios y asignaciones
- Acceder a notas y detalles personalizados
- Gestionar leads y actividades

## 🚀 Instalación Rápida

### Requisitos Previos

- **Node.js 18+** instalado
- **Git** configurado
- Cuenta activa de **Pipedrive** con API Token
- **Claude Desktop** instalado

### Paso 1: Clonar el Repositorio Original

Como este repositorio base solo contiene la configuración, necesitas obtener el código completo del servidor MCP:

```bash
# Clonar el repositorio fuente oficial
git clone https://github.com/WillDent/pipedrive-mcp-server
cd pipedrive-mcp-server
```

### Paso 2: Instalar Dependencias

```bash
npm install
```

### Paso 3: Compilar el Proyecto

```bash
npm run build
```

Esto generará la carpeta `build/` con el código JavaScript compilado.

### Paso 4: Configurar Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto:

```bash
# Pipedrive API Configuration
PIPEDRIVE_API_TOKEN=tu_token_de_pipedrive_aqui
PIPEDRIVE_DOMAIN=tu-empresa.pipedrive.com

# Opcional: Rate Limiting Configuration
# PIPEDRIVE_RATE_LIMIT_MIN_TIME_MS=250
# PIPEDRIVE_RATE_LIMIT_MAX_CONCURRENT=2

# Opcional: Transport Configuration (por defecto: stdio)
# MCP_TRANSPORT=stdio  # o 'sse' para acceso remoto
```

**Dónde obtener tu API Token:**
1. Inicia sesión en Pipedrive
2. Ve a **Settings** > **Personal preferences** > **API**
3. Copia tu **Personal API token**

**Tu dominio de Pipedrive:**
- Si tu URL es `https://miempresa.pipedrive.com`, tu dominio es `miempresa.pipedrive.com`

### Paso 5: Configurar Claude Desktop

Edita el archivo `claude_desktop_config.json`:

**En macOS:**
```bash
nano ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

**En Windows:**
```bash
notepad %APPDATA%\Claude\claude_desktop_config.json
```

Añade la configuración del servidor MCP:

```json
{
  "mcpServers": {
    "pipedrive": {
      "command": "node",
      "args": [
        "C:\\ruta\\completa\\a\\pipedrive-mcp-server\\build\\index.js"
      ],
      "env": {
        "PIPEDRIVE_API_TOKEN": "tu_token_aqui",
        "PIPEDRIVE_DOMAIN": "tu-empresa.pipedrive.com"
      }
    }
  }
}
```

**⚠️ Importante:**
- Usa la **ruta absoluta completa** al archivo `build/index.js`
- En Windows, usa barras invertidas dobles `\\` o barras normales `/`
- Ejemplo Windows: `"C:\\Users\\manger\\dev\\pipedrive-mcp-server\\build\\index.js"`
- Ejemplo macOS/Linux: `"/Users/manger/dev/pipedrive-mcp-server/build/index.js"`

### Paso 6: Reiniciar Claude Desktop

1. Cierra completamente Claude Desktop (no solo minimizar)
2. Vuelve a abrir la aplicación
3. Verifica que aparezca el ícono de herramientas (🔨) en el chat
4. Haz clic para ver las herramientas de Pipedrive disponibles

## 🐳 Alternativa con Docker (Acceso Remoto/SSE)

Si prefieres no instalar Node.js o quieres acceso remoto (por ejemplo, desde n8n):

### Usando Docker

```bash
docker run -d \
  -p 3000:3000 \
  -e PIPEDRIVE_API_TOKEN=tu_token \
  -e PIPEDRIVE_DOMAIN=tu-empresa.pipedrive.com \
  -e MCP_TRANSPORT=sse \
  ghcr.io/juhokoskela/pipedrive-mcp-server:main
```

### Configuración de Claude Desktop para SSE

```json
{
  "mcpServers": {
    "pipedrive": {
      "type": "sse",
      "url": "http://localhost:3000/sse"
    }
  }
}
```

## 🛠️ Herramientas Disponibles

Una vez conectado, Claude tendrá acceso a:

- `get-users` - Obtener lista de usuarios/propietarios
- `get-deals` - Buscar deals con filtros avanzados
- `get-deal` - Obtener deal específico por ID
- `get-deal-notes` - Notas y detalles de reserva de un deal
- `search-deals` - Buscar deals por término
- `get-persons` - Listar todas las personas
- `get-person` - Obtener persona por ID
- `search-persons` - Buscar personas
- `get-organizations` - Listar organizaciones
- `get-organization` - Obtener organización por ID
- `search-organizations` - Buscar organizaciones
- `get-pipelines` - Listar todos los pipelines
- `get-pipeline` - Obtener pipeline por ID
- `get-stages` - Obtener todas las etapas
- `search-leads` - Buscar leads
- `search-all` - Búsqueda global en todos los tipos

## 🧪 Prueba de Funcionamiento

Abre Claude Desktop y prueba con:

```
Muéstrame los últimos 10 deals en mi Pipedrive
```

O:

```
Busca todas las personas con el nombre "García" en mi CRM
```

## ❗ Solución de Problemas

### El servidor no aparece en Claude Desktop

1. Verifica que el JSON de configuración sea válido (sin comas extras)
2. Asegúrate de que la ruta al `build/index.js` sea absoluta y correcta
3. Revisa los logs de Claude en:
   - macOS: `~/Library/Logs/Claude/`
   - Windows: `%APPDATA%\Claude\logs\`

### Error de autenticación con Pipedrive

1. Verifica que tu `PIPEDRIVE_API_TOKEN` sea correcto
2. Confirma que tu dominio no incluya `https://` (solo `empresa.pipedrive.com`)
3. Asegúrate de que tu token tenga permisos activos

### No se compila el proyecto

```bash
# Limpia y reinstala
rm -rf node_modules package-lock.json
npm install
npm run build
```

## 📚 Recursos Adicionales

- [Documentación oficial de MCP](https://modelcontextprotocol.io)
- [API de Pipedrive](https://developers.pipedrive.com/docs/api/v1)
- [Repositorio original](https://github.com/WillDent/pipedrive-mcp-server)

## 📝 Licencia

MIT

---

**Autor:** Will Dent  
**Adaptación:** Manger Canterac (@mangerc007)
