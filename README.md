## Windows 10 LTSC - Configurándole `winget`

Las ediciones LTSC de Windows 10 y 11 son una solución perfecta para aquellas personas que buscan un sistema minimalista "out of the box". Ni bien desplegados, **vienen prácticamente sin software.** Son versiones de Windows que invierten esa relación típica entre el "usuario empoderado" (o "power user") y el sistema. Donde clásicamente luego de instalar, el usuario se disponía a comenzar a dar una larga serie de clicks y tipear comandos para desinstalar todo ese *bloatware* que el sistema sirve adjuntadas a su propio paquete.

Las versiones LTSC vienen casi vacías, lo cual revierte esa relación. Obligando al usuario a realizar una serie quizás no tan larga de clicks, y en este caso para irle integrando de manera modular esas cosas que sí se desean implementar al propio sistema. Esto es "mejor" desde una perspectiva de sistemas minimalistas, principalmente por los tres motivos que siguen...
1. Servir un sistema virgen, sin programas, al cual el usuario deba integrar selectivamente dichos programas a medida que los vaya necesitando, **convierte al sistema en uno más _personalizable._**
2. Eliminando de la ecuación la necesidad de des-instalar software innecesario, se hace del despliegue de este sistema algo mucho más prolijo. **Mientras que es sencillo integrar nuevos programas, librerías y dependencias para un sistema, no es tan fácil determinar si el trabajo de _de-bloatear_ ese sistema fue o no fue efectuado correcta y completamente.**
3. Los sistemas que vienen con lo mínimo indispensables por defecto son más livianos, y su instalación inicial es más rápida (obviamente descontando el tiempo post-instalación donde se configura el sistema para dejarlo listo).

