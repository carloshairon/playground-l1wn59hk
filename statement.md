# Classes e Structs 02 - Métodos
Métodos são funções associadas a classes, structs ou enums. Desse modo, os métodos definem um conjuto de operações que podem modificar os valores dos atributos. Também podem ser vistos como mensagens enviadas pelo programador de modo que um objeto possa modificar seu estado. 

==> Veja que na classe abaixo existem 3 métodos (func é a palavra chave que os define). veja que o código não executa por falta de implementação de um método. Você pode escrever esse método. 

```swift runnable
class Counter {
    var count = 0 //atributo
    
    func increment() { //método
        count += 1
    }
    func increment(amount: Int) { //método
        count += amount
    }
    func reset() { //método
        count = 0
    }
}

    let c1 =  Counter.init()
    c1.increment(amount: 5) // Veja que aqui mudados o estado do atributo count
    c1.increment(amount: 2) // logo mudamos o estado do objeto com o método increment
    c1.show()

```

## Labels e Parâmetros 
No swift é comum estabelecer labels (rótulos) e nome de parâmetros nas funções e métodos. Isso deixa mais descritivo o código que modo que o programador saiba o que colocar em cada um dos parâmetros só na leitura da função e não apenas qual a sequencia de parâmetros a ser passada considerando os tipos. 

Veja que nesse exmplo o método incremento tem um label "by"  e um parâmetro "amount". Veja também como fica a chamada. 

Por default o nome do parâmetro será também definido como nome do label caso o programador omita o nome do label, mas na chamada sempre deve ser usado o nome do label.


==> Você poderia criar um método chamado decrement e exemplificar uma chamada? Não esqueça de usar labels nos argumentos deste método

```swift runnable

class Counter {
    var count = 0
    
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {// by => label apenas descrição
        count += amount             // amount => parametro, pode ser usado na função
    }
    func reset() {
        count = 0
    }
    func show () {
            print(count)
        }
}

    let c1 =  Counter.init()
    c1.increment(by: 5) //chamada usando o label by
    c1.increment(by: 2)
    c1.show()

```

Vamos ver um exemplo de métodos com struct. Atenção para os labels x e y e os nomes de parâmetros  deltaX e deltaY.

==> você poderia criar no método moveBy usar a referência do 

struct Point {
    var x = 0.0, y = 0.0

    //atenção para os labels da declaração "x" e "y"
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX //deltaX é o nome do parâmetro e x é um atributo
        y += deltaY //deltaY é o nome do parâmetro e y é um atributo
    }
}

//veja que na chamada usamos os labels
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("O ponto é agora (\(somePoint.x), \(somePoint.y))")


## Modificando Value Types com Métodos Instaciados
Você reparou no modificador "mutating" antes da palavra reservada func? 

Estruturas e enumerações são tipos de valor. Por padrão, as propriedades de um tipo de valor não podem ser modificadas a partir de seus métodos de instância.

No entanto, se você precisar modificar as propriedades de sua estrutura ou enumeração em um método específico, poderá optar por alterar o comportamento desse método. O método pode então sofrer mutação (ou seja, alterar) suas propriedades de dentro do método, e quaisquer alterações feitas são gravadas de volta na estrutura original quando o método termina. O método também pode atribuir uma instância completamente nova à sua propriedade self implícita, e essa nova instância substituirá a existente quando o método terminar.

Então, por isso você deve usar o modificador "mutating" em métodos de structs e enums que precisam modificar um atributo. Isso não é necessário para classe porque classes são do tipo referência. 

==> Você poder criar um método em que o programador pode alterar os valores de x e de y se o ponto (x,y) estiver na origem? Ou seja se os parâmetros x>0 e y>0 o método altera o self.x e o self.y senão nada é modificado. Lebra-se que o self é o objeto corrente, não lembra? Veja a aula anterior para revisar. 

```swift runnable

struct Point {
    var x = 0.0, y = 0.0

    //atenção para os labels da declaração "x" e "y"
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX //deltaX é o nome do parâmetro e x é um atributo
        y += deltaY //deltaY é o nome do parâmetro e y é um atributo
    }
}

```

//veja que na chamada usamos os labels
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("O ponto é agora (\(somePoint.x), \(somePoint.y))")


Veja um exemplo com enum

==>Você pode criar um método para fazer o ant(), ou seja, leva a lâmpada não para o próximo estado, mas para o anterior. Em seguida tente fazer mais instâncias do enum e então comente o código de modo a exeplicar o self.  

```swift runnable

enum LampadaTresEstados {
    case desligado, baixo, alto
    mutating func prox() {
        switch self {
        case .desligado:
            self = .baixo
        case .baixo:
            self = .alto
        case .alto:
            self = .desligado
        }
    }
    func mostraEstado() {
        print(self)
    }
}

var lampada = LampadaTresEstados.desligado
lampada.mostraEstado()

lampada.prox()
lampada.mostraEstado()

lampada.prox()
lampada.mostraEstado()

```

## Inicialização de Objetos e Tipo de Retorno de Métodos

Para criar um método de inicialização (construtor) basta usar a palavra reservada init. e para estabelecer método com tipo de retorno use o operador "->" mais o tipo após a declaração de parâmetros e usar o "return" para retorar o valor desejado. Veja o exemplo abaixo. 

==> Você poder cirar um método que devlva a distância a origem (Double) de um determinado ponto? Use um dos tipos ponto que você já fez. Não esqueça que a distância da origem é uma aplicação do teorema de pitágoras. Recupere ai seus conhecimentos matemáticos e procure como calcular a raíz quadrada em Swift. 


```swift runnable
class Pessoa {
    var nome : String = ""
    
    //método inicializador - se tiver parâmetro vai ser necessário em swift
    init (nome: String) {
        self.nome = nome;
    }
    

    func saudacao(_ msg: String) -> String {
        let saudacao = msg+", " + self.nome + "!"
        return saudacao
    }

}

var hairon = Pessoa(nome: "Carlos Hairon")
print (hairon.saudacao("Bom dia"))
print (hairon.saudacao("Boa Noite"))
```