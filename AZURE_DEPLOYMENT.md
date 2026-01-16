# Guia de Implantação na Azure

## Alterações Realizadas

O código foi revisado e as seguintes correções foram implementadas:

### 1. **Controllers/FuncionarioController.cs**
- ✅ Adicionado `_context.Funcionarios.Update(funcionarioBanco)` no método `Atualizar`
- ✅ Adicionado `_context.Funcionarios.Remove(funcionarioBanco)` no método `Deletar`
- ✅ Removidos TODOs do código

### 2. **appsettings.json (Produção)**
- ✅ Preenchido com placeholders para Azure SQL Database
- ✅ Preenchido com placeholders para Azure Storage Account
- ✅ Template pronto para substituir com valores reais

### 3. **appsettings.Development.json (Desenvolvimento)**
- ✅ Configurado para LocalDB (desenvolvimento local)
- ✅ Configurado para Azure Storage Emulator (desenvolvimento local)

### 4. **Program.cs**
- ✅ Comentário melhorado para clareza

## Próximos Passos para Implantação

### 1. Configurar Azure SQL Database
```
Server: <seu-servidor>.database.windows.net
Database: <seu-banco>
User ID: <seu-usuario>
<!-- Password: <sua-senha> (NÃO SUBA SUA SENHA NUM REPOSITÓRIO PÚBLICO) -->
```

Atualize a string de conexão em `appsettings.json`:
```json
"ConexaoPadrao": "Server=tcp:<seu-servidor>.database.windows.net,1433;Initial Catalog=<seu-banco>;Persist Security Info=False;User ID=<seu-usuario>;Password=<sua-senha>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
```

### 2. Configurar Azure Storage Account
```
Account Name: <sua-conta-armazenamento>
Account Key: <sua-chave>
```

Atualize a string de conexão em `appsettings.json`:
```json
"SAConnectionString": "DefaultEndpointsProtocol=https;AccountName=<sua-conta-armazenamento>;AccountKey=<sua-chave>;EndpointSuffix=core.windows.net"
```

### 3. Criar a Tabela no Azure Table Storage
A tabela "FuncionarioLog" será criada automaticamente pelo código no primeiro acesso (método `CreateIfNotExists()`).

### 4. Executar Migrations
```bash
dotnet ef database update
```

### 5. Publicar na Azure
```bash
dotnet publish -c Release
```

## Variáveis de Ambiente (Alternativa Segura)

Para maior segurança, use Azure Key Vault e configure as variáveis de ambiente:

```bash
ASPNETCORE_ENVIRONMENT=Production
ConnectionStrings__ConexaoPadrao=<connection-string>
ConnectionStrings__SAConnectionString=<storage-connection-string>
ConnectionStrings__AzureTableName=FuncionarioLog
```

## Checklist antes da implantação

- [ ] Azure SQL Database criado e acessível
- [ ] Azure Storage Account criado
- [ ] Strings de conexão atualizadas em `appsettings.json`
- [ ] Migrations executadas com sucesso
- [ ] Código compilado sem erros: `dotnet build`
- [ ] Testes locais realizados
- [ ] Variáveis de ambiente configuradas no Azure App Service

## Considerações de Segurança

- **Não armazene credenciais em appsettings.json em produção**
- Use Azure Key Vault ou Managed Identity
- Mantenha appsettings.Development.json local (não commitar credenciais reais)
- Valide as strings de conexão antes de implantar


<!-- Dicas do Copilot -->