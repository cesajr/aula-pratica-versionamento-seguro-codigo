Com certeza! Aqui está o material formatado em Markdown, pronto para ser copiado e colado no seu arquivo `README.md` no repositório do GitHub.

```markdown
# 🔒 Guia Prático de Versionamento Seguro com Git e GitHub

## Um Workshop de DevSecOps para Futuros Desenvolvedores de Sistemas

Olá, futuros(as) desenvolvedores(as)!

Bem-vindos a este workshop prático focado em um dos pilares da engenharia de software moderna: **versionamento seguro**. Este repositório serve como material didático para nossa aula, onde vamos além do `git commit` e `git push`.

Nosso objetivo é estabelecer um mindset de **DevSecOps** (Desenvolvimento, Segurança e Operações) desde o início de sua carreira. Vamos aprender a usar o Git e o GitHub não apenas para salvar nosso código, mas para protegê-lo ativamente contra o vazamento de informações sensíveis usando ferramentas automatizadas.

```

### 📝 Conteúdo

1.  [Visão Geral e Objetivos](#1-visão-geral-e-objetivos)
2.  [O Que é um "Segredo" no Código?](#2-o-que-e-um-segredo-no-código)
3.  [Pré-requisitos](#3-pré-requisitos)
4.  [Cenário Proposto](#4-cenário-proposto)
5.  [Passo 1: A Primeira Linha de Defesa (Ambiente Local)](#5-passo-1-a-primeira-linha-de-defesa-ambiente-local)
    *   [1.1 Configuração Inicial do Git](#11-configuração-inicial-do-git)
    *   [1.2 O Artefato de Defesa Essencial: .gitignore](#12-o-artefato-de-defesa-essencial-gitignore)
6.  [Passo 2: Criando o Código e a Vulnerabilidade](#6-passo-2-criando-o-código-e-a-vulnerabilidade)
7.  [Passo 3: O Commit Perigoso e a Refatoração Segura](#7-passo-3-o-commit-perigoso-e-a-refatoração-segura)
    *   [3.1 O Erro: "Hardcoding" e Comitando Segredos](#31-o-erro-hardcoding-e-comitando-segredos)
    *   [3.2 A Solução: Externalizando Segredos com .env](#32-a-solução-externalizando-segredos-com-env)
8.  [Passo 4: Configurando a Fortaleza no GitHub](#8-passo-4-configurando-a-fortaleza-no-github)
    *   [4.1 Criando e Enviando o Repositório](#41-criando-e-enviando-o-repositório)
    *   [4.2 🛡️ Protegendo a Branch main](#42-protegendo-a-branch-main)
    *   [4.3 🤖 Ativando o Secret Scanning (Análise de Segredos)](#43-ativando-o-secret-scanning-análise-de-segredos)
9.  [Passo 5: O Fluxo de Trabalho Seguro (Branch, PR e Análise Automatizada)](#9-passo-5-o-fluxo-de-trabalho-seguro-branch-pr-e-análise-automatizada)
    *   [5.1 O Padrão de Trabalho: Feature Branches](#51-o-padrão-de-trabalho-feature-branches)
    *   [5.2 Simulando um Vazamento Acidental](#52-simulando-um-vazamento-acidental)
    *   [5.3 Abrindo o Pull Request (PR)](#53-abrindo-o-pull-request-pr)
    *   [5.4 🚨 A Segurança Automatizada em Ação!](#54-a-segurança-automatizada-em-ação)
10. [Conclusão e Próximos Passos](#10-conclusão-e-próximos-passos)
11. [Sobre o Autor](#11-sobre-o-autor)

---

### 1. Visão Geral e Objetivos

Nesta aula, simularemos um ciclo de desenvolvimento completo, desde a configuração inicial até a entrega de uma nova funcionalidade, com foco total em segurança.

Ao final deste material, você será capaz de:

*   Compreender os riscos reais do vazamento de credenciais (segredos) no código.
*   Configurar um repositório Git local seguro, utilizando o `.gitignore` de forma eficaz.
*   Implementar um padrão de desenvolvimento que externaliza segredos do código-fonte (usando arquivos `.env`).
*   Configurar proteções de repositório no GitHub (Branch Protection Rules).
*   Utilizar as ferramentas de segurança automatizadas do GitHub (Secret Scanning) para detectar vazamentos antes que eles entrem na base de código principal.

---

### 2. O Que é um "Segredo" no Código?

No contexto de desenvolvimento, um **segredo** é qualquer informação confidencial que concede acesso a um recurso. Se exposto, ele pode levar a uma violação de dados, perdas financeiras ou comprometimento do sistema.

Exemplos comuns:

*   Chaves de API (AWS, Google Cloud, Stripe, SendGrid)
*   Senhas de banco de dados
*   Tokens de autenticação (OAuth, JWT)
*   Certificados SSL/TLS (chaves privadas)
*   Chaves SSH

A regra de ouro é: **Se um valor é confidencial e não deve ser público, ele é um segredo.**

---

### 3. Pré-requisitos

Para acompanhar este workshop, você precisará de:

*   **Git instalado** ([git-scm.com](https://git-scm.com/)).
*   Uma **conta no GitHub** ([github.com](https://github.com/)).
*   Um **editor de código** (ex: VS Code).
*   Conhecimento básico de linha de comando (Terminal, PowerShell, etc.).
*   **Python 3 instalado** (para executar nosso exemplo).
*   A biblioteca `python-dotenv`:
    ```bash
    pip install python-dotenv
    ```

---

### 4. Cenário Proposto

Nosso projeto será uma simples aplicação em Python. Ela precisa se conectar a um "banco de dados" (simulado) e a um "serviço externo" (simulado). Para isso, ela usará uma senha de banco de dados e uma chave de API. Nosso desafio é gerenciar o código desse projeto sem nunca expor essas duas credenciais no repositório do GitHub.

---

### 5. Passo 1: A Primeira Linha de Defesa (Ambiente Local)

A segurança começa na sua máquina.

#### 5.1. Configuração Inicial do Git

Se é sua primeira vez usando Git, configure sua identidade. Isso é vital para a rastreabilidade dos *commits*.

```bash
git config --global user.name "Seu Nome Completo"
git config --global user.email "seuemail@exemplo.com"
git config --global init.defaultBranch main # Boa prática: define a branch padrão como 'main'
```

#### 5.2. O Artefato de Defesa Essencial: .gitignore

O `.gitignore` é um arquivo de texto que diz ao Git quais arquivos ou pastas ele deve ignorar ativamente. Ele é sua principal ferramenta para evitar que segredos e "lixo" de desenvolvimento (como dependências ou arquivos de *build*) sejam *commitados* acidentalmente.

1.  Crie a pasta do projeto e entre nela:
    ```bash
    mkdir projeto-seguro-aula
    cd projeto-seguro-aula
    ```
2.  Crie o arquivo `.gitignore` (no Windows, pode usar `echo. > .gitignore` ou criar no VS Code):
    ```bash
    touch .gitignore
    ```
3.  Abra o `.gitignore` e adicione o seguinte conteúdo:
    ```
    # Arquivos de segredos e configuração de ambiente
    .env
    config.ini
    *.pem
    *.key

    # Dependências e cache do Python
    venv/
    __pycache__/
    *.pyc

    # Arquivos de IDE (ex: VS Code)
    .vscode/
    ```
    **Análise:** Ao adicionar `.env` aqui, estamos instruindo o Git a nunca rastrear qualquer arquivo com esse nome. Este é o passo preventivo mais importante.

---

### 6. Passo 2: Criando o Código e a Vulnerabilidade

Vamos criar nossa aplicação. Para fins didáticos, vamos começar da forma errada, inserindo os segredos diretamente no código (prática conhecida como *hardcoding*).

1.  Crie o arquivo `app.py` com o seguinte conteúdo:
    ```python
    # app.py
    # 
    # ⚠️ ALERTA DE VULNERABILIDADE ⚠️
    # As credenciais estão "hardcoded" (fixas no código).
    # Esta é uma prática extremamente perigosa!

    DB_PASSWORD = "PasswordSuperSecreta123"
    API_KEY = "CHAVE DISPONIBILIZADA AQUI" # Formato comum de API key

    def conectar_banco():
        print(f"Conectando ao banco com a senha: {DB_PASSWORD}")
        # ... lógica de conexão ...
        print("Conexão ao banco bem-sucedida!")

    def chamar_api():
        print(f"Chamando API externa com a chave: {API_KEY[:4]}...") # Mostra só o início
        # ... lógica de chamada da API ...
        print("API respondeu com sucesso!")

    if __name__ == "__main__":
        print("Iniciando aplicação...")
        conectar_banco()
        chamar_api()
        print("Aplicação finalizada.")
    ```
    Neste ponto, qualquer pessoa com acesso a este arquivo `app.py` tem acesso total à nossa senha do banco e à nossa chave de API.

---

### 7. Passo 3: O Commit Perigoso e a Refatoração Segura

#### 7.1. O Erro: "Hardcoding" e Comitando Segredos

Vamos simular o que um desenvolvedor desatento faria: iniciar o repositório e *commitar* o código vulnerável.

```bash
git init
git add app.py
git commit -m "feat: Versão inicial do app com conexão de DB e API"
```
❌ **PERIGO:** Este é o ponto sem volta. A partir deste *commit*, a `DB_PASSWORD` e a `API_KEY` estão permanentemente gravadas no histórico do Git. Mesmo que as apaguemos em um *commit* futuro, qualquer pessoa que clonar o repositório pode navegar pelo histórico e encontrar essas credenciais.

#### 7.2. A Solução: Externalizando Segredos com .env

A prática correta é externalizar os segredos para um arquivo de ambiente local (que já está no nosso `.gitignore`) e fazer o código ler desse arquivo.

1.  Crie o arquivo `.env` (na mesma pasta do `app.py`):
    ```ini
    # Este arquivo NUNCA deve ser comitado.
    # Ele contém os segredos do ambiente de DESENVOLVIMENTO.
    DB_PASSWORD_ENV="PasswordSuperSecreta123_DoAmbiente"
    API_KEY_ENV="CHAVE DISPONIBILIZADA AQUI"
    ```
2.  Refatore o `app.py` para usar a biblioteca `python-dotenv` e ler essas variáveis:
    ```python
    # app.py
    #
    # ✅ BOA PRÁTICA ✅
    # O código agora lê os segredos das variáveis de ambiente.
    # O arquivo .env (que contém os segredos) é ignorado pelo Git.

    import os
    from dotenv import load_dotenv

    # Carrega as variáveis do arquivo .env para o ambiente do sistema
    load_dotenv()

    # Lê os segredos das variáveis de ambiente
    DB_PASSWORD = os.getenv("DB_PASSWORD_ENV")
    API_KEY = os.getenv("API_KEY_ENV")

    # Validação de segurança: Verifica se as variáveis foram carregadas
    if not DB_PASSWORD or not API_KEY:
        print("ERRO: Variáveis de ambiente DB_PASSWORD_ENV ou API_KEY_ENV não definidas.")
        exit(1) # Encerra o script se os segredos não estiverem presentes

    def conectar_banco():
        # Prática de log seguro: NUNCA imprima a senha em logs!
        print("Conectando ao banco...")
        # ... lógica de conexão ...
        print("Conexão ao banco bem-sucedida!")

    def chamar_api():
        print(f"Chamando API externa com a chave: {API_KEY[:4]}...")
        # ... lógica de chamada da API ...
        print("API respondeu com sucesso!")

    if __name__ == "__main__":
        print("Iniciando aplicação...")
        conectar_banco()
        chamar_api()
        print("Aplicação finalizada.")
    ```
3.  Faça o *commit* da correção:
    ```bash
    # Verifique o status:
    # O Git deve mostrar 'app.py' modificado e NADA sobre o '.env' (pois está ignorado)
    git status 

    # Adicione apenas a alteração segura do app.py
    git add app.py
    git commit -m "refactor: Remove segredos do código (hardcoded) e usa .env"
    ```
    Agora, o nosso código está limpo e seguro, mas o histórico do Git ainda contém o *commit* perigoso. É por isso que a prevenção (Passo 1) é tão importante.

---

### 8. Passo 4: Configurando a Fortaleza no GitHub

Agora vamos para a nuvem e configurar as defesas automatizadas do GitHub.

#### 8.1. Criando e Enviando o Repositório

1.  No GitHub, crie um **novo repositório** (ex: `projeto-seguro-aula`).
2.  **Importante:** Deixe-o **Público (Public)** para que as ferramentas de segurança gratuitas funcionem.
3.  **NÃO** marque "Add a README", ".gitignore" ou "license" (já os temos localmente).
4.  Siga as instruções para "push an existing repository":
    ```bash
    git remote add origin https://github.com/SEU_USUARIO/projeto-seguro-aula.git
    git push -u origin main
    ```
    Substitua `SEU_USUARIO` pelo seu usuário.

#### 8.2. 🛡️ Protegendo a Branch main

A `main` é sua fonte da verdade. Ninguém deve ser capaz de enviar *commits* diretamente para ela. Todo código deve passar por uma revisão (Pull Request).

1.  No seu repositório GitHub, vá em **Settings (Configurações) > Branches**.
2.  Clique em **Add branch protection rule (Adicionar regra de proteção de branch)**.
3.  Em "Branch name pattern", digite `main`.
4.  Marque as seguintes caixas (essenciais):
    *   ✅ **Require a pull request before merging (Exigir um pull request antes de mesclar)**
        Isso impede *commits* diretos (`git push origin main` será bloqueado).
    *   ✅ **Require approvals (1) (Exigir aprovações)**
        Exige que pelo menos 1 outra pessoa revise e aprove a mudança.

#### 8.3. 🤖 Ativando o Secret Scanning (Análise de Segredos)

Esta é a ferramenta de IA do GitHub que varre seu código em busca de formatos de segredos conhecidos (chaves de API, tokens, etc.). Para repositórios públicos, ela é gratuita e já vem ativada por padrão.

1.  No seu repositório GitHub, vá em **Settings > Code security and analysis**.
2.  Encontre "Secret scanning" e clique em **Enable (Ativar)**.
    *   Se já estiver ativado para repositórios públicos, você verá um visto verde.

Pronto. Seu repositório agora está ativamente monitorado contra vazamentos.

---

### 9. Passo 5: O Fluxo de Trabalho Seguro (Branch, PR e Análise Automatizada)

Vamos simular a adição de uma nova funcionalidade, mas desta vez, um desenvolvedor (nós) cometerá um erro e será pego pelas nossas defesas.

#### 9.1. O Padrão de Trabalho: Feature Branches

Nunca trabalhe na `main`. Crie uma *branch* (ramificação) para cada nova tarefa.

```bash
# Cria uma nova branch e muda para ela
git checkout -b feature/adicionar-logs
```

#### 9.2. Simulando um Vazamento Acidental

Vamos adicionar uma função de log. O desenvolvedor, com pressa, cola um token de um serviço de log diretamente no código.

1.  Edite o `app.py` e adicione a função `registrar_log` (e sua chamada no `if __name__ == "__main__":`):
    ```python
    # ... (início do app.py) ...

    # ⚠️ VAZAMENTO PROPOSITAL PARA TESTE ⚠️
    # O desenvolvedor esqueceu a boa prática e colou um token!
    LOG_SERVICE_TOKEN = "glpat-xY_1a2b3c4d5e6f7g8h9i-ABC" # Formato de token do GitLab

    def registrar_log(mensagem):
        print(f"Registrando log no serviço externo (token: {LOG_SERVICE_TOKEN[:5]}...): {mensagem}")
        # ... lógica de envio de log ...

    def conectar_banco():
    # ... (resto do código) ...

    if __name__ == "__main__":
        print("Iniciando aplicação...")
        registrar_log("Aplicação iniciada.") # <-- Nova chamada
        conectar_banco()
        chamar_api()
        registrar_log("Aplicação finalizada.") # <-- Nova chamada
        print("Aplicação finalizada.")
    ```

#### 9.3. Abrindo o Pull Request (PR)

Vamos *commitar* e enviar esta *branch* vulnerável para o GitHub.

```bash
git add app.py
git commit -m "feat: Adiciona sistema de logging"
git push origin feature/adicionar-logs
```
Agora, vá ao GitHub. O site sugerirá que você "Compare & pull request". Clique e crie o Pull Request (PR) da `feature/adicionar-logs` para a `main`.

#### 9.4. 🚨 A Segurança Automatizada em Ação!

Quase imediatamente, duas coisas acontecerão:

1.  **Bloqueio pela Regra de Branch:** O GitHub impedirá você de clicar no botão "Merge pull request". Ele mostrará uma mensagem dizendo: "Merging is blocked" e "At least 1 approving review is required."
    **DEFESA 1 (Humana) ATIVADA:** A regra de proteção forçou a necessidade de uma revisão.
2.  **Alerta do Secret Scanning:** Em poucos segundos ou minutos, o GitHub detectará o formato do token (`glpat-...`)!
    **DEFESA 2 (Automatizada) ATIVADA:** O GitHub fará duas coisas:
    *   Enviará um e-mail para você (o autor do *commit*) e para os administradores do repositório, alertando sobre o segredo exposto.
    *   Criará um alerta na aba **Security > Secret scanning** do seu repositório, mostrando exatamente o arquivo e a linha onde o segredo foi encontrado.

Neste ponto, o vazamento foi contido. O segredo não chegou à *branch main* graças à combinação da proteção de *branch* (exigindo revisão) e do Secret Scanning (que alertou o revisor sobre o perigo).

A ação correta seria: Rejeitar o PR, voltar ao código, mover o `LOG_SERVICE_TOKEN` para o arquivo `.env`, fazer um novo *commit* e atualizar o PR.

---

### 10. Conclusão e Próximos Passos

Parabéns! Você implementou um fluxo de desenvolvimento seguro de ponta a ponta, usando as melhores práticas e ferramentas de automação do mercado.

**Principais Lições:**

*   **Prevenção é Tudo:** O `.gitignore` é sua ferramenta de prevenção mais importante.
*   **Externalize Segredos:** Código-fonte e segredos NUNCA devem andar juntos. Use variáveis de ambiente (`.env`).
*   **O Histórico é Permanente:** Um segredo *commitado* é um segredo comprometido. A única solução real é **revogar (invalidar)** esse segredo imediatamente (ex: trocar a senha no banco, gerar uma nova API key).
*   **Proteja a `main`:** A *branch* principal deve ser tratada como um ambiente de produção: protegida e revisada.
*   **Confie na Automação:** Ferramentas como o Secret Scanning são seus "olhos" 24/7, garantindo que erros humanos sejam detectados rapidamente.

**Próximos Passos (Avançado):**

*   **Limpeza de Histórico:** Pesquise sobre ferramentas como o `bfg-repo-cleaner`. Elas são usadas para remover segredos de um histórico já comprometido. (É um processo complexo e destrutivo, usado apenas em emergências).
*   **Vaults de Segredos:** Em ambientes de produção, não usamos arquivos `.env`. Usamos "cofres" de segredos, como o HashiCorp Vault, AWS Secrets Manager ou GitHub Actions Secrets.
*   **Code Scanning:** Explore a outra ferramenta do GitHub, "Code Scanning" (usando CodeQL), que analisa seu código em busca de vulnerabilidades (SQL Injection, XSS, etc.), não apenas segredos.

---

### 11. Sobre o Autor

Este material foi preparado por:

**Professor Paulo César Junior**

---
```
