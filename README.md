# ☁️ Google Cloud Function + GitHub Actions 🚀

Este proyecto demuestra cómo desplegar una función HTTP en **Google Cloud Functions Gen 2** utilizando **GitHub Actions** como pipeline de CI/CD automatizado.

---

## 📦 ¿Qué hace este ejemplo?

- Expone una función HTTP que responde con:
  ```
  Hola desde una función en Google Cloud 🚀
  ```

- El despliegue se hace automáticamente cada vez que haces un `push` a la rama `main`.

---

## 🧱 Estructura del Proyecto

```
.
├── index.js               # Lógica de la función
├── package.json           # Configuración de Node.js
├── package-lock.json      # Generado con npm install
└── .github/
    └── workflows/
        └── deploy.yml     # GitHub Actions Workflow
```

---

## 🚀 ¿Cómo lo configuro?

### 1. Prepara el entorno en Google Cloud

#### ✅ Crea un proyecto GCP (si no tienes uno)
#### ✅ Habilita las APIs necesarias:
```bash
gcloud services enable cloudfunctions.googleapis.com
gcloud services enable run.googleapis.com
gcloud services enable iam.googleapis.com
gcloud services enable cloudbuild.googleapis.com
```

#### ✅ Crea una Service Account para GitHub Actions:

Asigna estos roles a la cuenta:

- `Cloud Functions Developer`
- `Service Account User`
- `Cloud Run Admin`
- `Cloud Build Builds Editor` (o `Builder`)
- `IAM Service Account User`

Y luego otórgale `actAs` sobre la cuenta `compute@developer.gserviceaccount.com`.

#### ✅ Descarga la clave JSON de la service account.

---

### 2. Configura el secreto en GitHub

#### 🔐 Codifica el JSON a base64

En PowerShell:

```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("ruta/a/tu-clave.json"))
```

En macOS/Linux:

```bash
base64 tu-clave.json
```

#### 🔐 Crea un secreto en tu repositorio:
- Nombre: `GCP_SA_KEY`
- Valor: el base64 completo del JSON

---

### 3. Verifica el workflow (`.github/workflows/deploy.yml`)

Puntos importantes:

- Debe usar `gcloud auth activate-service-account` con tu secreto
- Debe tener:
  ```yaml
  --runtime=nodejs20
  --gen2
  --trigger-http
  --entry-point=helloWorld
  ```

---

### 4. Haz push a main

```bash
git add .
git commit -m "Primer despliegue"
git push origin main
```

GitHub Actions se encargará de todo ✨

---

## 🧪 Cómo probar la función

1. Ve a [Google Cloud Console > Cloud Functions](https://console.cloud.google.com/functions)
2. Copia la **URL pública**
3. Pega en el navegador o usa `curl`:

```bash
curl https://REGION-PROYECTO.cloudfunctions.net/helloWorld
```

---

## 🔄 Cómo actualizar la función

1. Modifica `index.js` (por ejemplo, cambia el texto de respuesta)
2. Haz commit y push
3. GitHub Actions volverá a desplegar automáticamente

---

## 📚 Recursos

- [Documentación oficial Cloud Functions](https://cloud.google.com/functions/docs)
- [GitHub Actions para GCP](https://github.com/google-github-actions/setup-gcloud)

---

## 🧠 Créditos

Creado por [Javi](https://github.com/tuusuario)  
Consultor Salesforce & Marketing Cloud 🚀
