# Estudo de SOLID

## Martin R.C. - Agile software development_ principles, patterns, and practices-Pearson

## O que é Design Ágil? (Capítulo 07)

### Odores de Design - Os Odores de Software Apodrecendo

1. **Rigidez** - O sistema é difícil de mudar porque toda mudança força muitas outras mudanças em outras partes do sistema.
2. **Fragilidade** - Mudanças causam falhas no sistema em lugares que não têm relação conceitual com a parte que foi alterada.
3. **Imobilidade** - É difícil desembaraçar o sistema em componentes que podem ser reutilizados em outros sistemas.
4. **Viscosidade** - Fazer as coisas corretamente é mais difícil do que fazer as coisas erradas.
5. **Complexidade Desnecessária** - O design contém infraestrutura que não adiciona nenhum benefício direto.
6. **Repetição Desnecessária** - O design contém estruturas repetitivas que poderiam ser unificadas sob uma única abstração.
7. **Opacidade** - É difícil de ler e entender. Não expressa bem sua intenção.

### Exemplo Ruim (código degradado)


```python
ptFlag = False
punchFlag = False
# remember to reset these flags
def Copy():
    c = None
    while ((c := RdPt() if ptFlag else RdKbd()) != EOF):
        WrtPunch(c) if punchFlag else WrtPrt(c)
```

bom exemplo

```python
from abc import ABC, abstractmethod

class Reader(ABC):
    @abstractmethod
    def read(self):
        pass

class KeyboardReader(Reader):
    def read(self):
        return RdKbd()  # RdKbd() needs to be defined

GdefaultReader = KeyboardReader()

def Copy(reader = GdefaultReader):
    c = None
    while ((c := reader.read()) != EOF):  # EOF needs to be defined
        WrtPrt(c)  # WrtPrt(c) needs to be defined
```

## SRP: O Princípio da Responsabilidade Única

O Princípio da Única Responsabilidade (Single Responsibility Principle - SRP) é um princípio de design de software que afirma que uma classe ou módulo deve ter apenas uma razão para mudar. Em outras palavras, uma classe deve ter apenas uma tarefa ou responsabilidade.

Este princípio é importante porque ajuda a manter o código mais limpo, mais fácil de manter e mais fácil de testar. Quando uma classe tem mais de uma responsabilidade, essas responsabilidades tornam-se acopladas, o que pode levar a um código frágil e difícil de manter.

### Exemplo Ruim

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_user_info(self):
        return f"Nome: {self.name}, Email: {self.email}"

    def save_user(self):
        pass  # código para salvar o usuário no banco de dados
```

### Exemplo Bom
```python

class Produto:
    def __init__(self, nome, descricao):
        self.nome = nome
        self.descricao = descricao

class ProdutoInfo:
    def get_user_info(self, produto):
        return f"Nome: {produto.nome}, Email: {produto.descricao}"

class userComportamentoSetorVendas:
    def comportamentoSetorVendas(self, produto):

class userComportamentoSetorLogistica:
    def comportamentoSetorLogistica(self, produto):

class userComportamentoSetorArmazenamento:
    def comportamentoSetorArmazenamento(self, produto)


class ProdutoSave:
    def produtoSave(self, produto):
        pass  # código para salvar o produto
```

### Vídeo sobre O Princípio da Responsabilidade Única

[Dev Eficiente - Uma reflexão sobre o Princípio da responsabilidade única](https://youtu.be/GGe0o_v5vjM)

[Otavio Lemos -  Princípio da responsabilidade única](https://youtu.be/fKAf74I3Yas)

#### Pontos citados nos vídeos

* Princípio da coesão
* O autor do vídeo traz uma visão mais crítica sobre o Princípio da Responsabilidade Única, uma vez que pelo seu entendimento a responsabilidade de cada classe é subjetiva

### Referências

* [The Single Responsibility Principle](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)
* [Principles of Software Engineering](https://www.d.umn.edu/~gshute/softeng/principles.html)

### Minhas notas sobre o princípio

O Princípio da Responsabilidade Única pode ser visto como uma divisão de módulos em que mudanças em códigos não devem quebrar códigos não relacionados. Temos como exemplo o código de Python acima. Adicionar novos comportamentos e informações a `UserComportamentoSetorArmazenamento` não vai quebrar as funcionalidades relacionadas a vendas e logística.

## OCP: O Princípio Aberto-Fechado

O Princípio Aberto-Fechado (Open-Closed Principle - OCP) é um dos cinco princípios de design orientado a objetos conhecidos como SOLID. O OCP afirma que "as entidades de software (classes, módulos, funções, etc.) devem estar abertas para extensão, mas fechadas para modificação". Em outras palavras, você deve ser capaz de adicionar novas funcionalidades ou comportamentos a uma entidade sem alterar seu código existente.

Módulos que estão de acordo com o Princípio Aberto-Fechado possuem dois atributos principais:

### Abertos para extensão

Isso significa que o comportamento do módulo pode ser estendido. À medida que os requisitos da aplicação mudam, somos capazes de estender o módulo com novos comportamentos que satisfazem essas mudanças. Em outras palavras, somos capazes de mudar o que o módulo faz.

### Fechados para modificação

Estender o comportamento de um módulo não resulta em alterações no código fonte ou binário do módulo. A versão executável binária do módulo, seja em uma biblioteca linkável, um DLL ou um .jar Java, permanece intocada.

Agora, vamos  a um exemplo de código Python que quebra o OCP :

### Exemplo Ruim

```python
from abc import ABC, abstractmethod

