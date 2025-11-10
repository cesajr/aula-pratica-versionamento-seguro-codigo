```markdown
# 🔒 Versionamento Seguro de Código com Git e GitHub

## Uma Aula Prática para Desenvolvedores Iniciantes

Olá, futuros desenvolvedores e desenvolvedoras de sistemas!

Este repositório contém o material didático e um exemplo prático para nossa aula sobre Versionamento Seguro de Código, utilizando as ferramentas Git e GitHub. Meu objetivo é guiá-los através das melhores práticas para gerenciar o código de seus projetos de forma segura, evitando vazamentos de informações sensíveis e garantindo um fluxo de trabalho robusto.

---

### 📝 Conteúdo

1.  [Visão Geral e Objetivos](#1-visão-geral-e-objetivos)
2.  [Pré-requisitos](#2-pré-requisitos)
3.  [Cenário Proposto](#3-cenário-proposto)
4.  [Passo 1: Preparando o Ambiente](#4-passo-1-preparando-o-ambiente)
    *   [1.1 Instalação e Configuração Inicial do Git](#11-instalação-e-configuração-inicial-do-git)
    *   [1.2 O Arquivo Mágico: `.gitignore`](#12-o-arquivo-mágico-gitignore)
5.  [Passo 2: Criando Nosso Código (e a Primeira Vulnerabilidade)](#5-passo-2-criando-nosso-código-e-a-primeira-vulnerabilidade)
6.  [Passo 3: O Primeiro Commit e a Correção do Risco](#6-passo-3-o-primeiro-commit-e-a-correção-do-risco)
    *   [3.1 Iniciando o Repositório e o Primeiro Commit (Errado!)](#31-iniciando-o-repositório-e-o-primeiro-commit-errado)
    *   [3.2 A Forma Correta: Externalizando os Segredos](#32-a-forma-correta-externalizando-os-segredos)
7.  [Passo 4: Subindo para o GitHub e Configurando a Segurança](#7-passo-4-subindo-para-o-github-e-configurando-a-segurança)
    *   [4.1 Criando o Repositório no GitHub](#41-criando-o-repositório-no-github)
    *   [4.2 Protegendo a Branch `main`](#42-protegendo-a-branch-main)
    *   [4.3 Ativando o Secret Scanning](#43-ativando-o-secret-scanning)
8.  [Passo 5: O Fluxo de Trabalho Seguro (Branch, PR e Análise)](#8-passo-5-o-fluxo-de-trabalho-seguro-branch-pr-e-análise)
    *   [5.1 Criando uma Nova Branch](#51-criando-uma-nova-branch)
    *   [5.2 Desenvolvendo e "Vazando" um Segredo (de propósito!)](#52-desenvolvendo-e-vazando-um-segredo-de-propósito)
    *   [5.3 Fazendo o Commit e Abrindo o Pull Request](#53-fazendo-o-commit-e-abrindo-o-pull-request)
    *   [5.4 A Mágica da Segurança em Ação!](#54-a-mágica-da-segurança-em-ação)
9.  [Conclusão e Próximos Passos](#9-conclusão-e-próximos-passos)
10. [Sobre o Autor](#10-sobre-o-autor)

---

### 1. Visão Geral e Objetivos

Nesta aula prática, vamos mergulhar no universo do versionamento de código, com um foco especial na **segurança**. Utilizaremos Git para gerenciar as versões do nosso projeto localmente e GitHub para hospedar nosso repositório na nuvem, aplicando diversas camadas de proteção.

**Ao final desta aula, vocês serão capazes de:**

*   Compreender a **importância do versionamento seguro** e os riscos de um mau gerenciamento de código.
*   **Configurar um ambiente de desenvolvimento seguro**, prevenindo o vazamento de informações sensíveis.
*   Aplicar **boas práticas em commits e branches**, criando um histórico de projeto limpo, seguro e rastreável.
*   **Utilizar ferramentas de segurança no GitHub**, como a proteção de *branches* e o *Secret Scanning*, para proteger o código e automatizar a detecção de vulnerabilidades.

---

### 2. Pré-requisitos

Para acompanhar esta aula, você precisará de:

*   **Git instalado** em seu sistema operacional (Windows, macOS, Linux). Você pode baixá-lo em [git-scm.com](https://git-scm.com/).
*   Uma **conta no GitHub**. Se ainda não tem, crie uma gratuitamente em [github.com](https://github.com/).
*   Um editor de texto ou IDE (VS Code, Sublime Text, PyCharm, etc.).
*   Conhecimentos básicos de linha de comando (terminal/prompt de comando).
*   Noções básicas de Python (para o exemplo de código).

---

### 3. Cenário Proposto

Vamos desenvolver uma pequena aplicação em Python que simula a conexão a um "banco de dados" (um arquivo de texto) e a utilização de uma chave de API para um serviço externo. Nosso principal desafio será versionar este projeto de forma **segura**, garantindo que segredos como senhas e chaves de API nunca sejam expostos no repositório.

---

### 4. Passo 1: Preparando o Ambiente (A Base de Tudo)

A primeira linha de defesa contra vazamentos de segredos começa na configuração do seu ambiente local.

#### 1.1. Instalação e Configuração Inicial do Git

Após instalar o Git, configure suas credenciais de usuário. Isso associará seus *commits* ao seu nome e e-mail.

```bash
git config --global user.name "Seu Nome Completo"
git config --global user.email "seuemail@exemplo.com"
git config --global init.defaultBranch main # Opcional: define a branch padrão como 'main'
```

#### 1.2. O Arquivo Mágico: `.gitignore`

O `.gitignore` é crucial. Ele instrui o Git a ignorar arquivos e pastas específicos, impedindo que dados sensíveis, arquivos de configuração locais ou binários de compilação sejam acidentalmente versionados.

1.  Crie uma nova pasta para o nosso projeto:
    ```bash
    mkdir projeto-seguro-aula
    cd projeto-seguro-aula
    ```
2.  Crie o arquivo `.gitignore` dentro dela:
    ```bash
    touch .gitignore
    ```
3.  Edite o `.gitignore` e adicione as seguintes regras:
    ```
    # Ambiente Virtual Python
    venv/
    *.pyc
    __pycache__/

    # Arquivos de configuração local e segredos
    config.ini
    .env
    ```
    **Explicação:**
    *   `venv/`: Ignora a pasta do ambiente virtual do Python.
    *   `*.pyc`, `__pycache__/`: Ignora arquivos de cache do Python.
    *   `config.ini`, `.env`: **Extremamente importante!** Ignora arquivos que tipicamente contêm senhas, chaves de API e outras informações sensíveis.

---

### 5. Passo 2: Criando Nosso Código (e a Primeira Vulnerabilidade)

Vamos criar a estrutura básica do projeto e, propositalmente, introduzir uma vulnerabilidade para aprender a corrigi-la.

1.  **Estrutura do Projeto:**

    ```
    projeto-seguro-aula/
    |-- .gitignore
    |-- app.py
    |-- db.txt
    ```

2.  **`db.txt` (Nosso "Banco de Dados" Simulado):**
    Crie o arquivo `db.txt` com o seguinte conteúdo:
    ```
    usuário:admin
    senha:batata_frita_123
    ```

3.  **`app.py` (O Código com Vazamento de Segredos):**
    Crie o arquivo `app.py` com este código:

    ```python
    # app.py
    # CUIDADO: Chave de API e senha diretamente no código! ISSO É UMA VULNERABILIDADE!

    API_KEY = "CHAVE_SUPER_SECRETA_DA_NASA_123"
    DB_PASSWORD = "batata_frita_123"

    def conectar_banco():
        print(f"Conectando ao banco com a senha: {DB_PASSWORD}")
        # Lógica de conexão simulada...
        print("Conexão bem-sucedida!")

    def chamar_api():
        print(f"Chamando serviço externo com a chave: {API_KEY}")
        # Lógica de chamada da API simulada...
        print("API respondeu com sucesso!")

    if __name__ == "__main__":
        print("Iniciando aplicação...")
        conectar_banco()
        chamar_api()
        print("Aplicação finalizada.")
    ```
    **⚠️ Alerta de Segurança:** Observe que a `API_KEY` e a `DB_PASSWORD` estão "hardcoded" (escritas diretamente) no código. Esta é uma prática **extremamente perigosa** e a causa de muitos vazamentos de dados em projetos reais.

---

### 6. Passo 3: O Primeiro Commit e a Correção do Risco

Vamos iniciar o repositório Git e, em seguida, corrigir a vulnerabilidade que introduzimos.

#### 3.1. Iniciando o Repositório e o Primeiro Commit (Errado!)

Se um desenvolvedor desatento *commitasse* o `app.py` com os segredos, eles estariam para sempre no histórico do Git. Vamos simular isso para entender a gravidade.

```bash
git init
git add .
git commit -m "feat: Versão inicial do projeto com funcionalidades básicas"
```
Neste ponto, a `API_KEY` e a `DB_PASSWORD` foram *commitadas* e fazem parte do histórico do seu repositório Git. Mesmo que você as remova em um *commit* futuro, elas ainda poderão ser recuperadas ao navegar pelo histórico do projeto. **O ideal é NUNCA cometer este erro!**

#### 3.2. A Forma Correta: Externalizando os Segredos

A solução robusta é armazenar informações sensíveis em arquivos de configuração locais (ignorados pelo Git) ou em variáveis de ambiente.

1.  **Crie o arquivo `config.ini`**: Este arquivo já está listado no nosso `.gitignore`, então o Git o ignorará automaticamente.
    ```ini
    [SECRETS]
    API_KEY = CHAVE_SUPER_SECRETA_DA_NASA_123_DO_CONFIG
    DB_PASSWORD = batata_frita_123_do_config
    ```
    *(Mudei os valores propositalmente para mostrar que estamos usando os do `config.ini`.)*

2.  **Refatore o `app.py` para ler as configurações de `config.ini`**:

    ```python
    # app.py
    import os
    import configparser

    # Carrega as configurações do arquivo config.ini
    config = configparser.ConfigParser()
    config.read('config.ini')

    API_KEY = config['SECRETS']['API_KEY']
    DB_PASSWORD = config['SECRETS']['DB_PASSWORD']

    def conectar_banco():
        # A senha não é impressa diretamente para evitar exposição em logs
        print(f"Conectando ao banco com a senha: [SENHA OCULTADA]")
        # Lógica de conexão simulada...
        print("Conexão bem-sucedida!")

    def chamar_api():
        # A chave não é impressa diretamente para evitar exposição em logs
        print(f"Chamando serviço externo com a chave: [API_KEY OCULTADA]")
        # Lógica de chamada da API simulada...
        print("API respondeu com sucesso!")

    if __name__ == "__main__":
        print("Iniciando aplicação...")
        conectar_banco()
        chamar_api()
        print("Aplicação finalizada.")
    ```

Agora, nosso código está limpo de segredos. Vamos fazer um novo *commit* com a correção.

```bash
git add app.py config.ini # O config.ini será adicionado ao stage, mas o Git o ignorará no commit devido ao .gitignore
git commit -m "refactor: Remove segredos do código e usa config.ini"
```
**Atenção:** Embora o `config.ini` tenha sido adicionado ao `stage` com `git add config.ini`, ele **não** será incluído no *commit* final porque está no `.gitignore`. Isso é o comportamento esperado e correto!

---

### 7. Passo 4: Subindo para o GitHub e Configurando a Segurança

É hora de levar nosso projeto para o GitHub, aplicando as primeiras camadas de segurança no próprio repositório.

#### 4.1. Criando o Repositório no GitHub

1.  Acesse sua conta no [GitHub](https://github.com).
2.  Crie um **novo repositório** (botão verde "New" ou ícone "+").
3.  Dê um nome, por exemplo, `projeto-seguro-aula`.
4.  **Importante:** **NÃO** marque as opções "Add a README file", "Add .gitignore" ou "Choose a license", pois já temos esses arquivos localmente.
5.  Clique em "Create repository".
6.  Após criar, o GitHub mostrará instruções. Siga as para "push an existing repository from the command line":

    ```bash
    git remote add origin https://github.com/SEU_USUARIO/projeto-seguro-aula.git
    git branch -M main
    git push -u origin main
    ```
    (Substitua `SEU_USUARIO` pelo seu nome de usuário do GitHub).

#### 4.2. Protegendo a Branch `main`

A *branch* `main` (ou `master`) é a linha de código principal do seu projeto. É uma boa prática **nunca** permitir *commits* diretos nela. Todas as alterações devem passar por um processo de revisão via *Pull Requests*.

1.  No seu repositório no GitHub, vá em **Settings > Branches**.
2.  Na seção "Branch protection rules", clique em **Add branch protection rule**.
3.  Em "Branch name pattern", digite `main`.
4.  Marque as seguintes opções:
    *   **✅ Require a pull request before merging**: Isso força que toda alteração na `main` venha de um *Pull Request* (PR).
    *   **✅ Require approvals**: Exige que pelo menos um revisor (para este exemplo, pode deixar em `1`) aprove o PR antes do *merge*.
5.  Clique em **Create**.

Agora, a *branch* `main` está protegida contra *commits* diretos e exige revisão.

#### 4.3. Ativando o Secret Scanning

O GitHub Advanced Security oferece o *Secret Scanning*, uma ferramenta poderosa que varre seu repositório em busca de segredos que possam ter sido acidentalmente *commitados*. Para repositórios públicos, é gratuito.

1.  No seu repositório no GitHub, vá em **Settings > Code security and analysis**.
2.  Na seção "GitHub Advanced Security", clique em **Enable**. (Se já estiver ativado, pule este passo).
3.  Na seção "Secret scanning", clique em **Enable**.
    *   Confirme a ativação.

O GitHub agora monitorará todos os *pushes* em busca de formatos de tokens conhecidos (AWS, Google Cloud, Stripe, etc.), alertando você caso encontre algum.

---

### 8. Passo 5: O Fluxo de Trabalho Seguro (Branch, PR e Análise)

Vamos simular a adição de uma nova funcionalidade, seguindo nosso fluxo de trabalho seguro, e ver as defesas do GitHub em ação.

#### 8.1. Criando uma Nova Branch

Nunca trabalhe diretamente na `main`. Para cada nova funcionalidade ou correção de bug, crie uma *branch* específica.

```bash
git checkout -b feature/adicionar-logs
```

#### 8.2. Desenvolvendo e "Vazando" um Segredo (de propósito!)

Vamos supor que, por um descuido, você adicione um token de um serviço de log diretamente no código, esquecendo-se da prática de externalização.

No arquivo `app.py`, adicione a função `registrar_log` e a chamada no `if __name__ == "__main__":`:

```python
# app.py (adicionar a nova função e a chamada)

