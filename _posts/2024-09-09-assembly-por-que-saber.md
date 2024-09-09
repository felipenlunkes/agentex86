---
title: "Assembly: por que todo desenvolvedor deveria conhecer?"
date: 2024-09-09
author: Felipe Lunkes (Lunx)
tags: ["Assembly", "Dev", "OSDev", "Software"]
revised: Felipe Lunkes (Lunx)
update: 2024-09-09 10:00:00
---

<div align="justify">

Há quem pense que, em pleno 2024, a linguagem **Assembly** caiu em desuso. Quem pensa isso está profundamente enganado. Mas para começar, você conhece essa linguagem?

`Assembly` é um conjunto de linguagens de programação de **baixo nível**, o que quer dizer que estamos escrevendo o código mais próximo possível do código de máquina. Diferentes de linguagens de alto nível, como C, C++, Java, Rust, Python, etc, que possuem uma série de abstrações e permitem que o código seja escrito em uma "linguagem" mais próxima do natural, em Assembly construímos algoritmos utilizando diretamente intruções do processador.

Embora apresentem uma curva de aprendizado maior, pelo uso dos `mnemônicos`, códigos que designam operações realizados pelo processador para operações aritméticas e acesso à memória, que facilitam a memorização dos códigos destas operações, a linguagem Assembly oferece uma série de vantagens, como pro exemplo:

* **Correspondência 1:1**: cada mnemônico, ou seja, cada instrução do código, irá gerar exatamente uma instrução ao processador, o que pode resultar em binários mais leves e otimizados;
* **Acesso direto aos registradores e memória**: a linguagem permite o endereçamento de memória facilitado, além de manipulação de dados diretamente nos registradores do processador. Os registradores são um conjunto de locais em uma memória interna do processador, extremamente rápida (porém limitada), que são utilizados para armazenar dados que serão utilizados para operações aritméticas ou como ponteiros para a memória em instruções que manipulem diretamente a memória, funcionando como índices;
* **Utilização otimizada da arquitetura**: diversas características, facilidades e recursos de uma arquitetura podem ficar ocultos em linguagens de alto nível. Em Assembly, estes recursos podem ser acessados muito mais facilmente.

Quando falamos em Assembly, falamos em um **conjunto de diversas linguagens**, chamadas também de **linguagens de montagem**. Isso porque cada arquitetura de processador apresenta características distintas. Sendo assim, cada arquitetura também apresenta uma linguagem de montagem (Assembly) única, com mnemônicos próprios e regras próprias. Sendo assim, temos `Assembly x86`, para processadores da arquitetura `x86` (arquitetura estreada com o Intel 8086, em 1978, e utilizada até os dias atuais como base para a arquitetura `x86_64` - que introduz extensões para 64 bits - utilizada em todos os processadores Intel e AMD), `Assembly ARM` para processadores da arquitetura `ARM` e `Assembly MIPS` para processadores da arquitetura MIPS, por exemplo. Dentro da linguagem Assembly ARM, por exemplo, temos ainda uma série de dialetos, que variam de acordo com a versão da arquitetura e recursos proprietários implementados por cada fabricante/designer de processadores que licenciam a arquitetura. O mesmo pode acontecer para a linguagem Assembly para processadores `RISC-V`, uma arquitetura RISC de código aberto que visa concorrer como uma alternativa à proprietária ARM.

Além das instruções específicas para cada arquitetura, cada assembler (ou montador), programa que faz a conversão, ou montagem, do código Assembly para linguagem de máquina, pode apresentar uma sintaxe diferente. No munto x86, temos duas sintaxes bastante utilizadas: a sintaxe `Intel` e a sintaxe `AT&T`. A sintaxe AT&T foi criada e utilizada na AT&T no processo de criação do montador para a arquitetura x86 para o UNIX. Como foi utilizada no UNIX, ela pode ser encontrada em seus descendentes, como os BSD (FreeBSD, NetBSD e OpenBSD) e Solaris (incluindo illumos e OpenSolaris), bem como outras variações do software criado pelo Bell Labs. Outros softwares que não utilizam código da AT&T, mas que se baseiam no modelo do UNIX, como o Linux e o MINIX, também utilizam a sintaxe AT&T, utilizando, geralmente, o GNU as como montador. Já a sintaxe Intel é a sintaxe padrão de diversos outros montadores, como o Microsoft MASM, o NASM e o *flat assembler* (fasm), além de poder ser utilizada, com o uso de uma diretiva, no GNU as, cujo padrão é a sintaxe AT&T.

