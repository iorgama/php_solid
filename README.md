# SOLID 

SOLID é um acrônimo para cinco boas práticas no desenvolvimento de software abordadadas por Robert Martin, o famoso Tio Bob. 

### Por que utilizar?

* Tornam nosso software mais fácil de manter.
* Evitam duplicação de código.
* Garantem uma maior legibilidade.
* Possibilita reaproveitamento de código.
* Facilitam na refatoração.

### Acrônimo - SOLID
* S: Single-responsibility principle (Princípio da responsabilidade única)
* O: Open-closed principle (Princípio aberto/fechado)
* L: Liskov substitution principle (Princípio da substituição de Liskov)
* I: Interface segregation principle  (Princípio da segregação da interface)
* D: Dependency Inversion Principle  (Princípio da inversão da dependência
Let’s dive in deeper those principles!

## 1. Princípio da responsabilidade única — Single Responsibility Principle:
> Uma classe ou função deve ter uma e apenas uma razão para ser modificada, o que significa que a mesma deve ter apenas uma responsabilidade ou ser responsável por executar uma ação. Evitando assim que tenhamos um alto acomplamento 

* A classe Person abaixo apresenta um exemplo de violação desse príncipio, pois o método 'store' deve pertencer a uma classe que gerencie o banco de dados.

```
<?php

class Person {
  
    private $name;    
    private $age;
    private $weight;
    
    public function store() {
        // Responsável por armazenar atributos no banco de dados...
    }
}

```
* Para resolver o problema acima, o ideal é criar duas classes cada uma com suas responsabilidades: 

```
<?php

class Person {
  
    private $name;    
    private $age;
    private $weight;

}

```


```
<?php

class PersonDB {
  
    public function store(Person $person) {
        // Responsável por armazenar atributos no banco de dados...
    }
}
```


## 2. Princípio aberto/fechado — Open-closed Principle:
> Classes ou métodos devem estar abertos para extensão, mas fechados para alteração. Nesse sentido, quando for necessário adicionar novos comportamentos a uma classe, nós devemos estendê-la e não alterar essa classe já existente.

* No exemplo abaixo temos a classe AreaCalculator cujo método foi implementado para calcular a área do retângulo. Se for necessário calcular a área do quadrado, vai ser preciso alterar o método já existente criando uma validação para saber de qual forma geométrica me refiro e implementar as demais alterações para calcular a área de acordo com a forma selecionada.

```
class AreaCalculator 
{
    public $width;
    public $height;
        
    public function __construct($width, $height)
    {
        $this->width = $width;
        $this->height = $height;
    }
    public function area(){
        $area = $this->height *  $this->width;
        return $area;
    }
}
  

```

* Para corrigir esse problema, sempre que precisarmos adicionar um novo comportameto, devemos criar uma interface que o implemente. 

```

interface Shape {
    public function area();
}

class Rectangle implements Shape
{
    public $width;
    public $height;
    public function __construct($width, $height)
    {
        $this->width = $width;
        $this->height = $height;
    }
    public  function area(){
        $area = $this->height *  $this->width;
        return $area;
    }
}
  
class Square implements Shape {
  
    private $length;
    
    public function __construct($length) {
        $this->length = $length;
    }
    
    public function area() {
        return pow($this->length, 2);
    }
}
```
Ao isolar o método area atrás de uma interfaace, garantimos que cada uma das classes que a implemente possa sobrescrever esse método de acordo com suas necessidades.

## 3. Princípio da substituição de Liskov — Liskov Substitution Principle:
> Seja q(x) uma propriedade demonstrável de objetos x do tipo T. Então q(y) deve ser demonstrável para objetos y de tipo S onde S é um subtipo de T. Ou seja, qualquer classe que implemente uma interface deve ser capaz de cumprimir o contrato estabelecido.

* Ao observarmos o exemplo abaixo, percebemos que as duas classes implementam a classe Image, porém cada uma tem seu próprio método para renderizar. 
```
interface Image 
{
    ...
}

class Renderer implements Image  {
     public function render();
}

class PhotoRenderer implements Image{
    public function renderJpg();
}

```
* Nesse caso o ideal é estruturar muito bem essa abstração para garantir que o método possam ser reaproveitado por cada uma delas.

```
<?php

abstract class Image 
{     
    public function render() 
    {
        ...           
    }
}


class Renderer extends Image
{
    
}

class PhotoRenderer extends Image
{
    
}

```
## 4. Princípio da segregação da interface — Interface Segregation Principle:

> Uma classe não deve ser forçado a implementar uma interface com métodos que não usa. Nesse caso, o ideal é quebrar nossa interface em outras menores ao invés de desenvolver interfaces mais genéricas. 

* No exemplo abaixo atribuímos comportamentos genéricos a nossa interface, o que acabou obrigando as classes que a implementemm a ter métodos que não utilizam.

```
<?php

interface Vehicle {
    public function drive();
    public function quantityPeople();
    public function sail();
   
}

class Car implements Vehicle {
    
    public function drive() {
        ...
    }
        
    public function quantityPeople() {
        ...
    }
     
    public function sail() {
        throw new Exception('Not implemented method');
    }
}

class Ship implements Vehicle {
    
    public function drive() {
        throw new Exception('Not implemented method');
    }
    
    public function quantityPeople() {
        ...
    }
    
    public function sail() {
        ...
    }
}

```

* A solução é dividir essa interface em outras mais específicas que podem ser usadas quando necessário, veja a seguir:

```
<?php

interface Vehicle {
    public function quantityPeople();
}

interface CarInterface {
    public function drive();
}

interface ShipInterface {
    public function sail();
}

class Car implements CarInterface, Vehicle {
    
    public function drive() {
      ...
    }
  
    public function quantityPeople() {
      ...
    }
}

class Ship implements ShipInterface, Vehicle {
    
    public function sail() {
      ...
    }
  
    public function quantityPeople() {
      ...
    }
}

```

## 5. Princípio da inversão da dependência — Dependency Inversion Principle:
> Classes devem depender de abstrações e não de outra classe. Permitindo uma maior reusabilidade de codigo e um menor grau de acoplamento.

* A classe do exemplo abaixo tem a responsabilidade de criar uma instância da classe MySQLConnection para salvar um usuário. Se precisássemos alterar a conexão para OracleConnection seria necessário reescrever essa classe.

```
<?php

class UserDB {
  
    private $dbConnection;
    
    public function __construct(MySQLConnection $dbConnection) {
        $this->$dbConnection = $dbConnection;
    }
  
    public function store(User $user) {
        // ...
    }
}
```

* A solução é abstrair e criar uma interface de conexão com o banco:

```
<?php

interface DBConnectionInterface {
    public function connect();
}

class OracleConnection implements DBConnectionInterface
{
    public function connect()
    {
        // ...
    }
}

class MySQLConnection implements DBConnectionInterface {
  
    public function connect() {
       // ...
    }
}

class UserDB {
  
    private $dbConnection;
    
    public function __construct(DBConnectionInterface $dbConnection) {
        $this->$dbConnection = $dbConnection;
    }
  
    public function store(User $user) {
       // ...
    }
}

```
