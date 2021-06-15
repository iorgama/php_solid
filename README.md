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
