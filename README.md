# 🎵 Sistema Musical — Backend PHP & MySQL

Sistema web dinâmico para gerenciamento de biblioteca musical, desenvolvido em **PHP** e **MySQL**. O projeto conta com autenticação de usuários, controle de sessões, operações completas de CRUD (Artistas e Músicas), upload e redimensionamento de imagens, integração com player do YouTube e busca com filtros dinâmicos.

---

## 🚀 Funcionalidades

- **🔐 Autenticação & Segurança:**
  - Login seguro com verificação de hash bcrypt (`password_verify`).
  - Proteção de rotas contra acessos não autorizados via validação de sessão.
  - Encerramento completo de sessão (destruição do cookie e dados no servidor).

- **🎤 Gerenciamento de Artistas (CRUD):**
  - Cadastro, listagem, edição e exclusão de artistas.
  - Upload de foto do artista com redimensionamento automático via biblioteca GD.
  - Validação de integridade referencial (impede exclusão de artistas com músicas cadastradas).

- **🎵 Gerenciamento de Músicas (CRUD):**
  - Associação de músicas a um artista cadastrado (chave estrangeira).
  - Suporte a links do YouTube (conversão e extração dinâmica do ID do vídeo para reprodução).
  - Upload e redimensionamento da capa do álbum.
  - Remoção automática da imagem do servidor ao excluir a música (`unlink`).

- **🎼 Biblioteca Musical Dinâmica:**
  - Exibição de catálogo completo com suporte a múltiplos filtros combinados (busca por texto, artista e gênero).
  - Preservação dos parâmetros de busca na URL após ações de exclusão (`http_build_query`).

---

## 🧠 Conceitos Aplicados

### 1. Segurança Web & Boas Práticas
* **Prevenção contra SQL Injection:** Uso de **Prepared Statements** (`$conn->prepare` e `bind_param`) em todas as operações de banco de dados que utilizam entradas de usuários.
* **Prevenção contra XSS (Cross-Site Scripting):** Sanitização de saídas HTML com `htmlspecialchars()` em todos os dados renderizados em tela.
* **Criptografia de Senhas:** Armazenamento seguro de senhas no banco usando o algoritmo **bcrypt** via `password_hash()`.
* **Sanitização de Parâmetros de URL:** Uso de `intval()` em identificadores passados via `GET` para evitar manipulação de SQL/tipagem.

### 2. Manipulação de Arquivos e Mídia
* **Processamento de Imagens:** Utilização da extensão **GD** do PHP para tratar, redimensionar (`imagecopyresampled`) e otimizar fotos de perfil e capas de álbuns no formato JPG/PNG.
* **Gerenciamento do File System:** Verificação e criação dinâmica do diretório `img/`, validação de extensão/tamanho (máx. 5 MB) e limpeza de arquivos antigos ao deletar registros no banco (`unlink()`).
* **RegEx (Expressões Regulares):** Extração dinâmica de IDs de vídeos do YouTube a partir de múltiplos formatos de URLs (URLs padrão, encurtadas, embed, etc.).

### 3. Arquitetura e Estrutura de Código
* **Sessões HTTP:** Gerenciamento de estado em requisições stateless por meio do array superglobal `$_SESSION`.
* **Modularização:** Separação de responsabilidades em arquivos isolados para conexão (`conexao.php`), controle de autorização (`valida_sessao.php`) e regras de layout/estilo (`styles.css`).
* **Regras de Negócio & Integridade de Dados:** Garantia de integridade relacional entre tabelas (`FOREIGN KEY`) e validações condicionais antes da remoção de registros dependentes.

---

## 🛠️ Tecnologias Utilizadas

- **Linguagem Backend:** PHP 8.x
- **Banco de Dados:** MySQL / MariaDB
- **Processamento de Imagem:** Extensão GD (`imagecreatefromstring`, `imagecopyresampled`)
- **Estilização:** CSS3 puro (`styles.css`)
- **Servidor Web Recomendado:** Apache (XAMPP, WAMP, Laragon)

---

## 📂 Estrutura do Projeto

```text
sistema-musical/
├── styles.css               # Estilização global da aplicação
├── bd.sql                   # Script SQL para criação do banco e tabelas
├── conexao.php              # Conexão MySQLi e criação da pasta img/
├── geraSenha.php            # Script utilitário para gerar hashes bcrypt
├── valida_sessao.php        # Middleware de proteção de páginas protegidas
├── login.php                # Tela e tratamento de autenticação
├── logout.php               # Encerramento seguro da sessão
├── index.php                # Painel principal / Menu do sistema
├── cadastro_artista.php     # CRUD completo de Artistas + Upload de foto
├── cadastro_musica.php      # CRUD completo de Músicas + YouTube + Upload de capa
├── listagem_musicas.php     # Biblioteca Musical com busca e filtros dinâmicos
└── img/                     # Diretório de uploads (criado automaticamente)
