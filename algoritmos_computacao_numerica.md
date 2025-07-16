# Algoritmos de Métodos Numéricos

## Raiz, Interpolação, Integração, Derivada e Problemas de Valor Inicial

## Bisseção

 O código implementa o método da bisseção para encontrar a raiz de uma função.
 Ele funciona da seguinte maneira:

 1. **Inicialização**:
    - Define uma tolerância que determina a precisão desejada para a raiz.
    - Define os valores iniciais `esq` e `dir` que delimitam o intervalo onde a raiz é esperada.
    - Define a função `f(t)` que representa a equação cuja raiz queremos encontrar.

 2. **Função `bissecao(esq, dir)`**:
    - Inicializa um contador de iterações.
    - Verifica se a raiz já está em uma das extremidades do intervalo (esq ou dir).
    - Entra em um loop que continua até que a tolerância seja atingida:
        - Calcula o ponto médio `meio` do intervalo.
        - Avalia a função `f` no ponto médio.
        - Se o valor absoluto de `f(meio)` for menor que a tolerância, a raiz foi encontrada e o loop é interrompido.
        - Caso contrário, o intervalo é ajustado:
            - Se `f(esq) * f(meio)` for negativo, a raiz está no intervalo [esq, meio], então `dir` é atualizado para `meio`.
            - Caso contrário, a raiz está no intervalo [meio, dir], então `esq` é atualizado para `meio`.

 **Vantagens do método da bisseção**:
 - Simplicidade e fácil implementação.
 - Garantia de convergência para funções contínuas que mudam de sinal no intervalo inicial.
 - Robustez, pois não é sensível a oscilações na função.

 **Desvantagens do método da bisseção:**
 - Convergência relativamente lenta em comparação com outros métodos, como o método de Newton-Raphson.
 - Requer o conhecimento prévio de um intervalo que contenha a raiz.
 - Pode não encontrar todas as raízes se a função tiver múltiplas raízes dentro do intervalo.



```python
# RAIZ ENTRE VALORES INICIAIS

tolerancia = 0.01
esq = 0.001
dir = 0.021


def f(t):
    return (50000 * t - (50000 * (1 / 60 + 7.5 / 1200))) * ((1 + t) ** 60) + (50000 * (1 / 60 + 7.5 / 1200))


def bissecao(esq, dir):
    iteracoes = 0
    if f(esq) == 0:
        return esq
    if f(dir) == 0:
        return dir

    while True:
        iteracoes += 1
        meio = (esq + dir) / 2

        if abs(f(meio)) < tolerancia:
            return meio, iteracoes
        if (f(esq) * f(meio)) < 0:
            dir = meio
        else:
            esq = meio
    return meio, iteracoes


bissecao(esq, dir)

```




    (0.011098876953125, 14)




```python

```

## Newton

O código implementa o método de Newton-Raphson para encontrar a raiz de uma função.
 Ele funciona da seguinte maneira:

 1. **Inicialização**:
    - Define a função `f(x)` que representa a equação cuja raiz queremos encontrar.
    - Define a função `fDerivada(x)` que calcula a derivada da função `f(x)`.
    - Define a função `newtonRaphson(valorInicial, maxIteracoes=100)` que recebe o valor inicial da iteração e o número máximo de iterações (opcional, com valor padrão de 100).

 2. **Função `newtonRaphson(valorInicial, maxIteracoes)`**:
    - Inicializa a variável `xAtual` com o valor inicial.
    - Inicializa um contador de iterações.
    - Entra em um loop que continua até que o número máximo de iterações seja atingido ou a tolerância seja atingida:
        - Calcula o próximo valor de `xAtual` usando a fórmula de Newton-Raphson:
            xAtual = xAtual - f(xAtual) / fDerivada(xAtual)
        - Avalia a função `f` no novo valor de `xAtual`.
        - Se o valor absoluto de `f(xAtual)` for menor que a tolerância, a raiz foi encontrada e o loop é interrompido.

 **Vantagens do método de Newton-Raphson**:
 - Convergência rápida para a raiz, geralmente quadrática, o que significa que o número de dígitos corretos dobra a cada iteração.
 - Eficiente em termos de número de iterações necessárias para encontrar a raiz.

 **Desvantagens do método de Newton-Raphson**:
 - Requer o cálculo da derivada da função, o que pode ser complicado ou impossível em alguns casos.
 - Pode não convergir se o valor inicial estiver muito longe da raiz ou se a derivada da função for zero ou próxima de zero em algum ponto próximo da raiz.
 - Pode ser sensível à escolha do valor inicial, convergindo para diferentes raízes ou divergindo dependendo do valor escolhido.



