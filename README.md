# Nem todo dia é dia de Cucumber - O dia em que eu deixei o cucumber de lado.

Sim, tu não leu errado, eu realmente tomei a decisão (pontual) de deixar de usar o cucumber nos times em que eu trabalho e o motivo é simples, onde passei a enxergar que o time não via valor nisso e apesar de eu sempre lutar a favor por um ambiente de aprendizado contínuo, ter a famosa "documentação viva", acabei me perguntando se eu também via valor e o mais interessante é que definitivamente nesse contexto em que eu vivo hoje não estava agregando valor algum e comecei a pontuar algumas coisas que me levaram a chegar nessa conclusão:

1. Por mais eu orientasse o time, em seis meses não houve sequer uma pessoa que tivesse entrado (produto ou tecnologia) na empresa, sabendo que há uma Especificação por Exemplo sobre todo o produto que chegasse a ler o material;

2. Reescrever os critérios de aceite em formato de cucumber estava tomando muito tempo;

3. Pensar no critério de aceite de forma genérica é muito mais produtivo que pensar em Dado, Quando e Então;

4. Dentro do meu contexto, apresentar critérios em formato de cucumber prejudica mais do que ajuda (velha escola de desenvolvimento).

5. Abstrair da camada do cucumber deixa meu testes um pouco mais rápido;

6. No fim das contas todos querem o produto em produção, tendo ou não cucumber no processo de desenvolvimento.

Em cima desses seis pontos, comecei a absrair da camada do cucumber e passei a trabalhar com RSPEC para novas features, onde o PM escreve uma user story de um jeito elegante mais ou menos assim:

```ruby
Funcionalidade: Adesão a novo plano de compras

Eu, como usuário de sistema
Desejo ter a opção de escolher novos planos relativos a compras
Com o objetivo de aumentar minha receita

Critérios de aceite:
- Para aderir ao novo plano, o usuário deverá ter um plano inicial;
- Novos usuários poderão aderir a todos os planos;
- Usuários que aderiram ao novo plano poderão realizar o cancelamento;
```
Considerando por hora apenas esses três critérios de aceite, olha a quantidade de linhas que vou passar a escrever considerando apenas esses critérios critérios:

```ruby
Funcionalidade: Adesão ao novo plano de compras

Eu, como usuário de sistema
Desejo ter a opção de escolher novos planos relativos a compras
Com o objetivo de aumentar minha receita

@adesao
Cenário: Aderir ao novo plano com sucesso para clientes existentes
  Dado que eu tenha um plano inicial
  Quando eu escolher o novo plano de compras
  Então o novo plano de compras será alterado com sucesso

Cenário: Aderir ao novo plano com sucesso para novos clientes
  Dado que eu realize o cadastro no sistema
  Quando eu escolher o novo plano de compras
  E realizar o pagamento
  Então o plano será ativado com sucesso

Cenário: Cancelamento do novo plano de compras
  Dado que eu tenha o novo plano de compras
  Quando eu realizar o cancelamento
  Então o plano será cancelado
```

Ou seja, implementando apenas esses critérios de aceite tive que colocar ao menos **dezoito linhas** no meu arquivo de feature contra apenas **oito** que foram escritos na user story e  fora o fato que eu vou precisar implementar os steps. Agora vamos pensar que nós QAs quando olhamos uma user story conseguimos além dos critérios de aceite que o PO observou, conseguimos encontrar mais alguns que são totalmente relevantes, por exemplo:

```ruby
- Só usuários ativos adimplentes poderão aderir ao novo plano;
- Usuário que cancela o novo plano não tem extorno e poderá utilizar o plano até o fechamento;
- Usuário que aderiu ao novo plano possuindo um plano anterior só conseguirá utilizar o novo plano no próximo ciclo;
- Ativação do novo plano apenas após o pagamento para novos clientes.
- Etc.
```

Então imagemos que para cada item desses novos critérios de aceite temos ao menos um Dado, Quando e Então vai resultar em uma feature de pelo menos mais doze linhas, depois implementar e isso toma tempo para que no final ninguém venha utilizar, ninguém vai ler, nenhuma pessoa da equipe tem cucumber como valor, então, porque não abstrairmos dessa camada e deixá-la de lado?

No meu caso, eu continuo a encontrar novos "critérios de aceite" juntamente com a área de produtos, mas agora eu não escrevo mais em cucumber, agora eu escrevo da mesma maneira que eles nas user stories e implemento usando RSPEC, abstraio do cucumber e ainda tenho o critério de aceitção em cada "it", por exemplo:

```ruby
describe 'Adesão ao novo plano de compras' do
  it 'Para aderir ao novo plano, o usuário deverá ter um plano inicial' do
    'implementação'
  end

  it 'Novos usuários poderão aderir a todos os planos' do
    'implementação'
  end

  it 'Usuários que aderiram ao novo plano poderão realizar o cancelamento' do
    'implementação'
  end

  it 'Para aderir ao novo plano, o usuário deverá ter um plano inicial' do
    'implementação'
  end
end
```
Definir os testes dessa maneira eu continuo de certa forma tendo controle do que é automatizado, e por ser uma pessoa mais técnica, tenho controle exato dos steps que são implementados, controle das regras de negócio tanto quanto eu tinha quando eu escrevia em cucumber, agora com a diferença que meu testes corre um pouco mais rápido, não "perco" tempo gerando algo sem valor para o time/empresa.

O meu ecossistema continua sendo o mesmo, usando capybara como framework, usando faker, usando page objects, page step objetcs, usando airborne ou httparty e apenas o cucumber eu deixei de lado nessa minha aventura atual.

A iniciativa Automation For All que eu tenho no meu github vai ter em breve material sobre rspec e vou chamar de rspec for all e assim como o [capybara for all](https://github.com/thiagomarquessp/capybaraforall), [httparty for all](https://github.com/thiagomarquessp/httpartyforall), [calabash for all](https://github.com/thiagomarquessp/calabash_android_for_all) será feito com o intuito de ensinar desde a base até "posso me virar sozinho".

A grande questão é saber o momento certo de parar de seguir seus ideais (e para mim foi muito difícil largar mão de usar cucumber) para tentar uma nova abordagem com o time em que atuamos. Não tenham medo de observar se o time em que trabalham enxergam valor nas coisas que tu faz com cucumber, se é realmente necessário ter essa camada a mais, se perguntar se realmente alguém olha para isso, se utiliza como forma de aprendizado e, se a resposta for negativa, mude para uma nova abordagem, que será fatalmente mais rápida de geração de valor.
