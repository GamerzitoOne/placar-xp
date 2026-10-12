# Leaderboard XP da turma — público com administrador

## O que este projeto faz
- O ranking e os perfis são públicos e somente para leitura para visitantes.
- Apenas a conta autenticada cujo UID estiver autorizado nas regras do Firestore pode alterar XP e anotações.
- Os dados ficam no Firebase Firestore e são compartilhados entre dispositivos.
- O login usa Firebase Authentication com e-mail e senha.

## 1. Criar projeto Firebase
1. Abra https://console.firebase.google.com/ e crie um projeto.
2. Em Authentication > Sign-in method, ative **Email/Password**.
3. Em Authentication > Users, crie o usuário administrador com seu e-mail e uma senha forte.
4. Em Project settings > General > Your apps, adicione um aplicativo Web e copie a configuração.
5. Em Firestore Database, crie um banco de dados.

## 2. Configurar `index.html`
Abra `index.html` e substitua o objeto `firebaseConfig` pelos valores exatos mostrados pelo Firebase. Não coloque a senha do administrador no HTML.

## 3. Autorizar apenas o administrador
1. Em Firebase Authentication > Users, abra o usuário administrador e copie o **UID**.
2. Abra `firestore.rules`.
3. Substitua `ADMIN_UID_AQUI` pelo UID copiado, mantendo as aspas.
4. Publique as regras em Firestore Database > Rules.

As regras fornecidas permitem leitura pública, mas bloqueiam escrita de qualquer pessoa que não esteja autenticada com o UID autorizado. A proteção real está nas regras do Firestore, não apenas nos botões da página.

## 4. Inicializar os documentos de estudantes
Depois de configurar o Firebase e publicar as regras:
1. Entre no site usando a conta administradora.
2. Se a coleção `students` estiver vazia, o site tentará criar os 27 perfis iniciais.
3. Se os documentos já existirem, o site lê e atualiza os dados existentes.

Se a coleção estiver vazia e o site não conseguir inicializar os perfis, confira se o usuário autenticado tem o UID permitido nas regras.

## 5. Publicar no GitHub Pages
1. Crie um repositório no GitHub.
2. Coloque `index.html` na raiz do repositório.
3. Em Settings > Pages, escolha Deploy from a branch, branch `main`, pasta `/ (root)`.
4. Publique. O arquivo `firestore.rules` é configuração do Firebase; o GitHub Pages não publica essas regras automaticamente. Copie-as para a seção Rules do Firestore Console.

## Segurança e limites
- Nunca publique a senha do administrador nem tokens privados.
- A configuração Firebase do app web é identificadora do projeto, não é senha; a segurança depende das regras do Firestore.
- Autorize somente o UID do administrador nas regras.
- A conta administradora pode alterar XP e anotações. Visitantes não podem escrever no banco.
- Para maior rigor, uma versão futura pode validar no servidor os campos permitidos (por exemplo, impedir XP negativo e impedir alteração de nomes).