```python
def f:
    return (50000 * x - (50000 * (1 / 60 + 7.5 / 1200))) * ((1 + x) ** 60) + (50000 * (1 / 60 + 7.5 / 1200))


def fDerivada(x):
    return 50000 * ((1 + x) ** 60) + (50000 * x - (50000 * (1 / 60 + 7.5 / 1200))) * 60 * (1 + x) ** (59)


def newtonRaphson(valorInicial, maxIteracoes=100):
    xAtual = valorInicial

    iteracoes = 0

    for iteracoes in range(maxIteracoes):
        iteracoes += 1

        xAtual = xAtual - f(xAtual) / fDerivada(xAtual)

        if abs(f(xAtual)) < tolerancia:
            return xAtual, iteracoes


newtonRaphson(0.075)

```




    (0.011099113445644168, 10)



## Falsa Posição

 O método da falsa posição, também conhecido como método da regula falsi, é um método numérico iterativo usado para encontrar raízes de uma função.
 Ele é uma variação do método da bisseção, mas em vez de simplesmente dividir o intervalo de busca ao meio, ele usa uma interpolação linear para estimar a posição da raiz.

 **Funcionamento**:

 1. Inicialização: O método começa com dois valores iniciais, `esq` e `dir`, que delimitam um intervalo que contém a raiz da função.
 2. Interpolação Linear: Uma reta é traçada entre os pontos (esq, f(esq)) e (dir, f(dir)). O ponto onde essa reta cruza o eixo x é a estimativa da raiz, chamada de `pontoFalsaPosicao`.
 3. Atualização do Intervalo: O intervalo de busca é atualizado com base no sinal de f(pontoFalsaPosicao):
     - Se f(esq) * f(pontoFalsaPosicao) < 0, a raiz está entre `esq` e `pontoFalsaPosicao`. O valor de `dir` é atualizado para `pontoFalsaPosicao`.
     - Caso contrário, a raiz está entre `pontoFalsaPosicao` e `dir`. O valor de `esq` é atualizado para `pontoFalsaPosicao`.
 4. Critério de Parada: O processo iterativo continua até que o valor absoluto de f(pontoFalsaPosicao) seja menor que uma tolerância predefinida.

 **Vantagens**:

 - Convergência mais rápida que o método da bisseção em muitos casos.
 - Robusto, pois garante a convergência para uma raiz dentro do intervalo inicial.

 **Desvantagens**:

 - Pode convergir mais lentamente que outros métodos, como o método de Newton-Raphson, em algumas situações.
 - A convergência pode ser lenta se a função tiver uma curvatura acentuada perto da raiz.



```python
# RAIZ ENTRE VALORES INICIAIS

tolerancia = 0.01
esq = 0.001
dir = 0.021



def f(x):
  return (50000 * x - (50000 * (1 / 60 + 7.5 / 1200))) * ((1 + x) ** 60) + (50000 * (1 / 60 + 7.5 / 1200))



def falsaPosicao(esq, dir):
  iteracoes = 0

  if f(esq) == 0:
    return esq
  if f(dir) == 0:
    return dir

  while True:
    iteracoes += 1

    pontoFalsaPosicao = ((esq * f(dir)) - (dir * f(esq))) / (f(dir) - f(esq))

    if abs(f(pontoFalsaPosicao)) < tolerancia:
      break

    if (f(esq) * f(pontoFalsaPosicao)) < 0:
      dir = pontoFalsaPosicao
    else:
      esq = pontoFalsaPosicao

  return pontoFalsaPosicao, iteracoes

falsaPosicao(esq, dir)
```




    (0.011098797261285829, 30)



