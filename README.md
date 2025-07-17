# CSPM_AZURE.sh

# Criar Aplicação no Azure AD com Permissões e Role através do shell script - Debian/Ubuntu

Este script automatiza o processo de criação de uma aplicação no Azure Active Directory (Azure AD), geração de client secret, atribuição de permissões no Microsoft Graph e concessão da role `Reader` em uma subscription do Azure.

> ✅ Recomendado para ambientes que desejam realizar automações seguras com acesso controlado à API do Azure.

---

## ⚙️ Funcionalidades

- Instala automaticamente dependências: `azure-cli`, `jq`, `curl`, `apt-transport-https`, `lsb-release`, `gnupg`.
- Realiza autenticação via Azure CLI (`az login`).
- Cria uma aplicação no Azure AD com suporte multitenant.
- Gera um client secret válido por 1 ano.
- Atribui permissões de aplicação ao Microsoft Graph:
  - `Directory.Read.All`
  - `Policy.Read.All`
  - `UserAuthenticationMethod.Read.All`
- Concede `admin-consent` para ativar as permissões.
- Cria um *Service Principal* para a aplicação.
- Atribui a role `Reader` na subscription ativa.
- Salva as credenciais geradas em um arquivo `.csv`.

---

## 📁 Arquivos gerados

- `vulneri_azure_credentials.csv` com as seguintes colunas:
  - `Client ID`
  - `Client Secret`
  - `Tenant ID`

---

## 🧾 Pré-requisitos

- Sistema baseado em Debian/Ubuntu.
- Permissões de **Global Administrator** no tenant do Azure AD (necessárias para o `admin-consent`).
- Permissões para criar aplicações, service principals e atribuir roles.
- Acesso à internet.
- Conta com pelo menos uma subscription ativa.

---

## 🚀 Como usar

### 1. Torne o script executável

```bash
chmod +x criar-app-vulneri-azure.sh
```

### 2. Execute o script

```bash
./criar-app-vulneri-azure.sh
```

Durante a execução, o script:

- Abrirá o navegador para você realizar login com a conta do Azure.
- Mostrará as subscriptions disponíveis e selecionará a padrão.
- Criará os recursos e mostrará as informações passo a passo.
- Exibirá ao final o conteúdo do arquivo `.csv` com as credenciais.

---

## 📌 Permissões Microsoft Graph utilizadas

| Permissão                      | Tipo        | GUID                                     |
|-------------------------------|-------------|------------------------------------------|
| Directory.Read.All            | Application | `7ab1d382-f21e-4acd-a863-ba3e13f7da61`   |
| Policy.Read.All               | Application | `5d6b6bb7-de71-4623-b4af-96380a352509`   |
| UserAuthenticationMethod.Read.All | Application | `df021288-bdef-4463-88db-98f22de89214` |

Essas permissões são atribuídas com `admin-consent`, ou seja, efetivas imediatamente após o script ser executado com sucesso.

---

## 📥 Exemplo de saída

```bash
[INFO] Aplicacao registrada com App ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
[INFO] Client secret criado.
[INFO] Permissoes aplicadas e consentidas.
[INFO] Permissao atribuida na subscription.
[INFO] Credenciais salvas em: vulneri_azure_credentials.csv
```

---

## 📤 Após a execução

Envie o arquivo `vulneri_azure_credentials.csv` para a equipe de segurança da informação:

```
security@vulneri.io
```

---

## 🛠 Suporte

Caso encontre erros relacionados a permissões (ex: `Consent validation failed` ou `scope required`), verifique:

- Se o usuário autenticado possui as permissões necessárias.
- Se a propagação do Service Principal foi concluída (o script aguarda automaticamente).
- Se há mais de uma aplicação com o mesmo nome no diretório (o script usa nome fixo `vulneri`).

---

## 🧪 Testado em

- Ubuntu 22.04 LTS
- Azure CLI versão `2.61.0`
- Conta com `Global Admin` e subscription ativa no modelo "Pagar conforme o uso"

---

## 📝 Licença

Uso interno pela equipe Vulneri. Reutilização ou redistribuição requer autorização.






# CSPM_AZURE.ps1
# Script PowerShell: Registro Automático de Aplicação no Azure AD - Windows

Este script PowerShell automatiza o processo de criação e configuração de uma aplicação no Azure Active Directory (Azure AD), ideal para integrações que exigem autenticação com `client_id` e `client_secret`.

---

## 🚀 Funcionalidades

- Autenticação no Azure via CLI
- Registro de aplicação no Azure AD
- Criação de client secret
- Concessão de permissões Microsoft Graph:
  - Directory.Read.All
  - Policy.Read.All
  - UserAuthenticationMethod.Read.All
- Criação de Service Principal
- Atribuição da role `Reader` na subscription
- Exportação das credenciais em um arquivo CSV
-  Permitir execução temporária de scripts nesta sessão
   - Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
---

## 📋 Pré-requisitos

- PowerShell 5.1+ ou PowerShell Core 7+
- Permissões administrativas no Azure AD
- Azure CLI instalado (o script instala automaticamente se não estiver presente)


---

## 📁 Arquivos Gerados

- `vulneri_powershell_azure_credentials.csv`: Contém as seguintes colunas:
  - `client_id`
  - `client_secret`
  - `tenant_id`
  - `subscription_id`

---

## 🔧 Como Executar

1. Baixe e salve o script como `access_azure.ps1`.

2. No PowerShell, navegue até o diretório do script:

```powershell
cd "C:\caminho\para\o\script"
```

3. Execute o script:

```powershell
.ccess_azure.ps1
```

> 💡 O script aplica automaticamente a política de execução temporária com `Bypass` para permitir sua execução sem exigir alterações permanentes no sistema.

---

## 🛡️ Segurança

- Os dados do client secret são exibidos e armazenados no CSV apenas no momento da criação.
- Guarde o CSV em local seguro.
- O client secret **não poderá ser recuperado** depois.

---

## 🧼 Exemplo de saída do CSV

```
client_id,client_secret,tenant_id,subscription_id
e1234567-abcd-1234-abcd-e123456789ab,wxyzSecret9876,11111111-2222-3333-4444-555555555555,66666666-7777-8888-9999-000000000000
```

---

## 📚 Referências

- [Documentação do Azure CLI](https://learn.microsoft.com/cli/azure/)
- [Permissões do Microsoft Graph](https://learn.microsoft.com/graph/permissions-reference)

---

## ✍️ Observações

- O script evita interações manuais sempre que possível, priorizando uma execução fluida para automação.
- Caso deseje alterar as permissões ou roles atribuídas, edite as variáveis no bloco de **CONFIGURAÇÃO INICIAL** do script.

---

## 🙋 Suporte

Caso encontre erros ou deseje sugerir melhorias, entre em contato com a equipe de desenvolvimento.

---
