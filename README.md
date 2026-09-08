# Cofre a Dois — como colocar no ar

Aplicativo web de controle financeiro do casal. Roda em qualquer celular pelo navegador,
com login por e-mail e senha para o Guilherme e para a Julia, e os dados ficam num banco
gratuito do Google (Firebase Firestore), sincronizados em tempo real entre os dois aparelhos.

Arquivos desta pasta:

| Arquivo | Para que serve |
|---|---|
| `index.html` | O aplicativo inteiro. Não precisa mexer. |
| `config.js` | **Único arquivo que você edita**: chaves do Firebase e os dois e-mails. |
| `firestore.rules` | Regras de segurança do banco. Você cola o conteúdo no console do Firebase. |
| `manifest.webmanifest`, `icon.svg` | Deixam o app instalável na tela inicial do celular. |

O processo tem três partes: **(A) criar o projeto no Firebase**, **(B) preencher o `config.js`**
e **(C) publicar no GitHub Pages**. Leva uns 20 minutos, tudo pelo navegador, sem instalar nada.

---

## Parte A — Criar o projeto no Firebase

### A1. Criar o projeto

1. Acesse **https://console.firebase.google.com** e entre com sua conta Google.
2. Clique em **Criar um projeto** (ou **Adicionar projeto**).
3. Nome do projeto: `cofre-a-dois`. Clique em **Continuar**.
4. Na tela do Google Analytics, **desative** a chave "Ativar o Google Analytics" (não é necessário). Clique em **Criar projeto**.
5. Aguarde e clique em **Continuar** quando aparecer "Seu novo projeto está pronto".

### A2. Ativar o login por e-mail e senha

1. No menu lateral esquerdo, abra **Criação** (Build) → **Authentication**.
2. Clique em **Vamos começar**.
3. Na aba **Sign-in method**, clique em **E-mail/senha**.
4. Ative a primeira chave (**E-mail/senha**). Deixe "Link de e-mail (login sem senha)" desativado. Clique em **Salvar**.

### A3. Criar os dois usuários

1. Ainda em **Authentication**, abra a aba **Users**.
2. Clique em **Adicionar usuário**.
3. E-mail: o seu (pode ser o seu e-mail real ou um inventado, como `guilherme@cofre.app` — o Firebase não envia nada para ele). Senha: escolha uma com pelo menos 6 caracteres. Clique em **Adicionar usuário**.
4. Repita para a Julia: **Adicionar usuário** → e-mail dela → senha dela → **Adicionar usuário**.
5. Anote os dois e-mails: eles vão para o `config.js` na Parte B.

### A4. Criar o banco de dados

1. Menu lateral → **Criação** → **Firestore Database**.
2. Clique em **Criar banco de dados**.
3. Local do banco: escolha **southamerica-east1 (São Paulo)**. Clique em **Avançar**.
4. Regras de segurança: escolha **Iniciar no modo de produção**. Clique em **Criar**.
5. Aguarde a criação. Vai abrir a aba **Dados**, vazia. Isso é o esperado.

### A5. Colar as regras de segurança

1. No Firestore, abra a aba **Regras**.
2. Apague todo o conteúdo do editor.
3. Abra o arquivo `firestore.rules` desta pasta, copie tudo e cole no editor.
4. Clique em **Publicar**.

O que as regras fazem: só quem fez login (você ou a Julia) consegue ler e gravar. Qualquer outra pessoa é bloqueada, mesmo que descubra o link.

### A6. Registrar o app da Web e copiar as chaves

1. No topo do menu lateral, clique na **engrenagem** ao lado de "Visão geral do projeto" → **Configurações do projeto**.
2. Role até a seção **Seus apps** e clique no ícone **`</>`** (Web).
3. Apelido do app: `cofre`. **Não** marque "Configurar o Firebase Hosting". Clique em **Registrar app**.
4. Vai aparecer um trecho de código com um objeto chamado `firebaseConfig`, parecido com este:

   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "cofre-a-dois-xxxxx.firebaseapp.com",
     projectId: "cofre-a-dois-xxxxx",
     storageBucket: "cofre-a-dois-xxxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```

5. Deixe essa tela aberta (ou copie o trecho): você vai usar na Parte B. Clique em **Continuar no console**.

> Essas chaves não são secretas: elas identificam o projeto, e a proteção vem das regras da etapa A5 e do login. Pode deixá-las no arquivo público.

---

## Parte B — Preencher o `config.js`

1. Abra o arquivo `config.js` desta pasta no Bloco de Notas (ou no VS Code).
2. Substitua cada `"COLE_AQUI"` pelo valor correspondente do `firebaseConfig` da etapa A6. Ficará assim:

   ```js
   firebase: {
     apiKey: "AIzaSy...",
     authDomain: "cofre-a-dois-xxxxx.firebaseapp.com",
     projectId: "cofre-a-dois-xxxxx",
     storageBucket: "cofre-a-dois-xxxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef",
   },
   ```