## Secante

O método da secante é um método numérico iterativo usado para encontrar raízes de uma função.
 Ele é semelhante ao método de Newton-Raphson, mas em vez de exigir a derivada da função, ele usa uma aproximação da derivada calculada a partir de dois pontos anteriores.

 **Funcionamento**:

 1. Inicialização: O método começa com dois valores iniciais, `inicial1` e `inicial2', que são estimativas iniciais da raiz.
 2. Aproximação da Derivada: A derivada da função em `xAtual` é aproximada usando a fórmula da secante:
    `f'(xAtual) ≈ (f(xAtual) - f(xAnterior)) / (xAtual - xAnterior)`
 3. Atualização da Estimativa: A próxima estimativa da raiz, `xAtual`, é calculada usando a fórmula:
    `xAtual = xAtual - f(xAtual) / f'(xAtual)`
 4. Critério de Parada: O processo iterativo continua até que o valor absoluto de `f(xAtual)` seja menor que uma tolerância predefinida ou o número máximo de iterações seja atingido.

 **Vantagens**:

 - Não requer o cálculo explícito da derivada da função, o que pode ser útil quando a derivada é difícil ou impossível de calcular.
 - Convergência rápida, geralmente superlinear, o que significa que o número de dígitos corretos aumenta mais rapidamente que linearmente a cada iteração.

 **Desvantagens**:

 - Pode não convergir se os valores iniciais estiverem muito longe da raiz ou se a função tiver um comportamento irregular perto da raiz.
 - A convergência pode ser mais lenta que o método de Newton-Raphson em algumas situações.
 - Requer duas estimativas iniciais, o que pode ser um problema em alguns casos.



```python
# RAIZ ENTRE VALORES INICIAIS

tolerancia = 0.01
inicial1 = 0.05
inicial2 = 0.3


def f(t):
    return (50000 * t - (50000 * (1 / 60 + 7.5 / 1200))) * ((1 + t) ** 60) + (50000 * (1 / 60 + 7.5 / 1200))


def secante(inicial1, inicial2, maxIteracoes=100):
    xAnterior = inicial1
    xAtual = inicial2

    iteracoes = 0

    for iteracoes in range(maxIteracoes):

        iteracoes += 1
        temp = xAtual
        xAtual = xAtual - (f(xAtual) / ((f(xAtual) - f(xAnterior)) / (xAtual - xAnterior)))
        xAnterior = temp

        if abs(f(xAtual)) < tolerancia:
            return xAtual, iteracoes

    print("divergiu")
    return xAtual, iteracoes


secante(inicial1, inicial2)

```




    (0.011099161440244695, 13)



# Interpolação

