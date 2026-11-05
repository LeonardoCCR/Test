# Roteiro de testes e demonstração — Sistema de Grafos

## 1. Objetivo

Este material reúne uma bateria de **40 testes automatizados** para demonstrar o comportamento do sistema de grafos em situações variadas. Os testes não demonstram somente operações bem-sucedidas, como também são exercitados dados inválidos, operações incompatíveis com o tipo do grafo, grafos desconexos, ciclos, laços, vértices isolados e caminhos inexistentes.

A classe principal é `GraphTestRoutine.java`. Ela cria os grafos de teste, executa as operações, compara os resultados obtidos com os resultados esperados e interrompe a execução com `AssertionError` caso alguma verificação falhe. Ao final, imprime `Todos os testes passaram.` somente quando todos os testes terminam sem falhas.

Cada imagem da galeria contém, lado a lado, o grafo/cenário utilizado e um recorte do resultado real registrado na execução da classe.

## 2. Como executar

1. Mantenha `GraphTestRoutine.java` no mesmo projeto em que estão `grafos.model.AdjMatrixGraph` e `grafos.model.Node`.
2. Disponibilize os arquivos de entrada DOT nos caminhos relativos abaixo:
   - `DotArchives/Read/nao_direcionado.dot`
   - `DotArchives/Read/direcionado.dot`
3. Garanta que a dependência `br.com.davesmartins.api.Graph` utilizada pelos métodos de geração de PNG esteja disponível no projeto, além do Graphviz (`dot`) se essa dependência utilizar o executável local.
4. Execute `GraphTestRoutine.main()` com a pasta raiz do projeto como diretório de trabalho.

Exemplo pelo terminal, ajustando os caminhos e dependências ao projeto:

```bash
javac -encoding UTF-8 -d out \
  grafos/model/Node.java \
  grafos/model/AdjMatrixGraph.java \
  GraphTestRoutine.java

java -Dfile.encoding=UTF-8 -cp out GraphTestRoutine
```

> **Atenção:** o ZIP de código usado como base nesta preparação contém `Node.java`, `AdjMatrixGraph.java` e `Application.java`, mas não contém a implementação da dependência `br.com.davesmartins.api.Graph`. Por isso, a compilação completa da funcionalidade de geração de imagens depende de essa classe estar disponível no projeto original. Não substitua essa dependência por um arquivo fictício na entrega.

Para salvar a saída do terminal em um arquivo:

```bash
java -Dfile.encoding=UTF-8 -cp out GraphTestRoutine > resultados-execucao.txt
```

## 3. Como os testes funcionam

- **Caminho positivo:** cria um cenário válido, executa a funcionalidade e verifica valores, quantidades, ordem, caminho, componentes ou pesos esperados.
- **Caminho negativo:** fornece uma entrada inválida ou um tipo de grafo incompatível e verifica se a implementação rejeita a operação com a exceção esperada.
- **Preservação do grafo:** quando aplicável, verifica que algoritmos de consulta não alteram os vértices, arestas ou pesos do grafo original.
- **Tolerância numérica:** comparações de resultados decimais usam uma pequena tolerância para evitar falsos erros por representação binária de números de ponto flutuante.

## 4. Catálogo dos 40 testes

