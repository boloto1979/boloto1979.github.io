---
layout: post
title: Introdução ao Anti-Debugging
image: /assets/img/anti-debugging-intro.png
description: "Essa é uma série de artigos “The Debug Breaker” que visa introduzir e compartilhar técnicas de anti-debugging voltadas para malware development (porém não limitadas a isso). Sendo o primeiro artigo “Introdução ao Anti-Debugging” uma apresentação teórica e prática sobre análise dinâmica, funcionamento dos debuggers e técnicas de anti-debugging utilizando principalmente de interações com essas ferramentas."
---

# TL;DR

Essa é uma série de artigos “The Debug Breaker” que visa introduzir e compartilhar técnicas de anti-debugging voltadas para malware development (porém não limitadas a isso). Sendo o primeiro artigo “Introdução ao Anti-Debugging” uma apresentação teórica e prática sobre análise dinâmica, funcionamento dos debuggers e técnicas de anti-debugging utilizando principalmente de interações com essas ferramentas.

# Introdução

O mundo da cibersegurança evolui a cada dia, com inúmeras vulnerabilidades e mecanismos de defesa surgindo a todo instante. Como costumam dizer, é uma “briga de gato e rato”, onde por um lado pesquisadores buscam proteger sistemas, do outro *malfeitores* buscam por meios de quebra-los.

Uma área mais específica da cibersegurança na qual essa descrição se encaixa perfeitamente é a análise e desenvolvimento de malwares, isso pois para que hoje se tenha meios de defesa contra um determinado malware, foi um dia necessário efetuar a análise sobre o comportamento e funcionamento daquele.

Entretanto, assim como existem mecanismos de defesa para sistemas, malwares mundo afora também criaram meios de se defender dos antivírus, ou melhor, dos analistas que buscarão entender seu funcionamento. Quais são esses meios e como e por que um malware buscaria se “proteger” de um analista de segurança?

# Sobre os Mecanismos de Defesa

