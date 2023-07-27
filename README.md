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
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_user_info(self):
        return f"Nome: {self.name}, Email: {self.email}"

class UserDB:
    def save_user(self, user):
        pass  # código para salvar o usuário no banco de dados
```