Abaixo, um exemplo de código em linguagem Assembly para a arquitetura x86, utilizando a sintaxe Intel (*flat assembler* - fasm). Note os mnemônicos utilizados, como `mov`, `or`, `and` e `ret`, que designam operações lógicas ou aritméticas que devem ser realizadas pelo processador:

<div align="center">

![Assembly-x86](/images/posts/2024-09-09-assembly-por-que-saber_image5.png)

Figura 1: código Assembly para x86, do projeto de sistema operacional [Hexagonix](https://github.com/hexagonix). Fonte: autor, 2024.

</div>

## Tudo bem, mas pra quê utilizamos Assembly?

A linguagem Assembly tem uma série de utilidades, tanto em software histórico, quanto em  software moderno. Vamos dar uma olhada rápida em lugares onde podemos encontrar o uso da linguagem:

### Implementação de sistemas operacionais

A linguagem Assembly é de fundamental importância para a implementação de um sistema operacional, independente da arquitetura alvo. Desde o boot até a escrita de drivers, a linguagem pode estar em vários locais, como:

* **Firmware**: parte ou todo o firmware do computador ou dispositivo móvel é escrito em Assembly, uma vez que, entre outras coisas, pode haver limitação de espaço para software escrito em linguagens de mais alto nível, aproveitando a correspondência 1:1. Como exemplos temos o `BIOS` (*Basic Input/Output System*), `UEFI` (*Unified Extensible Firmware Interface*), entre outros;
* **Gerenciadores de inicialização e código de inicialização**: em partições do tipo MBR (*Master Boot Record*), existe um setor no disco com código utilizado para identificar a partição padrão de boot, além de diversas informações gravadas, como início e fim da uma partição, tamanho e se a partição é inicializável. Além disso, no primeiro setor da partição, temos o MBR, com o código responsável por encontrar o kernel do sistema operacional, implementando um driver de sistema de arquivos, carregá-lo na memória e entregar o controle do processador a ele;
* **Kernel de sistemas oepracionais**: em um kernel, podemos ter a totalidade do seu código escrito em Assembly, bem como trechos específicos responsáveis por permitir uma interação mais estreita com o hardware do computador. Relembrando: o kernel, de forma resumida, é a porção mais privilegiada e central de um sistema operacional, responsável pela interação do usuário e do restante do software com o hardware. Como exemplos temos o [`Linux`](https://kernel.org), `XNU` (kernel do macOS e de outros sistemas operacionais da Apple, baseado no kernel Mach e no FreeBSD), `Zircon` (kernel do sistema operacional Fuchsia, da Google), etc.

Na história, diversos sistemas operacionais foram parcialmente ou totalmente implementados em linguagem Assembly, como por exemplo:

* **UNIX**: o mais famoso e influente sistema operacional da história foi totalmente implementado em Assembly para minicomputadores da série PDP (PDP-7 e PDP-11) em suas primeiras versões. Tanto o kernel quando os utilitários e shell foram escritos em Assembly. Mais tarde, seus criadores desenvolveram a linguagem C e reescreveram o sistema em C, embora o uso da linguagem Assembly tenha sido mantido em drivers e no código de inicialização do sistema. Atualmente, as primeiras versões do UNIX foram liberados sob licença livre. Assim, você pode analisar o código da primeira versão [aqui](https://github.com/felipenlunkes/unix-history-repo). No mesmo repositório, em branchs diferentes, você pode acompanhar todo o histórico da portabilidade do UNIX do Assembly para o C;

<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/6/6e/SVR1-PDP11.png">

Figura 2: UNIX System V para PDP-11 sendo executado no simulador SIMH. Fonte: Missileboi, 2022.

</div>

* **86-DOS e seus descendentes, MS-DOS e PC DOS**: o [86-DOS](https://en.wikipedia.org/wiki/86-DOS), sistema operacional desenvolvido pela Seattle Computer Products em 1980, foi desenvolvido inteiramente em Assembly x86 para o processador Intel 8086. Você pode obter e executar o 86-DOS [aqui](https://archive.org/details/86-dos-version-0.1-c-serial-11-original-disk). Ele foi comprado pela Microsoft para se tornar a base do PC DOS, após o estabelecimento de um contrato com a IBM. Mais tarde, usou a base de código para lançar o MS-DOS, sistema operacional que se manteve escrito inteiramente em Assembly até a sua última versão, 8.0, que integrava o Windows ME (Millenium Edition), lançado em 2000. A Microsoft recentemente liberou as versões 1.25, 2.0 e 4.0 do MS-DOS como software livre. Isso quer dizer que você pode consultar, estudar e criar derivados do MS-DOS utilizando os códigos em Assembly disponibilizados. Você pode acessar o repositório oficial [aqui](https://github.com/microsoft/MS-DOS);

<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/4/40/86-DOS_running_assembler_and_HEX2BIN_%28screenshot%29.png">

Figura 3: 86-DOS. Fonte: Retron, 2016.

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b0/Ms-dosdir.png">

Figura 4: MS-DOS 6. Fonte: [Przemub](https://commons.wikimedia.org/wiki/User:Przemub), 2023.

</div>

* **MenuetOS**: agora um exemplo de sistema operacional moderno escrito em Assembly x86 para a arquitetura **x86** e **x86_64**. O [MenuetOS](https://www.menuetos.net/) é totalmente escrito em Assembly x86 (incluindo kernel, drivers de dispositivo, utilitários e aplicativos). Com boot em menos de 3 segundos, o MenuetOS impressiona pela sua velocidade de inicialização e abertura de utilitários e aplicativos. Ele inclui uma série de emuladores, além de ports e de jogos populares, como Doom (1993). Além de tudo isso, todo o sistema cabe em uma imgagem de disquete (1.44 MB), possuindo uma IDE, interface gráfica, suporte a USB e webcams, multitarefa preemptiva e até mesmo um navegador de internet, mostrando o potencial e a utilidade da linguagem Assembly ainda em 2024;

<div align="center">

<img src="https://www.menuetos.net/09814.png">

Figura 5: MenuetOS. Fonte: [menuetOS.net](https://www.menuetos.net), 2024.

</div>

* **KolibriOS**: o [KolibriOS](https://kolibrios.org/en/) é um fork do MenuetOS com desenvolvimento independente desde 2004, possuindo uma série de características semelhantes, dada a origem comum, e uma série de outros recursos, como suporte a sistemas de arquivos ext2, ext3 e ext4, utilizados por distribuições Linux;

Tudo bem, mostrei acima alguns sistemas operacionais históricos e projetos de sistemas que não são utilizados amplamente. Mas temos Assembly em sistemas operacionais de produção mais recentes e em uso atualmente, veja só:

<div align="center">

<img src="https://kolibrios.org/i/slaid/slaid1.png">

Figura 6: KolibriOS. Fonte: [KolibriOS.org](kolibrios.org), 2024.

</div>

* **Linux**: existem arquivos com código Assembly, responsáveis principalmente pela inicialização do kernel em cada uma das arquiteturas. Enquanto a esmagadora maioria do código esteja em C e seja portável entre as várias arquiteturas suportadas, existe código em Assembly específico para cada arquitetura. Como exemplos, temos os arquivos [header.S](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch/x86/boot/header.S?h=v6.11-rc7), em Assembly x86, responsável pela inicialização do kernel na arquitetura x86, [entry.S](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch/arm64/kernel/entry.S?h=v6.11-rc7), responsável pela manipulação de exceções em baixo nível para a arquitetura ARM, e assim sucessivamente. Você pode acessar o diretório [arch](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch?h=v6.11-rc7) do repositório do kernel e observar os arquivos com extensão **.S**. Estes arquivos utilizados como exemplo estão disponíveis na versão 6.11-rc7 do kernel. Lembrando que a kernel utiliza o GNU as como assembler (montador), que utiliza como padrão a sintaxe AT&T;

<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/0/08/Ubuntu_23.04_Lunar_Lobster_English.png">

Figura 7: Ubuntu 23.04. Fonte: Canonical Ltd., 2023.

</div>

* **FreeBSD**: o FreeBSD também tem uma série de arquivos em Assembly responsáveis pela inicialização do kernel e interação com o hardware. Como exemplo, o arquivo [bioscall.S](https://github.com/freebsd/freebsd-src/blob/main/sys/i386/i386/bioscall.S), responsável pela chamada de funções do (*Basic Input/Output System*) na arquitetura x86. Para ARM, temos o arquivo [exception.S](https://github.com/freebsd/freebsd-src/blob/main/sys/arm64/arm64/exception.S) que, para a arquitetura ARM, é responsável pelo gerenciamento de exceções. Como no Linux, existem arquivos específicos com código Assembly para cada arquitetura;

<div align="center">

<img src="https://raw.githubusercontent.com/felipenlunkes/freebsd-config/main/img/screenshot.png">

Figura 8: FreeBSD 13.1. Fonte: Autor, 2023.

</div>

* **Windows**: apesar de apresentar código fechado, o Windows também apresenta uma série de arquivos em Assembly, principalmente na Camada de Abstração de Hardware (*Hardware Abstraction Layer* - HAL). Este componente é usado como a parte não portável do kernel, sendo específico para cada arquitetura e, muitas vezes, específico para processadores e componentes específicos dentro de uma mesma arquitetura, enquanto o restante dos componentes do kernel é portável, escrito em linguagens de alto nível como C ou C++;

### Sistemas embarcados

Em sistemas embarcados, normalmente desenvolvidos para executar tarefas determinadas, podemos ter uma série de limitações: **processamento**, **memória de execução** (RAM), **armazenamento**, entre outras. Em todos os casos, a linguagem pode fornecer binários de menor tamanho, mais otimizados e rápidos. Um exemplo de sistema embarcado seria dispositivos Arduino;

### Sistemas de tempo real

Sistemas de tempo real (*Real-time systems*) exigem uma baixíssima latência entre uma ação e uma resposta. Neste contexto, o emprego da linguagem Assembly pode ser uma alternativa.

### Assembly na *Java Virtual Machine* (JVM)

A *Java Virtual Machine*, ou JVM, nada mais é que a implementação de uma arquitetura de processador, com suas próprias instruções e, desta forma, de uma linguagem Assembly. Embora não implemente uma arquitetura física, podemos escrever código em Assembly para a JVM, que é então montado como um `bytecode Java`, a linguagem binária da arquitetura. Todo código Java é compilado para bytecode Java, a linguagem nativa da JVM, cujas instruções são então traduzidas para o sistema operacional hosdepeiro. 

Para desenvolver um código Assembly para JVM, precisamos de um montador que suporte a arquitetura (conjunto de instruções). Um exemplo é o [flat assembler](https://flatassembler.net/) g (fasm). Abaixo, exemplos de códigos Assembly JVM gerados por mim utilizando o [fasm g](https://flatassembler.net/docs.php?article=fasmg), executando o método toString():

<div align="center">

![Assembly-JVM-1](/images/posts/2024-09-09-assembly-por-que-saber_image1.png)

Figura 9: código Assembly para JVM. Fonte: autor, 2023.

![Assembly-JVM-2](/images/posts/2024-09-09-assembly-por-que-saber_image2.png)

Figura 10: código Assembly para JVM. Fonte: autor, 2023.

</div>

Como podemos ver, a linguagem Assembly está em todo lugar!

## Tá, mas por que todo programador deveria conhecer?

Depois disso tudo, não é óbvio? 

Bom, pode não ser, então vamos lá. 

As linguagens de alto nível abstraem uma série de particularidades do dispositivo, em nome da facilidade do desenvolvimento e portabilidade entre arquiteturas, uma vez que você pode escrever uma única vez um programa em C, por exemplo, e compilá-lo para uma ou mais arquiteturas alvo. Ou seja, o código é o mesmo, e quem se encarrega de encapsular os detalhes de como isso deverá ser convertido para a linguagem de máquina é o compilador, não mais o desenvolvedor. Mas isso é uma vantagem, né? A resposta é sim! Muitas vezes, o compilador, em seu algoritmo, em uma série de passos que podem dar origem a outro post, decide como será a correspondência do código em alto nível para o código em linguagem de montagem, realizando as otimizações necessárias. Mas isso tira do desenvolvedor uma série de conhecimentos e experiência. Vou falar um pouco mais sobre isso à seguir.

### Conhecimentos "perdidos" ao não se conhecer linguagens de baixo nível

Em linguagens de alto nível e gerenciadas, como `C#`, `Java` ou `Python`, por exemplo, o desenvolvedor não se encarrega de alocar a memória necessária para manter objetos em memória. Essa tarefa é executada de forma invisível ao usuário pelo [*Common Language Runtime*](https://pt.wikipedia.org/wiki/Common_Language_Runtime), no caso do C#, da *Java Virtual Machine*, ou JVM, no caso do Java, e do interpretador Python, respectivamente. Desta forma, os desenvolvedores perdem o controle do uso da memória e também podem se distanciar de uma série de etapas de otimização que eram necessárias em épocas em que a memória não era tão barata e abundante quanto hoje. Existem pesquisadores que relacionam isso à má otimização de softwares e jogos modernos, mas isso é assunto para um outro post. Em linguagens de alto nível não gerenciadas, ou seja, compiladas diretamente para código de máquina, como C ou C++, o desenvolvedor é responsável por alocar e liberar manualmente recursos de memória, embora C++ ofereça uma série de recursos e algoritmos responsáveis por gerenciar de forma mais automática a memória. Isso acaba levando o desenvolvedor a ter um contato mais "íntimo" com o dispositivo, de certa forma.

Além disso, embora o compilador quase sempre utilize a melhor otimização para a maioria de códigos mais generalistas, pode ser que determinado código exiga uma otimização específica, o que pode ser conquistado com o uso de Assembly. Isso também pode desentimular o desenvolvedor a buscar diferentes formas de otimizar o software, sempre confiando no código final gerado. 

Tudo isso pode levar a uma diminuição, por parte do desenvolvedor, da compreensão de como um código final é executado pelo processador, de como o processador, endereçamento de memória e entrada e saída funcionam ou de como ele pode atuar para melhorar a performance de seu código, de um aplicativo ou de um serviço, bem como podem levar à ideia de que a grande quantidade de memória disponível hoje significa que um código não precisa ser otimizado o máximo possível. Muito se fala sobre a criatividade dos desenvolvedores para desenvolver soluções em hardwares extremamente limitados e todo os esforço e otimização envolvidos no processo, principalmente em consoles de videogame e jogos antigos para computadores com menos de 1 MB de RAM. O uso de linguagens de alto nível, que se apoiam sempre no Assembly por baixo dos panos, pode levar a um distanciamento e desconhecimento do hardware por parte do desenvolvedor.

Agora, um exercício mental: você precisa escrever um código simples, para Linux, que exiba uma mensagem no console e termine a execução do programa. Sem o uso de bibliotecas, como a libc, que abstrai a lógica necessária para isso, e utilizando Assembly, você conseguiria escrever o código? A maioria das operações realizadas por um aplicativo, que eixgem a interação com o usuário, com o sistema operacional ou hardware são executadas via `chamadas de sistema`. Nas chamadas de sistema, o desenvolvedor precisa especificar, dentro de uma lista com vários recursos expostos ao usuário, o número da função desejada, bem como informar os seus parâmetros. Você pode exibir informações na tela, iniciar um outro utilitário, matar algum processo, tudo via chamada de sistema, cuja interface é feita em Assembly, fornecendo o número da função e os parâmetros nos registradores. Além disso, você precisa solicitar a interrupção de software para pular para o código que manipula chamadas de sistema no kernel, em Assembly x86 utilizando as instruções `int numero_da_interrupção` ou `syscall`, caso esteja em uma arquitetura x86_64 (o sistema operacional também deve suportar a chamada via instrução syscall). Agora, como ficaria o código, utilizando a sintaxe Intel:

```assembly
section .data
    hello db 'Hello, World!', 10  ;; String terminada no caractere de nova linha (10)

section .text
    global _start  ;; Ponto de entrada da imagem final que será gerada

_start:
    
    mov eax, 4     ;; sys_write (função para output, número 4)
    mov ebx, 1     ;; 1 indica a saída padrão (stdout)
    mov ecx, hello ;; Endereço da string a ser impressa
    mov edx, 13    ;; Comprimento da string (13 caracteres)
    int 0x80       ;; Faz a chamada de sistema (via interrupção de software)

    mov eax, 1     ;; sys_exit (função 1) é responsável por finalizar o processo em execução
    xor ebx, ebx   ;; Código de saída (erro)
    int 0x80       ;; Faz a chamada de sistema (via interrupção de software)
```

Isso seria equivalente às funções printf() e exit() em C, que utilizam implementações que realizam o que foi exibido assim e estão disponíveis na biblioteca padrão C (libc).

Você pode obter uma lista das principais chamadas de sistema suportadas pelo Linux [aqui](https://man7.org/linux/man-pages/man2/syscalls.2.html).

### No quê conhecer Assembly me ajudou, mesmo não trabalhando com a linguagem?

Além de conhecer a linguagem, consigo elencar uma série de situações onde conhecer Assembly (no meu caso, Assembly x86) me auxiliou na minha vida profissional e pessoal:

* **Lógica de programação**: praticar Assembly x86 foi essencial para melhorar minha lógica de programação e algoritmos, com pulos condicionais, por exemplo;
* **Conhecimentos de hardware**: praticar a linguagem me proporcional conhecimentos mais profundos de hardware como:
    * Registradores;
    * Endereço de memória, ponteiros e símbolos;
    * Avaliações condicionais;
    * Endianness;
    * Alinhamento;
    * Pilha;
    * Estruturas de controle do processador, como IDT e GDT;
    * Portas e entrada e saída.

Mesmo atuando profissionalmente com Java, uma linguagem gerenciada, o conhecimento de comoe ponteiros funcionam facilita bastante como são alocados objetos e como os parâmetros são passados em métodos, por exemplo. Entender como funciona o uso de ponteiros facilitou a minha compreensão do borrowing em Rust, por exemplo, bem como grande parte de como a memória é alocada na linguagem.

#### Como pratico Assembly?

Desde 2015, desenvolvo um projeto de sistema operacional completamente escrito em Assembly x86. Isso me possibilitou compreender e praticar diversos aspectos ligados à arquitetura, como:

* Processo de inicialização do hardware e do sistema operacional;
* Interação com o hardware, utilizando instruções responsáveis pela manipulação de memória e entrada e saída (IO);
* Alocação de memória;
* Uso das principais instruções disponíveis para a arquitetura;
* Manipulação de dados em registradores ou diretamente na memória, entre outras coisas.

Este projeto de sistema oepracional se tornou o Hexagonix, um sistema operacional que liberei como software livre sob a licenã BSD-3-Clause, que você pode encontrar [aqui](https://github.com/hexagonix). Abaixo, algumas imagens do Hexagonix, sendo executado em máquina virtual (qemu):

<div align="center">

![Hexagonix-1](/images/posts/2024-09-09-assembly-por-que-saber_image3.png)

Figura 11: Inicialização do Hexagonix em máquina virtual (qemu). Fonte: autor, 2023.

![Hexagonix-2](/images/posts/2024-09-09-assembly-por-que-saber_image4.png)

Figura 12: Utilização do Hexagonix em máquina virtual (qemu). Fonte: autor, 2023.

</div>

Além disso, desenvolvi vários códigos para a disciplina de Organização de Computadores I da Universidade Federal de Minas Gerais, utilizando a linguagem Assembly para a arquitetura MIPS. Dentre eles, desenvolvi dois códigos, sendo um iterativo e um recursivo, que podem ser encontrados [aqui](https://github.com/felipenlunkes/MIPS-asm). Os dois códigos podem ser executados utilizando o *MIPS Assembler and Runtime Simulator* (MARS), disponível [aqui](https://courses.missouristate.edu/KenVollmar/MARS/).

## Fim!

Sinta-se a vontade para entrar em contato para conversar sobre Assembly, OSDev ou qualquer outro assunto, bem como para sugerir alguma alteração, correção do conteúdo ou adição!

Como entrar em contato:

Na página [Contatos e portfólio]({{ site.baseurl }}/pages/contacts.html) você encontrará minhas redes sociais e forma de contato!

</div>