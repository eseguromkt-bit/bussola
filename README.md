# Bússola Grupo É

Ferramenta interna da equipe de expansão do Grupo É: roteiro comercial, checklist de atendimento ao vivo, tabela de comissões e calculadora de ganhos.

O site é uma única página, sem servidor e sem banco de dados. Os atendimentos de cada consultor ficam salvos no navegador dele (`localStorage`) e nunca saem do computador.

## Como o conteúdo é protegido

Este repositório é público, mas o conteúdo do app **não** está aqui em texto legível.

O `index.html` contém apenas a tela de senha e um bloco cifrado com **AES-256-GCM**, cuja chave é derivada da senha da equipe por **PBKDF2 (250.000 iterações)**. Quem abre o código-fonte do site — ou este repositório — vê só bytes embaralhados. O roteiro, as comissões e as taxas só são decifrados dentro do navegador de quem digita a senha certa.

Por isso o `app.html`, que é a fonte em texto puro, está no `.gitignore` e **não** é versionado aqui.

Duas consequências práticas:

- **A senha é a única chave.** Quem a tiver, lê tudo; quem não a tiver, não lê nada. Trate-a como uma credencial de verdade e evite senhas curtas ou óbvias.
- **A fonte vive fora do repositório.** A cópia de referência do `app.html` fica no projeto do Claude ("script de vendas expansao"). É de lá que as alterações partem.

## Publicação

O GitHub Pages serve direto da branch `main`, pasta raiz — não há build no servidor nem GitHub Actions. O `index.html` que está aqui é exatamente o que vai ao ar.

Para ativar, uma única vez: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / `(root)` → Save**.

## Publicar uma alteração

```bash
node build.js                 # recifra o app.html com a senha de senha.txt
git add -A
git commit -m "descreva a alteração"
git push
```

O site atualiza em cerca de um minuto. Sem terminal, dá para fazer o mesmo pela interface do GitHub: abrir o `index.html`, usar **Upload files** e substituir pelo arquivo novo.

## Trocar a senha da equipe

```bash
node build.js "novaSenha"
git add -A && git commit -m "troca a senha da equipe" && git push
```

Cada build gera um `salt` e um `iv` novos, então o `index.html` muda por inteiro mesmo que a senha continue a mesma. Quem estiver com a aba aberta segue usando; na próxima visita, a senha nova é exigida.

## Arquivos

| Arquivo | O que é |
| --- | --- |
| `app.html` | **A fonte.** Fora do Git — guardada no projeto do Claude. |
| `build.js` | Cifra o `app.html` com a senha e gera o `index.html`. |
| `index.html` | O que vai para o ar. **Gerado — não edite à mão.** |
| `senha.txt` | Senha atual em texto puro. Fora do Git. |

## Onde os dados ficam

Nada é enviado para lugar nenhum. Cada consultor tem seus próprios atendimentos, salvos no navegador em que usa a ferramenta. Trocar de computador ou limpar os dados do navegador apaga tudo — por isso existe o exportar/importar `.json` em **Configurações**.
