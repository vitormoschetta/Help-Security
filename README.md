# Segurança de Dados

###### Estudos sobre segurança de dados, Autenticação, Autorização, Sessões, Cookie, LocalStorage, HTTPS, Web Tokens, etc..
<br>
<br>

## Autenticação
Autenticação é capacidade de identificar um determinado usuário de um determinado sistema.  
A autenticação mais comum se dá por comparação de _login_ e _password_, ou seja, usuário e senha.

Quando um novo usuário se registra, ou é restriado, em um sistema, geralmente inform um _login_ e _password_. Essas informações são guardadas em um **Banco de Dados**. 

#### Armazenamento / Base de Dados

Abaixo, algumas observações sobre o armazenamento das credenciais de acesso dos usuários:
- A senha não deve ser armazenada em texto simples, uma vez que a base de dados pode ser acessada por uma parte/pessoa mal intencionada. Deve existir um algoritmo para criptografar a senha de acesso antes de armazená-la no Banco de Dados.
- Tem sido uma boa prática utilizar algorimos de geração de _Hash_ para as senhas. Esse tipo de algoritmo é irreversível, ou seja, não é possível descobrir a senha a partir do valor criptografado e salvo no banco de dados. 
Você deve se perguntar: Como um usuário será validado dessa forma? O que fazemos é gerar um novo _Hash_ com os dados informados pelo usuário e comparar se é igual ao _Hash_ salvo na base de dados.
- Além de usar um _Hash_, também tem sido uma boa prática 'salgar' esse _hash_ com um valor a mais, a fim de evitar que vários usuários com a mesma senha tenham o mesmo _hash_. 
Quando um invasor percebe vários _hashs_ iguais, ele ataca estes usuários com ataques de repetição, ou "força bruta", pois sabe que estes usuários possuem alguma das senhas mais usadas pelas pessoas. Ele tentará uma lita de senhas que possui e acabará acertando. E pior, terá acesso não somente a um usuário, mas a varios usuários. 
Salgar uma senha significa usar um valor/_Salt_ do próprio usuário para tornar o _hash_ dele único. Pegar o email do usuário por exemplo, gerar um _Salt_, e somando esse valor à senha do usuário gerar um _hash_.
<br>


#### Session / Sessões
Uma das práticas mais seguras para se trabalhar com autenticação no protocolo HTTP / Páginas Web, é utilizar **estado de sessão no servidor**. 

###### Como funciona?
Quando um usuário requisita o login, informando _login_ e _password_, geralmente, em uma aplicação segura, um determinado algoritmo gera o _hash_ das credenciais informadas pelo usuário e compara ao que existe salvo na base de dados. Se as informações forem iguais, a aplicação abre uma **sessão na memória do servidor**, onde guardará todas as informações referentes a este usuário, mais um ID (valor de identificação única). A aplicação então seta o ID em _Cookie_ seguro, do tipo _HTTP Only_ e envia o HTML requisitado ao _Client / Web Browser_. 

Toda vez que uma nova solicitação for feita ao Servidor a aplicação identificará o ID existente no _Cookie_ e irá comparar com a lista de ID nas Sessões em memória para identificar o usuário. 

Essa abordagem de fato tem sido segura, mas com o avanço dos meios de comunicação e da internet as aplicações _frontend_ modernas precisam fazer comunicação com diversos Servidores. Logo, em um ambiente de **microsserviços** se fez necessário a implementação de uma outra forma de autenticação. Onde os dados do usuário não ficassem restritos a um Servidor, mas pudessem ser transportados para o _Client_ para que assim pudessem se comunicar e ser identificado por quaiquer Servidor. Esse é o conceito da **autenticação sem estado**, ou **autenticação por Token**.
<br>

#### Token Web
O mais conhecido é o **Jason Web Token (JWT)**. É o mais utilizado atualmente. 

###### O que é?
JWT é um padrão aberto de transferência de dados. 
Ele é dividido em três partes principais:
1. **Header**: O Algoritmo utilizado (Simétrico ou Assimétrico)
2. **Payload**: A carga principal, onde estão os dados do usuário
3. **Signature**: A assinatura do Token. É o que garante que um determinado token é válido. 

O que garante a validade de um Tokem é a sua assinatura. A assinatura é gerada a partir de um **Secret / Segredo**, que nada mais é que uma sequência de caracteres, uma _string_ que deve ser guardada. É necessário possuir esse segredo para poder validar um JWT.

Existem dois tipos básicos de Algoritmos utilizados em um JWT:
1. **Simétrico (HS256)**: É utilizado o mesmo Segredo para gerar e para validar um JWT. 
2. **Assimétrico (RS256)**: É utilizado um par de Chaves: **a)**: Um Segredo de posse do servidor, que serve apenas para Gerar um Token. **b)**: Uma chave pública que serve apenas para validar o Token. 

Quando usar um ou outro?   
**Resposta**: Se quem gerencia o Servidor também gerencia os _Clients_ que irão validar o Token, ele pode usar um algoritmo simétrico sem problemas.
Agora, se for necessário que terceiros validem seus tokens, não é uma boa idéia dar a eles a única chave que você possui, pois não poderás gerenciar o armazenamento desta chave. Logo no segundo casao cabe o uso do algoritmo Assimétrico.

