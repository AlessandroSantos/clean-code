# Clean Code Notes

## Table of contents

- [Capítulo 1 - Código Limpo](#chapter1)
- [Capítulo 2 - Nomes Significativos](#chapter2)
- [Capítulo 3 - Funções](#chapter3)
- [Chapter 4 - Comentários](#chapter4)
- [Chapter 5 - Formatting](#chapter5)
- [Chapter 6 - Objects and Data Structures](#chapter6)
- [Chapter 7 - Error Handling](#chapter7)
- [Chapter 8 - Boundaries](#chapter8)
- [Chapter 9 - Unit Tests](#chapter9)
- [Chapter 10 - Classes](#chapter10)
- [Chapter 11 - Systems](#chapter11)
- [Chapter 12 - Emergence](#chapter12)
- [Chapter 13 - Concurrency](#chapter13)
- [Chapter 14 - Successive Refinement](#chapter14)
- [Chapter 15 - JUnit Internals](#chapter15)
- [Chapter 16 - Refactoring SerialDate](#chapter15)
- [Chapter 17 - Smells and Heuristics](#chapter17)

<a name="chapter1">
<h1>Capítulo 1 -  Código Limpo</h1>
</a>
Este livro é sobre boa programação. É sobre como escrever um bom código e como transformar um código ruim em um bom código.

O código representa o detalhe dos requisitos e os detalhes não podem ser ignorados ou abstraídos. Podemos criar linguagens mais próximas dos requisitos. Podemos criar ferramentas que nos ajudem a analisar e montar esses requisitos em estruturas formais. Mas nunca eliminaremos a precisão necessária.

### Por que escrever código ruim?

- Você está com pressa?
- Você tenta ir "rápido"?
- Você não tem tempo para fazer um bom trabalho?
- Está cansado de trabalhar no mesmo programa/módulo?
- Seu chefe te pressiona para terminar logo?

Os argumentos anteriores podem criar um pântano de código sem sentido.

Se você disser "Voltarei para consertar mais tarde", você pode cair na [lei de LeBlanc](https://en.wikipedia.org/wiki/Talk%3AList_of_eponymous_laws#Proposal_to_add_LeBlanc.27s_law) "Mais tarde é igual a nunca"

Você é um profissional e o código é de sua responsabilidade. Vamos analisar a seguinte anedota:

> E se você fosse um médico e tivesse um paciente que exigisse que você parasse de lavar as mãos na preparação para a cirurgia porque estava demorando muito? Claramente, o paciente é o chefe; e, no entanto, o médico deve se recusar absolutamente a obedecer. Por que? Porque o médico sabe mais do que o paciente sobre os riscos de doenças e infecções. Seria antiprofissional (muito menos criminoso) o médico concordar com o paciente.

Da mesma forma, não é profissional que os programadores se submetam à vontade de gerentes que não entendem os riscos de fazer bagunça.

Talvez em algum momento você pense em ir rápido para cumprir o prazo. A única maneira de ir rápido é manter o código o mais limpo possível o tempo todo.

### O que é Código Limpo?

Cada programador experiente tem sua própria definição de código limpo, mas algo é claro, um código limpo é um código que você pode ler facilmente. O código limpo é o código que foi cuidado.

Em seu livro, o tio Bob diz o seguinte:

> Considere este livro uma descrição da "Object Mentor School of Clean Code". As técnicas e ensinamentos contidos são a forma como praticamos nossa arte. Estamos dispostos a afirmar que, se você seguir esses ensinamentos, desfrutará dos benefícios que desfrutamos e aprenderá a escrever códigos limpos e profissionais. Mas não cometa o erro de pensar que estamos de alguma forma “certos” em qualquer sentido absoluto. Existem outras escolas e outros mestres que reivindicam tanto profissionalismo quanto nós. Caberia a você aprender com eles também.

### A regra do escoteiro

Não basta escrever bem o código. O código deve ser mantido limpo ao longo do tempo. Todos nós vimos o código apodrecer e degradar com o passar do tempo. Portanto, devemos assumir um papel ativo na prevenção dessa degradação.

É uma boa prática aplicar a [Regra do Escoteiro](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule)

> Sempre deixe o acampamento mais limpo do que o encontrou.

<a name="chapter2">
<h1>Capítulo 2 - Nomes Significativos</h1>
</a>

Os nomes estão por toda parte no software. Arquivos, diretórios, variáveis, funções, etc. Porque fazemos muito disso, é melhor fazê-lo bem.

### Use nomes reveladores de intenções

É fácil dizer que nomes revelam intenções. Escolher bons nomes leva tempo, mas economiza mais do que leva. Portanto, tome cuidado com seus nomes e troque-os quando encontrar nomes melhores.

O nome de uma variável, função ou classe deve responder a todas as grandes questões. Deve dizer por que existe, o que faz e como é usado. **Se um nome exigir um comentário, o nome não revela sua intenção**.

| Não revela intenção                 | Revela intenção            |
| ----------------------------------- | -------------------------- |
| `int d; // tempo decorrido em dias` | `int tempoDecorridoEmDias` |

A escolha de nomes que revelem a intenção pode facilitar muito a compreensão e a alteração do código. Exemplo:

```java
public List<int[]> getThem() {
  List<int[]> lista1 = new ArrayList<int[]>();
  for (int[] x : aLista)
    if (x[0] == 4)
      lista1.add(x);
  return lista1;
}
```

Este código é simples, mas gera muitas dúvidas:

1. Qual é o conteúdo de `aLista`?
2. Qual é o significado do item `x[0]` na lista?.
3. Por que comparamos `x[0]` com o valor `4`?
4. Como eu usaria a lista retornada?

As respostas a essas perguntas não estão presentes no exemplo de código, mas poderiam estar. Digamos que estamos trabalhando em um jogo de campo-minado, podemos refatorar o código anterior da seguinte maneira:

```java
public List<int[]> getCelulasSinalizadas() {
  List<int[]> celulasSinalizadas = new ArrayList<int[]>();
  for (int[] celula : tabuleiro)
    if (celula[VALOR_ESTADO] == SINALIZADA)
      celulasSinalizadas.add(celula);
  return celulasSinalizadas;
}
```

Agora sabemos as próximas informações:

1. `aLista` representa o `tabuleiro`
2. `x[0]` representa uma célula no tabuleiro e `4` representa uma célula sinalizada.
3. A lista retornada representa as `celulasSinalizadas`

Observe que a simplicidade do código não mudou. Ele ainda tem exatamente o mesmo número de operadores e constantes, com exatamente o mesmo número de níveis de aninhamento. Mas o código se tornou muito mais explícito.

Podemos melhorar o código escrevendo uma classe simples para células em vez de usar um array de `ints`. Ela pode incluir uma **função reveladora de intenções** (chamada de `estaSinalizada`) para ocultar os números mágicos. Isso resulta em uma nova função da função.

```java
public List<Celula> getCelulasSinalizadas() {
  List<Celula> celulasSinalizadas = new ArrayList<Celula>();
  for (Celula celula : tabuleiro)
    if (celula.estaSinalizada())
      celulasSinalizadas.add(celula);
  return celulasSinalizadas;
}
```

### Evite Desinformação

Os programadores devem evitar deixar pistas falsas que obscureçam o significado do código. Devemos evitar palavras cujo significado determinado varie de nosso significado pretendido.

Não se refira a um agrupamento de contas como `listaDeContas`, a menos que seja realmente uma `Lista`. A palavra `Lista` significa algo específico para programadores. Se o contêiner que contém as contas não for realmente uma lista, isso pode levar a conclusões falsas. Então `grupoDeContas` ou `monteDeContas` ou simplesmente `contas` seria melhor.

Cuidado com o uso de nomes que variam em pequenas formas. Quanto tempo leva para identificar a diferença sutil entre um `XYZControllerForEfficientHandlingOfStrings` em um módulo e, em algum lugar um pouco mais distante, `XYZControllerForEfficientStorageOfStrings`? As palavras têm formas assustadoramente semelhantes.

### Faça distinções significativas

Os programadores criam problemas para si mesmos, quando escrevem código apenas para satisfazer um compilador, ou interpretador. Por exemplo, porque você não pode usar o mesmo nome para referir duas coisas diferentes no mesmo escopo, você pode ser tentado a mudar um nome de forma arbitrária. Às vezes, isso é feito com erros de ortografia, levando a uma situação surpreendente em que corrigir erros de ortografia leva à incapacidade de compilar. Exemplo, você cria a variável `klasse` porque o nome `classe` foi usado para outra coisa.

Na próxima função, os argumentos não são informativos, `a1` e `a2` não fornecem pistas sobre a intenção do autor.

```java
public static void copiarCaracteres(char a1[], char a2[]) {
  for (int i = 0; i < a1.length; i++) {
    a2[i] = a1[i];
  }
}
```

Podemos melhorar o código selecionando nomes de argumentos mais explícitos:

```java
public static void copiarCaracteres(char origem[], char destino[]) {
  for (int i = 0; i < origem.length; i++) {
    destino[i] = origem[i];
  }
}
```

As palavras barulhentas são outra distinção sem sentido. Imagine que você tenha uma classe Produto. Se você tiver outra chamada `InfoProduto` ou `DadosProduto`, você fez os nomes diferentes sem fazê-los significar algo diferente. Informações e dados são palavras de ruído indistintas como (no inglês) a, an e the.

Palavras de ruído são redundantes. A palavra variável nunca deve aparecer em um nome de variável. A palavra tabela nunca deve aparecer no nome de uma tabela.

### Use nomes pronunciáveis

Imagine que você tem a variável `geramdhms` (data de geração, ano, mês, dia, hora, minuto e segundo) e imagine uma conversa onde você precisa falar sobre essa variável chamando-a de "gera eme de agá eme esse". Você pode considerar converter uma classe como esta:

```java
class DtaRcrd102 {
  private Date geramdhms;
  private Date modymdhms;
  private final String pszqint = "102";
  /* ... */
};
```

Para

```java
class Cliente {
  private Date geracaoDataEHora;
  private Date modificacaoDataEHora;
  private final String idRegistro = "102";
  /* ... */
};
```

### Use nomes pesquisáveis

Nomes de uma única letra e constantes numéricas têm um problema específico, pois não são fáceis de localizar em um corpo de texto.

### Evitar Codificação

Temos codificações suficientes para lidar sem adicionar mais ao nosso fardo. Codificar informações de tipo ou escopo em nomes, simplesmente adiciona uma carga extra de decifração. Nomes codificados raramente são pronunciáveis e são facilmente digitados incorretamente. Um exemplo disso é o uso da [Notação Húngara](https://en.wikipedia.org/wiki/Hungarian_notation) ou o uso de prefixos de membros.
#### Interfaces e Implementações

Às vezes, esses são um caso especial para codificações. Por exemplo, digamos que você esteja construindo uma FÁBRICA ABSTRATA para a criação de formas. Esta fábrica será uma interface e será implementada por uma classe concreta. Como você deve nomeá-los? `IFabricaDeFormas` e `FabricaDeFormas`? É preferível deixar as interfaces sem adornos. Não quero que meus usuários saibam que estou entregando uma interface a eles. Eu só quero que eles saibam que é uma `FabricaDeFormas`. Portanto, entre codificar a interface ou a implementação, escolho a implementação. Chamá-lo `FabricaDeFormasImp`, ou mesmo o hediondo `CFabricaDeFormas`, é preferível a codificar a interface.

### Evite mapas mentais

Os leitores não deveriam ter que traduzir mentalmente seus nomes para outros nomes que eles já conhecem.

Uma diferença entre um programador inteligente e um programador profissional é que o profissional entende que a clareza é fundamental. Os profissionais usam seus poderes para o bem e escrevem códigos que outros possam entender.

### Nomes de classe

Classes e objetos devem ter nomes de substantivos ou frases de substantivos como `Cliente`, `PaginaWiki`, `Conta` e `ConversorDeEnderecos`. Evite palavras como `Gerente`, `Processador`, `Dados` ou `Info` no nome de uma classe. Um nome de classe não deve ser um verbo.

### Nomes de métodos

Os métodos devem ter nomes de verbos ou frases verbais como `postarPagamento`, `apagarPagina` ou `salvar`. Acessadores, modificadores e predicados devem ser nomeados por seus valores e prefixados com get, set e is de acordo com o padrão javabean.

Quando os construtores estiverem sobrecarregados, use "factory methods" estáticos com nomes que descrevam os argumentos. Por exemplo:

```java
Complex pontoDeApoio = Complex.DoNumeroReal(23.0);
```

Geralmente é melhor do que

```java
Complex pontoDeApoio = new Complex(23.0);
```

Considere impor seu uso tornando privados os construtores correspondentes.

### Não seja bonito

| Nome bonito           | Nome limpo    |
| --------------------- | ------------- |
| `granadaDeMaoSagrada` | `deleteItems` |
| `castigar`            | `matar`       |
| `comaMeusShorts`      | `abortar`     |

### Escolha uma palavra por conceito

Escolha uma palavra para um conceito abstrato e fique com ela. Por exemplo, é confuso ter fetch, retrieve e get como métodos equivalentes de diferentes classes.

### Não faça trocadilhos

Evite usar a mesma palavra para dois propósitos. Usar o mesmo termo para duas ideias diferentes é essencialmente um trocadilho.

Exemplo: em uma classe use `add` para criar um novo valor adicionando ou concatenando dois valores existentes, e em outra classe use `add` para colocar um parâmetro simples em uma coleção. É uma opção melhor usar um nome como `inserir` ou `acrescentar` em vez disso.

### Use nomes de domínio da solução

Lembre-se de que as pessoas que lerão seu código serão programadores. Portanto, vá em frente e use termos de ciência da computação, nomes de algoritmos, nomes de padrões, termos matemáticos e assim por diante.

### Use nomes de domínio de negócio

Quando você estiver trabalhando em questões de negócio, use o nome do domínio de negócio. Pelo menos o programador que mantém seu código pode perguntar a um especialista em regras de negócio o que isso significa.

Separar conceitos e soluções de negócio faz parte de ser um bom desenvolvedor e designer. O código que precisa se preocupar com os conceitos do negócio deve trazer o nome do negócio.

### Adicionar contexto significativo

Existem alguns nomes que são significativos por si mesmos - a maioria não é. Em vez disso, você precisa colocar os nomes no contexto para o seu leitor, colocando-os em classes, funções ou namespaces bem nomeados. Quando tudo mais falhar, prefixar o nome pode ser necessário como último recurso.

Variáveis como: `primeiroNome`, `ultimoNome`, `rua`, `cidade`, `estado`, juntos, é bastante claro que eles formam um endereço. Mas, e se você visse a variável estado sendo usada sozinha em um método? Você poderia adicionar contexto usando prefixos como: `estadoEndereco`, pelo menos os leitores entenderão que a variável faz parte de uma estrutura maior. Claro, uma solução melhor é criar uma classe chamada `Endereço`, assim até mesmo o compilador saberá que as variáveis pertencem a um conceito maior.

### Não adicione contexto desnecessário

Em um aplicativo imaginário chamado “Posto De Gasolina De luxo”, é uma má ideia prefixar todas as classes com PDGDL. Exemplo: `PDGDLEnderecoDaConta`.

Nomes mais curtos geralmente são melhores do que nomes mais longos, desde que sejam claros. Não adicione mais contexto a um nome do que o necessário.

<a name="chapter3">
<h1>Capítulo 3 -  Funções</h1>
</a>

As funções são a primeira linha de organização em qualquer tópico.


### Pequeno!!

A primeira regra das funções é que elas devem ser pequenas. A segunda regra das funções é que elas devem ser menores que isso.

#### Blocos e recuo

Isso implica que os blocos dentro das instruções if, else, while e assim por diante devem ter o comprimento de uma linha. Provavelmente essa linha deve ser uma chamada de função. Isso não apenas mantém a função envolvente pequena, mas também adiciona valor documental porque a função chamada dentro do bloco pode ter um nome bem descritivo.

Isso também implica que as funções não devem ser grandes o suficiente para conter estruturas aninhadas. Portanto, o nível de recuo de uma função não deve ser maior que um ou dois. Isso, é claro, torna as funções fáceis de ler e entender.

### Faça uma coisa

**AS FUNÇÕES DEVEM FAZER UMA COISA. ELES DEVEM FAZER BEM. SÓ DEVEM FAZER ISSO.**

#### Seções dentro de funções

Se você tem uma função dividida em seções como declarações, inicialização, etc., é um sintoma óbvio de que a função está fazendo mais de uma coisa. Funções que fazem uma coisa não podem ser razoavelmente divididas em seções.

### Um nível de abstração por função

Para garantir que nossas funções estejam fazendo "uma coisa", precisamos garantir que as instruções dentro de nossa função estejam todas no mesmo nível de abstração.

#### Lendo o código de cima para baixo: _a regra Stepdown_

Queremos que o código seja lido como uma narrativa de cima para baixo. 5 Queremos que cada função seja seguida por aquelas no próximo nível de abstração para que possamos ler o programa, descendo um nível de abstração de cada vez enquanto lemos a lista de funções.

Para dizer isso de forma diferente, queremos ser capazes de ler o programa como se fosse um conjunto de parágrafos TO, cada um dos quais descrevendo o nível atual de abstração e referenciando os parágrafos TO subsequentes no próximo nível abaixo.

```
- Para incluir as configurações e desmontagens, incluímos as configurações, depois incluímos o conteúdo da página de teste e, em seguida, incluímos as desmontagens.
- Para incluir as configurações, incluímos a configuração da suíte, se for uma suíte, depois incluímos a configuração normal.
- Para incluir a configuração do conjunto, procuramos na hierarquia pai a página "SuiteSetUp" e adicionamos uma declaração de inclusão com o caminho dessa página.
- Para procurar o pai...
```

Acontece que é muito difícil para os programadores aprender a seguir essa regra e escrever funções que ficam em um único nível de abstração. Mas aprender esse truque também é muito importante. É a chave para manter as funções curtas e garantir que elas façam “uma coisa”. Fazer o código ser lido como um conjunto de parágrafos TO de cima para baixo é uma técnica eficaz para manter o nível de abstração consistente.

### Declarações de switch

É difícil fazer uma pequena declaração switch. 6 Mesmo uma instrução switch com apenas dois casos é maior do que eu gostaria que um único bloco ou função fosse. Também é difícil fazer uma instrução switch que faça uma coisa. Por sua natureza, as instruções switch sempre fazem N coisas. Infelizmente, nem sempre podemos evitar as instruções switch, mas podemos garantir que cada instrução switch seja ocultada em uma classe de baixo nível e nunca seja repetida. Fazemos isso, é claro, com polimorfismo.

### Use nomes descritivos

> Você sabe que está trabalhando em código limpo quando cada rotina acaba sendo exatamente o que você esperava

Metade da batalha para alcançar esse princípio é escolher bons nomes para pequenas funções que fazem uma coisa. Quanto menor e mais focada for uma função, mais fácil será escolher um nome descritivo.

Não tenha medo de fazer um nome longo. Um nome longo e descritivo é melhor do que um nome curto e enigmático. Um nome descritivo longo é melhor do que um comentário descritivo longo. Use uma convenção de nomenclatura que permita que várias palavras sejam facilmente lidas nos nomes das funções e, em seguida, use essas várias palavras para dar à função um nome que diga o que ela faz.

A escolha de nomes descritivos esclarecerá o design do módulo em sua mente e o ajudará a melhorá-lo. Não é incomum que a busca por um bom nome resulte em uma reestruturação favorável do código.

### Argumentos de função

O número ideal de argumentos para uma função é zero (niládico). Em seguida vem um (monádico), seguido de perto por dois (diádico). Três argumentos (triádicos) devem ser evitados sempre que possível. Mais de três (poliádico) requer justificativa muito especial – e então não deve ser usado de qualquer maneira.

Os argumentos são ainda mais difíceis do ponto de vista do teste. Imagine a dificuldade de escrever todos os casos de teste para garantir que todas as várias combinações de argumentos funcionem corretamente. Se não houver argumentos, isso é trivial. Se houver um argumento, não é muito difícil. Com dois argumentos, o problema fica um pouco mais desafiador. Com mais de dois argumentos, testar cada combinação de valores apropriados pode ser assustador.

Os argumentos de saída são mais difíceis de entender do que os argumentos de entrada. Quando lemos uma função, estamos acostumados com a ideia de que as informações entram na função por meio de argumentos e saem pelo valor de retorno. Nós não costumamos

#### 
Formas Monádicas Comuns

Existem duas razões muito comuns para passar um único argumento para uma função. Você pode estar fazendo uma pergunta sobre esse argumento, como em `boolean fileExists(“MyFile”)` . Ou você pode estar operando com base nesse argumento, transformando-o em outra coisa e devolvendo-o. Por exemplo, `InputStream fileOpen(“MyFile”)` transforma um nome de arquivo `String` em um valor de retorno `InputStream`. Esses dois usos são o que os leitores esperam quando veem uma função. Você deve escolher nomes que tornem clara a distinção e sempre usar as duas formas em um contexto consistente.

#### Argumentos sinalizadores

Argumentos sinalizadores são feios. Passar um booleano para uma função é uma prática realmente terrível. Isso complica imediatamente a assinatura do método, proclamando em voz alta que essa função faz mais de uma coisa. Ele faz uma coisa se a flag for `true` e outra se a flag for `false`!

#### Funções diádicas

Uma função com dois argumentos é mais difícil de entender do que uma função monádica. Por exemplo, `writeField(name)` é mais fácil de entender do que `writeField(output-Stream, name)`

Há momentos, é claro, em que dois argumentos são apropriados. Por exemplo, `Point p = new Point(0,0);` é perfeitamente razoável. Os pontos cartesianos naturalmente levam dois argumentos.

Mesmo funções diádicas óbvias como assertEquals(esperado, real) são problemáticas. Quantas vezes você colocou o real onde deveria estar o esperado? Os dois argumentos não têm ordem natural. A ordem esperada e real é uma convenção que requer prática para aprender.

Díades não são más, e você certamente terá que escrevê-las. No entanto, você deve estar ciente de que eles têm um custo e devem aproveitar os mecanismos disponíveis para convertê-los em mônadas. Por exemplo, você pode tornar o método writeField um membro de outputStream para poder dizer outputStream. escrevaCampo(nome) . Ou você pode tornar o outputStream uma variável de membro da classe atual para que não precise transmiti-lo. Ou você pode extrair uma nova classe como FieldWriter que usa o outputStream em seu construtor e tem um método de gravação.

#### Tríades

Funções que levam três argumentos são significativamente mais difíceis de entender do que díades. Os problemas de ordenar, pausar e ignorar são mais do que duplicados. Sugiro que pense bem antes de criar uma tríade.

#### Objetos de Argumento


Compare:

```java
Circle makeCircle(double x, double y, double radius);
```

vs

```java
Circle makeCircle(Point center, double radius);
```

#### Verbos e palavras-chave

Escolher bons nomes para uma função pode ajudar muito a explicar a intenção da função e a ordem e intenção dos argumentos. No caso de uma mônada, a função e o argumento devem formar um belo par verbo/substantivo. Por exemplo, `write(name)` é muito sugestivo. Qualquer que seja essa coisa de “nome”, ela está sendo “escrita”. Um nome ainda melhor pode ser `writeField(name)` , que nos diz que o "nome" é um "campo".

Este último é um exemplo da forma de palavra-chave de um nome de função. Usando esta forma, codificamos os nomes dos argumentos no nome da função. Por exemplo, `assertEquals` pode ser melhor escrito como `assertExpectedEqualsActual(expected, actual)`. Isso atenua fortemente o problema de ter que lembrar a ordem dos argumentos.

### Argumentos de saída

Em geral, argumentos de saída devem ser evitados. Se sua função precisar alterar o estado de algo, faça com que ela altere o estado de seu próprio objeto.

### Separação de consulta de comando

As funções devem fazer algo ou responder a algo, mas não ambos. Sua função deve alterar o estado de um objeto ou deve retornar algumas informações sobre esse objeto. Fazer as duas coisas geralmente leva à confusão.

### Prefira Exceções a Retornar Códigos de Erro

Retornar códigos de erro de funções de comando é uma violação sutil da separação de consulta de comando.

### Não se repita

A duplicação pode ser a raiz de todos os males do software. Muitos princípios e práticas foram criados com o objetivo de controlá-lo ou eliminá-lo.

### Programação estruturada

Alguns programadores seguem as regras de programação estruturada de Edsger Dijkstra. Dijkstra disse que toda função, e todo bloco dentro de uma função, deveria ter uma entrada e uma saída. Seguir essas regras significa que deve haver apenas uma instrução return em uma função, nenhuma instrução `break` ou `continue` em um loop e nunca, _ever_, nenhuma instrução `goto`.

Embora simpatizemos com os objetivos e disciplinas da programação estruturada, essas regras trazem poucos benefícios quando as funções são muito pequenas. É apenas em funções maiores que tais regras fornecem benefícios significativos.

Portanto, se você mantiver suas funções pequenas, as declarações múltiplas ocasionais `return`, `break` ou `continue` não causam danos e às vezes podem até ser mais expressivas do que a regra de entrada única e saída única. Por outro lado, `goto` só faz sentido em funções grandes, por isso deve ser evitado.

<a name="chapter4">
<h1>Capítulo 4 - Comentários</h1>
</a>

Nada pode ser tão útil quanto um comentário bem colocado. Nada pode tornar um módulo mais confuso do que comentários desnecessários e inflexíveis. Nada pode ser tão prejudicial quanto um comentário antigo que propaga mentiras e informações equivocadas.

Se nossas linguagens de programação fossem expressivas o suficiente, ou se tivéssemos talento para utilizar essas linguagens de forma sutil para expressar nossa intenção, não precisaríamos de comentários com tanta frequência - talvez nem precisássemos deles.

### Comentários Não Compensam Código Ruim

Código claro e expressivo, com poucos comentários, é muito superior a um código complexo e confuso, com muitos comentários. Em vez de gastar seu tempo escrevendo comentários que explicam a bagunça que você fez, gaste-o limpando essa bagunça.

### Explique-se através do Código

```java
// Verifiqua se o funcionário possui direito a benefícios completos
if ((funcionario.flags & BANDEIRA_HORARIA) && (funcionario.idade > 65))
```

vs

```java
if (funcionario.temDireitoAosBeneficiosCompletos())
```

O primeiro trecho de código é menos claro e mais difícil de entender, pois requer que o leitor entenda a lógica por trás dos valores das bandeiras e da idade do funcionário para determinar se ele é elegível para benefícios completos. Já o segundo trecho é muito mais claro e autoexplicativo, pois utiliza um método descritivo e bem nomeado para verificar a elegibilidade do funcionário.

### Bons Comentários

Alguns comentários são necessários ou benéficos. No entanto, o único comentário perfeito é aquele que você não escreve.

#### Comentários Legais

Às vezes, nossos padrões corporativos de codificação nos obrigam a escrever determinados comentários por motivos legais. Por exemplo, declarações de direitos autorais e autoria são coisas necessárias e razoáveis a serem incluídas em um comentário no início de cada arquivo de origem.

#### Comentários Informativos

Às vezes, é útil fornecer informações básicas com um comentário. Por exemplo, considere este comentário que explica o valor de retorno de um método abstrato:

```java
// Retorna uma instância do Responder sendo testado.
protected abstract Responder responderInstance();
```

Um comentário como este pode ser útil em alguns casos, mas é melhor usar o nome da função para transmitir as informações, sempre que possível. Por exemplo, neste caso, o comentário poderia ser redundante se a função fosse renomeada para responderSendoTestado.

#### Explicação da Intenção

Às vezes, um comentário vai além de informações úteis sobre a implementação e fornece a intenção por trás de uma decisão. Exemplo:

```java
public int compareTo(Object o)
{
  if(o instanceof WikiPagePath)
  {
    WikiPagePath p = (WikiPagePath) o;
    String nomeComprimido = StringUtil.join(nomes, "");
    String nomeComprimidoArgumento = StringUtil.join(p.nomes, "");
    return nomeComprimido.compareTo(nomeComprimidoArgumento);
  }
  return 1; // somos maiores porque somos do tipo correto.
}
```

#### Esclarecimento

Às vezes é útil traduzir o significado de algum argumento estranho ou valor de retorno para algo que seja legível. Em geral, é melhor encontrar uma maneira de deixar claro esse argumento ou valor de retorno por conta própria; mas quando ele faz parte da biblioteca padrão ou de um código que você não pode alterar, um comentário esclarecedor pode ser útil.

#### Aviso sobre consequências

Sometimes it is useful to warn other programmers about certain consequences.

```java
// Não execute a menos que você tenha algum tempo livre para gastar.
public void _testComArquivoMuitoGrande() {
  escreverLinhasEmArquivo(10000000);
  resposta.setCorpo(arquivoTeste);
  resposta.prontoParaEnviar(this);
  String respostaString = saida.toString();
  assertSubString("Content-Length: 1000000000", respostaString);
  assertTrue(bytesEnviados > 1000000000);
}
```

#### Comentários TODO

Às vezes é razoável deixar notas de "A fazer" na forma de comentários TODO. No caso a seguir, o comentário TODO explica por que a função tem uma implementação degenerada e qual deve ser o futuro dessa função.

```java
// TODO-MdM estes não são necessários
// Esperamos que isso desapareça quando fizermos o modelo de checkout
protected VersionInfo criaVersao() throws Exception {
  return null;
}
```

TODOs são tarefas que o programador acha que devem ser feitas, mas por algum motivo não pode fazer no momento. Pode ser um lembrete para excluir uma funcionalidade obsoleta ou um pedido para que outra pessoa olhe para um problema. Pode ser um pedido para outra pessoa pensar em um nome melhor ou um lembrete para fazer uma mudança que depende de um evento planejado. O que quer que seja um TODO, não é uma desculpa para deixar código ruim no sistema.

#### Ampliação

Um comentário pode ser usado para dar ênfase a algo que, de outra forma, pode parecer inconsequente.

```java
String conteudoItemLista = match.group(3).trim();
// O trim é realmente importante. Ele remove os espaços iniciais
// que poderiam fazer o item ser reconhecido
// como outra lista.
new ListItemWidget(this, conteudoItemLista, this.level + 1);
return buildList(text.substring(match.end()));
```

#### Javadocs em APIs públicas

Não há nada tão útil e satisfatório quanto uma API pública bem descrita. A documentação do javadoc para a biblioteca Java padrão é um exemplo disso. Seria difícil, na melhor das hipóteses, escrever programas Java sem eles.

### Comentários ruins

A maioria dos comentários se enquadra nessa categoria. Geralmente são muletas ou desculpas para código ruim ou justificativas para decisões insuficientes, que não passam de um programador falando consigo mesmo.

#### Murmúrios

Adicionar um comentário só porque você sente que deveria ou porque o processo o exige, é um "hack". Se você decidir escrever um comentário, dedique o tempo necessário para garantir que seja o melhor comentário que você possa escrever. Exemplo:

```java
public void carregarPropriedades() {

  try {
    String caminhoPropriedades = propertiesLocation + "/" + PROPERTIES_FILE;
    FileInputStream fluxoPropriedades = new FileInputStream(caminhoPropriedades);
    propriedadesCarregadas.load(fluxoPropriedades);
  }
  catch(IOException e) {
    // Se não houver arquivo de propriedades, todas as configurações padrão serão carregadas
  }
}
```

O que significa aquele comentário no bloco catch? Claramente, isso significava algo para o autor, mas o significado não é tão claro. Aparentemente, se recebermos uma IOException, significa que não havia um arquivo de propriedades e, nesse caso, todos os valores padrão são carregados. Mas quem carrega todos os valores padrão? O comentário é confuso e não acrescenta valor ao código. Ele é um exemplo de "murmúrio" (mumbling), que é um tipo de comentário ruim que é confuso, vago ou incompreensível.

#### Comentários Redundantes

```java
// Método utilitário que retorna quando this.closed é verdadeiro. Lança uma exceção
// se o tempo limite for atingido.
public synchronized void esperarFechar(final long timeoutMillis) throws Exception {
  if(!closed) {
    wait(timeoutMillis);
    if(!closed)
      throw new Exception("MockResponseSender não pôde ser fechado");
  }
}
```

Qual é o propósito deste comentário? Certamente não é mais informativo do que o código. Ele não justifica o código ou fornece intenção. Não é mais fácil de ler do que o código. Na verdade, é menos preciso do que o código e incentiva o leitor a aceitar essa falta de precisão em vez de uma verdadeira compreensão.

#### Comentários Enganosos

Às vezes, com as melhores intenções, um programador faz uma declaração em seus comentários que não é precisa o suficiente para ser precisa. Considere por mais um momento o exemplo da seção anterior. O método não retorna quando this.closed se torna true. Ele retorna se this.closed é true; caso contrário, ele espera por um tempo limite cego e depois lança uma exceção se this.closed ainda não é verdadeiro.

#### Comentários Obrigatórios

É simplesmente bobo ter uma regra que diz que cada função deve ter um javadoc ou cada variável deve ter um comentário. Comentários assim só tornam o código confuso, propagam mentiras e contribuem para a confusão e desorganização em geral.

```java
/**
*
* @param titulo O título do CD
* @param autor O autor do CD
* @param faixas O número de faixas no CD
* @param duracaoEmMinutos A duração do CD em minutos
*/
public void adicionarCD(String titulo, String autor, int faixas, int duracaoEmMinutos) {
  CD cd = new CD();
  cd.titulo = titulo;
  cd.autor = autor;
  cd.faixas = faixas;
  cd.duracao = duracaoEmMinutos;
  listaCDs.add(cd);
}
```

#### Comentários "Log Change"

Às vezes, as pessoas adicionam um comentário ao início de um módulo toda vez que o editam. Exemplo:

```java
* Alterações (a partir de 11-Out-2001)
* --------------------------
* 11-Out-2001: Reorganizou a classe e a moveu para o novo pacote com.jrefinery.date (DG);
* 05-Nov-2001: Adicionou um método getDescription() e eliminou a classe NotableDate (DG);
* 12-Nov-2001: IBD requer o método setDescription(), agora que a classe NotableDate se foi (DG); Alterou getPreviousDayOfWeek(),
getFollowingDayOfWeek() e getNearestDayOfWeek() para corrigir bugs (DG);
* 05-Dez-2001: Corrigiu bug na classe SpreadsheetDate (DG);
```

Hoje em dia, temos sistemas de controle de código-fonte e não precisamos desse tipo de registro.

#### Comentários ruidosos

Os comentários nos exemplos a seguir não fornecem novas informações.

```java
/**
* Construtor padrão.
*/
protected RegraAnual() {
}
```

```java
/** O dia do mês. */
private int diaDoMes;
```

Os comentários do Javadoc podem entrar nessa categoria. Muitas vezes, são apenas comentários ruidosos e redundantes escritos por algum desejo mal colocado de fornecer documentação.

#### Não use um comentário quando puder usar uma função ou uma variável

Exemplo:

```java
// o módulo da lista global <mod> depende do
// subsistema do qual fazemos parte?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

vs

```java
ArrayList dependenciasDoModulo = smodule.getDependSubsystems();
String nossoSubsistema = subSysMod.getSubSystem();
if (dependenciasDoModulo.contains(nossoSubsistema))
```

#### Marcadores de posição

Esse tipo de comentário pode gerar ruído no código.

```java
// Ações //////////////////////////////////
```

#### Comentários de fechamento de chaves

Exemplo:

```java
public class wc {
  public static void main(String[] args) {
    BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
    String linha;
    int contLinhas  = 0;
    int contCaracteres = 0;
    int contPalavras = 0;
    try {
      while ((linha = in.readLine()) != null) {
        contLinhas++;
        contCaracteres += linha.length();
        String words[] = linha.split("\\W");
        contPalavras += words.length;

      } //while
      System.out.println("contPalavras = " + contPalavras);
      System.out.println("contLinhas = " + contLinhas);
      System.out.println("contCaracteres = " + contCaracteres);

    } // try
    catch (IOException e) {
      System.err.println("Erro:" + e.getMessage());

    } //catch

  } //main
```

Em vez de usar comentários de fechamento de chaves, é melhor dividir o código em funções menores e autoexplicativas. Dessa forma, o código fica mais fácil de ler e entender, sem a necessidade de usar comentários desnecessários.

#### Atribuições e Créditos

Example:

`/* Adicionado por Rick */`

O sistema de controle de versão (VCS) pode gerenciar essa informação automaticamente, portanto esse tipo de comentário é desnecessário e pode ser removido.

#### Código comentado

```java
InputStreamResponse response = new InputStreamResponse();
response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
// StreamReader reader = new StreamReader(resultsStream);
// response.setContent(reader.read(formatter.getByteCount()));
```

Se você não precisa mais disso, por favor delete-o, você pode recuperá-lo posteriormente com o seu VCS se precisar novamente.

#### HTML Comments

HTML in source code comments is an abomination, as you can tell by reading the code below.

```java
/**
* Tarefa para executar testes Fit.
* Esta tarefa executa testes Fitnesse e publica os resultados.
* <p/>
* <pre>
* Usage:
* &lt;taskdef name=&quot;execute-fitnesse-tests&quot;
* classname=&quot;fitnesse.ant.ExecuteFitnesseTestsTask&quot;
* classpathref=&quot;classpath&quot; /&gt;
* OR
* &lt;taskdef classpathref=&quot;classpath&quot;
* resource=&quot;tasks.properties&quot; /&gt;
* <p/>
* &lt;execute-fitnesse-tests
* suitepage=&quot;FitNesse.SuiteAcceptanceTests&quot;
* fitnesseport=&quot;8082&quot;
* resultsdir=&quot;${results.dir}&quot;
* resultshtmlpage=&quot;fit-results.html&quot;
* classpathref=&quot;classpath&quot; /&gt;
* </pre>
*/
```

#### Informações não locais

Se você precisar escrever um comentário, certifique-se de que ele descreva o código próximo a ele. Não ofereça informações de todo o sistema no contexto de um comentário local.

#### Informações demais

Não coloque discussões históricas interessantes ou descrições irrelevantes de detalhes em seus comentários.

#### Conexão não óbvia

A conexão entre um comentário e o código que ele descreve deve ser óbvia. Se você se der ao trabalho de escrever um comentário, pelo menos o leitor deve olhar para o comentário, comparar com o código e entender do que o comentário está falando.

#### Cabeçalhos de função

Funções curtas não precisam de muita descrição. Um nome bem escolhido para uma pequena função que faz uma coisa geralmente é melhor do que um cabeçalho de comentário.

#### Javadocs em código não público

Javadocs são para APIs públicas, em código não público podem ser uma distração mais do que uma ajuda.

<a name="chapter5">
<h1>Chapter 5 -  Formatting</h1>
</a>

Code formatting is important. It is too important to ignore and it is too important to treat religiously. Code formatting is about communication, and communication is the professional developer’s first order of business.

### Vertical Formatting

#### Vertical Openness Between Concepts

This concept consist in how to you separate concepts in your code, In the next example we can appreciate it.

```java
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
  public static final String REGEXP = "'''.+?'''";
  private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
      Pattern.MULTILINE + Pattern.DOTALL
      );

  public BoldWidget(ParentWidget parent, String text) throws Exception {
    super(parent);
    Matcher match = pattern.matcher(text);
    match.find();
    addChildWidgets(match.group(1));
  }

  public String render() throws Exception {
    StringBuffer html = new StringBuffer("<b>");
    html.append(childHtml()).append("</b>");
    return html.toString();
  }
}
```

```java
package fitnesse.wikitext.widgets;
import java.util.regex.*;
public class BoldWidget extends ParentWidget {
  public static final String REGEXP = "'''.+?'''";
  private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
  Pattern.MULTILINE + Pattern.DOTALL);
  public BoldWidget(ParentWidget parent, String text) throws Exception {
    super(parent);
    Matcher match = pattern.matcher(text); match.find(); addChildWidgets(match.group(1));
  }
  public String render() throws Exception { StringBuffer html = new StringBuffer("<b>"); html.append(childHtml()).append("</b>"); return html.toString();
  }
}
```

As you can see, the readability of the first example is greater than that of the second.

#### Vertical Density

The vertical density implies close association. So lines of code that are tightly related should appear vertically dense. Check the follow example:

```Java
public class ReporterConfig {

	/**
	 * The class name of the reporter listener */
	private String m_className;

	/**
	 * The properties of the reporter listener */
	private List<Property> m_properties = new ArrayList<Property>();

	public void addProperty(Property property) { m_properties.add(property);
	}
```

```java
public class ReporterConfig {
  private String m_className;
  private List<Property> m_properties = new ArrayList<Property>();

  public void addProperty(Property property) {
    m_properties.add(property);
  }
}
```

The second code it's much easier to read. It fits in an "eye-full".

#### Vertical Distance

**Variable Declarations**. Variables should be declared as close to their usage as possible. Because our functions are very short, local variables should appear at the top of each function,

**Instance variables**, on the other hand, should be declared at the top of the class. This should not increase the vertical distance of these variables, because in a well-designed class, they are used by many, if not all, of the methods of the class.

There have been many debates over where instance variables should go. In C++ we commonly practiced the so-called scissors rule, which put all the instance variables at the bottom. The common convention in Java, however, is to put them all at the top of the class. I see no reason to follow any other convention. The important thing is for the instance variables to be declared in one well-known place. Everybody should know where to go to see the declarations.

**Dependent Functions**. If one function calls another, they should be vertically close, and the caller should be above the callee, if at all possible. This gives the program a natural flow. If the convention is followed reliably, readers will be able to trust that function definitions will follow shortly after their use.

**Conceptual Affinity**. Certain bits of code want to be near other bits. They have a certain conceptual affinity. The stronger that affinity, the less vertical distance there should be between them.

#### Vertical Ordering

In general we want function call dependencies to point in the downward direction. That is, a function that is called should be below a function that does the calling. This creates a nice flow down the source code module from high level to low level. _(This is the exact opposite of languages like Pascal, C, and C++ that enforce functions to be defined, or at least declared, before they are used)_

### Horizontal Formatting

#### Horizontal Openness and Density

We use horizontal white space to associate things that are strongly related and disassociate things that are more weakly related. Example:

```java
private void measureLine(String line) {
  lineCount++;
  int lineSize = line.length();
  totalChars += lineSize;
  lineWidthHistogram.addLine(lineSize, lineCount);
  recordWidestLine(lineSize);
}
```

Assignment statements have two distinct and major elements: the left side and the right side. The spaces make that separation obvious.

#### Horizontal Alignment

```java
public class Example implements Base
{
  private   Socket      socket;
  private   inputStream input;
  protected long        requestProgress;

  public Expediter(Socket      s,
                   inputStream input) {
    this.socket =     s;
    this.input  =     input;
  }
}
```

In modern languages this type of alignment is not useful. The alignment seems to emphasize the wrong things and leads my eye away from the true intent.

```java
public class Example implements Base
{
  private Socket socket;
  private inputStream input;
  protected longrequestProgress;

  public Expediter(Socket s, inputStream input) {

    this.socket = s;
    this.input = input;
  }
}
```

This is a better approach.

### Indentation

The indentation it's important because help us to have a visible hierarchy and well defined blocks.

### Team Rules

Every programmer has his own favorite formatting rules, but if he works in a team, then the team rules.

A team of developers should agree upon a single formatting style, and then every member of that team should use that style. We want the software to have a consistent style. We don't want it to appear to have been written by a bunch of disagreeing individuals.

<a name="chapter6">
<h1>Chapter 6 -  Objects and Data Structures</h1>
</a>

### Data Abstraction

Hiding implementation is not just a matter of putting a layer of functions between the variables. Hiding implementation is about abstractions! A class does not simply push its variables out through getters and setters. Rather it exposes abstract interfaces that allow its users to manipulate the essence of the data, without having to know its implementation.

### Data/Object Anti-Symmetry

These two examples show the difference between objects and data structures. Objects hide their data behind abstractions and expose functions that operate on that data. Data structure expose their data and have no meaningful functions.

**Procedural Shape**

```java
public class Square {
  public Point topLeft;
  public double side;
}

public class Rectangle {
  public Point topLeft;
  public double height;
  public double width;
}

public class Circle {
  public Point center;
  public double radius;
}

public class Geometry {
  public final double PI = 3.141592653589793;

  public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceof Square) {
      Square s = (Square)shape;
      return s.side * s.side;
    }
    else if (shape instanceof Rectangle) { Rectangle r = (Rectangle)shape; return r.height * r.width;
    }
    else if (shape instanceof Circle) {
      Circle c = (Circle)shape;
      return PI * c.radius * c.radius;
    }
    throw new NoSuchShapeException();
  }
}
```

**Polymorphic Shape**

```java
public class Square implements Shape {
  private Point topLeft;
  private double side;

  public double area() {
    return side*side;
  }
}

public class Rectangle implements Shape {
  private Point topLeft;
  private double height;
  private double width;

  public double area() {
    return height * width;
  }
}

public class Circle implements Shape {
  private Point center;
  private double radius;
  public final double PI = 3.141592653589793;

  public double area() {
    return PI * radius * radius;
  }
}
```

Again, we see the complimentary nature of these two definitions; they are virtual opposites! This exposes the fundamental dichotomy between objects and data structures:

> Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.

The complement is also true:

> Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.

Mature programmers know that the idea that everything is an object is a myth. Sometimes you really do want simple data structures with procedures operating on them.

### The Law of [Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter)

There is a well-known heuristic called the Law of Demeter that says a module should not know about the innards of the objects it manipulates.

More precisely, the Law of Demeter says that a method `f` of a class `C` should only call the methods of these:

- `C`
- An object created by `f`
- An object passed as an argument to `f`
- An object held in an instance variable of `C`

The method should not invoke methods on objects that are returned by any of the allowed functions. In other words, talk to friends, not to strangers.

### Data Transfer Objects

The quintessential form of a data structure is a class with public variables and no functions. This is sometimes called a data transfer object, or DTO. DTOs are very useful structures, especially when communicating with databases or parsing messages from sockets, and so on. They often become the first in a series of translation stages that convert raw data in a database into objects in the application code.

<a name="chapter7">
<h1>Chapter 7 -  Error Handling</h1>
</a>

Many code bases are completely dominated by error handling. When I say dominated, I don't mean that error handling is all that they do. I mean that it is nearly impossible to see what the code does because of all of the scattered error handling. Error handling is important, but if it obscures logic, it's wrong.

### Use Exceptions Rather Than Return Codes

Back in the distant past there were many languages that didn't have exceptions. In those languages the techniques for handling and reporting errors were limited. You either set an error flag or returned an error code that the caller could check

### Write Your Try-Catch-Finally Statement First

In a way, try blocks are like transactions. Your catch has to leave your program in a consistent state, no matter what happens in the try. For this reason it is good practice to start with a try-catch-finally statement when you are writing code that could throw exceptions. This helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the try.

### Provide Context with Exceptions

Each exception that you throw should provide enough context to determine the source and location of an error.

Create informative error messages and pass them along with your exceptions. Mention the operation that failed and the type of failure. If you are logging in your application, pass along enough information to be able to log the error in your catch.

### Don't Return Null

If you are tempted to return null from a method, consider throwing an exception or returning a SPECIAL CASE object instead. If you are calling a null-returning method from a third-party API, consider wrapping that method with a method that either throws an exception or returns a special case object.

### Don't Pass Null

Returning null from methods is bad, but passing null into methods is worse. Unless you are working with an API which expects you to pass null, you should avoid passing null in your code whenever possible.

<a name="chapter8">
<h1>Chapter 8 -  Boundaries</h1>
</a>

We seldom control all the software in our systems. Sometimes we buy third-party pack- ages or use open source. Other times we depend on teams in our own company to produce components or subsystems for us. Somehow we must cleanly integrate this foreign code with our own.

### Using Third-Party Code

There is a natural tension between the provider of an interface and the user of an interface. Providers of third-party packages and frameworks strive for broad applicability so they can work in many environments and appeal to a wide audience. Users, on the other hand, want an interface that is focused on their particular needs. This tension can cause problems at the boundaries of our systems. Example:

```java
Map sensors = new HashMap();
Sensor s = (Sensor)sensors.get(sensorId);
```

VS

```java
public class Sensors {
  private Map sensors = new HashMap();

  public Sensor getById(String id) {
    return (Sensor) sensors.get(id);
  }
  //snip
}
```

The first code exposes the casting in the Map, while the second is able to evolve with very little impact on the rest of the application. The casting and type management is handled inside the Sensors class.

The interface is also tailored and constrained to meet the needs of the application. It results in code that is easier to understand and harder to misuse. The Sensors class can enforce design and business rules.

### Exploring and Learning Boundaries

Third-party code helps us get more functionality delivered in less time. Where do we start when we want to utilize some third-party package? It’s not our job to test the third-party code, but it may be in our best interest to write tests for the third-party code we use.

It's a good idea write some test for learn and understand how to use a third-party code. Newkirk calls such tests learning tests.

### Learning Tests Are Better Than Free

The learning tests end up costing nothing. We had to learn the API anyway, and writing those tests was an easy and isolated way to get that knowledge. The learning tests were precise experiments that helped increase our understanding.

Not only are learning tests free, they have a positive return on investment. When there are new releases of the third-party package, we run the learning tests to see whether there are behavioral differences.

### Using Code That Does Not Yet Exist

Some times it's necessary work in a module that will be connected to another module under develop, and we have no idea about how to send the information, because the API had not been designed yet. In this cases it's recommendable create an interface for encapsulate the communication with the pending module. In this way we maintain the control over our module and we can test although the second module isn't available yet.

### Clean Boundaries

Interesting things happen at boundaries. Change is one of those things. Good software designs accommodate change without huge investments and rework. When we use code that is out of our control, special care must be taken to protect our investment and make sure future change is not too costly.

<a name="chapter9">
<h1>Chapter 9 -  Unit Tests</h1>
</a>

**T**est
**D**riven
**D**evelopment

### The Three Laws of TDD

- **First Law** You may not write production code until you have written a failing unit test.
- **Second Law** You may not write more of a unit tests than is sufficient to fail, and not comipling is failing.
- **Third Law** You may not write more production code than is sufficient to pass the currently failing tests.

### Clean Tests

If you don't keep your tests clean, you will lose them.

The readability it's very important to keep clean your tests.

### One Assert per test

It's recomendable maintain only one asserts per tests, because this helps to maintain each tests easy to understand.

### Single concept per Test

This rule will help you to keep short functions.

- **Write one test per each concept that you need to verify**

### F.I.R.S.T

- **Fast** Test should be fast.
- **Independient** Test should not depend on each other.
- **Repeatable** Test Should be repeatable in any environment.
- **Self-Validating** Test should have a boolean output. either they pass or fail.
- **Timely** Unit tests should be written just before the production code that makes them pass. If you write tests after the production code, then you may find the production code to be hard to test.

<a name="chapter10">
<h1>Chapter 10 -  Classes</h1>
</a>

## Class Organization

### Encapsulation

We like to keep our variables and utility functions small, but we're not fanatic about it. Sometimes we need to make a variable or utility function protected so that it can be accessed by a test.

## Classes Should be Small

- First Rule: Classes should be small
- Second Rule: **Classes should be smaller than the first rule**

### The Single Responsibility Principle

**Classes should have one responsibility - one reason to change**

SRP is one of the more important concept in OO design. It's also one of the simple concepts to understand and adhere to.

### Cohesion

Classes Should have a small number of instance variables. Each of the methods of a class should manipulate one or more of those variables. In general the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.

### Maintaining Cohesion Results in Many Small Classes

Just the act of breaking large functions into smaller functions causes a proliferation of classes.

## Organizing for change

For most systems, change is continual. Every change subjects us to the risk that the remainder of the system no longer works as intended. In a clean system we organize our classes so as to reduce the risk of change.

### Isolating from Change

Needs will change, therefore code will change. We learned in OO 101 that there are concrete classes, which contain implementation details (code), and abstract classes, which represent concepts only. A client class depending upon concrete details is at risk when those details change. We can introduce intefaces and abstract classes to help isolate the impact of those details.

<a name="chapter11">
<h1>Chapter 11 -  Systems</h1>
</a>

## Separe Constructing a System from using It

_Software Systems should separate the startuo process, when the application objects are constructed and the dependencies are "wired" thogether, from the runtime logic that takes over after startup_

### Separation from main

One way to separate construction from use is simply to move all aspects of construction to `main`, or modules called by `main`, and to design the rest of the system assuming that all objects have been created constructed and wired up appropriately.

The Abstract Factory Pattern is an option for this kind of approach.

### Dependency Injection

A powerful mechanism for separating construction from use is Dependency Injection (DI), the application of Inversion of control (IoC) to dependency management. Inversion of control moves secondary responsibilities from an object to other objects that are dedicated to the purpose, thereby supporting the Single Responsibility Principle. In context of dependency management, an object should not take responsibility for instantiating dependencies itself. Instead, it, should pass this responsibility to another "authoritative" mechanism, thereby inverting the control. Because setup is a global concern, this authoritative mechanism will usually be either the "main"
routine or a special-purpose _container_.

<a name="chapter12">
<h1>Chapter 12 -  Emergence</h1>
</a>

According to Kent Beck, a design is "simple" if it follows these rules

- Run all tests
- Contains no duplication
- Expresses the intent of programmers
- Minimizes the number of classes and methods

<a name="chapter13">
<h1>Chapter 13 -  Concurrency</h1>
</a>

Concurrence is a decoupling strategy. It helps us decouple what gets fone from when it gets done. In single-threaded applications what and when are so strongly coupled that the state of the entire application can often be determined by looking at the stack backtrace. A programmer who debugs such a system can set a breakpoint, or a sequence of breakpoints, and know the state of the system by which breakpoints are hit.

Decoupling what from when can dramatically improve both the throughput and structures of an application. From a structural point of view the application looks like many lit- tle collaborating computers rather than one big main loop. This can make the system easier to understand and offers some powerful ways to separate concerns.

## Miths and Misconceptions

- Concurrency always improves performance.
  Concurrency can sometimes improve performance, but only when there is a lot of wait time that can be shared between multiple threads or multiple processors. Neither situ- ation is trivial.
- Design does not change when writing concurrent programs.
  In fact, the design of a concurrent algorithm can be remarkably different from the design of a single-threaded system. The decoupling of what from when usually has a huge effect on the structure of the system.
- Understanding concurrency issues is not important when working with a container such as a Web or EJB container.
  In fact, you’d better know just what your container is doing and how to guard against the issues of concurrent update and deadlock described later in this chapter.
  Here are a few more balanced sound bites regarding writing concurrent software:
- Concurrency incurs some overhead, both in performance as well as writing additional code.
- Correct concurrency is complex, even for simple problems.
- Concurrency bugs aren’t usually repeatable, so they are often ignored as one-offs instead of the true defects they are.
- Concurrency often requires a fundamental change in design strategy.

#

<a name="chapter14">
<h1>Chapter 14 -  Successive Refinement</h1>
</a>
This chapter is a study case. It's recommendable to completely read it to understand more.

<a name="chapter15">
<h1>Chapter 15 -  JUnit Internals</h1>
</a>
This chapter analize the JUnit tool. It's recommendable to completely read it to understand more.

<a name="chapter16">
<h1>Chapter 16 -  Refactoring SerialDate</h1>
</a>
This chapter is a study case. It's recommendable to completely read it to understand more.

<a name="chapter17">
<h1>Chapter 17 -  Smells and Heuristics</h1>
</a>

A reference of code smells from Martin Fowler's _Refactoring_ and Robert C Martin's _Clean Code_.

While clean code comes from discipline and not a list or value system, here is a starting point.

## Comments

#### C1: Inappropriate Information

Reserve comments for technical notes referring to code and design.

#### C2: Obsolete Comment

Update or delete obsolete comments.

#### C3: Redundant Comment

A redundant comment describes something able to sufficiently describe itself.

#### C4: Poorly Written Comment

Comments should be brief, concise, correctly spelled.

#### C5: Commented-Out Code

Ghost code. Delete it.

## Environment

#### E1: Build Requires More Than One Step

Builds should require one command to check out and one command to run.

#### E2: Tests Require More Than One Step

Tests should be run with one button click through an IDE, or else with one command.

## Functions

#### F1: Too Many Arguments

Functions should have no arguments, then one, then two, then three. No more than three.

#### F2: Output Arguments

Arguments are inputs, not outputs. If somethings state must be changed, let it be the state of the called object.

#### F3: Flag Arguments

Eliminate boolean arguments.

#### F4: Dead Function

Discard uncalled methods. This is dead code.

## General

#### G1: Multiple Languages in One Source File

Minimize the number of languages in a source file. Ideally, only one.

#### G2: Obvious Behavior is Unimplemented

The result of a function or class should not be a surprise.

#### G3: Incorrect Behavior at the Boundaries

Write tests for every boundary condition.

#### G4: Overridden Safeties

Overriding safeties and exerting manual control leads to code melt down.

#### G5: Duplication

Practice abstraction on duplicate code. Replace repetitive functions with polymorphism.

#### G6: Code at Wrong Level of Abstraction

Make sure abstracted code is separated into different containers.

#### G7: Base Classes Depending on Their Derivatives

Practice modularity.

#### G8: Too Much Information

Do a lot with a little. Limit the amount of _things_ going on in a class or functions.

#### G9: Dead Code

Delete unexecuted code.

#### G10: Vertical Separation

Define variables and functions close to where they are called.

#### G11: Inconsistency

Choose a convention, and follow it. Remember no surprises.

#### G12: Clutter

Dead code.

#### G13: Artificial Coupling

Favor code that is clear, rather than convenient. Do not group code that favors mental mapping over clearness.

#### G14: Feature Envy

Methods of one class should not be interested with the methods of another class.

#### G15: Selector Arguments

Do not flaunt false arguments at the end of functions.

#### G16: Obscured Intent

Code should not be magic or obscure.

#### G17: Misplaced Responsibility

Use clear function name as waypoints for where to place your code.

#### G18: Inappropriate Static

Make your functions nonstatic.

#### G19: Use Explanatory Variables

Make explanatory variables, and lots of them.

#### G20: Function Names Should Say What They Do

...

#### G21: Understand the Algorithm

Understand how a function works. Passing tests is not enough. Refactoring a function can lead to a better understanding of it.

#### G22: Make Logical Dependencies Physical

Understand what your code is doing.

#### G23: Prefer Polymorphism to If/Else or Switch/Case

Avoid the brute force of switch/case.

#### G24: Follow Standard Conventions

It doesn't matter what your teams convention is. Just that you have on and everyone follows it.

#### G25: Replace Magic Numbers with Named Constants

Stop spelling out numbers.

#### G26: Be Precise

Don't be lazy. Think of possible results, then cover and test them.

#### G27: Structure Over Convention

Design decisions should have a structure rather than a dogma.

#### G28: Encapsulate Conditionals

Make your conditionals more precise.

#### G29: Avoid Negative Conditionals

Negative conditionals take more brain power to understand than a positive.

#### G31: Hidden Temporal Couplings

Use arguments that make temporal coupling explicit.

#### G32: Don’t Be Arbitrary

Your code's structure should communicate the reason for its structure.

#### G33: Encapsulate Boundary Conditions

Avoid leaking +1's and -1's into your code.

#### G34: Functions Should Descend Only One Level of Abstraction

The toughest heuristic to follow. One level of abstraction below the function's described operation can help clarify your code.

#### G35: Keep Configurable Data at High Levels

High level constants are easy to change.

#### G36: Avoid Transitive Navigation

Write shy code. Modules should only know about their neighbors, not their neighbor's neighbors.

## Names

#### N1: Choose Descriptive Names

Choose names that are descriptive and relevant.

#### N2: Choose Names at the Appropriate Level of Abstraction

Think of names that are still clear to the user when used in different programs.

#### N3: Use Standard Nomenclature Where Possible

Use names that express their task.

#### N4: Unambiguous Names

Favor clearness over curtness. A long, expressive name is better than a short, dull one.

#### N5: Use Long Names for Long Scopes

A name's length should relate to its scope.

#### N6: Avoid Encodings

Do not encode names with type or scope information.

#### N7: Names Should Describe Side-Effects

Consider the side-effects of your function, and include that in its name.

## Tests

#### T1: Insufficient Tests

Test everything that can possibly break

#### T2: Use a Coverage Tool!

Use your IDE as a coverage tool.

#### T3: Don’t Skip Trivial Tests

...

#### T4: An Ignored Test is a Question about an Ambiguity

If your test is ignored, the code is brought into question.

#### T5: Test Boundary Conditions

The middle is usually covered. Remember the boundaries.

#### T6: Exhaustively Test Near Bugs

Bugs are rarely alone. When you find one, look nearby for another.

#### T7: Patterns of Failure Are Revealing

Test cases ordered well will reveal patterns of failure.

#### T8: Test Coverage Patterns Can Be Revealing

Similarly, look at the code that is or is not passed in a failure.

#### T9: Tests Should Be Fast

Slow tests won't get run.
