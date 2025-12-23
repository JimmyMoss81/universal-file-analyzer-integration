# Universal File Analyzer - Integración para JIMEcosystem

## 📋 Descripción

Documentación completa sobre Universal File Analyzer (UFA) y sus capacidades de análisis de archivos mediante firmas digitales. Este repositorio contiene información útil para integrar funcionalidades similares en JIMEcosystem y proyectos relacionados.

## 🎯 Características Principales del UFA

### 1. Analizador de Archivos por Firma Digital
- **Detección precisa de tipos de archivo**: Analiza el contenido del archivo en lugar de confiar solo en la extensión
- **Escaneo de firmas digitales**: Identifica tipos de archivo mediante patrones únicos en el contenido binario
- **Más de 400 formatos soportados**: Amplia compatibilidad con diversos tipos de archivos

### 2. Información Extraída por el Analizador

#### Metadatos del Tipo de Archivo:
- **File Extension**: Extensión específica identificada (ej: .PDF, .DOCX, .ZIP)
- **File Type**: Nombre descriptivo del formato (ej: "Adobe Portable Document Format")
- **MIME Type**: Tipo MIME para manejo en navegadores y aplicaciones web

#### Atributos del Sistema de Archivos Windows:
- **File Size**: Tamaño en bytes/KB/MB/GB
- **Created Time**: Fecha y hora de creación
- **Modified Time**: Última modificación
- **Accessed Time**: Último acceso al archivo
- **Location**: Ruta completa en el sistema de archivos
- **Owner**: Usuario propietario del archivo
- **Computer**: Nombre del dispositivo donde reside

### 3. Visor Multiformato Integrado

UFA incluye 10 módulos de visualización:

1. **Microsoft Office**: Word, Excel, PowerPoint
2. **Media files**: Audio y video (MP3, MP4, AVI, etc.)
3. **PDF y eBooks**: PDF, ePub, Mobi, djvu, azw
4. **Imágenes**: PNG, BMP, JPEG, TIFF, GIF
5. **Archivos comprimidos**: 7z, zip, gzip, tgz
6. **Email**: EML, MSG
7. **CAD 2D**: DWG, DXF
8. **Archivos de texto**: TXT, JSON, XML, etc.
9. **HEX viewer**: Visualización hexadecimal de cualquier archivo
10. **PE files**: Ejecutables EXE, DLL, DPL

## 💡 Aplicaciones para JIMEcosystem

### 1. Verificación de Archivos de Viaje
- **Validar documentos de identidad**: Verificar que archivos PDF sean genuinos
- **Analizar tickets y reservas**: Confirmar autenticidad de documentos de viaje
- **Detectar archivos corruptos**: Identificar problemas antes de procesar

### 2. Gestión de Documentos Digitales
- **Academia Amparo Marta Cánovas**: Validar materiales educativos subidos
- **Detección automática de tipo**: Sin depender de extensiones que pueden ser incorrectas
- **Organización inteligente**: Clasificar automáticamente por tipo real de contenido

### 3. Seguridad y Integridad
- **Prevención de malware**: Detectar ejecutables disfrazados con extensiones falsas
- **Validación de uploads**: Asegurar que los archivos subidos son del tipo esperado
- **Auditoría de archivos**: Registrar metadatos completos para trazabilidad

### 4. Automatización de Workflows
- **Procesamiento por lotes**: Analizar múltiples archivos automáticamente
- **Integración con n8n**: Crear workflows que validen archivos antes de procesarlos
- **APIs de análisis**: Desarrollar servicios que analicen archivos en la nube

## 🔧 Implementación Técnica Sugerida

### Arquitectura de Integración

```
┌─────────────────────────────────────────────┐
│         JIMEcosystem Frontend               │
│    (React/Next.js con accesibilidad)        │
└──────────────┬──────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────┐
│       API Gateway / n8n Workflow            │
│  - Validación de archivos                   │
│  - Análisis de firmas                       │
│  - Extracción de metadatos                  │
└──────────────┬──────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────┐
│    Servicio de Análisis de Archivos         │
│  - Librería de firmas digitales             │
│  - Motor de detección                       │
│  - Base de datos de tipos MIME              │
└─────────────────────────────────────────────┘
```

### Librerías y Herramientas Recomendadas

#### Node.js / JavaScript
```javascript
// file-type: Detecta tipo de archivo por magic numbers
const FileType = require('file-type');

// mmmagic: Binding de libmagic para Node.js
const mmm = require('mmmagic');

// Ejemplo de uso:
const detectFileType = async (buffer) => {
  const fileTypeResult = await FileType.fromBuffer(buffer);
  return {
    extension: fileTypeResult.ext,
    mimeType: fileTypeResult.mime
  };
};
```

#### Python (para scripts de automatización)
```python
# python-magic: Identificación de archivos
import magic

# PyPDF2: Análisis de PDFs
import PyPDF2

# Ejemplo:
def analyze_file(file_path):
    mime = magic.Magic(mime=True)
    file_type = mime.from_file(file_path)
    return file_type
```

## 🚀 Casos de Uso Específicos

### Caso 1: Validación de Documentos en JimInCruise

**Problema**: Los usuarios suben documentos de identidad para reservas de viaje.

**Solución**:
1. Usuario sube archivo con nombre "pasaporte.pdf"
2. Sistema analiza firma digital del archivo
3. Verifica que realmente sea PDF y no archivo malicioso
4. Extrae metadatos (fecha creación, tamaño)
5. Valida que el archivo no esté corrupto
6. Registra información en base de datos para auditoría

### Caso 2: Academia Digital - Validación de Material Educativo

**Problema**: Profesores suben material en diversos formatos.

