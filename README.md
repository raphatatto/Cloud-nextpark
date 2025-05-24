
# NextParkAPI — Gestão Inteligente de Motos no Pátio

## 🚀 Descrição do Projeto

Este projeto entrega uma API RESTful em .NET Core para a gestão de motos nos pátios da Mottu, permitindo:

- Cadastro, consulta, atualização e exclusão de motos e vagas.
- Visualização das motos por meio de interface Swagger (OpenAPI).
- Integração com banco Oracle via EF Core.
- Deploy completo em nuvem usando Docker em VM Linux no Azure.

---

## 🔧 Rotas Disponíveis

| Método  | Endpoint           | Descrição                     |
|---------|---------------------|-------------------------------|
| GET     | /api/Moto          | Lista todas as motos          |
| GET     | /api/Moto/{id}     | Consulta moto por ID          |
| POST    | /api/Moto          | Cadastra nova moto            |
| PUT     | /api/Moto/{id}     | Atualiza moto existente       |
| DELETE  | /api/Moto/{id}     | Remove moto por ID            |
| GET     | /api/Vaga          | Lista todas as vagas          |
| POST    | /api/Vaga          | Cadastra nova vaga           |

✅ Interface Swagger disponível em:
http://<ip-da-vm>:8080/swagger

---

## 🏗️ Instalação Local

1️⃣ Clonar o repositório:
```bash
git clone <link-do-repo>
cd NextParkAPI
```

2️⃣ Restaurar pacotes:
```bash
dotnet restore
```

3️⃣ Rodar localmente:
```bash
dotnet run
```

4️⃣ Acessar:
https://localhost:<porta>/swagger

---

## 🌐 Instalação em Nuvem (Azure + Docker)

### Scripts CLI usados:
```bash
# Criar grupo
az group create --name nextpark-rg --location brazilsouth

# Criar VM
az vm create --name vm_sprint1_nextpark --resource-group nextpark-rg --image Ubuntu2204 --size Standard_B2s --authentication-type password --admin-username nextpark --admin-password Nextpark@123Fiap

# Abrir porta 8080
az vm open-port --name vm_sprint1_nextpark --resource-group nextpark-rg --port 8080
```

### Dentro da VM:
```bash
# Instalar Docker
sudo apt update
sudo apt install -y docker.io

# Adicionar usuário ao grupo docker
sudo usermod -aG docker nextpark
exit

# Reconectar
ssh nextpark@<ip-da-vm>

# Puxar imagem
docker pull rm554983/nextpark-api:latest

# Rodar container
docker run -d -p 8080:80 --name nextpark-api rm554983/nextpark-api:latest
```

---

## 🐳 Dockerfile

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "NextParkAPI.dll"]
```

---


## 👨‍💻 Integrantes

- Raphaela Oliveira Tatto – RM: *554983*
- Tiago Ribeiro Capela – RM: *558021*

	
---
