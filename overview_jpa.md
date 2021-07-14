# Java Persistence API - JPA

A Java Persistence API fornece aos desenvolvedores Java um recurso de mapeamento de objeto / relacional para gerenciar dados relacionais em aplicativos Java. A persistência de Java consiste em quatro áreas:

- A Java Persistence API

- A linguagem de consulta

- A API Java Persistence Criteria

- Metadados de mapeamento de objeto / relacional

## Entities

Uma entidade é um objeto de domínio de persistência leve. Normalmente, uma entidade representa uma tabela em um banco de dados relacional e cada instância da entidade corresponde a uma linha dessa tabela. O principal artefato de programação de uma entidade é a classe de entidade, embora as entidades possam usar classes auxiliares.

### Requisitos para classes de entidade

Uma classe de entidade deve seguir esses requisitos.

- A classe deve ser anotada com a anotação `javax.persistence.Entity`.

- A classe deve ter um construtor público ou protegido, sem argumento. A classe pode ter outros construtores.

- A classe não deve ser declarada `final`. Nenhum método ou variável de instância persistente deve ser declarado `final`.

- Se uma instância de entidade é passada por valor como um objeto separado, como por meio de uma interface de negócios remota do bean de sessão, a classe deve implementar a interface `Serializable`.

- As entidades podem estender as classes de entidade e não entidade, e as classes de não entidade podem estender as classes de entidade.

- Variáveis de instância persistentes devem ser declaradas privadas, protegidas ou privadas do pacote e podem ser acessadas diretamente apenas pelos métodos da classe de entidade. Os clientes devem acessar o estado da entidade por meio de acessadores ou métodos de negócios.

### Campos e propriedades persistentes em classes de entidade

O estado persistente de uma entidade pode ser acessado por meio de variáveis de instância ou propriedades da entidade. Os campos ou propriedades devem ser dos seguintes tipos de linguagem Java:

- Tipos primitivos Java

- `java.lang.String`

- Outros tipos serializáveis, incluindo:

    - Wrappers de tipos primitivos Java

    - `java.math.BigInteger`

    - `java.math.BigDecimal`

    - `java.util.Date`

    - `java.util.Calendar`

    - `java.sql.Date`

    - `java.sql.Time`

    - `java.sql.TimeStamp`

    - Tipos serializáveis definidos pelo usuário

    - `byte[]`

    - `Byte[]`

    - `char[]`

    - `Character[]`

- Tipos enumerados

- Outras entidades e / ou coleções de entidades

- Classes incorporáveis

### Usando coleções em propriedades e campos de entidade

Os campos e propriedades persistentes com valor de coleção devem usar as interfaces de coleção Java suportadas, independentemente de a entidade usar campos ou propriedades persistentes. As seguintes interfaces de coleção podem ser usadas:

- `java.util.Collection`

- `java.util.Set`

- `java.util.List`

- `java.util.Map`

Se a classe de entidade usa campos persistentes, o tipo nas assinaturas de método anteriores deve ser um desses tipos de coleção. Variantes genéricas desses tipos de coleção também podem ser usadas. Por exemplo, se ela tiver uma propriedade persistente que contém um conjunto de números de telefone, a entidade `Customer` terá os seguintes métodos:

```java
Set<PhoneNumber> getPhoneNumbers() {}
void setPhoneNumbers(Set<PhoneNumber>) {}
```

e um campo ou propriedade de uma entidade consiste em uma coleção de tipos básicos ou classes incorporáveis, use a anotação `javax.persistence.ElementCollection` no campo ou propriedade.

Os dois atributos de `@ElementCollection` são `targetClass` e `fetch`. O atributo `targetClass` especifica o nome da classe da classe básica ou incorporável e é opcional se o campo ou propriedade for definido usando genéricos da linguagem de programação Java. O `fetch` atributo opcional é usado para especificar se a coleção deve ser recuperada preguiçosamente ou avidamente, usando as constantes `javax.persistence.FetchType` de `LAZY` ou `EAGER`, respectivamente. Por padrão, a coleção será buscada lentamente.

A entidade a seguir, `Persont` em um campo persistente, `nicknames` que é uma coleção de classes `String` que serão buscadas avidamente. O elemento `targetClass` não é obrigatório, pois usa generics para definir o campo:

```java
@Entity
public class Person {
    @ElementCollection(fetch=EAGER)
    protected Set<String> nickname = new HashSet();
}
```

### Chaves primárias em entidades
Cada entidade possui um identificador de objeto exclusivo. Uma entidade cliente, por exemplo, pode ser identificada por um número de cliente. O identificador exclusivo, ou chave primária, permite que os clientes localizem uma instância de entidade específica. Cada entidade deve ter uma chave primária. Uma entidade pode ter uma chave primária simples ou composta.

