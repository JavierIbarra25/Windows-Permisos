## ✅ 1. Crear usuarios y grupos

### 🔹 Paso 1: Abrir PowerShell como administrador
Antes de escribir comandos, debes abrir PowerShell con permisos elevados:

1. Pulsa **Win + X**.
2. Selecciona **“Windows PowerShell (Administrador)”** o **“Terminal (Administrador)”**.
3. Acepta el control de cuentas (UAC) si te lo pide.

🔍 **Esto es importante porque crear usuarios y grupos requiere permisos de administrador.**

---

### 🔹 Paso 2: Crear los grupos
```powershell
New-LocalGroup -Name "GrupoAdmin"
New-LocalGroup -Name "GrupoUsuario"
```

🔍 **¿Qué hace esto?**
- `New-LocalGroup` → crea un nuevo grupo local (es decir, del equipo, no de dominio).
- `-Name "GrupoAdmin"` → le pone ese nombre al grupo.
- Luego se repite el proceso para crear `GrupoUsuario`.

⚠️**Importante:** los nombres deben ser únicos. Si ya existe un grupo con ese nombre, te dará error.

---

### 🔹 Paso 3: Crear los usuarios
Vamos a crear dos usuarios: uno con permisos de administrador y otro sin ellos.

#### 👤 Usuario con permisos de administrador
```powershell
New-LocalUser -Name "AdminUser" -Password (Read-Host -AsSecureString "Introduce una contraseña")
```

🔍 **¿Qué hace esta línea?**
- `New-LocalUser` → crea un nuevo usuario local.
- `-Name "AdminUser"` → nombre de inicio de sesión del usuario.
- `-Password (Read-Host -AsSecureString ...)` →
  - Pide que escribas una contraseña segura cuando se ejecuta el comando.
  - `AsSecureString` → oculta la contraseña mientras la escribes.

A continuación, lo añadimos a dos grupos:
```powershell
Add-LocalGroupMember -Group "Administradores" -Member "AdminUser"
Add-LocalGroupMember -Group "GrupoAdmin" -Member "AdminUser"
```

🔍 **¿Qué hace esto?**
- `Add-LocalGroupMember` → sirve para meter un usuario en un grupo.
- "Administradores" → es el grupo especial del sistema que da permisos de administrador reales.
- "GrupoAdmin" → es el grupo personalizado que creaste antes, solo para cumplir el requisito del enunciado.

**Este usuario ahora:**
- Tiene privilegios de administrador en el sistema.
- Pertenece al grupo `GrupoAdmin`.

---

#### 👤 Usuario SIN permisos de administrador
```powershell
New-LocalUser -Name "NormalUser" -Password (Read-Host -AsSecureString "Introduce una contraseña")
Add-LocalGroupMember -Group "GrupoUsuario" -Member "NormalUser"
```

🔍 **¿Qué hace esto?**
- `New-LocalUser -Name "NormalUser"` → Crea un nuevo usuario llamado `NormalUser`.
- `-Password (Read-Host -AsSecureString ...)` → Como en el paso anterior, crea la contraseña para el usuario.
- `Add-LocalGroupMember -Group "GrupoUsuario" -Member "NormalUser"` → Añade al usuario `NormalUser` al grupo personalizado `GrupoUsuario`.

Este grupo puede ser útil para gestionar permisos más adelante, por ejemplo, para acceso a carpetas.

---

## ✅ 2. Permisos sobre carpetas

### 📁 Crear dos carpetas:
- Una para el usuario con permisos de administrador.
- Otra para el usuario normal.
- Y que **cada usuario tenga acceso solo a su carpeta**.

---

### 🔹 Paso 1: Crear las carpetas
Usamos el comando `New-Item` para crear carpetas nuevas:
```powershell
New-Item -ItemType Directory -Path "C:\CarpetaAdmin"
New-Item -ItemType Directory -Path "C:\CarpetaUsuario"
```

🔍 **Explicación:**
- `New-Item` → comando que crea un nuevo archivo o carpeta.
- `-ItemType Directory` → le estamos diciendo que el "item" será una carpeta.
- `-Path "C:\CarpetaAdmin"` → esta será la ruta donde se creará la carpeta. En este caso en la raíz del disco C:\.

---

### 🔹 Paso 2: Modificar los permisos de las carpetas
Queremos que **solo el usuario correspondiente** pueda entrar en su carpeta.

🔍**¿Por qué usamos `icacls`?**
`icacls` es un comando que sirve para **ver y cambiar los permisos de archivos y carpetas en Windows**.

---

### Primero quitamos los permisos heredados
```powershell
icacls "C:\CarpetaAdmin" /inheritance:r
```

🔍 **¿Qué hace esto?**
- `inheritance:r` → Elimina la herencia de permisos.
- Normalmente, cuando creas una carpeta, hereda los permisos del padre (por ejemplo, de `C:\`).
- Como queremos **controlar nosotros los permisos**, desactivamos esa herencia.

---

### Luego asignamos permisos solo al usuario correspondiente
```powershell
icacls "C:\CarpetaAdmin" /grant "AdminUser:(OI)(CI)F"
```

🔍 **¿Qué significa esto?**
- `/grant` → da permisos a un usuario o grupo.
- `"AdminUser"` → nombre del usuario que recibirá los permisos.
- `(OI)` → Object Inherit → los permisos se aplicarán a todos los archivos dentro de la carpeta.
- `(CI)` → Container Inherit → los permisos se aplicarán también a todas las subcarpetas.
- `F` → Full Control → ese usuario puede hacer cualquier cosa con los archivos y carpetas: leer, escribir, eliminar, etc.

---

### Lo mismo para el usuario normal:
```powershell
icacls "C:\CarpetaUsuario" /inheritance:r
icacls "C:\CarpetaUsuario" /grant "NormalUser:(OI)(CI)F"
```

---

## ✅ RESUMEN RÁPIDO

| Carpeta            | Usuario     | Permisos     |
|--------------------|-------------|--------------|
| C:\CarpetaAdmin    | AdminUser   | Acceso total |
| C:\CarpetaUsuario | NormalUser  | Acceso total |

🔐 Y **nadie más** tendrá acceso a esas carpetas porque quitamos la herencia.

