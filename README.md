# Detector de Borda implementado na FPGA com C

## Sumário
*Fazer o sumário*

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

## Referências
- [MATURANA, Patrícia Salles. **Algoritmos de detecção de bordas implementados em FPGA**. Dissertação (Mestrado em Engenharia Elétrica) - Faculdade de Engenharia de Ilha Solteira, Universidade Estadual Paulista, Ilha Solteira, 2010](https://repositorio.unesp.br/server/api/core/bitstreams/707c2411-09fb-46fe-9c59-f4d9352683cb/content)
- [Manual da Placa DE1-SoC (PDF)](https://drive.google.com/file/d/1dBaSfXi4GcrSZ0JlzRh5iixaWmq0go2j/view)
- [Manual do ARMv7-A](https://developer.arm.com/documentation/ddi0406/latest/)
- [Quartus Prime - Documentação Oficial](https://www.intel.com/content/www/us/en/software/programmable/quartus-prime/overview.html)
