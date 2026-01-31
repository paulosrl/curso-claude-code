Faça commit das alterações e envie para o GitHub.

Do the following:

1. Verifique se o diretório é um repositório Git com `git status`. Se não for, informe o usuário e sugira usar o comando `/criarprojetogit`.
2. Verifique se existe um remote configurado com `git remote -v`. Se não houver, pergunte ao usuário a URL do repositório.
3. Execute `git status` para mostrar os arquivos modificados, adicionados e removidos.
4. Execute `git diff` para ver as alterações staged e unstaged.
5. Execute `git log --oneline -5` para ver o estilo dos commits recentes.
6. Adicione os arquivos relevantes ao staging com `git add` (prefira adicionar arquivos específicos em vez de `git add -A`; nunca inclua arquivos sensíveis como `.env`).
7. Crie um commit com uma mensagem descritiva baseada nas alterações feitas.
8. Faça o push com `git push`.
9. Confirme o sucesso mostrando o resultado do push.

Notas:
- Se o commit falhar por causa de assinatura GPG, pergunte ao usuário se deseja prosseguir sem GPG (`--no-gpg-sign`).
- Se houver conflitos no push, informe o usuário e sugira fazer `git pull` antes.
- Sempre mostre ao usuário o que está acontecendo em cada etapa.