class Produto:
    def __init__(self, nome, descricao):
        self.nome = nome
        self.descricao = descricao

class ProdutoInfo:
    def get_user_info(self, produto):
        return f"Nome: {produto.nome}, Email: {produto.descricao}"

class UserComportamento(ABC):
    @abstractmethod
    def comportamento(self, produto):
        pass

class UserComportamentoSetorVendas(UserComportamento):
    def comportamento(self, produto):
        pass  # implementação específica para o setor de vendas

class UserComportamentoSetorLogistica(UserComportamento):
    def comportamento(self, produto):
        pass  # implementação específica para o setor de logística

class UserComportamentoSetorArmazenamento(UserComportamento):
    def comportamento(self, produto):
        pass  # implementação específica para o setor de armazenamento

class ProdutoSave:
    def produtoSave(self, produto):
        pass  # código para salvar o produto

```

Neste exemplo, a classe `UserComportamento` tem métodos específicos para cada setor. Se quiséssemos adicionar um novo setor, teríamos que modificar a classe `UserComportamento` para adicionar um novo método. Isso viola o OCP porque a classe `UserComportamento` não está fechada para modificação.

Além disso, essa abordagem não é muito flexível. Se quiséssemos adicionar um novo comportamento que é específico para apenas um setor, teríamos que adicionar esse comportamento a todos os setores, mesmo que não seja relevante para eles. Isso pode levar a um código desnecessariamente complexo e difícil de manter.

### Exemplo Bom (Strategy pattern:O cliente é tanto aberto quanto fechado)
Uma maneira de aplicar o OCP seria criar uma interface ou classe abstrata que define um método para o comportamento do usuário, e então estender essa classe para cada setor. Aqui está um exemplo de como você poderia fazer isso em Python:

```python
from abc import ABC, abstractmethod

class Produto:
    def __init__(self, nome, descricao):
        self.nome = nome
        self.descricao = descricao

class ProdutoInfo:
    def get_user_info(self, produto):
        return f"Nome: {produto.nome}, Email: {produto.descricao}"

class UserComportamento(ABC):
    @abstractmethod
    def comportamento(self, produto):
        pass

class UserComportamentoSetorVendas(UserComportamento):
    def comportamento(self, produto):
        pass  # implementação específica para o setor de vendas

class UserComportamentoSetorLogistica(UserComportamento):
    def comportamento(self, produto):
        pass  # implementação específica para o setor de logística

class UserComportamentoSetorArmazenamento(UserComportamento):
    def comportamento(self, produto):
        pass  # implementação específica para o setor de armazenamento

class ProdutoSave:
    def produtoSave(self, produto):
        pass  # código para salvar o produto
```

Nesta versão refatorada, criamos uma classe abstrata `UserComportamento` com um método abstrato `comportamento()`. Cada setor tem sua própria classe que estende `UserComportamento` e implementa o método `comportamento()`. Agora, cada classe `UserComportamentoSetorX` está aberta para extensão (podemos adicionar novos comportamentos específicos do setor), mas fechada para modificação (não precisamos alterar a classe `UserComportamento` ou qualquer outra classe `UserComportamentoSetorX` existente para adicionar um novo comportamento).

Ao seguir o Princípio Aberto-Fechado, podemos tornar nosso código mais flexível e fácil de manter.

### Referências / leituras
* [SOLID: OCP - Open/Closed Principle (Princípio do Aberto/Fechado - André Secco)](https://youtu.be/UlSNpWFTU3Q)
* [SOLID #2: Princípio do aberto/fechado - (Dev Eficiente)](https://youtu.be/UlSNpWFTU3Q)
* [The Open Closed Principle](https://blog.cleancoder.com/uncle-bob/2014/05/12/TheOpenClosedPrinciple.html)
* [Open/Closed Principle: objetos flexíveis](https://github.com/caelum/apostila-oo-avancado-em-java/blob/master/05-open-closed-principle.md)
* [Princípio aberto/fechado Otavio Lemos](https://youtu.be/hKQgL-RSgRw)


## LSP: O Princípio de Substituição de Liskov

O Princípio de Substituição de Liskov (LSP, do inglês Liskov Substitution Principle) é um dos cinco princípios do SOLID, um conjunto de práticas de programação orientada a objetos que visa melhorar a manutenção e extensibilidade do código.

Este princípio foi introduzido por Barbara Liskov em 1987 e afirma que, em um programa de computador, se S é um subtipo de T, então os objetos de tipo T podem ser substituídos pelos objetos de tipo S (ou seja, os objetos de tipo S podem substituir os objetos de tipo T) sem alterar as propriedades desejáveis desse programa (corretude, tarefas executadas, etc.).

Por exemplo, se você tem uma classe "Animal" com uma função "falar", e tem uma subclasse "Cão" que também tem a função "falar", então de acordo com o LSP, você deveria poder usar a classe "Cão" em qualquer lugar que você usaria a classe "Animal" e o programa ainda funcionaria corretamente.

Se a substituição do "Animal" pelo "Cão" causa algum comportamento inesperado, então o código está violando o Princípio de Substituição de Liskov. A importância deste princípio é que ele permite a polimorfismo e a reutilização de código sem surpresas.

```python
from abc import ABC, abstractmethod

