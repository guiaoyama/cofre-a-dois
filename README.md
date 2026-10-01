# MeuSaldo — como colocar no ar

Aplicativo web de controle financeiro do casal. Roda em qualquer celular pelo navegador,
com login por e-mail e senha para o Guilherme e para a Julia, e os dados ficam num banco
gratuito do Google (Firebase Firestore), sincronizados em tempo real entre os dois aparelhos.

Arquivos desta pasta:

| Arquivo | Para que serve |
|---|---|
| `index.html` | O aplicativo inteiro. Não precisa mexer. |
| `config.js` | **Único arquivo que você edita**: as chaves do Firebase. Pessoas e casas são cadastradas dentro do app. |
| `firestore.rules` | Regras de segurança do banco. Você cola o conteúdo no console do Firebase. Tem o e-mail do administrador na primeira função. |
| `manifest.webmanifest`, `icon-180.png`, `icon-512.png` | Deixam o app instalável na tela inicial do celular, com nome e ícone MeuSaldo. |

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
   `index.html`, `config.js`, `manifest.webmanifest`, `icon-180.png`, `icon-512.png`, `README.md`.
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

## Casas e pessoas (várias famílias no mesmo app)

O app atende várias **casas** no mesmo link e no mesmo projeto Firebase. Cada casa tem uma ou duas pessoas e seus próprios meses, lançamentos, cartões e categorias. As regras do servidor garantem que ninguém lê nem grava fora da própria casa, mesmo que tente pelo código. O administrador (o e-mail que está na primeira função do `firestore.rules`) só cadastra casas e pessoas; pelo app ele não vê os dados das outras casas.

- **Casa com duas pessoas:** funciona como sempre, com os dois nomes e o "Conjunto" como responsáveis.
- **Casa com uma pessoa:** o app não pergunta responsável, não mostra "Conjunto" nem o gráfico por responsável.

### Primeira entrada depois de atualizar para a versão 5 (só o administrador)

1. Entre com seu e-mail e senha. O app reconhece que você é o administrador e abre a **Configuração inicial**.
2. Confira o nome da casa, seu nome e cor, e o e-mail e nome da segunda pessoa (deixe o e-mail vazio se a casa for só sua). As chaves internas `guilherme` e `julia` não mudam, porque os lançamentos antigos usam esses valores.
3. A tela mostra quantos meses, lançamentos, cartões e categorias existem para mover. Toque em **Criar casa e migrar dados**. Em segundos tudo está dentro da sua casa, e o app abre normalmente.

### Incluir uma família nova

1. Console do Firebase → **Authentication → Users → Adicionar usuário**: e-mail da pessoa e uma senha inicial. Prefira e-mail real: assim o botão "Esqueci minha senha" do app funciona sozinho.
2. No app, seletor de mês → **Administração** → **Nova casa** → nome da casa → **Criar casa**.
3. Na casa criada, **+ Pessoa** → e-mail (o mesmo do passo 1), nome e cor → **Cadastrar pessoa**. Para um casal, repita para a segunda pessoa (máximo duas por casa).
4. A pessoa abre o mesmo link, entra com e-mail e senha e encontra o app vazio, com as categorias padrão. Ela cria o primeiro mês dela e começa.

Se alguém entrar com um e-mail que ainda não foi cadastrado em nenhuma casa, vê a tela "Quase lá" pedindo para falar com o administrador.

**Remover uma pessoa** (Administração → Editar → Remover) tira o acesso dela; os lançamentos que ela fez ficam na casa. **Editar** muda nome e cor; a chave interna não muda.

**Excluir uma casa** (Administração → botão vermelho **Excluir** na casa): apaga a casa, o cadastro das pessoas dela e todos os meses, lançamentos, cartões e categorias. Só é liberado depois de digitar **EXCLUIR**. A sua própria casa não tem esse botão. As senhas continuam existindo no Firebase (Authentication → Users); apague-as lá se a pessoa não for mais usar o app. Para isso funcionar, as regras precisam permitir que o administrador leia e apague dentro das casas (o `firestore.rules` desta pasta já faz isso); ele continua sem poder criar ou alterar nada nelas, e o app nunca mostra os dados de outra casa.

## As cinco abas