As chaves primárias simples usam a anotação `javax.persistence.Id` para denotar a propriedade ou campo da chave primária.

As chaves primárias compostas são usadas quando uma chave primária consiste em mais de um atributo, que corresponde a um conjunto de propriedades ou campos persistentes únicos. As chaves primárias compostas devem ser definidas em uma classe de chave primária. As chaves primárias compostas são denotadas com as anotações `javax.persistence.EmbeddedId` e `javax.persistence.IdClass`.

A chave primária, ou a propriedade ou campo de uma chave primária composta, deve ser um dos seguintes tipos de linguagem Java:

- Tipos primitivos Java

- Tipos de wrapper primitivo Java

- `java.lang.String`

- `java.util.Date` (o tipo temporal deve ser `DATE`)

- `java.sql.Date`

- `java.math.BigDecimal`

- `java.math.BigInteger`

Uma classe de chave primária deve atender a esses requisitos.

- O modificador de controle de acesso da classe deve ser `public`.

- As propriedades da classe de chave primária devem ser `public` ou `protected` se o acesso baseado em propriedade for usado.

- A classe deve ter um construtor público padrão.

- A classe deve implementar os métodos `hashCode()` e `equals(Object other)`.

- A classe deve ser serializável.

- Uma chave primária composta deve ser representada e mapeada para vários campos ou propriedades da classe de entidade ou deve ser representada e mapeada como uma classe incorporável.

- Se a classe for mapeada para vários campos ou propriedades da classe de entidade, os nomes e tipos dos campos ou propriedades de chave primária na classe de chave primária devem corresponder aos da classe de entidade.

### Multiplicidade em relacionamentos de entidades
As multiplicidades são dos seguintes tipos.

- **Um para um:** Cada instância de entidade está relacionada a uma única instância de outra entidade. Por exemplo, para modelar um armazém físico no qual cada posição de armazenamento contém um único widget `StorageBin` e `Widget` teria um relacionamento um para um. Os relacionamentos um para um usam a anotação `javax.persistence.OneToOne` na propriedade ou campo persistente correspondente.

- **Um para muitos:** uma instância de entidade pode ser relacionada a várias instâncias de outras entidades. Um pedido de venda, por exemplo, pode ter vários itens de linha. No aplicativo de pedido, `CustomerOrder` teria um relacionamento um-para-muitos com `LineItem`. Os relacionamentos um para muitos usam a anotação `javax.persistence.OneToMany` na propriedade ou campo persistente correspondente.

- **Muitos para um:** várias instâncias de uma entidade podem ser relacionadas a uma única instância da outra entidade. Essa multiplicidade é o oposto de um relacionamento um-para-muitos. No exemplo que acabamos de mencionar, o relacionamento com `CustomerOrder` da perspectiva de `LineItem` é de muitos para um. Os relacionamentos muitos para um usam a anotação `javax.persistence.ManyToOne` na propriedade ou campo persistente correspondente.

- **Muitos para muitos:** as instâncias da entidade podem ser relacionadas a várias instâncias umas das outras. Por exemplo, cada curso universitário tem muitos alunos, e cada aluno pode fazer vários cursos. Portanto, em um aplicativo de inscrição, `Course` e `Student` teria um relacionamento muitos para muitos. Os relacionamentos muitos para muitos usam a anotação `javax.persistence.ManyToMany` na propriedade ou campo persistente correspondente.

#### Direção nas Relações de Entidades

A direção de um relacionamento pode ser bidirecional ou unidirecional. Um relacionamento bidirecional tem um lado de propriedade e um lado inverso. Um relacionamento unidirecional tem apenas um lado proprietário. O lado proprietário de um relacionamento determina como o tempo de execução do Persistence faz atualizações no relacionamento no banco de dados.

##### Relações Bidirecionais

Em um relacionamento bidirecional, cada entidade possui um campo ou propriedade de relacionamento que se refere à outra entidade. Por meio do campo ou propriedade de relacionamento, o código de uma classe de entidade pode acessar seu objeto relacionado. Se uma entidade tiver um campo relacionado, diz-se que a entidade "sabe" sobre seu objeto relacionado. Por exemplo, se `CustomerOrder` sabe quais `LineItem` instâncias ele tem e se `LineItem` sabe a que `CustomerOrder` pertence, eles têm uma relação bidirecional.

Os relacionamentos bidirecionais devem seguir essas regras.

- O lado inverso de uma relação bidireccional deve referir-se ao seu lado possuir usando o elemento `mappedBy` da anotação `@OneToOne`, `@OneToMany`, ou `@ManyToMany`. O elemento `mappedBy` designa a propriedade ou campo na entidade que é o proprietário do relacionamento.

