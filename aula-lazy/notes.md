#  Lazy & Eager loading

### Eager loading:
O carregamento Eager inicializa o recurso no momento em que a entidade é carregada do banco de dados. 
Todos os dados são carregados, incluindo a inicialização dos recursos associados, independente 
se serão de fato usados ou não.<br>
* Isso pode gerar sobrecarga, caso existam muitas associações ou dados relacionados.

### Lazy loading:
Adia o carregamento dos dados associados até que sejam **explicitamente** acessados no 
código. Dessa forma a aplicação não fica sobrecarregada com dados que não são pertinentes ao contexto.
* Dependendo do fluxo da aplicação, o carregamento lazy pode gerar degradação de performance, especialmente
no caso onde muitas entidades associadas são acessadas individualmente, gerando várias consultas ao
banco de dados - um problema conhecido como **N+1 Select Problem**.

***

No caso de entidades associadas, ***<ins>usando Spring Data JPA + Hibernate*** o comportamento é o seguinte:

>**Relacionamento para-um (@ManyToOne, @OneToOne)**
>
> Quando uma entidade *X(@ManyToOne)* consultada possui um relacionamento -1 com outra entidade *Y(@OneToMany)*, 
> ou seja, cada entidade *X* está associada a UMA outra entidade *Y*, o carregamento é do
> tipo **<ins>Lazy**.
>
> 
> * **IMPORTANTE**: O padrão do Hibernate para carregamentos ManyToOne é o **LAZY**. Porém, na conversão
> do resultado da consulta para JSON pelo Spring o mecanismo de serialização, força o proxy Lazy a inicializar 
> e o Hibernate faz o carregamento da entidade associada Y. 
> É necessário considerar outros processos realizados pelo Spring que podem ocasionar esse comportamento.

>**Relacionamento para-muitos**
>
> Quando uma entidade *X(@OneToMany)* consultada possui um relacionamento -n com outra entidade *Y(@ManyToOne)*, 
> ou seja, cada entidade *X* está associada a VÁRIAS outras entidades *Y*, o carregamento por padrão é do
> tipo **<ins>Lazy**.
> 
> Carregar X vai carregar apenas X. Outros recursos e entidades relacionadas a X só serão recuperados
> do banco de dados e carregados quando **explicitamente chamados no código**.

***
<p align="center">
    <img alt="alt text" src="three-layer-arq.jpg" title="3-layer-arq"/>
</p>    