## Newton


 O método acima implementa a interpolação polinomial de Newton, que é uma técnica para encontrar um polinômio que passa por um conjunto de pontos dados.

 **Funcionamento detalhado**:

 1. Cálculo das Diferenças Divididas:
    - A função `diferencasDivididas` calcula as diferenças divididas, que são coeficientes usados na fórmula de interpolação de Newton.
    - Ela recebe duas listas: `listaValoresX` (valores de x dos pontos) e `listaValoresY` (valores de y correspondentes).
    - Cria uma tabela (matriz numpy) para armazenar as diferenças divididas.
    - A primeira coluna da tabela é preenchida com os valores de `listaValoresY`.
    - As demais colunas são calculadas recursivamente usando a fórmula das diferenças divididas:
        `tabelaDif[i][j] = (tabelaDif[i + 1][j - 1] - tabelaDif[i][j - 1]) / (listaValoresX[i + j] - listaValoresX[i])`
    - A função retorna a primeira linha da tabela, que contém os coeficientes para o polinômio interpolador.

 2. Construção do Polinômio Interpolador:
    - A função `newton` recebe as listas `listaValoresX` e `listaValoresY`.
    - Chama a função `diferencasDivididas` para obter os coeficientes.
    - Inicializa o polinômio interpolador com o primeiro coeficiente (termo constante).
    - Itera pelos demais coeficientes, construindo o polinômio termo a termo:
        - Cria um termo polinomial inicializado com 1.
        - Multiplica o termo por fatores lineares da forma (x - xi), onde xi são os valores de x dos pontos.
        - Adiciona o termo multiplicado pelo coeficiente correspondente ao polinômio interpolador.
    - Retorna o polinômio interpolador como um objeto `numpy.poly1d`.

 3. Exemplo de Uso:
    - Define as listas `listaX` e `listaY` com os pontos (1, 1), (2, 4) e (3, 9).
    - Chama a função `newton` para obter o polinômio interpolador.
    - Avalia o polinômio em x = 10 usando `result = newton(listaX, listaY)(10)`.
    - Imprime o resultado.

 Vantagens da Interpolação de Newton:
 - Fácil de entender e implementar.
 - Eficiente para adicionar novos pontos de dados, pois apenas as diferenças divididas adicionais precisam ser calculadas.
 - Permite a avaliação do polinômio interpolador em qualquer ponto dentro do intervalo dos dados.

 Desvantagens da Interpolação de Newton:
 - Pode sofrer do fenômeno de Runge, onde oscilações indesejadas podem ocorrer perto das extremidades do intervalo de interpolação, especialmente para conjuntos de dados com muitos pontos.
 - Pode ser numericamente instável para grandes conjuntos de dados.



```python
import numpy as np


def diferencasDivididas(listaValoresX, listaValoresY):
    numDePontos = len(listaValoresX)
    tabelaDif = np.zeros((numDePontos, numDePontos))
    tabelaDif[:, 0] = listaValoresY

    for j in range(1, numDePontos):
        for i in range(numDePontos - j):
            tabelaDif[i][j] = (tabelaDif[i + 1][j - 1] - tabelaDif[i][j - 1]) / (
                        listaValoresX[i + j] - listaValoresX[i])

    return tabelaDif[0, :]


def newton(listaValoresX, listaValoresY):
    listaCoeficientes = diferencasDivididas(listaValoresX, listaValoresY)
    numDePontos = len(listaValoresX)

    polinomio = np.poly1d([listaCoeficientes[0]])

    for i in range(1, numDePontos):
      termo = np.poly1d([1])
      for k in range(i):
        termo *= np.poly1d([1, -listaValoresX[k]])

      polinomio += listaCoeficientes[i] * termo
    return polinomio



listaX = [1, 2, 3]
listaY = [1, 4, 9]

result = newton(listaX, listaY)(10)
print(result)

```

    100.0
    

## Lagrange

 O código implementa o método de interpolação de Lagrange para encontrar um polinômio que passa por um conjunto de pontos dados.
 Ele funciona da seguinte maneira:

 1. **Inicialização**:
    - Define a função `lagrange(listaValoresX, listaValoresY)` que recebe duas listas:
      - `listaValoresX` contém as coordenadas x dos pontos.
      - `listaValoresY` contém as coordenadas y correspondentes.

 2. **Função `lagrange(listaValoresX, listaValoresY)`**:
    - Calcula o número de pontos `numDePontos`.
    - Inicializa um polinômio `polinomio` como zero.
    - Entra em um loop que itera sobre cada ponto:
        - Inicializa um polinômio base `polinomioBase` como 1.
        - Entra em um loop interno que itera sobre todos os outros pontos:
            - Se o ponto atual do loop externo for diferente do ponto atual do loop interno,
              multiplica `polinomioBase` por um polinômio linear que se anula no ponto atual do loop interno
              e é igual a 1 no ponto atual do loop externo.
        - Multiplica o polinômio base `polinomioBase` pelo valor y correspondente ao ponto atual do loop externo
          e adiciona o resultado ao polinômio `polinomio`.

 3. Retorno do Polinômio:
    - A função retorna o polinômio `polinomio` que interpola os pontos dados.

 4. Exemplo de Uso:
    - Define as listas `listaX` e `listaY` com as coordenadas dos pontos.
    - Chama a função `lagrange` com essas listas para obter o polinômio interpolador.
    - Avalia o polinômio no ponto x = 5 e imprime o resultado.

 Vantagens do método de interpolação de Lagrange:
 - Fácil de entender e implementar.
 - Fornece uma solução exata para interpolar um conjunto de pontos.

 Desvantagens do método de interpolação de Lagrange:
 - Pode ser computacionalmente caro para um grande número de pontos.
 - Pode sofrer do fenômeno de Runge, onde a interpolação oscila perto das extremidades do intervalo para um grande número de pontos.
 - Adicionar um novo ponto requer o recálculo de todo o polinômio.