- Os muitos lados dos relacionamentos bidirecionais muitos para um não devem definir o elemento `mappedBy`. O lado múltiplo é sempre o lado proprietário do relacionamento.

- Para relacionamentos bidirecionais um para um, o lado proprietário corresponde ao lado que contém a chave estrangeira correspondente.

- Para relacionamentos bidirecionais muitos para muitos, qualquer um dos lados pode ser o lado proprietário.

##### Relacionamentos Unidirecionais

Em um relacionamento unidirecional, apenas uma entidade possui um campo ou propriedade de relacionamento que se refere à outra. Por exemplo, `LineItem` teria um campo de relacionamento que identifica `Product`, mas `Product` não teria um campo de relacionamento ou propriedade para `LineItem`. Em outras palavras, `LineItem` conhece `Product`, mas `Product` não sabe quais instâncias `LineItem` se referem a ele.

##### Consultas e direção de relacionamento

A linguagem de consulta de persistência Java e as consultas de API de critérios geralmente navegam entre relacionamentos. A direção de um relacionamento determina se uma consulta pode navegar de uma entidade para outra. Por exemplo, uma consulta pode navegar de `LineItem` para, `Product` mas não pode navegar na direção oposta. Para `CustomerOrder` e `LineItem`, uma consulta pode navegar em ambas as direções porque essas duas entidades têm um relacionamento bidirecional.

##### Operações e relacionamentos em cascata
As entidades que usam relacionamentos geralmente têm dependências da existência da outra entidade no relacionamento. Por exemplo, um item de linha faz parte de um pedido; se o pedido for excluído, o item de linha também deve ser excluído. Isso é chamado de relacionamento de exclusão em cascata.

O tipo enumerado `javax.persistence.CascadeType` define as operações em cascata que são aplicadas no elemento `cascade` das anotações de relacionamento.

###### Operações em cascata para entidades

- **ALL** Todas as operações em cascata serão aplicadas à entidade relacionada da entidade pai. `All` é equivalente a especificar `cascade={DETACH, MERGE, PERSIST, REFRESH, REMOVE}`

- **DETACH** Se a entidade pai for desanexada do contexto de persistência, a entidade relacionada também será desanexada.

- **MERGE** Se a entidade pai for mesclada no contexto de persistência, a entidade relacionada também será mesclada.

- **PERSIST** Se a entidade pai for persistida no contexto de persistência, a entidade relacionada também será persistida.

- **REFRESH** Se a entidade pai for atualizada no contexto de persistência atual, a entidade relacionada também será atualizada.

- **REMOVE** Se a entidade pai for removida do contexto de persistência atual, a entidade relacionada também será removida.

Os relacionamentos de exclusão em cascata são especificados usando a especificação do elemento `cascade=REMOVE` para os relacionamentos `@OneToOne` e `@OneToMany`. Por exemplo:

```java
@OneToMany(cascade=REMOVE, mappedBy="customer")
public Set<CustomerOrder> getOrders() { return orders; }
```

##### Remoção de órfãos em relacionamentos
Quando uma entidade de destino em um relacionamento um-para-um ou um-para-muitos é removida do relacionamento, geralmente é desejável cascatear a operação de remoção para a entidade de destino. Essas entidades de destino são consideradas "órfãs" e o atributo `orphanRemoval` pode ser usado para especificar que as entidades órfãs devem ser removidas. Por exemplo, se um pedido tiver muitos itens de linha e um deles for removido do pedido, o item de linha removido é considerado órfão. Se `orphanRemoval` for definido como `true`, a entidade do item de linha será excluída quando o item de linha for removido do pedido.

O atributo `orphanRemoval` em `@OneToMany` e `@oneToOne` assume um valor booleano e é, por padrão, falso.

O exemplo a seguir irá cascatear a operação de remoção para a `order` entidade órfã quando a entidade `customer` for excluída:

```java
@OneToMany(mappedBy="customer", orphanRemoval="true")
public List<CustomerOrder> getOrders() {}
```

#### Classes incorporáveis em entidades

As classes incorporáveis são usadas para representar o estado de uma entidade, mas não têm uma identidade persistente própria, ao contrário das classes de entidade. As instâncias de uma classe incorporável compartilham a identidade da entidade que a possui. As classes incorporáveis ​​existem apenas como o estado de outra entidade. Uma entidade pode ter atributos de classe incorporáveis ​​com valor único ou com valor de coleção.

As classes incorporáveis têm as mesmas regras que as classes de entidade, mas são anotadas com a anotação `javax.persistence.Embeddable` em vez de `@Entity`.

A seguinte classe incorporável, `ZipCode` tem os campos `zip` e `plusFour`:

