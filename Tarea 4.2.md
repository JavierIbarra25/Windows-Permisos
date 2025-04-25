## ✅ 1. Crear usuarios y grupos

### 🔹 Paso 1: Abrir PowerShell como Administrador
Presiona Win + X → selecciona Windows PowerShell (Admin) o Terminal (Admin).

### 🔹 Paso 2: Crear los grupos
```powershell
New-LocalGroup -Name "GrupoAdmin"
New-LocalGroup -Name "GrupoUsuario"
```
### 🔹 Paso 3: Crear los usuarios
```powershell
# Usuario con permisos de administrador
New-LocalUser -Name "AdminUser" -Password (Read-Host -AsSecureString "Introduce una contraseña")
Add-LocalGroupMember -Group "Administradores" -Member "AdminUser"
Add-LocalGroupMember -Group "GrupoAdmin" -Member "AdminUser"

# Usuario sin permisos de administrador
New-LocalUser -Name "NormalUser" -Password (Read-Host -AsSecureString "Introduce una contraseña")
Add-LocalGroupMember -Group "GrupoUsuario" -Member "NormalUser"
```

## ✅ 2. Permisos sobre carpetas
### 🔹 Paso 1: Crear las carpetas
```powershell
New-Item -ItemType Directory -Path "C:\CarpetaAdmin"
New-Item -ItemType Directory -Path "C:\CarpetaUsuario"
```
### 🔹 Paso 2: Asignar permisos a cada carpeta
```powershell
# Quitar permisos heredados y dar control total solo al usuario correspondiente

# Para la carpeta del usuario administrador
icacls "C:\CarpetaAdmin" /inheritance:r
icacls "C:\CarpetaAdmin" /grant "AdminUser:(OI)(CI)F"

# Para la carpeta del usuario normal
icacls "C:\CarpetaUsuario" /inheritance:r
icacls "C:\CarpetaUsuario" /grant "NormalUser:(OI)(CI)F"
```
