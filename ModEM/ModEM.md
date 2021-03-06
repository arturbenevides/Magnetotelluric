# Inversão de dados MT

É evidente que os modelos 3D são os que podem representar mais adequadamente as características do interior da Terra. 
Idealmente, a inversão 3D seria a opção mais prática de modelagem, pois pode ser aplicada diretamente após o processamento dos dados, 
enquanto uma série de análises de dimensionalidade e hipóteses (já que estruturas puramente 2D praticamente não existem na natureza) 
precisam ser feitas antes de se aplicar a inversão 2D. Porém, o grande empecilho dos métodos de inversão 3D é o custo computacional
consideravelmente alto (impraticáveis até alguns anos). Contudo, o desenvolvimento de novos métodos e algoritmos na última década trouxeram 
significativo avanço da técnica, tornado a mesma possível ainda com certo custo computacional (Antunes, 2012).

## Método de Inversão

A inversão de dados EM é um método automatizado para determinar um modelo de condutividade elétrica que melhor represente a Terra
e que seja aceitável em termos geológicos. Para solucionar o problema, duas condições devem ser respeitadas simultaneamente: 
o modelo deve gerar dados (através do cálculo direto) que se ajustem aos dados medidos dentro de um nível de erro aceitável; 
e o modelo deve ser suave com a menor norma possível. Para isso, os programas de inversão buscam minimizar uma função de penalidade
(também chamada função objetivo ou unconstrained functional). Para o algoritmo usado nesse estudo, a função de penalidade tem a forma:
<img src='https://github.com/arturbenevides/Magnetotelurico/blob/master/Figs/funcao_penalty.png' width =600>

onde **m** é o vetor de dimensão *m* de parâmetros do modelo de condutividade da Terra, **d** é o vetor de dados com dimensão n, **C**d ´r a covariância do erro dos dados, **f(m)** é o resultado do cálculo direto de **m**, **m0** é o modelo inicial, **C**m (ou mais
apropriadamente *ν*−1**C**m) é a covariância do modelo ou termo de regularização e *ν* é um multiplicador Lagrangiano (parâmetro de amortecimento). O primeiro termo da Equção acima representa o ajuste dos dados medidos **d** com a resposta do cálculo direto para o modelo **m**. O segundo termo representa a norma do modelo, que reflete a suavidade do mesmo. Para um valor baixo de *ν* a inversão irá priorizar o ajuste dos dados medidos (**d**) com os modelados (**f(m)**). Para *ν* alto, o desajuste nos dados é menos importante e a suavidade do modelo é priorizada. Logo, o peso entre a suavidade do modelo e o ajuste dos dados é dado por *ν*, que devido a isso pode ser chamado de parâmetro de amortecimento.

Alguns métodos matemáticos têm sido aplicados em diferentes algoritmos de inversão, tais como: Occam clássico, Occam no espaço dos dados, método de GaussNewton (GN), método de Gauss-Newton com gradiente conjugado (GN-CG), quasiNewton (QG), método de gradientes conjugados não lineares (NLCG), entre outros.
Uma revisão geral desses métodos é apresentada em Siripunvaraporn (2011). A minimização de uma função de penalidade para qualquer dos métodos mencionados é resolvida de modo iterativo. Por exemplo, no método de Gauss-Newton em que a linearização dessa equação nas vizinhanças do modelo **m**k (referente a k-ésima iteração) para uma pequena perturbação do modelo **δ**m leva a um sistema de mxm
equações definidas por:

<img src='https://github.com/arturbenevides/Magnetotelurico/blob/master/Figs/minimizacao.png' width=300>

onde **r = d − f(m)** k é o resíduo dos dados. Solucionando a equação (2) para **δ**m é possível encontrar um novo modelo dado
por **m**k+1 = **m**k + **δ**m.

Uma peça fundamental dos métodos de inversão é encontrar a matriz Jacobiana, ou matriz de sensibilidade **J** com dimensões
n × m. Essa matriz define as derivadas de **f** em relação ao modelo **m**, tal que J = Drond**f**/Dronde**m** e os elementos da matriz são dados por Jij = Drond *f*i/Drond *m*j. Obter **J** é um dos grandes empecilhos dos métodos clássicos de inversão devido a sua grande dimensão e dificuldades para ser calculada e armazenada. No entanto, métodos alternativos como o NLCG podem minimizar a função
objetivo sem a necessidade de calcular a Jacobiana completa para o modelo de cada iteração.

## Método de Inversão do ModEM

O Modular Eletromagnetic Inversion Software **(ModEM)** é um programa de modelagem de dados eletromagnéticos escrito em Fortran 95 por Gary Egbert, Anna Kelbert e Naser Meqbel do College of Oceanic and Atmospheric Sciences (COAS), Oregon State University (OSU). O ModEM é descrito em detalhes por Egbert e Kelbert (2012). Seu desenvolvimento foi focado na solução de problemas de complexidade estrutural 
2D e especialmente 3D a partir de dados magnetotelúricos, mas também permite a entrada de outros dados eletromagnéticos. Além de resolver problemas de cálculo direto, o ponto principal do ModEM é a inversão de dados EM. O programa utiliza o método de
gradientes conjugados não lineares (*Nonlinear Conjugate Gradient* - NLCG) para a minimização da equação (1).

A função de penalidade (1) pode ser diretamente minimizada através de algoritmos baseados em gradientes como no 
método NLCG (KELBERT et al., 2008; EGBERT, KELBERT, 2012). Este método requer a avaliação de ambas as função de penalidade (1) e o seu gradiente em relação aos parâmetros do modelo para cada iteração, dado por:

<img src='https://github.com/arturbenevides/Magnetotelurico/blob/master/Figs/avaliaca_dos_parametros.png' width=200>

O modelo é atualizado através da expressão genérica para o método NLCG

<img src='https://github.com/arturbenevides/Magnetotelurico/blob/master/Figs/atualizacao_do_modelo.png' width=200>

em que o gradiente definido por (3) é usado para determinar a nova direção conjugada **u**k no espaço do modelo. Usa-se então 
uma busca em linha nessa direção para encontrar **α**k tal que P(**m**k + **α**k**u**k, **d**) é minimizada em relação a P(**m**k, **d**). A direção do gradiente conjugado dado por **u**k é atualizada usando o método de Polak-Ribère (POLAK, RIBIÈRE , 1969). A aplicação desse método requer o cálculo do produto de **J**(transposto) com o resíduo dos dados **r** como visto na equação (3), 
porém para obter esse produto não é necessário encontrar a Jacobiana completa (KELBERT et al., 2008).

Para minimizar a função de penalidade (1) o algoritmo NLCG roda para diferentes valores de *ν*. Cada série de cálculos termina
quando o nível de erro é alcançado, então a norma do modelo é definida e os resultados são comparados. O valor de *ν* para o
qual se obteve a menor norma é tomado como solução para a iteração.