```java
@Embeddable
public class ZipCode {
    String zip;
    String plusFour;
}
```

Esta classe incorporável é usada pela entidade `Address`:

```java
@Entity
public class Address {
    @Id
    protected long id;
    String street1;
    String street2;
    String city;
    String province;
    @Embedded
    ZipCode zipCode;
    String country;
}
```

Entidades que possuem classes incorporáveis como parte de seu estado persistente podem anotar o campo ou propriedade com a anotação `javax.persistence.Embedded`, mas não são obrigadas a fazer isso.

## Herança de Entidade

As entidades oferecem suporte à herança de classes, associações polimórficas e consultas polimórficas. As classes de entidade podem estender as classes sem entidade e as classes sem entidade podem estender as classes de entidade. As classes de entidade podem ser abstratas e concretas.

### Entidades Abstratas

Uma classe abstrata pode ser declarada uma entidade decorando a classe com `@Entity`. Entidades abstratas são como entidades concretas, mas não podem ser instanciadas.

```java
@Entity
public abstract class Employee {
    @Id
    protected Integer employeeId;
}

@Entity
public class FullTimeEmployee extends Employee {
    protected Integer salary;
}

@Entity
public class PartTimeEmployee extends Employee {
    protected Float hourlyWage;
}
```

### Superclasses mapeadas

As entidades podem herdar de superclasses que contêm estado persistente e informações de mapeamento, mas não são entidades. Ou seja, a superclasse não é decorada com a anotação `@Entity` e não é mapeada como uma entidade pelo provedor Java Persistence. Essas superclasses são usadas com mais frequência quando você tem informações de estado e mapeamento comuns a várias classes de entidade.

As superclasses mapeadas são especificadas decorando a classe com a anotação `javax.persistence.MappedSuperclass`:

```java
@MappedSuperclass
public class Employee {
    @Id
    protected Integer employeeId;
}

@Entity
public class FullTimeEmployee extends Employee {
    protected Integer salary;
}

@Entity
public class PartTimeEmployee extends Employee {
    protected Float hourlyWage;
}
```

As superclasses mapeadas não podem ser consultadas e não podem ser usadas em operações `EntityManager` ou `Query`. Você deve usar subclasses de entidade da superclasse mapeada em operações `EntityManager` ou `Query`. As superclasses mapeadas não podem ser alvos de relacionamentos de entidade. As superclasses mapeadas podem ser abstratas ou concretas.

### Superclasses de não entidades

As entidades podem ter superclasses sem entidade, e essas superclasses podem ser abstratas ou concretas. O estado das superclasses de não entidade é não persistente e qualquer estado herdado da superclasse de não entidade por uma classe de entidade é não persistente. Superclasses sem entidade não podem ser usadas em operações `EntityManager` ou `Query`. Qualquer mapeamento ou anotações de relacionamento em superclasses de não entidade são ignorados.

## EntityManager

As entidades são gerenciadas pelo gerenciador de entidades, que é representado por instâncias de `javax.persistence.EntityManager`. Cada instância de `EntityManager` está associada a um contexto de persistência: um conjunto de instâncias de entidades gerenciadas que existem em um armazenamento de dados específico. Um contexto de persistência define o escopo sob o qual instâncias de entidades específicas são criadas, mantidas e removidas. A interface `EntityManager` define os métodos que são usados para interagir com o contexto de persistência.

### A interface EntityManager

A API `EntityManager` cria e remove instâncias de entidade persistentes, encontra entidades pela chave primária da entidade e permite que consultas sejam executadas em entidades.

Com um gerenciador de entidade gerenciado por contêiner `EntityManager`, o contexto de persistência de uma instância é propagado automaticamente pelo contêiner para todos os componentes do aplicativo que usam a instância `EntityManager` em uma única transação Java Transaction API (JTA).

As transações JTA geralmente envolvem chamadas entre os componentes do aplicativo. Para concluir uma transação JTA, esses componentes geralmente precisam de acesso a um único contexto de persistência. Isso ocorre quando um `EntityManager` é injetado nos componentes do aplicativo por meio da anotação `javax.persistence.PersistenceContext`. O contexto de persistência é propagado automaticamente com a transação JTA atual e as referências `EntityManager` que são mapeadas para a mesma unidade de persistência fornecem acesso ao contexto de persistência dentro dessa transação. Ao propagar automaticamente o contexto de persistência, os componentes do aplicativo não precisam passar referências a instâncias `EntityManager` entre si para fazer alterações em uma única transação. O contêiner Java EE gerencia o ciclo de vida de gerenciadores de entidade gerenciados por contêiner.

Para obter uma instância `EntityManager`, injete o gerenciador de entidade no componente do aplicativo:

```java
@PersistenceContext
EntityManager em;
```