## ¿Qué es, y cómo habilitar `winget` en Windows LTSC?
[`winget`](https://github.com/microsoft/winget-cli) es el gestor de paquetes via terminal que se utiliza en los sistemas Windows. De manera similar a como lo hace un gestor de paquetes para Sistemas UNIX, con `winget` podemos utilizar la terminal PowerShell para escribir comandos de terminal con argumentos que busquen programas determinados, que desplieguen distintos datos acerca de esos programas, y obviamente que instalen estos programas en nuestros sistemas. 

`winget` busca estos paquetes desde repositorios oficiales revisados por Microsoft. Así que de entre los varios gestores de paquetes que existen, es el más "fiable en términos de seguridad" por así decirlo. Aun así, caben mencionar los otros dos contendientes en cuanto a gestores de paquetes que no se quedan atrás para Windows, siendo estos [`chocolatey`](https://github.com/chocolatey/choco) y [`scoop`](https://github.com/ScoopInstaller/Scoop).
Cualquiera de los tres ofrece lo mismo: argumentos por línea de comandos mediante los cuales monitorear de entre una larga variedad de programas instalables, como navegadores, editores de texto, gestores de archivos, etc... 
- Buscar estos programas por palabras clave, por ejemplo tipear `winget search "browser"`. Y obtener una larga lista de navegadores web, o cualquier programa que en su descripción contenga el string:"browser", compatibles con nuestro sistema de entre los cuales elegir.
- Podemos buscar información sobre la proveniencia de estos programas, por ejemplo tipeando `winget info Programa.X`, para obtener datos como el repositorio o fuente de origen para su descarga, autor/es del programa, número de versión, etc...
- En general nos ofrece un control granular y muy flexible sobre los programas que tenemos disponibles y que nos interesaría incorporar a nuestro sistema. Por eso es interesante contar con esta clase de herramientas en LTSC.

`winget` no viene por defecto en LTSC, pero es una herramienta indispensable para un power-user que quiera poblar su despliegue windows minimalista con herramientas seleccionadas a medida. Para lograr esta cheff-kiss edition de Windows, tenemos que habilitarlo.

Por fortuna el proceso para instalar `winget` en LTSC es muy sencillo, y nos trae como beneficio adicional algo más: vamos a introducir la Microsoft Store en nuestro LTSC. La cual hasta cierto punto funciona como un equivalente para Flathub en Linux o App-Store en Android. 

Como requisito para llegar al gestor de paquetes en LTSC, tenemos que inputar el siguiente comando en nuestra PowerShell, estando esta última abierta con Permisos de Administrador:
```powershell
wsreset -i
```
**Esto va a forzar la re-instalación de la tienda de Microsoft.** O, en nuestro caso con LTSC, como la tienda no estaba instalada desde un inicio, lo que forzaría es su primer instalación. Una vez ejecutado, hay que esperar hasta que aparezca una notificación pop-up que indique que "La tienda ha sido instalada.". Luego de eso [se recomienda instalar el instalador de aplicaciones](https://apps.microsoft.com/detail/9nblggh4nns1?hl=en-US&gl=AR), que incluye `winget`, y que permite la instalación de paquetes en formato `*.appx`, como los que `winget` usa.

Cabe aclarar que `winget` y la tienda _no son lo mismo_. `winget` es capaz de sourcear programas de la tienda, sí. Pero también de los repositorios comunitarios de `winget` (esos que mencionaba antes). Por esto su cobertura es más amplia, y es una herramienta más poderosa que la tienda. Sin embargo, ambos no son excluyentes entre sí. Es conveniente contar con las dos opciones. Personalmente en la mayoría de los casos me gusta priorizar `winget` para buscar información sobre, e instalar los programas que necesito, y en caso de no encontrar nada, entonces si caigo sobre la Microsoft Store como recurso secundario.

En la tienda tendremos la opción de instalar [`UniGet-UI`](https://github.com/marticliment/UniGetUI). El cual incluye `winget` y le da una interfaz gráfica mediante la cual utilizarlo. Esto es muy útil para exprimir un poco más de las capacidades adicionales de este gestor de paquetes. De hecho fue el método "de entrada" que utilicé para instalar `winget` en mí sistema, al no poder encontrar el instalador de aplicaciones disponible en la ms-store.

Eso es todo. Diviértete con tu minimal Windows personalizable a medida.
## Intentos previos fallidos...
Aunque existen métodos de tipo script para integrar el gestor de paquetes sin pasar por la tienda, estos scripts eventualmente se van a quedar obsoletos, y a largo plazo es más sencillo utilizar `wsreset -i`.
En mis intentos, me basé sobre [un repositorio Github de hace algunos años.](https://github.com/SasaDermanovic/2024-Winget-Setup-Guide-Windows-10-LTSC)
Este script realizaba lo siguiente:
1. Buscaba e instalaba VC Libs, las librerías Visual Code Studio necesarias.
2. Buscaba e instalaba Microsoft.UI.Xaml 2.8, dependencias necesarias.
3. Buscaba e instalaba `winget` con su licencia `*_License1.xml`.

Intenté re-ajustar el script para que siempre buscase la paquetería más reciente. En especial porque `winget` y `xaml` habían quedado "congelados" en el tiempo en que se escribió el script original, hacia mitad de 2024. La manera de funcionar era que buscaba enlaces web específicos donde estaban las librerías y dependencias almacenadas. Al estar escrito a mitad de 2024, hubo muchas versiones posteriores de `winget` y `xaml` publicadas desde entonces, mientras que los enlaces de referencia scripteados se quedaron iguales. Esto resulta en la instalación de versiones obsoletas de los paquetes, que tienen que ser actualizadas por sistema y usando `winget upgrade --all`.

```powershell
# 1. Habilitar la ejecución de scripts en la PowerShell.
Set-ExecutionPolicy RemoteSigned

# 2. Llamado al script mencionado, hosteado en su repositorio.
Invoke-Expression (Invoke-WebRequest -Uri "https://raw.githubusercontent.com/SasaDermanovic/2024-Winget-Setup-Guide-Windows-10-LTSC/refs/heads/main/Winget_LTSC_Installer.ps1").Content
```

Comprobé que es posible correr este script > reiniciar mí equipo > buscar en el escritorio el paquete desempaquetado en la instalación, "instalador de paquetes" > darle doble click para re-instalarlo, porque la primer instalación por terminal a veces viene rota > y asi llegar a winget _desactualizado_ funcionando en mí sistema (sí, es una instalación muy rara) > luego podría dar `winget upgrade --all`, y asegurarme de que todos los paquetes desactualizados se actualicen.

Y si bien eso funciona, me pareció buena idea intentar con un script que desde el vamos tuviese la posibilidad de "buscar siempre" los paquetes más actuales. Terminé llegando a lo siguiente...
```powershell
# ==============================================
# Winget Setup Script for Windows 10 LTSC
# Enhanced version based on Sasa Dermanovic's work
# ==============================================

# 1. Check for Administrator privileges

$IsAdmin = ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)

if (-not $IsAdmin) {
Write-Host "[ERROR] Este script debe ser ejecutado como administrador." -ForegroundColor Red
Read-Host "Presione [Enter] para salir..."
exit 1
}

Write-Host "[INFO] Lanzando LTSC setup script..." -ForegroundColor Cyan

# 1.1 Forces use of TLS 1.2 for all downloads.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

# 2. Detect system architecture
$systemType = (Get-CimInstance -ClassName Win32_ComputerSystem).SystemType.ToLower()

switch -Wildcard ($systemType) {
"*x86*" { $arch = "x86"; break }
"*x64*" { $arch = "x64"; break }
"*arm64*" { $arch = "arm64"; break }
"*arm*" { $arch = "arm"; break }
default { Write-Host "[!] Arquitectura no reconocida: $systemType" -ForegroundColor Red; exit 1 }
}
Write-Host "[*] Arquitectura detectada: $arch" -ForegroundColor Cyan

# 3. Function to get latest GitHub release asset
function Get-Latest-GitHub-Release-Asset {
param(
[string]$Repo,
[string]$AssetPattern
)
try {
$apiUrl = "https://api.github.com/repos/$Repo/releases/latest"
$release = Invoke-RestMethod -Uri $apiUrl -UseBasicParsing
$asset = $release.assets | Where-Object { $_.name -like $AssetPattern } | Select-Object -First 1
if ($asset) { return $asset } else { return $null }
} catch { return $null }
}

# 4. Install VCLibs
try {
Add-AppxPackage "https://aka.ms/Microsoft.VCLibs.$arch.14.00.Desktop.appx" -Verbose
}
catch {
Write-Host "Failed to install VCLibs package. Error: $_" -ForegroundColor Red
exit 1
}
  
# 5. Install Microsoft.UI.Xaml
$xamlVersion = "2.8.7"
try {
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri "https://www.nuget.org/api/v2/package/Microsoft.UI.Xaml/$xamlVersion" -OutFile "microsoft.ui.xaml.$xamlVersion.zip" -ErrorAction Stop
}
catch {
Write-Host "Failed to download XAML package. Error: $_" -ForegroundColor Red
exit 1
}

 
try {
Expand-Archive .\microsoft.ui.xaml.$xamlVersion.zip -Force -ErrorAction Stop
}
catch {
Write-Host "Failed to extract XAML package. Error: $_" -ForegroundColor Red
exit 1
}

 
try {
Add-AppPackage .\microsoft.ui.xaml.$xamlVersion\tools\AppX\$arch\Release\Microsoft.UI.Xaml.2.8.appx -Verbose
}
catch {
Write-Host "Failed to install XAML package. Error: $_" -ForegroundColor Red
exit 1
}
  
Write-Host "Dependencies installed successfully." -ForegroundColor Green
  
# 6. Install Winget (latest release)
$wingetAsset = Get-Latest-GitHub-Release-Asset -Repo "microsoft/winget-cli" -AssetPattern "*.msixbundle"
$licenseAsset = Get-Latest-GitHub-Release-Asset -Repo "microsoft/winget-cli" -AssetPattern "*_License1.xml"

if (-not $wingetAsset -or -not $licenseAsset) {
Write-Host "[ERROR] No se pudo encontrar la última versión de Winget" -ForegroundColor Red
exit 1
}

try {
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri $wingetAsset.browser_download_url -OutFile $wingetAsset.name -ErrorAction Stop
Invoke-WebRequest -Uri $licenseAsset.browser_download_url -OutFile $licenseAsset.name -ErrorAction Stop
}
catch {
Write-Host "Failed to download Winget package. Error: $_" -ForegroundColor Red
exit 1
}

try {
Add-AppxProvisionedPackage -Online -PackagePath (Resolve-Path $wingetAsset.name) -LicensePath (Resolve-Path $licenseAsset.name) -Verbose
}
catch {
Write-Host "Failed to install Winget package. Error: $_" -ForegroundColor Red
exit 1
}

Write-Host "Winget installed successfully." -ForegroundColor Green
```
"Parece" funcionar correctamente, porque ejecuta completo sin mostrar errores en el output, pero este script reconfigurado misteriosamente no funciona. O al menos no funciona siempre. Sospecho que la causa radica en parte sobre la manera en que desplegué mi sistema antes de probar este último script al instalar la versión Evaluation. Donde algunos de los pasos conllevaron apagar varios servicios de mí sistema que no necesitaba para su uso. Esto podría haber vuelto a mi sistema inmune contra el script. El script original fue probado en condiciones diferentes. Lo dejo cargado en caso de se quiera probar.

-----
Finalmente acabé por descartarlo:
- Porque le había dedicado demasiado tiempo, y ver que no funcionaba se comenzó a volver frustrante.
- Porque era mucho más rápido y fluido pasar por la tienda para llegar a `winget` que seguir trastabillando con un script.
- El acercamiento por enlaces eventualmente se va a romper. La manera "future-proof" de hacer esto **_es por medio de la tienda._**

Microsoft declaró que los enlaces de descarga para VC Libs que los scripts usan están deprecados. Y que estas librerías van a ser conseguibles para ese entonces exclusivamente como componentes empaquetados con aplicaciones de la tienda, como Visual Code o el mismo instalador de paquetes. En el futuro, va a ser necesario pasar por la tienda sí o sí. Más vale hacerlo temprano. Además siendo honesto, no sabía que se podía hacer tan fácil. De haberlo sabido no me habría molestado.

En caso de querer intentar hacer pruebas con este script, se puede usar su RAW como enlace a invocar de la misma manera que se demuestra como ejemplo con el script original, reemplazando el enlace.