| Aba | O que mostra |
|---|---|
| **Início** | Alerta de contas vencendo, disponível do mês, os quadrados Entradas, Saídas e Cartões (cada um leva à aba correspondente; Cartões rola até a seção de cartões em Saídas), o gráfico de evolução do disponível dos últimos 12 meses e o Exportar. |
| **Lançar** | O formulário de lançamento, como antes. |
| **Entradas** | Total de entradas, quanto entrou por pessoa, rosca por categoria e barras por responsável. |
| **Saídas** | Total de saídas com cartões e total comprometido, saídas por pessoa, contas programadas, cartões de crédito, rosca por categoria e gastos por responsável. |
| **Histórico** | A lista de lançamentos com filtros, como antes. |

No gráfico de evolução, cada barra é o disponível de um mês: acima da linha do zero é positivo, abaixo é negativo. Tocar numa barra mostra as entradas, saídas e cartões daquele mês. Para o gráfico não precisar ler todos os lançamentos de todos os meses, cada mês guarda um resumo dos seus totais, recalculado sozinho sempre que alguém abre o mês e algo mudou. Meses antigos sem resumo são calculados uma única vez na primeira visita ao Início.

## Como o app trata cada tipo de saída

| Ao lançar uma saída | O que acontece |
|---|---|
| Saída comum | Entra em Saídas e reduz o Disponível. Sem tag no Histórico. |
| **Conta programada** ligada | Além do acima, aparece na lista "Contas programadas do mês" na aba Saídas, com interruptor de paga/pendente que pode ser mudado a qualquer hora. A data vira o vencimento. Contas pendentes vencendo em até 3 dias (ou já vencidas) aparecem em vermelho no topo do Início, com botão "Marcar como paga". Marcar paga não muda nenhum cálculo. |
| **Já está no cartão de crédito** ligada | O valor **não** entra em Saídas nem no Disponível, porque a fatura do cartão já o contém. Entra só na rosca "Saídas por categoria", para você ver quanto gastou em cada coisa. No Histórico recebe a tag "Cartão". |

Entradas nunca têm tag nem interruptores.

## Virada de mês: repetir lançamentos

Ao tocar em **Novo mês**, a folha mostra a lista de lançamentos do mês que você está vendo (dá para escolher outro mês em "Repetir lançamentos de"). Marque os que devem se repetir e toque em **Criar mês**. As contas programadas já vêm marcadas; entradas e saídas comuns, não. Os botões **Todas** e **Nenhuma** ajudam.

Cada lançamento copiado nasce com o mesmo valor, categoria, responsável e descrição, na mesma data do mês novo (dia 31 vira 30 em meses curtos), e com "pago" desligado. Cartões de crédito não são copiados: cada mês começa sem cartões, como na especificação.

## Excluir um mês

No seletor de mês (topo), o botão vermelho **Excluir [mês]…** apaga o mês que está aberto, com todos os seus lançamentos e cartões. Para evitar acidente, a exclusão só é liberada depois de digitar **EXCLUIR** na caixa de confirmação. Categorias e outros meses não são afetados. Se o mês apagado era o "atual", o mais recente que sobrar assume.

## Conferir qual versão está no celular

Abra o seletor de mês: no rodapé aparece "MeuSaldo · versão N". Se, depois de enviar um `index.html` novo ao GitHub, o número não mudou:

1. Confira no repositório que o arquivo enviado se chama exatamente `index.html` e que o commit aparece na lista (às vezes o upload gera `index (1).html`).
2. Aguarde 2 minutos: o GitHub Pages leva esse tempo para publicar, e o navegador guarda a página por até 10 minutos.
3. No iPhone, com o app na tela inicial: feche-o de verdade (deslize para cima no seletor de apps) e abra de novo. Se ainda não mudou, abra o link no Safari, recarregue, e depois volte ao ícone.
4. Último recurso: remova o ícone da tela inicial e adicione de novo.

## Segurança: o que fazer depois de publicar

O `config.js` fica público no GitHub, e isso é normal: as chaves do Firebase identificam o projeto, não dão acesso. Quem protege os dados são os três itens abaixo. Faça todos.

### S1. Regras por casa (obrigatório)

O arquivo `firestore.rules` desta pasta define: cada pessoa só acessa a casa em que está cadastrada; só o administrador (e-mail na primeira função do arquivo) cadastra casas e pessoas. Publique-o:

1. Console do Firebase → **Firestore Database** → aba **Regras**.
2. Apague tudo, cole o conteúdo do `firestore.rules` e clique em **Publicar**.

