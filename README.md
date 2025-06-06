# Implementação de um Detector de Bordas em FPGA com Suporte via Software em C

## Sumário

- [Introdução](#introdução)
- [Equipe](#equipe)
- [Requisitos do Problema](#requisitos-do-problema)
- [Recursos Utilizados](#recursos-utilizados)
- [Arquitetura do Sistema](#arquitetura-do-sistema)
- [Filtros Implementados](#filtros-implementados)
- [Como Executar](#como-executar)
- [Uso do Sistema](#uso-do-sistema)
- [Arquivos de Saída](#arquivos-de-saída)
- [Referências](#referências)

## Introdução
Este projeto implementa um detector de bordas de imagens utilizando uma FPGA, no qual todo o cálculo dos filtros convolucionais (matriz G) é realizado diretamente em hardware. A imagem é convertida em dados matriciais no software, que envia as janelas de pixels e os filtros apropriados para a FPGA. O valor resultante de cada operação de convolução é então utilizado para construir a matriz final que representa as bordas da imagem.

## Equipe
- **[Cleidson Ramos de Carvalho](https://github.com/cleidson21)**  
- **[Pedro Arthur Nery da Rocha Costa](https://github.com/pedroarthur2002)**  
- **[Uemerson Jesus](https://github.com/Uemersonjesus)**
 
## Requisitos do Problema
- O código do programa principal deve ser escrito em linguagem C;
- O programa deverá ler o arquivo de uma foto, com tamanho de 320 x 240 pixels;
- O programa deverá pré processar a foto em escala de cinza, utilizando 8 bits;
- Após o pré processamento, deve implementar os seguintes filtros detector de borda:
  - Sobel (3x3);
  - Sobel expandido (5x5);
  - Prewitt (3x3);
  - Roberts (2x2);
  - Laplaciano (5x5).

## Recursos Utilizados

### Quartus Prime

O **Quartus Prime** foi a principal ferramenta de desenvolvimento utilizada para síntese, compilação e implementação do projeto em Verilog HDL. As funções desempenhadas pelo software incluem:

- **Síntese e Análise de Recursos**: Tradução do código Verilog para circuitos lógicos, permitindo a avaliação de recursos da FPGA, como **LUTs** e **DSPs**.
- **Compilação e Geração de Bitstream**: Compilação do projeto e geração do arquivo necessário para programar a FPGA.
- **Gravação na FPGA**: Programação da FPGA utilizando a ferramenta **Programmer** e o cabo **USB-Blaster**.
- **Pinagem com Pin Planner**: Ferramenta para mapear sinais de entrada e saída do projeto aos pinos físicos da FPGA, como **LEDs**, **switches**, **botões** e **displays**.

### FPGA DE1-SoC

A **FPGA DE1-SoC** foi a plataforma utilizada para a implementação e execução dos cálculos dos filtros do detector de bordas diretamente em hardware. Essa placa combina um FPGA da Intel com diversos periféricos integrados, oferecendo uma solução robusta para aplicações de processamento de imagens acelerado por hardware reconfigurável.

- **Dispositivo FPGA**: Cyclone® V SE 5CSEMA5F31C6N.
- **Memória Embarcada**: 4.450 Kbits e 6 blocos DSP de 18x18 bits.
- **Entradas e Saídas**: Utilização de 4 botões de pressão, 10 chaves deslizantes e 10 LEDs vermelhos de usuário.

Para mais informações técnicas, consulte o [Manual da Placa DE1-SoC (PDF)](https://drive.google.com/file/d/1dBaSfXi4GcrSZ0JlzRh5iixaWmq0go2j/view).

### Icarus Verilog

O **Icarus Verilog** foi utilizado para simulação funcional de operações específicas durante o desenvolvimento, como a verificação da implementação da raiz quadrada usada no projeto. Essa ferramenta open-source permitiu validar o comportamento correto dessas operações isoladamente, facilitando a depuração e garantindo a precisão antes da síntese em hardware.

**Principais funções no projeto:**

- **Simulação de Operações Isoladas**: Testes funcionais de blocos específicos, como a raiz quadrada, sem a necessidade de programar a FPGA.
- **Depuração e Validação**: Auxiliou na identificação de erros e no ajuste fino da lógica matemática utilizada no detector de bordas.
- **Agilização do Desenvolvimento**: Permitindo iterar rapidamente sobre o código Verilog antes da síntese e implementação física.

## Desenvolvimento

## Arquitetura do Sistema

### Abordagem Híbrida

O sistema foi projetado seguindo uma arquitetura que divide as responsabilidades entre software e hardware:

* **Frontend em Software:** Responsável pelo carregamento de imagens, conversão para escala de cinza, interface com usuário e gerenciamento de dados
* **Backend em Hardware:** Executa as operações intensivas de convolução matemática para os filtros de detecção de bordas

Essa divisão permite aproveitar a flexibilidade do software para operações de E/S e controle, enquanto utiliza a capacidade de processamento paralelo da FPGA para acelerar os cálculos matemáticos.

## Fluxo de Processamento

O processamento segue um pipeline bem definido:

1. **Carregamento:** Imagens são carregadas em diversos formatos (PNG, JPG, BMP, etc.)
2. **Pré-processamento:** Redimensionamento automático e conversão para escala de cinza
3. **Extração de Janelas:** Pixels são organizados em janelas de convolução de tamanho apropriado
4. **Processamento em Hardware:** Filtros são aplicados usando a FPGA
5. **Pós-processamento:** Resultados são combinados e saturados para o formato de saída
6. **Salvamento:** Imagem processada é salva em formato PNG

## Filtros Implementados

O sistema suporta cinco tipos diferentes de filtros de detecção de bordas:

### Filtros de Gradiente (Bidirecionais)

* **Sobel 3x3:** Filtro clássico com boa relação sinal-ruído, ideal para uso geral
* **Sobel 5x5:** Versão expandida que oferece maior precisão em bordas suaves
* **Prewitt 3x3:** Similar ao Sobel mas com pesos uniformes, computacionalmente mais simples
* **Roberts 2x2:** Operador compacto especializado em bordas diagonais nítidas

### Filtros Direcionais

* **Laplaciano 5x5:** Operador de segunda derivada que detecta bordas através de mudanças de intensidade

> Todos os filtros bidirecionais calculam a magnitude euclidiana dos gradientes horizontal e vertical, enquanto o Laplaciano trabalha com uma única direção.

## Características Técnicas

### Processamento de Imagens

* Utiliza a biblioteca **STB Image** para compatibilidade com vários formatos
* Redimensionamento com algoritmo **nearest neighbor**
* Conversão para escala de cinza conforme o padrão **ITU-R BT.601**

### Interface Hardware-Software

* Envio eficiente de janelas de pixels e kernels de filtros
* Processamento paralelo das operações de convolução
* Recepção dos resultados com **saturação automática**

### Tratamento de Bordas

* Utiliza **zero padding** para tratamento de bordas, garantindo comportamento previsível e implementação simples

### Otimizações Implementadas

* **Buffers Estáticos**: Reduzem overhead de alocação de memória
* **Processamento em Fases**: Otimiza o uso de recursos da FPGA
* **Saturação em Hardware**: Limita valores diretamente na FPGA
* **Interface Unificada**: Suporte a diferentes tamanhos de kernel com a mesma lógica

### Precisão e Qualidade

* Aritmética de 16 bits para cálculos intermediários
* Saturação automática para evitar overflow
* Magnitude euclidiana para filtros bidirecionais
* Representação adequada de valores negativos

## Como Executar

### Pré-requisitos

* Compilador GCC com suporte a `-lm`
* Assembler `as` para arquivos assembly
* `make` para automação da compilação
* Arquivo `imagem.png` no diretório do projeto

### Compilação

```bash
make all
```

### Execução

```bash
make run
```

Ou alternativamente:

```bash
./main
```

## Uso do Sistema

1. **Prepare a Imagem**
   Coloque um arquivo chamado `imagem.png` no diretório do programa.

2. **Execute**
   O sistema carregará automaticamente a imagem e a converterá para escala de cinza.

3. **Escolha o Filtro**
   Use o menu interativo para selecionar o tipo de filtro desejado:

   | Opção | Filtro         |
   | ----- | -------------- |
   | 1     | Sobel 3x3      |
   | 2     | Sobel 5x5      |
   | 3     | Prewitt 3x3    |
   | 4     | Roberts 2x2    |
   | 5     | Laplaciano 5x5 |
   | 6     | Sair           |

4. **Visualize os Resultados**
   As imagens processadas serão salvas automaticamente com nomes descritivos.

---

## Arquivos de Saída

* `imagem_cinza.png`: Versão em escala de cinza da imagem original
* `sobel_3x3_output.png`: Resultado do filtro Sobel 3x3
* `sobel_5x5_output.png`: Resultado do filtro Sobel 5x5
* `prewitt_3x3_output.png`: Resultado do filtro Prewitt 3x3
* `roberts_2x2_output.png`: Resultado do filtro Roberts 2x2
* `laplaciano_5x5_output.png`: Resultado do filtro Laplaciano 5x5

## Comandos Úteis

```bash
# Limpeza completa
test$ make clean

# Limpeza apenas das imagens geradas
test$ make clean-images

# Execução com depurador
test$ make debug
```

## Estrutura de Arquivos

| Arquivo         | Descrição                                      |
| --------------- | ---------------------------------------------- |
| `main.c`        | Programa principal e lógica de controle        |
| `interface.h`   | Definições da interface hardware-software      |
| `filters.c`     | Implementação dos kernels de filtros           |
| `matrix_io.s`   | Interface assembly para comunicação com FPGA   |
| `Coprocessor.v` | Módulo principal do coprocessador em Verilog   |
| `sqrt.v`        | Módulo de cálculo de raiz quadrada em hardware |
| `Makefile`      | Automação da compilação                        |

## Referências
- [MATURANA, Patrícia Salles. **Algoritmos de detecção de bordas implementados em FPGA**. Dissertação (Mestrado em Engenharia Elétrica) - Faculdade de Engenharia de Ilha Solteira, Universidade Estadual Paulista, Ilha Solteira, 2010](https://repositorio.unesp.br/server/api/core/bitstreams/707c2411-09fb-46fe-9c59-f4d9352683cb/content)
- [Manual da Placa DE1-SoC (PDF)](https://drive.google.com/file/d/1dBaSfXi4GcrSZ0JlzRh5iixaWmq0go2j/view)
- [Manual do ARMv7-A](https://developer.arm.com/documentation/ddi0406/latest/)
- [Quartus Prime - Documentação Oficial](https://www.intel.com/content/www/us/en/software/programmable/quartus-prime/overview.html)