3. Mais abaixo, em `usuarios`, coloque os dois e-mails cadastrados na etapa A3:

   ```js
   usuarios: {
     guilherme: "guilherme@cofre.app",
     julia: "julia@cofre.app",
   },
   ```

4. Salve o arquivo. Confira que cada linha termina com vírgula e que os valores estão entre aspas.

---

## Parte C — Publicar no GitHub Pages

### C1. Criar a conta e o repositório

1. Acesse **https://github.com** e crie uma conta, se ainda não tiver (é gratuito).
2. No canto superior direito, clique em **+** → **New repository**.
3. Repository name: `cofre-a-dois`.
4. Deixe em **Public** (o GitHub Pages gratuito exige repositório público; os dados financeiros **não** ficam aqui, só o código do app).
5. Não marque nenhuma outra opção. Clique em **Create repository**.

### C2. Enviar os arquivos

1. Na página do repositório recém-criado, clique no link **uploading an existing file**.
   (Se não aparecer, use o botão **Add file** → **Upload files**.)
2. Arraste para a área de upload estes cinco arquivos desta pasta:
   `index.html`, `config.js`, `manifest.webmanifest`, `icon.svg`, `README.md`.
   (O `firestore.rules` não precisa ir; ele já foi colado no Firebase.)
3. Role até o fim e clique em **Commit changes**.

### C3. Ligar o GitHub Pages

1. No repositório, abra a aba **Settings** (engrenagem, no topo).
2. No menu lateral esquerdo, clique em **Pages**.
3. Em **Build and deployment** → **Source**, deixe **Deploy from a branch**.
4. Em **Branch**, escolha **main** e a pasta **/ (root)**. Clique em **Save**.
5. Aguarde 1 a 2 minutos e recarregue a página. Vai aparecer: "Your site is live at **https://SEU-USUARIO.github.io/cofre-a-dois/**". Esse é o link do app.

### C4. Autorizar o endereço no Firebase (obrigatório)

Sem este passo o login dá erro "domínio não autorizado".

1. Volte ao **console do Firebase** → **Authentication** → aba **Settings** (Configurações).
2. Clique em **Domínios autorizados** → **Adicionar domínio**.
3. Digite `SEU-USUARIO.github.io` (só o domínio, sem `https://` e sem `/cofre-a-dois`). Clique em **Adicionar**.

---

## Testar

1. Abra **https://SEU-USUARIO.github.io/cofre-a-dois/** no celular.
2. Toque em **Guilherme**, digite sua senha, **Entrar**.
3. O app cria as categorias padrão sozinho na primeira entrada e mostra "Nenhum mês aberto". Toque em **Novo mês** → **Criar mês**.
4. Registre um lançamento de teste. No celular da Julia, entre com o e-mail dela: o lançamento deve aparecer na hora.
5. No topo, o indicador deve mostrar **Sincronizado · Guilherme** (ou Julia).

Para deixar com cara de aplicativo: no Chrome (Android) ou Safari (iPhone), menu do navegador → **Adicionar à tela inicial**.

---

## Se algo der errado

| Sintoma | Causa provável | O que fazer |
|---|---|---|
| Tela "Falta configurar" | `config.js` ainda tem `COLE_AQUI` ou o upload não incluiu o `config.js` | Revise a Parte B e reenvie o arquivo (Add file → Upload files, substitui o antigo). |
| "Senha incorreta ou usuário não encontrado" | E-mail do `config.js` diferente do cadastrado em Authentication → Users, ou senha errada | Compare os e-mails letra por letra. Para trocar a senha: Users → três pontinhos → Redefinir senha. |
| "Este endereço não está autorizado no Firebase" | Etapa C4 pulada | Adicione `SEU-USUARIO.github.io` em Domínios autorizados. |
| "Sem permissão para gravar / ler" | Regras da etapa A5 não publicadas | Firestore → Regras → cole o `firestore.rules` → Publicar. |
| Página 404 no GitHub | Pages ainda não terminou de publicar, ou o arquivo não se chama `index.html` | Aguarde 2 minutos; confira o nome do arquivo. |
| Alterei o `config.js` e nada mudou | Cache do navegador | Recarregue com Ctrl+F5 no computador ou feche e abra o navegador no celular. |

---

## Para atualizar o app no futuro

Substitua o `index.html` no repositório: **Add file → Upload files**, arraste o novo `index.html`, **Commit changes**. Em 1 a 2 minutos o link já mostra a versão nova. Os dados no Firebase não são afetados.

## Limites do plano gratuito do Firebase

50 mil leituras e 20 mil gravações por dia, 1 GB de armazenamento. Para o uso de duas pessoas isso é centenas de vezes mais do que o necessário. Não é preciso cadastrar cartão de crédito.