# ... (código existente) ...

def registrar_log(mensagem):
    # CUIDADO: Vazamento de segredo proposital para demonstração!
    LOG_SERVICE_TOKEN = "TOKEN_DE_LOG_SUPER_SECRETO_DO_SERVICO_XYZ"
    print(f"Registrando log com o token {LOG_SERVICE_TOKEN}: {mensagem}")

if __name__ == "__main__":
    print("Iniciando aplicação...")
    conectar_banco()
    chamar_api()
    registrar_log("Aplicação iniciada com sucesso.") # Nova chamada
    print("Aplicação finalizada.")
```

#### 8.3. Fazendo o Commit e Abrindo o Pull Request

```bash
git add app.py
git commit -m "feat: Adiciona funcionalidade de logging com token fixo"
git push origin feature/adicionar-logs
```

Agora, vá ao GitHub. Você verá um aviso para **abrir um Pull Request** da *branch* `feature/adicionar-logs` para a `main`. Clique para criá-lo.

#### 8.4. A Mágica da Segurança em Ação!

Ao abrir o Pull Request, observe o que acontece:

1.  **Branch Protegida:** O GitHub informará que você **não pode fazer o *merge* diretamente**, pois a *branch* `main` exige aprovações de revisão.
2.  **Secret Scanning:** Em poucos minutos (ou instantaneamente), o GitHub **deve detectar o `TOKEN_DE_LOG_SUPER_SECRETO_DO_SERVICO_XYZ`!** Ele criará um alerta de segurança na aba **Security** do seu repositório e poderá até mesmo enviar uma notificação por e-mail.

Este é um exemplo perfeito de como as camadas de proteção (revisão humana via PR + automação de *Secret Scanning*) trabalham em conjunto para prevenir que vulnerabilidades cheguem à *branch* principal do seu projeto. O revisor do seu PR deveria rejeitar essa alteração e solicitar que você remova o token do código, externalizando-o corretamente.

---

### 9. Conclusão e Próximos Passos

Parabéns! Você acaba de completar um ciclo de desenvolvimento de software aplicando práticas essenciais de versionamento seguro.

**Principais Aprendizados:**

*   **Prevenção é a chave:** Nunca guarde segredos diretamente no código-fonte. Utilize arquivos de configuração (`.gitignore`) ou variáveis de ambiente.
*   **Proteja sua `main`:** Garanta que todas as alterações na *branch* principal passem por revisão via *Pull Requests*.
*   **Automatize a segurança:** Ferramentas como o *Secret Scanning* do GitHub são seus aliados para detectar vazamentos antes que causem danos.
*   **O histórico é permanente:** Lembre-se que remover um segredo em um *commit* posterior não o apaga do histórico do Git. A prevenção é a melhor estratégia.

Como futuros profissionais da área, a responsabilidade de escrever e gerenciar código seguro é de vocês. Adotar essas práticas desde o início não apenas protegerá seus projetos e futuras empresas, mas também os tornará desenvolvedores mais completos e valorizados.

**Próximos Passos:**

*   Experimente o fluxo completo novamente, corrigindo o vazamento do token de log antes de abrir o PR.
*   Explore outras funcionalidades do GitHub Advanced Security, como o Code Scanning.
*   Pesquise sobre variáveis de ambiente (`.env` files e bibliotecas como `python-dotenv`) para gerenciar segredos em ambientes de produção.
*   Aprofunde-se nos conceitos de DevSecOps, que integra segurança em todas as fases do ciclo de vida do desenvolvimento.

Boas codificações e sucesso em sua jornada!

---

### 10. Sobre o Autor

Este material foi criado por:

**[Seu Nome Completo]**
*   **Título:** Docente Especialista Sênior em Desenvolvimento Seguro de Código e Versionamento.
*   **Instituição:** [Nome da sua Instituição de Ensino, se aplicável]
*   **LinkedIn (Opcional):** [Link para o seu perfil no LinkedIn]
*   **GitHub (Opcional):** [Link para o seu perfil no GitHub]

---
``````