```python
import numpy as np

def lagrange(listaValoresX, listaValoresY):
  numDePontos = len(listaValoresX)

  polinomio = np.poly1d(0.0)

  for i in range(numDePontos):
    polinomioBase = np.poly1d(1.0)
    for j in range(numDePontos):
      if i != j:
        polinomioBase *= np.poly1d([1.0, -listaValoresX[j]]) / (listaValoresX[i] - listaValoresX[j])

    polinomio += listaValoresY[i]*polinomioBase

  return polinomio


listaX = [1, 2, 3]
listaY = [1, 4, 9]

result = lagrange(listaX, listaY)(5)
print(result)
```

    25.0
    

# Integração

## Trapézio

 O método do trapézio é um método numérico para aproximar a integral definida de uma função.
 Ele funciona dividindo a área sob a curva da função em um número finito de trapézios e somando as áreas desses trapézios para obter uma aproximação da integral.

 Funcionamento:

 1. Divisão do Intervalo: O intervalo de integração `[a, b]` é dividido em n subintervalos de igual largura, denotada por h.
 2. Aproximação por Trapézios: Em cada subintervalo, a área sob a curva é aproximada pela área de um trapézio. A área de cada trapézio é calculada como a média das alturas das bases (valores da função nos extremos do subintervalo) multiplicada pela largura do subintervalo.
 3. Soma das Áreas: As áreas de todos os trapézios são somadas para obter uma aproximação da integral definida.

 **Fórmula**:

 A integral definida de `f(x)` de `a` a `b` é aproximada por:

 `∫[a,b] f(x) dx ≈ h/2 * [f(a) + 2f(a+h) + 2f(a+2h) + ... + 2f(b-h) + f(b)]`

 onde `h = (b-a)/n` é a largura de cada subintervalo.

 **Vantagens**:

 - Simplicidade e fácil implementação.
 - Convergência relativamente rápida para funções suaves.
 - Pode ser usado para aproximar integrais de funções que não possuem uma forma analítica conhecida.

 **Desvantagens**:

 - A precisão da aproximação depende do número de subintervalos utilizados. Quanto maior o número de subintervalos, mais precisa será a aproximação, mas também mais tempo de computação será necessário.
 - Pode não ser muito preciso para funções com alta variação ou com descontinuidades.

 Em resumo, o método do trapézio é uma técnica útil para aproximar integrais definidas, especialmente quando a função não possui uma forma analítica conhecida. No entanto, é importante estar ciente de suas limitações e escolher o número de subintervalos apropriado para obter a precisão desejada.



```python
def f(x):
  return x**2 + 2*x

def metodoTrapezio(inicio, fim, largura):
  baseTrapezio = (f(inicio) + f(fim)) / 2
  return baseTrapezio * largura

def trapezioExtendido(inicio, fim, numDeDivisoes):
  larguraDivisao = abs(fim - inicio) / numDeDivisoes
  xAtual = inicio
  xProx = xAtual + larguraDivisao
  areaTotal = 0

  for i in range(numDeDivisoes):
    areaTotal += metodoTrapezio(xAtual, xProx, larguraDivisao)
    xAtual = xProx
    xProx += larguraDivisao

  return areaTotal

trapezioExtendido(0, 10, 5)
```




    440.0



