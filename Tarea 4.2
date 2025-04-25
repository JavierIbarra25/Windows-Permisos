✅ 1. Crear usuarios y grupos (CE a)
🔹 Paso 1: Abrir PowerShell como Administrador
Presiona Win + X → selecciona Windows PowerShell (Admin) o Terminal (Admin).

🔹 Paso 2: Crear los grupos
powershell
Copiar
Editar
New-LocalGroup -Name "GrupoAdmin"
New-LocalGroup -Name "GrupoUsuario"
🔹 Paso 3: Crear los usuarios
powershell
Copiar
Editar
# Usuario con permisos de administrador
New-LocalUser -Name "AdminUser" -Password (Read-Host -AsSecureString "Introduce una contraseña") -FullName "Usuario Administrador"
Add-LocalGroupMember -Group "Administradores" -Member "AdminUser"
Add-LocalGroupMember -Group "GrupoAdmin" -Member "AdminUser"

# Usuario sin permisos de administrador
New-LocalUser -Name "NormalUser" -Password (Read-Host -AsSecureString "Introduce una contraseña") -FullName "Usuario Normal"
Add-LocalGroupMember -Group "GrupoUsuario" -Member "NormalUser"
🔐 Nota: Cuando creas los usuarios, se te pedirá que escribas una contraseña. Usa una que cumpla con los requisitos de seguridad (por ejemplo, con mayúsculas, minúsculas, número y símbolo).

✅ 2. Permisos sobre carpetas (CE b, d)
🔹 Paso 1: Crear las carpetas
powershell
Copiar
Editar
New-Item -ItemType Directory -Path "C:\CarpetaAdmin"
New-Item -ItemType Directory -Path "C:\CarpetaUsuario"
🔹 Paso 2: Asignar permisos a cada carpeta
powershell
Copiar
Editar
# Quitar permisos heredados y dar control total solo al usuario correspondiente

# Para la carpeta del usuario administrador
icacls "C:\CarpetaAdmin" /inheritance:r
icacls "C:\CarpetaAdmin" /grant "AdminUser:(OI)(CI)F"

# Para la carpeta del usuario normal
icacls "C:\CarpetaUsuario" /inheritance:r
icacls "C:\CarpetaUsuario" /grant "NormalUser:(OI)(CI)F"
