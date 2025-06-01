# How to have multiple Git profiles in same pc (with WSL)

## Ordem correta para clonar repositórios com usuários diferentes

 - Configure
 - Clone

## ✅ 1. Configure as chaves SSH
Crie chaves diferentes para cada identidade (se ainda não fez):
O padrão do git sem arquivos diferentes pode ser encontrado na [documentação oficial](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) que sem os passos seguinte pode ser sobrescrito sem querer o arquivo `~/.ssh/id_ed25519.pub`
 
~~~ bash
ssh-keygen -t ed25519 -C "clienteA@email.com" -f ~/.ssh/id_ed25519_clienteA
ssh-keygen -t ed25519 -C "clienteB@email.com" -f ~/.ssh/id_ed25519_clienteB
~~~

colocar o que vier de chave nos respectivos arquivos, ex:

~~~ bash
cat ~/.ssh/id_ed25519_clienteA.pub
~~~

nos perfis correspondente em `https://github.com/settings/keys`

## ✅ 2. Adicione as chaves ao ssh-agent (opcional mas recomendado)

~~~bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_clienteA
ssh-add ~/.ssh/id_ed25519_clienteB
~~~

## ✅ 3. Configure o arquivo ~/.ssh/config

~~~bash
nano ~/.ssh/config
~~~

Adicione:

~~~bash
# Cliente A
Host github-clienteA
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_clienteA
  IdentitiesOnly yes

# Cliente B
Host github-clienteB
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_clienteB
  IdentitiesOnly yes
~~~

💡 O nome github-clienteA e github-clienteB pode ser qualquer apelido. Ele será usado na URL do clone.

## ✅ 4. Clone usando o host configurado
No lugar de git@github.com, use o Host que você definiu (github-clienteA ou github-clienteB):

~~~bash
# Clone como cliente A
git clone git@github-clienteA:usuario/repo-cliente-a.git ~/projetos/clienteA

# Clone como cliente B
git clone git@github-clienteB:usuario/repo-cliente-b.git ~/projetos/clienteB
~~~

✅ 5. Configure o usuário Git local em cada projeto
Depois de clonar:

~~~bash
cd ~/projetos/clienteA
git config user.name "Nome Cliente A"
git config user.email "clienteA@email.com"

cd ~/projetos/clienteB
git config user.name "Nome Cliente B"
git config user.email "clienteB@email.com"
~~~

Se os repositórios forem públicos e não exigirem autenticação, você pode clonar via HTTPS e configurar o usuário Git local depois do clone.