| Nº | Cenário | O que é verificado |
|---:|---|---|
| 01 | Triângulo não orientado | Grafo simples, regular, completo e não bipartido; graus dos vértices. |
| 02 | Caminho A-B-C e vértice isolado D | Folhas, vértice isolado, grau, regularidade, completude e bipartição. |
| 03 | Grafo ponderado com laço | Detecção de laço, grau do vértice, peso zero, simplicidade e rejeição de aresta paralela. |
| 04 | Ciclo de quatro vértices | Ciclo par, regularidade e propriedade de ser bipartido. |
| 05 | Dígrafo A→B→C→D e isolado E | Orientação, fontes, sumidouros, antecessores, sucessores e fechos transitivos. |
| 06 | Dígrafo completo com três vértices | Completude, regularidade, graus de entrada e saída e fechos com ciclo. |
| 07 | Remoção de arco e vértice | Remoção correta, preservação de arcos independentes e atualização da ordem/tamanho. |
| 08 | Dijkstra em grafo não orientado | Menor caminho e custo total em grafo ponderado. |
| 09 | Dijkstra em grafo orientado | Respeito à direção dos arcos e identificação de destinos inalcançáveis. |
| 10 | Prim em grafo conexo | Árvore geradora mínima com peso total conhecido. |
| 11 | Prim com pesos empatados | Quantidade de arestas e peso mínimo mesmo quando há mais de uma solução possível. |
| 12 | Prim com laço e aresta paralela | Laço ignorado na AGM e multiaresta recusada pela matriz. |
| 13 | Prim em grafo desconexo | Floresta geradora mínima, componentes e vértice isolado. |
| 14 | Prim em grafo orientado | Rejeição de algoritmo que não se aplica a dígrafos. |
| 15 | Prim com um vértice | Caso limite: zero arestas e peso total zero. |
| 16 | Kruskal em grafo conexo | Seleção das arestas de menor peso e custo total conhecido. |
| 17 | Kruskal com pesos empatados | Resultado mínimo válido em cenário com empate. |
| 18 | Kruskal com laço e aresta paralela | Laço não selecionado e tentativa de multiaresta rejeitada. |
| 19 | Kruskal em grafo desconexo | Floresta mínima com componentes separados e vértice isolado. |
| 20 | Kruskal em grafo orientado | Rejeição de aplicação em grafo orientado. |
| 21 | Kruskal com um vértice | Caso limite: nenhum arco/aresta selecionado e custo zero. |
| 22 | Malgrange com componentes de tamanhos diferentes | Identificação de várias componentes fortemente conexas. |
| 23 | Malgrange em dígrafo fortemente conexo | Identificação de uma única componente contendo todos os vértices. |
| 24 | Malgrange em dígrafo sem ciclos | Cada vértice permanece em uma componente própria. |
| 25 | Malgrange com fonte e sumidouro | Separação correta da fonte, componente cíclica e sumidouro. |
| 26 | Malgrange com laço e arco paralelo | Laço tratado e tentativa de arco paralelo rejeitada. |
| 27 | Grafo reduzido acíclico | Construção do grafo de componentes e verificação das ligações entre elas. |
| 28 | Malgrange em grafo não orientado | Rejeição do algoritmo em tipo de grafo incompatível. |
| 29 | Geração visual das componentes | Exportação de PNG do grafo original colorido por componente e do grafo reduzido. |
| 30 | BFS e DFS em grafo não orientado | Ordens de visita e árvores de busca distintas, respeitando a estratégia de cada busca. |
| 31 | Busca em grafo desconexo | Identificação dos vértices alcançados e não alcançados a partir da origem. |
| 32 | Busca em dígrafo | Respeito à orientação durante BFS/DFS. |
| 33 | Busca em dígrafo com ciclo e laço | Cada vértice visitado uma única vez; ausência de repetição infinita. |
| 34 | Busca com um vértice e vértice inexistente | Caso limite e rejeição de origem que não pertence ao grafo. |
| 35 | Leitura e escrita DOT não direcionado | Importação, conferência dos dados, exportação e reimportação preservando pesos e estrutura. |
| 36 | Leitura e escrita DOT direcionado | Importação, preservação da orientação e dos pesos após exportar e importar novamente. |
| 37 | Validações negativas de entrada | ID vazio/duplicado, vértice inexistente, peso incompatível, infinito e NaN. |
| 38 | Dijkstra com entradas inválidas | Rejeição de grafo não ponderado, pesos negativos e origem/destino inexistentes. |
| 39 | Prim e Kruskal sem pesos | Rejeição dos dois algoritmos quando o grafo não é ponderado. |
| 40 | DOT com arquivo inexistente | Falha controlada de leitura por `IOException`, sem criar arquivo automaticamente. |

## 5. Galeria de evidências

As imagens foram organizadas por número de teste. Cada uma apresenta o grafo ou cenário utilizado à esquerda e o resultado correspondente da execução à direita.

<details>
<summary><strong>Testes 01 a 10 — propriedades, estrutura e Dijkstra</strong></summary>


### Teste 01

![Evidência do teste 01](README-images/teste_01.png)

### Teste 02

![Evidência do teste 02](README-images/teste_02.png)

### Teste 03

![Evidência do teste 03](README-images/teste_03.png)

### Teste 04

![Evidência do teste 04](README-images/teste_04.png)

### Teste 05

