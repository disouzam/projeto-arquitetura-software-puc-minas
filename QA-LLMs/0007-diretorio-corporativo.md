# Pergunta:

O que é um diretório corporativo no sentido do Active Directory / LDAP?

# Resposta:

Vamos direto ao ponto: **um diretório corporativo, no sentido de Active Directory ou LDAP, é uma base centralizada que armazena identidades, credenciais e informações organizacionais, permitindo que sistemas e aplicações consultem “quem é o usuário” e “o que ele pode fazer”.** Ele funciona como o “cérebro” de identidade de uma empresa.

A seguir, explico de forma clara e estruturada.

---

# 🧩 O que é um diretório corporativo?

Um **diretório corporativo** é um serviço especializado em armazenar e organizar informações sobre:

- Usuários
- Grupos
- Computadores
- Permissões
- Recursos da rede
- Políticas de segurança

Ele é otimizado para **leitura rápida**, consultas hierárquicas e autenticação.

Pense nele como uma **agenda telefônica corporativa superpoderosa**, usada por sistemas para validar identidades e autorizações.

---

# 🟦 Active Directory (AD)

O **Active Directory**, da Microsoft, é o diretório corporativo mais usado no mundo.

Ele oferece:

- Autenticação (Kerberos, NTLM)
- Autorização baseada em grupos
- Políticas de segurança (GPOs)
- Integração com Windows, servidores e aplicações corporativas
- Estrutura hierárquica (domínios, árvores, florestas)

É muito mais que um simples repositório: é um **ecossistema completo de identidade e governança**.

---

# 🟨 LDAP (Lightweight Directory Access Protocol)

**LDAP não é um diretório**, mas sim **o protocolo usado para consultar e manipular diretórios**.

Ele é usado por:

- Active Directory
- OpenLDAP
- Apache Directory
- Oracle Internet Directory
- Novell eDirectory

Ou seja:

> **LDAP é o “idioma” que os sistemas usam para conversar com o diretório.**

Quando alguém diz “autenticação via LDAP”, na prática significa:

- O sistema envia usuário/senha para o diretório via LDAP
- O diretório valida as credenciais
- O sistema recebe a resposta

---

# 🔐 Para que serve um diretório corporativo?

## 1. **Autenticação centralizada**
Todos os sistemas consultam o mesmo lugar para validar usuários.

## 2. **Autorização baseada em grupos**
Permissões são atribuídas a grupos, não a usuários individuais.

## 3. **Single Sign-On (SSO)**
O usuário faz login uma vez e acessa vários sistemas.

## 4. **Governança e auditoria**
Controle de quem acessa o quê, quando e como.

## 5. **Gestão de ciclo de vida**
- Entrada de novos funcionários
- Mudança de função
- Desligamento

Tudo controlado em um único ponto.

---

# 🧠 Resumo simples

| Conceito | O que é | Papel |
|---------|---------|-------|
| **Diretório corporativo** | Banco de identidades e permissões | Centralizar autenticação e autorização |
| **Active Directory** | Diretório corporativo da Microsoft | Identidade + políticas + governança |
| **LDAP** | Protocolo de comunicação | “Linguagem” usada para consultar diretórios |

---