Resultado: mesmo que alguém consiga uma conta no seu projeto, sem estar cadastrado numa casa não lê nem grava nada. E ninguém, nem o administrador pelo app, entra na casa dos outros.

### S2. Bloquear a criação de contas por estranhos (obrigatório)

Por padrão o Firebase deixa qualquer pessoa criar uma conta nova pelo próprio código. Desligue isso:

1. Console do Firebase → **Authentication** → aba **Settings** (Configurações).
2. Menu **User actions** (Ações do usuário).
3. Desmarque **Enable create (sign-up)** / **Permitir criação**. Se houver a opção **Enable delete**, desmarque também.
4. Clique em **Salvar**.

Você e a Julia continuam entrando normalmente; só não dá mais para criar contas novas. Se um dia precisar criar outra, ligue de novo, crie na aba Users e desligue.

### S3. Restringir a chave a este site (recomendado)

Impede que a chave do `config.js` seja usada a partir de outro site.

1. Acesse **https://console.cloud.google.com/apis/credentials** e escolha o projeto `cofre-a-dois` no seletor do topo.
2. Em **Chaves de API**, clique em **Browser key (auto created by Firebase)**.
3. Em **Restrições do aplicativo**, marque **Sites** (Websites / HTTP referrers).
4. Clique em **Adicionar** e digite `https://SEU-USUARIO.github.io/*`. Adicione também `https://cofre-a-dois-XXXXX.firebaseapp.com/*` (o `authDomain` do seu `config.js`).
5. Clique em **Salvar**. Pode levar até 5 minutos para valer.

### S4. E-mails não ficam mais em arquivo público

Desde a versão 5, nenhum e-mail entra no `config.js` nem precisa aparecer no `firestore.rules` além do e-mail do administrador, e as regras não são públicas. Os e-mails das pessoas ficam no banco de dados, protegidos pelas mesmas regras dos lançamentos. Por isso as pessoas novas podem usar e-mail real, o que faz o "Esqueci minha senha" funcionar sozinho. Se quiser trocar o seu e-mail fictício por um real, crie o usuário novo no Firebase, cadastre-o na sua casa pela Administração, troque o `adminEmail` no `firestore.rules` e publique as regras.

## Se algo der errado

| Sintoma | Causa provável | O que fazer |
|---|---|---|
| Tela "Falta configurar" | `config.js` ainda tem `COLE_AQUI` ou o upload não incluiu o `config.js` | Revise a Parte B e reenvie o arquivo (Add file → Upload files, substitui o antigo). |
| "Senha incorreta ou usuário não encontrado" | E-mail do `config.js` diferente do cadastrado em Authentication → Users, ou senha errada | Compare os e-mails letra por letra. Para trocar a senha: Users → três pontinhos → Redefinir senha. |
| "Este endereço não está autorizado no Firebase" | Etapa C4 pulada | Adicione `SEU-USUARIO.github.io` em Domínios autorizados. |
| "Sem permissão para gravar / ler" | Regras não publicadas, ou o e-mail não está cadastrado na casa | Firestore → Regras → cole o `firestore.rules` → Publicar. Confira em Administração se o e-mail está na casa certa. |
| Tela "Quase lá" | O e-mail entrou, mas não está em nenhuma casa | Administração → casa → + Pessoa com esse e-mail exato. |
| Administrador cai na tela "Quase lá" | O e-mail do `adminEmail` no `firestore.rules` é diferente do usado para entrar | Ajuste a primeira função do `firestore.rules` e publique. |
| Página 404 no GitHub | Pages ainda não terminou de publicar, ou o arquivo não se chama `index.html` | Aguarde 2 minutos; confira o nome do arquivo. |
| Alterei o `config.js` e nada mudou | Cache do navegador | Recarregue com Ctrl+F5 no computador ou feche e abra o navegador no celular. |

---

## Para atualizar o app no futuro

Substitua o `index.html` no repositório: **Add file → Upload files**, arraste o novo `index.html`, **Commit changes**. Em 1 a 2 minutos o link já mostra a versão nova. Os dados no Firebase não são afetados.

## Limites do plano gratuito do Firebase

50 mil leituras e 20 mil gravações por dia, 1 GB de armazenamento. Para o uso de duas pessoas isso é centenas de vezes mais do que o necessário. Não é preciso cadastrar cartão de crédito.