Como dissertado nos artigos [Técnicas de Ofuscação, pt. 1 e 2](https://inferi.zip/paper/tecnicas-de-ofuscacao-em-c-pt-1), uma das formas de um malware esconder suas intenções é através da ofuscação, seja das estruturas que o compõem como funções e instruções quanto dos dados que esse busca em um sistema. Entretanto, apesar dessa técnica ser relativamente eficaz contra mecanismos de defesa baseados em assinatura, para soluções avançadas como EDRs e análises dinâmicas, está longe de ser o suficiente.

## Análise Dinâmica

Hoje, uma das formas mais avançadas de detecção se dá através da análise dinâmica, geralmente feita de forma automatizada, mas nem sempre limitada a isso. Nessa técnica a aplicação é executada primeiramente em um ambiente controlado (geralmente uma VM ou sandbox) e monitorado, sendo suas ações observadas e classificadas como maliciosas ou não, dependendo da interação com os recursos do sistema. Hoje soluções avançadas como EDRs são capaz fazer essa análise de forma automatizada, porém em casos mais severos, uma analista de malware é o encarregado por essa tarefa. Nesse cenário, o analista busca compreender não só estaticamente a estrutura da aplicação como bibliotecas usadas, quanto também seu funcionamento em tempo execução, onde faz-se o uso geralmente de debuggers.

# Debuggers

Os debuggers, também chamados de depuradores, são ferramentas extremamente poderosas que em princíprio, servem para entender o funcionamento de um determinado programa, buscar por erros como memory leak, etc. Através dessas ferramentas o analista possui quase que um controle total da aplicação, podendo entender seu funcionamento em nível de código (assembly), modificar o fluxo de execução, entender os recursos do sistema em uso, além é claro de um dos recursos mais importantes, definir breakpoints.

## Breakpoints

Os breakpoints são um dos recursos mais poderosos dos debuggers, sendo basicamente indicações  de que a execução do programa deve ser pausada em uma determinada instrução ou endereço de memória. Majoritariamente existem dois tipos de breakpoints, os chamados hardware breakpoints e os software breakpoints, sendo esse último o qual focaremos por agora.

Um debugger é capaz de definir um breakpoint em um programa modificando uma de suas instruções para `int 0x3`, que possui o opcode `0xCC`. Essa instrução quando executada, emite um sinal (exceção) do tipo `BREAKPOINT_EXCEPTION`, a execução é pausada e o controle sobre a aplicação retoma ao debugger até que esse sinal seja tratado. Por ora não focarei tanto no funcionamento interno dos debuggers, entretanto entender breakpoints até esse ponto é essencial para os tópicos seguintes.

# Anti-Debugging

Como você deve imaginar, anti-debugging é uma das técnicas utilizadas por programadores para ou manter a integridade de um código ou esconder suas intenções. É comum o uso na indústria de jogos por exemplo, porém hoje focaremos no uso voltado ao desenvolvimento de malwares.

Essa técnica visa utilizar de recursos do sistema ou interações com debuggers para evitar o debugging de um programa, dessa forma evitando que seus segredos de códigos sejam revelados.

## IsDebuggerPresent()

Uma abordagem bastante simples e intuitiva que podemos fazer inicialmente para evitar o debugging de um programa é utilizar de chamadas de API. A própria API do Windows disponibiliza uma função chamada `IsDebuggerPresent()` para validar a presença de um debugger atrelado ao processo. Essa função possui o seguinte protótipo: `BOOL IsDebuggerPresent();`

Abaixo um exemplo de uso:

```c
#include <stdio.h>
#include <Windows.h>

int main(void) {
	if(IsDebuggerPresent()) {
		puts("Debugger detected!");
		return 1;
	}
	puts("You won!");
	return 0;
}

```

No código acima, caso a função retorne 1, indica a presença de um debugger, logo o bloco do if é executado, finalizando a execução do programa. Caso contrário, o bloco é ignorado e a mensagem “You won!” é exibida.

Infelizmente essa é uma das técnicas mais fáceis de bypassar, já que debuggers modernos são capazes de modificar a flag da PEB consultada por essa função.

## INT 3

Caso tenha feito uma leitura focada, deve lembrar que INT 3 é a instrução utilizada por debuggers para indicar um breakpoint. Assim como debuggers podem injetar breakpoints, programadores podem também arbitrariamente inserir essa instrução em seus códigos visando o anti-debugging.

<img src="/assets/img/tdb0-1.png">

*Na imagem acima uma demonstração da instrução sendo interpretada pelo debugger, mesmo não tendo sido inserida por ele.*

Acima, apesar do breakpoint ter sido inserido pelo programador, foi interpretado pelo debugger como um breakpoint válido. Dessa forma, a exceção `BREAKPOINT_EXCEPTION` é levantada ao debugger para que seja tratada.

Para que tornemos essa técnica em algo útil, precisamos validar se a exceção foi tratada, o que indicaria nesse caso, a presença de um debugger, mas como fazê-lo?

## MSVC Structured Exception Handling

Através de uma extensão do MSVC chamada Structured Exception Handling, podemos tratar certas exceções como acessos de memória inválidos, divisões por zero e, a pérola do dia, exceções de breakpoints. A sintaxe SEH é bastante simples:

```c
__try {
	// Bloco a ser executado e testado
} __except(/* Exceção a ser tratada */) {
	// Bloco que trata a exceção
}

```

No nosso caso, no bloco `__try` inserimos o código com a instrução do breakpoint `int 3`, já em `__except`, indicamos para que o controle seja mantido no exception handler através da macro `EXCEPTION_EXECUTE_HANDLER`, o que nesse caso indica a não presença de um debugger.

Pode parecer um pouco confuso, mas colocando em ordem, temos basicamente o seguinte:

1. O programa é executado
2. Caso haja um debugger e esse encontre a instrução 0xCC (breakpoint), a exceção é levantada ao debugger, que trata e retoma o controle para aplicação. Dessa forma o bloco `__try` é executado normalmente, indicando a presença de um debugger.
3. Caso não haja um debugger, a instrução 0xCC ainda sim é levantada, porém o bloco `__except` é o encarregado de tratá-la, indicando então a não presença de um debugger.

```c
#include <stdio.h>
#include <Windows.h>

int main(void) {
    __try {
        __asm int 3;
        puts("Debugger detected!");
    }
    __except (EXCEPTION_EXECUTE_HANDLER) {
        puts("No debugger detected!");
    }
    return 0;
}

```

Execução sem debugger:

<img src="/assets/img/tdb0-2.png">

*Na imagem acima, a exceção é tratada internamente pela aplicação, logo em teoria não há debugger.*

Execução com debugger:

<img src="/assets/img/tdb0-3.png">

*Na imagem acima a exceção é levantada ao debugger, que a trata e retoma o controle para a aplicação.*

## INT 0x2D

Outra instrução que funciona de uma forma semelhante é `int 0x2d`, a qual também levanta a exceção `EXCEPTION_BREAKPOINT`, porém incrementando EIP e apontando para a próxima instrução. Essa instrução é interessante devido ao seu comportamento com EIP, já que ao apontar para a próxima instrução pode ser perigoso ao debugger, além de que com ela podemos em teoria escolher qual exceção tratar no exception handler ao especificar uma instrução para tal.

Abaixo um breve exemplo onde é tratada a exceção `EXCEPTION_INT_DIVIDE_BY_ZERO` (levantada `i = i / 0`) ao invés de `BREAKPOINT_EXCEPTION`:

```c
#include <stdio.h>
#include <Windows.h>

int main(void) {
    int i = 1;
    __try {
        __asm int 2dh;
        i = i / 0; // EXCEPTION_INT_DIVIDE_BY_ZERO
        puts("Debugger detected! BREAKPOINT_EXCEPTION");
    }
    __except (EXCEPTION_EXECUTE_HANDLER) {
        if (GetExceptionCode() == EXCEPTION_INT_DIVIDE_BY_ZERO) {
            puts("Debugger detected! EXCEPTION_INT_DIVIDE_BY_ZERO");
            exit(1);
        }
        puts("No debugger detected!");
    }
    return 0;
}

```

<img src="/assets/img/tdb0-4.png">

# Conclusão

Anti-debugging é com certeza uma área extremamente complexa e gigante, há muito o que aprender e explorar. Conhecer não só o sistema operacional mas como também o funcionamento dessas aplicações como debuggers é essencial para técnicas desse tipo, nos próximos artigos comentarei sobre outras baseadas em recursos do sistema operacional e timing. Por hoje chego ao fim desse artigo, espero que tenha compreendido e acrescentado em seu arsenal de ideias. Até a próxima!