![Evidência do teste 05](README-images/teste_05.png)

### Teste 06

![Evidência do teste 06](README-images/teste_06.png)

### Teste 07

![Evidência do teste 07](README-images/teste_07.png)

### Teste 08

![Evidência do teste 08](README-images/teste_08.png)

### Teste 09

![Evidência do teste 09](README-images/teste_09.png)

### Teste 10

![Evidência do teste 10](README-images/teste_10.png)

</details>

<details>
<summary><strong>Testes 11 a 21 — Prim e Kruskal</strong></summary>

### Teste 11

![Evidência do teste 11](README-images/teste_11.png)

### Teste 12

![Evidência do teste 12](README-images/teste_12.png)

### Teste 13

![Evidência do teste 13](README-images/teste_13.png)

### Teste 14

![Evidência do teste 14](README-images/teste_14.png)

### Teste 15

![Evidência do teste 15](README-images/teste_15.png)

### Teste 16

![Evidência do teste 16](README-images/teste_16.png)

### Teste 17

![Evidência do teste 17](README-images/teste_17.png)

### Teste 18

![Evidência do teste 18](README-images/teste_18.png)

### Teste 19

![Evidência do teste 19](README-images/teste_19.png)

### Teste 20

![Evidência do teste 20](README-images/teste_20.png)

### Teste 21

![Evidência do teste 21](README-images/teste_21.png)

</details>

<details>
<summary><strong>Testes 22 a 29 — componentes fortemente conexas e visualização</strong></summary>

### Teste 22

![Evidência do teste 22](README-images/teste_22.png)

### Teste 23

![Evidência do teste 23](README-images/teste_23.png)

### Teste 24

![Evidência do teste 24](README-images/teste_24.png)

### Teste 25

![Evidência do teste 25](README-images/teste_25.png)

### Teste 26

![Evidência do teste 26](README-images/teste_26.png)

### Teste 27

![Evidência do teste 27](README-images/teste_27.png)

### Teste 28

![Evidência do teste 28](README-images/teste_28.png)

### Teste 29

![Evidência do teste 29](README-images/teste_29.png)

Imagens específicas produzidas pelo teste 29:

![Grafo original com componentes fortemente conexas coloridas](README-images/grafo_original_cfcs.png)

![Grafo reduzido](README-images/grafo_reduzido.png)

</details>

<details>
<summary><strong>Testes 30 a 40 — BFS, DFS, DOT e validações negativas</strong></summary>

### Teste 30

![Evidência do teste 30](README-images/teste_30.png)

### Teste 31

![Evidência do teste 31](README-images/teste_31.png)

### Teste 32

![Evidência do teste 32](README-images/teste_32.png)

### Teste 33

![Evidência do teste 33](README-images/teste_33.png)

### Teste 34

![Evidência do teste 34](README-images/teste_34.png)

### Teste 35

![Evidência do teste 35](README-images/teste_35.png)

### Teste 36

![Evidência do teste 36](README-images/teste_36.png)

### Teste 37

![Evidência do teste 37](README-images/teste_37.png)

### Teste 38

![Evidência do teste 38](README-images/teste_38.png)

### Teste 39

![Evidência do teste 39](README-images/teste_39.png)

### Teste 40

![Evidência do teste 40](README-images/teste_40.png)

</details>

## 6. Arquivos DOT utilizados

Os dois arquivos abaixo são entradas reais dos testes 35 e 36:

- `DotArchives/Read/nao_direcionado.dot`: grafo não orientado e ponderado com cinco vértices e sete arestas.
- `DotArchives/Read/direcionado.dot`: grafo orientado e ponderado com cinco vértices e oito arcos.

Os testes criam arquivos temporários de exportação em `DotArchives/Write`, importam esses arquivos novamente e removem os temporários ao final.

## 7. Resultado esperado da execução

Quando todos os testes são aprovados, a última linha exibida é:

```text
Todos os testes passaram.
```



## 8. Observação sobre o escopo

A classe é uma rotina de testes de demonstração, não substitui uma suíte de testes unitários como JUnit. Ela verifica os comportamentos públicos relevantes da implementação fornecida e imprime os resultados para facilitar a apresentação. Os testes negativos cobrem as validações que existem atualmente no código; não significam que toda entrada inválida imaginável já esteja coberta.