## Simpson

 O método de Simpson é um método numérico para aproximar a integral definida de uma função.
 Ele funciona aproximando a função por uma série de parábolas e somando as áreas sob essas parábolas para obter uma aproximação da integral.

 **Funcionamento**:

 1. Divisão do Intervalo: O intervalo de integração `[a, b]` é dividido em um número par de subintervalos de igual largura, denotada por h.
 2. Aproximação por Parábolas: Em cada par de subintervalos adjacentes, a função é aproximada por uma parábola que passa pelos três pontos correspondentes aos extremos e ao ponto médio dos subintervalos. A área sob cada parábola é calculada usando uma fórmula específica.
 3. Soma das Áreas: As áreas sob todas as parábolas são somadas para obter uma aproximação da integral definida.

 Fórmula:

 A integral definida de `f(x)` de a a b é aproximada por:

 `∫[a,b] f(x) dx ≈ h/3 * [f(a) + 4f(a+h) + 2f(a+2h) + 4f(a+3h) + ... + 2f(b-2h) + 4f(b-h) + f(b)]`

 onde `h = (b-a)/n` é a largura de cada subintervalo e n é um número par.

 **Vantagens**:

 - Maior precisão que o método do trapézio para a mesma quantidade de subintervalos, pois aproxima a função por parábolas em vez de retas.
 - Convergência rápida para funções suaves.
 - Pode ser usado para aproximar integrais de funções que não possuem uma forma analítica conhecida.

 **Desvantagens**:

 - Requer um número par de subintervalos.
 - A precisão da aproximação depende do número de subintervalos utilizados. Quanto maior o número de subintervalos, mais precisa será a aproximação, mas também mais tempo de computação será necessário.
 - Pode não ser muito preciso para funções com alta variação ou com descontinuidades.

 Em resumo, o método de Simpson é uma técnica eficiente e precisa para aproximar integrais definidas, especialmente quando a função é suave. Ele oferece uma melhoria em relação ao método do trapézio em termos de precisão, mas requer um número par de subintervalos.



```python
def f(x):
  return x**2 + 2*x

def metodoSimpson(inicio, fim):
  intervalo = (inicio - fim) / 2
  y = f(inicio) + 4 * f(inicio + intervalo) + f(fim)
  return intervalo / 3 * y

def simpsonExtendido(inicio, fim, numDeDivisoes):
  if numDeDivisoes % 2 != 0:
    numDeDivisoes += 1

  intervalo = abs(fim - inicio) / numDeDivisoes
  soma = f(inicio) + f(fim)

  for i in range(1, numDeDivisoes):
    xi = inicio + intervalo * i
    if i % 2 == 0:
      soma += 2 * f(xi)
    else:
      soma += 4 * f(xi)

  return intervalo / 3 * soma

simpsonExtendido(0, 10, 5)
```




    433.33333333333337



# Derivada

## Centrada


```python
def derivadaCentrada(x, intervalo):
  xMenor = f(x - intervalo)
  xMaior = f(x + intervalo)
  return (xMaior - xMenor) / (2 * intervalo)
```

## Atrasada



```python
def derivadaAtrasada(x, intervalo):
  xMenor = f(x - intervalo)
  return (f(x) - xMenor) / intervalo
```

## Avançada



```python
def derivadaCentrada(x, intervalo):
  xMaior = f(x + intervalo)
  return (xMaior - f(x)) / intervalo
```

# Problema de Valor Inicial

## Euler

 O método de Euler é um método numérico de primeira ordem para aproximar a solução de uma equação diferencial ordinária (EDO) com um valor inicial dado.
 Ele é um método explícito, o que significa que o valor da solução no próximo ponto no tempo é calculado diretamente a partir do valor no ponto atual.

 Funcionamento:
 1. Inicialização: O método começa com um valor inicial y(t_0) = y_0 e um passo de tempo h.
 2. Aproximação da Derivada: A derivada da solução em um ponto t_i é aproximada usando a função f(t_i, y_i) da EDO.
 3. Cálculo do Próximo Valor: O valor da solução no próximo ponto no tempo t_{i+1} = t_i + h é aproximado usando a fórmula:
    y_{i+1} = y_i + h * f(t_i, y_i)
 4. Iteração: O processo é repetido para calcular a solução em pontos subsequentes no tempo.

 Vantagens:
 - Simplicidade e fácil implementação.
 - Útil para obter uma primeira aproximação da solução de uma EDO.

 Desvantagens:
 - Baixa precisão, especialmente para passos de tempo grandes.
 - Pode ser instável para algumas EDOs, levando a soluções que divergem da solução real.

 Em resumo, o método de Euler é um método simples e fácil de implementar para aproximar a solução de uma EDO. No entanto, sua baixa precisão e potencial instabilidade o tornam inadequado para muitas aplicações práticas, especialmente quando alta precisão é necessária.
