# Gallagher Cardholders API (C#)

Aplicación ASP.NET Core para consultar cardholders de Gallagher Command Centre.

## Requisitos

- .NET 8 SDK
- Windows (para acceder al certificado desde store)
- Certificado cliente instalado en Current User → Personal
- Gallagher con REST habilitado y API Key

## Configuración

1. Edita `appsettings.json`:
   - `Host`: hostname de Gallagher (ej: `WIN-2UIUN05L20N`)
   - `Port`: puerto API (ej: `8904`)
   - `ApiKey`: tu `GGL-API-KEY-...`
   - `ClientCertificateThumbprint`: thumbprint del certificado (sin espacios)
   - `IgnoreServerCertificateErrors`: `true` si usas certificado autofirmado

2. Restaurar paquetes y ejecutar:
   ```bash
   dotnet restore
   dotnet run
   ```

La API estará en `https://localhost:5001/api/cardholders`.

## Probar

```http
GET https://localhost:5001/api/cardholders?limit=10
```

Respuesta esperada: JSON con la lista de cardholders.

## Importar certificado (si no lo tienes)

```powershell
# Generar certificado auto-firmado (opcional)
$cert = New-SelfSignedCertificate -DnsName "CommandCentreClient" -CertStoreLocation "Cert:\CurrentUser\My" -FriendlyName "Gallagher Client" -KeyUsage KeyEncipherment,DigitalSignature -KeySpec KeyExchange -KeyExportPolicy Exportable -Provider "Microsoft RSA SChannel Cryptographic Provider" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
$cert.Thumbprint
```

Luego usa ese thumbprint en `appsettings.json`.
