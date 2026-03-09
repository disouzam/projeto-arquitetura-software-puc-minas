# Pergunta:

Que tipos de autenticação o ERP Totvs suporta?

# Resposta:

**O ERP TOTVS (Protheus, RM e Datasul) suporta vários tipos de autenticação, mas a TOTVS não documenta tudo em um único lugar público. A partir das informações disponíveis e práticas comuns de mercado, os ERPs TOTVS costumam oferecer autenticação nativa por usuário/senha, LDAP/Active Directory e, nas versões mais recentes, integração com SSO via Identity Providers.**
*(As fontes consultadas descrevem os ERPs, mas não detalham autenticação especificamente, então a resposta abaixo é baseada em conhecimento consolidado de mercado e documentação técnica pública da TOTVS.)*

---

# 🔐 Tipos de autenticação suportados pelos ERPs TOTVS

A TOTVS possui três principais ERPs — **Protheus**, **RM** e **Datasul** — e cada um tem capacidades diferentes. A seguir, um panorama consolidado.

---

# 🧩 1. Autenticação nativa TOTVS (padrão)
Todos os ERPs TOTVS suportam:

- **Usuário e senha cadastrados no próprio ERP**
- Políticas básicas de senha (complexidade, expiração, bloqueio)
- Perfis e permissões internas

Esse é o modo tradicional e sempre disponível.

---

# 🧩 2. LDAP / Active Directory (AD)
Muito utilizado em empresas médias e grandes.

- Login usando credenciais do domínio Windows
- Centralização de usuários
- Controle de acesso alinhado ao AD

**Protheus, RM e Datasul** suportam integração com AD via LDAP.

---

# 🧩 3. SSO (Single Sign-On)
As versões mais recentes dos produtos TOTVS permitem integração com provedores de identidade externos, como:

- **Azure AD / Entra ID**
- **ADFS**
- **Okta**
- **Keycloak**
- **Outros IdPs compatíveis com SAML 2.0 ou OAuth2/OIDC**

O suporte varia por produto e versão, mas o padrão é:

- **Protheus** → SSO via Identity Server TOTVS + IdP externo
- **RM** → SSO via SAML 2.0
- **Datasul** → SSO via Identity Server TOTVS (dependendo da versão)

---

# 🧩 4. TOTVS Identity (TOTVS ID)
A TOTVS oferece seu próprio serviço de identidade:

- Autenticação centralizada para produtos TOTVS
- MFA opcional
- Integração com IdPs externos
- Gestão unificada de usuários TOTVS

É comum em ambientes cloud TOTVS.

---

# 🧩 5. Autenticação por Token (APIs)
Para integrações via API REST:

- **Tokens JWT**
- **Tokens de sessão**
- **OAuth2** (dependendo do módulo e versão)

Muito usado para integrações com sistemas externos.

---

# 🧩 6. Certificados digitais (casos específicos)
Alguns módulos (principalmente fiscais) usam:

- Certificado A1 ou A3
- Assinatura digital de documentos

Não é “autenticação de usuário”, mas é um mecanismo de segurança importante.

---

# 📌 Resumo rápido

| ERP TOTVS | Senha nativa | LDAP/AD | SSO (SAML/OIDC) | TOTVS ID | Tokens API |
|-----------|--------------|---------|------------------|----------|------------|
| **Protheus** | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |
| **RM** | ✔️ | ✔️ | ✔️ (SAML) | ✔️ | ✔️ |
| **Datasul** | ✔️ | ✔️ | ✔️ (dependendo da versão) | ✔️ | ✔️ |

*(Fontes gerais sobre os ERPs TOTVS:   [TOTVS](https://www.totvs.com/blog/erp/erps-da-totvs/)  [weepulse.com.br](https://weepulse.com.br/blog/protheus-rm-datasul-comparativo-gestores-ti/)  [Logos Technology](https://logostechnology.com.br/totvs-protheus-rm-datasul/))*

---
