# Pergunta:

OAuth, LDAP e SAML. São todos procolos de autenticação?

# Resposta:

**Não — OAuth, LDAP e SAML *não* são todos protocolos de autenticação.** Eles pertencem ao universo de identidade, mas cada um resolve um problema diferente: alguns são de **autenticação**, outros de **autorização**, e um deles nem é exatamente um “protocolo”, mas sim um **mecanismo de diretório** usado para validar credenciais. Vamos organizar isso de forma clara e objetiva.

---

# 🔐 O que cada um realmente é

## 🟦 **SAML — Protocolo de Autenticação (e SSO)**
- SAML 2.0 foi criado para **autenticar usuários** e fornecer *assertions* de identidade para aplicações.
- É muito usado em ambientes corporativos para **SSO (Single Sign-On)**.
- Ele entrega ao sistema um pacote XML dizendo: *“Este usuário já foi autenticado pelo IdP.”*
- Portanto, **SAML = Autenticação**.
  [Auth0](https://auth0.com/pt/intro-to-iam/saml-vs-oauth)

---

## 🟩 **OAuth 2.0 — Protocolo de Autorização**
- OAuth **não é um protocolo de autenticação**.
- Ele foi criado para permitir que um usuário dê a um aplicativo acesso limitado a um recurso **sem compartilhar a senha**.
- Exemplo: “Permitir que o Spotify acesse meus contatos do Google”.
- Ele entrega *tokens de acesso*, não identidade.
- Portanto, **OAuth = Autorização**.
  [Auth0](https://auth0.com/pt/intro-to-iam/saml-vs-oauth)

> Observação: quando você vê “Login com Google/Facebook”, na verdade quem faz a **autenticação** é o **OpenID Connect (OIDC)**, que é construído *em cima* do OAuth.

---

## 🟨 **LDAP — Protocolo de Diretório (usado para autenticação, mas não é um protocolo de autenticação)**
- LDAP é um **protocolo para consultar diretórios**, como o Active Directory.
- Ele permite verificar credenciais, mas **não define um fluxo de autenticação moderno** como SAML ou OIDC.
- É comum dizer “autenticação via LDAP”, mas tecnicamente o LDAP é apenas o **meio de comunicação** com o diretório.
- Portanto, **LDAP = Protocolo de diretório**, usado *para* autenticação, mas **não é um protocolo de autenticação** por si só.

---

# 🧩 Resumo rápido

| Tecnologia | Tipo | Para que serve |
|-----------|------|----------------|
| **SAML** | Autenticação | SSO corporativo, entrega identidade |
| **OAuth 2.0** | Autorização | Permitir acesso limitado a recursos |
| **LDAP** | Protocolo de diretório | Validar credenciais em diretórios como AD |

---