**Solución**:
1. Sistema acepta uploads de videos, PDFs, documentos
2. Analiza cada archivo para confirmar tipo real
3. Convierte automáticamente a formatos accesibles si es necesario
4. Genera metadatos para búsqueda y organización
5. Detecta archivos duplicados por contenido (no solo nombre)

### Caso 3: Marketplace de JIMEcosystem

**Problema**: Vendedores suben imágenes de propiedades/vehículos.

**Solución**:
1. Valida que las "imágenes" sean realmente imágenes
2. Extrae metadatos EXIF (ubicación, fecha, dispositivo)
3. Detecta imágenes manipuladas o de baja calidad
4. Optimiza automáticamente para web
5. Genera thumbnails en múltiples tamaños

## 📊 Datos Técnicos

### Firmas Digitales Comunes (Magic Numbers)

| Tipo | Firma (Hex) | Extensión |
|------|-------------|----------|
| PDF | 25 50 44 46 | .pdf |
| ZIP | 50 4B 03 04 | .zip |
| PNG | 89 50 4E 47 | .png |
| JPEG | FF D8 FF | .jpg |
| GIF | 47 49 46 38 | .gif |
| DOCX | 50 4B 03 04 (ZIP con estructura específica) | .docx |
| EXE | 4D 5A | .exe |

### Tipos MIME Importantes

```
application/pdf - Documentos PDF
application/zip - Archivos comprimidos
application/json - Datos JSON
image/jpeg - Imágenes JPEG
image/png - Imágenes PNG
video/mp4 - Videos MP4
audio/mpeg - Audio MP3
text/plain - Texto plano
application/vnd.openxmlformats-officedocument.wordprocessingml.document - DOCX
```

## 🔐 Consideraciones de Seguridad

1. **Validación del lado del servidor**: Nunca confiar solo en la extensión del archivo
2. **Límites de tamaño**: Implementar límites para prevenir ataques DoS
3. **Sanitización**: Limpiar metadatos sensibles antes de almacenar
4. **Cuarentena**: Archivos sospechosos deben ser aislados
5. **Logging**: Registrar todos los análisis para auditoría

## 🛠️ Integración con Stack Tecnológico Actual

### n8n Workflow Example

```json
{
  "nodes": [
    {
      "name": "Webhook",
      "type": "n8n-nodes-base.webhook",
      "parameters": {
        "path": "upload-file"
      }
    },
    {
      "name": "File Type Detector",
      "type": "n8n-nodes-base.code",
      "parameters": {
        "code": "const FileType = require('file-type');\nconst buffer = Buffer.from(items[0].binary.data);\nconst type = await FileType.fromBuffer(buffer);\nreturn [{json: {fileType: type}}];"
      }
    },
    {
      "name": "Conditional",
      "type": "n8n-nodes-base.if",
      "parameters": {
        "conditions": {
          "string": [
            {
              "value1": "={{$json.fileType.mime}}",
              "operation": "equals",
              "value2": "application/pdf"
            }
          ]
        }
      }
    }
  ]
}
```

### API REST Sugerida

```javascript
// Express.js endpoint
app.post('/api/analyze-file', upload.single('file'), async (req, res) => {
  try {
    const buffer = req.file.buffer;
    const fileType = await FileType.fromBuffer(buffer);
    const stats = await fs.promises.stat(req.file.path);
    
    const analysis = {
      extension: fileType.ext,
      mimeType: fileType.mime,
      size: stats.size,
      created: stats.birthtime,
      modified: stats.mtime,
      accessed: stats.atime
    };
    
    res.json({
      success: true,
      analysis: analysis
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
});
```

## 📚 Recursos Adicionales

### Documentación Original
- [Universal File Analyzer - Ayuda Oficial](https://www.lisappstudio.com/universal-file-analyzer/help/)
- Desarrollador: LISApp Studio
- Plataforma: Windows

### Librerías Open Source Recomendadas

1. **file-type** (Node.js)
   - NPM: `npm install file-type`
   - GitHub: [sindresorhus/file-type](https://github.com/sindresorhus/file-type)

2. **python-magic** (Python)
   - PyPI: `pip install python-magic`
   - GitHub: [ahupp/python-magic](https://github.com/ahupp/python-magic)

3. **Apache Tika** (Java/REST)
   - Detección de tipos y extracción de metadatos
   - Puede usarse via REST API

4. **libmagic** (C/Bindings múltiples)
   - Biblioteca estándar Unix para identificación de archivos
   - Base de muchas otras librerías

## 🎨 Consideraciones de Accesibilidad

Dado el perfil de JIMEcosystem con enfoque en accesibilidad:

1. **Feedback audible**: Anunciar resultados del análisis vía text-to-speech
2. **Descripciones alternativas**: Generar descripciones de imágenes automáticamente
3. **Navegación por teclado**: Toda interfaz de análisis accesible sin mouse
4. **Contraste alto**: Modo oscuro para todas las interfaces
5. **Notificaciones claras**: Estados de procesamiento bien comunicados

## 🔄 Próximos Pasos

- [ ] Evaluar librerías de detección de archivos para Node.js
- [ ] Crear workflow n8n de prueba para análisis de archivos
- [ ] Implementar API REST básica de análisis
- [ ] Integrar con sistema de uploads de JIMEcosystem
- [ ] Añadir validación en Academia Amparo Marta Cánovas
- [ ] Implementar detección en JimInCruise para documentos
- [ ] Crear dashboard de estadísticas de archivos procesados

## 📞 Contacto

**Proyecto**: JIMEcosystem
**Repositorio**: github.com/JimmyMoss81/universal-file-analyzer-integration
**Email**: contacto@jimecosystem.com

---

**Nota**: Esta documentación es para fines de investigación y desarrollo de funcionalidades similares en el ecosistema JIM. Universal File Analyzer es un producto de LISApp Studio.
