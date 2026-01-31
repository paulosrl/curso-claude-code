Crie um repositório Git e envie o projeto para o GitHub.

Do the following:

1. Verifique se o diretório já é um repositório Git com `git status`. Se já for, informe o usuário e pergunte se deseja continuar.
2. Verifique se o usuário está autenticado no GitHub CLI com `gh auth status`.
3. Verifique se existe um arquivo `.gitignore`. Se não existir, pergunte ao usuário se deseja criar um adequado ao projeto.
4. Inicialize o repositório Git com `git init` e renomeie a branch para `main` com `git branch -M main`.
5. Adicione todos os arquivos com `git add -A` e mostre o `git status` para o usuário revisar.
6. Pergunte ao usuário o nome do repositório no GitHub (formato: `usuario/nome-repo`).
7. Pergunte se o repositório deve ser público ou privado.
8. Faça o commit inicial com a mensagem "Initial commit".
9. Crie o repositório no GitHub com `gh repo create` usando as opções escolhidas pelo usuário.
10. Configure o remote origin apontando para o repositório criado (se não foi configurado automaticamente).
11. Faça o push com `git push -u origin main`.
12. Confirme o sucesso mostrando a URL do repositório no GitHub.

Notas:
- Se o commit falhar por causa de assinatura GPG, pergunte ao usuário se deseja prosseguir sem GPG (`--no-gpg-sign`).
- Se o push via HTTPS falhar, tente configurar o remote com SSH como alternativa.
- Sempre mostre ao usuário o que está acontecendo em cada etapa.