class Comportamento(ABC):
    @abstractmethod
    def executar(self, produto):
        pass
class ComportamentoSetorVendas(Comportamento):
    def executar(self, produto):
        return f"Vendendo {produto.nome}."

class ComportamentoSetorLogistica(Comportamento):
    def executar(self, produto):
        return f"Logística para {produto.nome}."

class ComportamentoSetorArmazenamento(Comportamento):
    def executar(self, produto):
        return f"Armazenando {produto.nome}."

class Produto:
    def __init__(self, nome, descricao):
        self.nome = nome
        self.descricao = descricao

class ProdutoInfo:
    def __init__(self, comportamento):
        self.comportamento = comportamento

    def get_info(self, produto):
        return f"Nome: {produto.nome}, Descrição: {produto.descricao}, Comportamento: {self.comportamento.executar(produto)}"
```
### 1. Classe Comportamento

A classe Comportamento é uma classe abstrata, que serve como uma espécie de "modelo" para outras classes. Essa classe define um método chamado executar, mas não fornece uma implementação para esse método - em vez disso, ela espera que as classes que herdam dela forneçam essa implementação. Esse é um conceito fundamental em programação orientada a objetos chamado "polimorfismo".

### 2. Classes ComportamentoSetorVendas, ComportamentoSetorLogistica e ComportamentoSetorArmazenamento

Essas são subclasses da classe Comportamento. Cada uma dessas classes fornece sua própria implementação do método executar. Cada implementação é única para a classe em que é definida. Por exemplo, a classe ComportamentoSetorVendas implementa executar de uma forma que indica que o produto está sendo vendido, enquanto a classe ComportamentoSetorLogistica implementa executar de uma forma que indica que o produto está sendo preparado para logística.

### 3. Classe Produto

A classe Produto é simplesmente uma classe que representa um produto. Ela tem dois atributos: nome e descricao.

### 4. Classe ProdutoInfo

A classe ProdutoInfo é uma classe que usa os objetos de Produto e Comportamento. Ela tem um método chamado get_info, que retorna uma string contendo o nome do produto, sua descrição, e o resultado de executar o comportamento.

Na prática, isso permite que você crie objetos ProdutoInfo que possuem diferentes comportamentos, e você pode chamar o método get_info para ver como esses diferentes comportamentos afetam o produto.

### Como isso se relaciona com o Princípio de Substituição de Liskov?

O Princípio de Substituição de Liskov está relacionado ao polimorfismo, que é a capacidade de usar um objeto de uma subclasse onde um objeto da superclasse é esperado. No exemplo que fornecemos, qualquer objeto que seja uma instância de uma subclasse de Comportamento (ou seja, ComportamentoSetorVendas, ComportamentoSetorLogistica ou ComportamentoSetorArmazenamento) pode ser usado onde um objeto Comportamento é esperado.

Em outras palavras, um objeto ComportamentoSetorVendas pode ser usado em lugar de um objeto Comportamento sem quebrar a aplicação. Isso é uma demonstração do Princípio de Substituição de Liskov.

### Referências / leituras
* [A Behavioral Notion of Subtyping](https://www.csnell.net/computerscience/Liskov_subtypes.pdf)
* [Applying “Design by Contract](https://pages.mtu.edu/~aebnenas/teaching/spring2010/cs3141/readings/meyerPDF.pdf)
* [SOLID #3: Princípio de substituição de Liskov - (Dev Eficiente)](https://youtu.be/MiV_tI3fNPQ)