def fDerivada(x, t):
  return t - 3*x

def euler(yInicial, tInicial, tamDoPasso, numDePassos):
  y = yInicial
  t = tInicial

  for i in range(numDePassos):
    t = tInicial + i * tamDoPasso
    y = y + tamDoPasso*fDerivada(y, t)

  return y
result = euler(1, 0, 0.2, 10)
print(result)



```python
def fDerivada(x, t):
  return t - 3*x

def euler(yInicial, tInicial, tamDoPasso, numDePassos):
  y = yInicial
  t = tInicial

  for i in range(numDePassos):
    t = tInicial + i * tamDoPasso
    y = y + tamDoPasso*fDerivada(y, t)

  return y
result = euler(1, 0, 0.2, 10)
print(result)
```

    0.555672064
    

## Euler Melhorado (Runge-Kutta 2)

 O método de Euler Melhorado, também conhecido como método de Runge-Kutta de segunda ordem,
 é um método numérico para aproximar a solução de uma equação diferencial ordinária (EDO) com um valor inicial dado.
 Ele é uma melhoria em relação ao método de Euler padrão, oferecendo maior precisão e estabilidade.

 Funcionamento:

 1. Inicialização: O método começa com um valor inicial y(t_0) = y_0 e um passo de tempo h.

 2. Previsão de Euler: Uma estimativa inicial da solução no próximo ponto no tempo t_{i+1} = t_i + h
    é calculada usando o método de Euler padrão:
    y_{i+1}^* = y_i + h * f(t_i, y_i)

 3. Correção da Inclinação: A inclinação da solução no ponto médio do intervalo [t_i, t_{i+1}] é calculada usando a função f(t_{i+1}, y_{i+1}^*)
    obtida na previsão de Euler.

 4. Cálculo do Próximo Valor: O valor da solução no próximo ponto no tempo t_{i+1} é então calculado usando a média das inclinações
    no início e no ponto médio do intervalo:
    y_{i+1} = y_i + h/2 * [f(t_i, y_i) + f(t_{i+1}, y_{i+1}^*)]

 5. Iteração: O processo é repetido para calcular a solução em pontos subsequentes no tempo.

 Vantagens:

 - Maior precisão em relação ao método de Euler padrão, pois leva em consideração a inclinação da solução no ponto médio do intervalo.
 - Maior estabilidade para algumas EDOs, reduzindo o risco de soluções divergentes.
 - Fácil implementação, com apenas uma etapa adicional em relação ao método de Euler padrão.

 Desvantagens:

 - Ainda pode apresentar erros de aproximação, especialmente para passos de tempo grandes.
 - Pode ser menos preciso que métodos de ordem superior, como o método de Runge-Kutta de quarta ordem, para algumas EDOs.

 Em resumo, o método de Euler Melhorado é uma técnica numérica eficaz para aproximar a solução de EDOs, oferecendo um bom equilíbrio entre precisão, estabilidade e facilidade de implementação.
 Ele é uma escolha popular para muitas aplicações práticas, especialmente quando o método de Euler padrão não oferece precisão suficiente.



```python
def fDerivada(x, t):
  return t - 3*x

def eulerMelhorado(yInicial, tInicial, tamDoPasso, numDePassos):
  y = yInicial
  t = tInicial

  for i in range(numDePassos):
    t += tamDoPasso
    y = y + (tamDoPasso / 2)*(fDerivada(y-tamDoPasso, t-tamDoPasso) + fDerivada(y, t))

  return y



result = eulerMelhorado(1, 0, 0.2, 10)
print(result)
```

    0.6889914163199999
    
