# Segurança de Dados

###### Estudos sobre segurança de dados, Autenticação, Autorização, Sessões, Cookie, LocalStorage, HTTPS, Web Tokens, etc..
<br>
<br>

## Autenticação
Autenticação é capacidade de identificar um determinado usuário de um determinado sistema.  
A autenticação mais comum se dá por comparação de _login_ e _password_, ou seja, usuário e senha.

Quando um novo usuário se registra, ou é restriado, em um sistema, geralmente inform um _login_ e _password_. Essas informações são guardadas em um **Banco de Dados**. 

Abaixo, algumas observações sobre o armazenamento das credenciais de acesso dos usuários:
- A senha não deve ser armazenada em texto simples, uma vez que a base de dados pode ser acessada por uma parte/pessoa mal intencionada. Deve existir um algoritmo para criptografar a senha de acesso antes de armazená-la no Banco de Dados.
- Tem sido uma boa prática utilizar algorimos de geração de _Hash_ para as senhas. Esse tipo de algoritmo é irreversível, ou seja, não é possível descobrir a senha a partir do valor criptografado e salvo no banco de dados. 
Você deve se perguntar: Como um usuário será validado dessa forma? O que fazemos é gerar um novo _Hash_ com os dados informados pelo usuário e comparar se é igual ao _Hash_ salvo na base de dados.
- Além de usar um _Hash_, também tem sido uma boa prática 'salgar' esse _hash_ com um valor a mais, a fim de evitar que vários usuários com a mesma senha tenham o mesmo _hash_. 
Quando um invasor percebe vários _hashs_ iguais, ele ataca estes usuários com ataques de repetição, ou "força bruta", pois sabe que estes usuários possuem alguma das senhas mais usadas pelas pessoas. Ele tentará uma lita de senhas que possui e acabará acertando. E pior, terá acesso não somente a um usuário, mas a varios usuários. 
Salgar uma senha significa usar um valor/_Salt_ do próprio usuário para tornar o _hash_ dele único. Pegar o email do usuário por exemplo, gerar um _Salt_, e somando esse valor à senha do usuário gerar um _hash_.


#### Session / Sessões



