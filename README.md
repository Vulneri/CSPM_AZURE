# CSPM_AZURE.sh

# Criar Aplicação no Azure AD com Permissões e Role

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
