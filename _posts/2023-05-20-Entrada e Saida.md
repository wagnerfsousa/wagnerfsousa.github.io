---
title: Entrada e Saída (E/S)
date: 2023-05-25
categories: [SO, Sistemas Operacionais Modernos]
tags: [entrance, exit]     # TAG names should always be lowercase
published: false
comments: true
---
Eu estou escrevendo isso aqui para apresentar no dia 30/05/2023
É uma apresentação sobere Entrada e Saída (E/S) em Sistemas Operacionais.

## Resumo

Um sistema operacional é responsável por controlar os dispositivos de entrada e saída (E/S) de um computador, além de oferecer abstrações como processos, espaços de endereçamento e arquivos. Isso envolve emitir comandos para os dispositivos, lidar com interrupções e erros, e fornecer uma interface simples e fácil de usar entre os dispositivos e o restante do sistema. O código de E/S representa uma parte significativa do sistema operacional.

Neste capítulo, abordaremos o gerenciamento de E/S no sistema operacional. Inicialmente, discutiremos os princípios do hardware de E/S, seguido pelo software de E/S em geral. O software de E/S pode ser organizado em camadas, cada uma com uma função específica. Analisaremos cada camada para entender o que ela faz e como se relaciona com as outras.

## 1. Princípios do hardware de E/S
O hardware de E/S pode ser visto de diferentes maneiras por diferentes pessoas. Engenheiros elétricos o enxergam como componentes físicos, como chips, cabos, motores e fontes de energia. Por outro lado, programadores focam na interface que o hardware oferece ao software, como os comandos que ele aceita, as funções que realiza e os possíveis erros que podem ocorrer.

Neste livro, estamos interessados na programação dos dispositivos de E/S, e não em seu projeto, construção ou manutenção. Nosso objetivo é compreender como programar o hardware, em vez de entender seu funcionamento interno. No entanto, a programação de muitos dispositivos de E/S está intimamente ligada à sua operação interna.

Nas próximas três seções, apresentaremos uma visão geral sobre o hardware de E/S, relacionando-o à programação. Isso servirá como uma revisão e ampliação do material introdutório da Seção 1.3.

### 1.1 Dispositivos de E/S
Resumo:
Os dispositivos de E/S podem ser divididos em duas categorias principais: dispositivos de blocos e dispositivos de caractere. Os dispositivos de blocos armazenam informações em blocos de tamanho fixo, enquanto os dispositivos de caractere enviam ou recebem fluxos contínuos de caracteres. Exemplos comuns de dispositivos de blocos incluem discos rígidos, discos Blu-ray e pendrives, enquanto dispositivos como impressoras, interfaces de rede e mouses são considerados dispositivos de caractere. No entanto, existem dispositivos que não se encaixam nessa classificação, como relógios e telas mapeadas na memória. O modelo de dispositivos de blocos e de caractere é utilizado como base na criação de software de sistema operacional, como o sistema de arquivos. Os dispositivos de E/S possuem diferentes velocidades de transferência de dados, o que requer que o software seja capaz de lidar eficientemente com essas variações.

Tópicos:
1. Categorias de dispositivos de E/S
   - Dispositivos de blocos armazenam informações em blocos de tamanho fixo.
   - Dispositivos de caractere enviam ou recebem fluxos contínuos de caracteres.

2. Exemplos de dispositivos de blocos
   - Discos rígidos, discos Blu-ray, pendrives, etc.
   - Transferências de dados ocorrem em unidades de blocos inteiros.

3. Exemplos de dispositivos de caractere
   - Impressoras, interfaces de rede, mouses, etc.
   - Não possuem endereçamento nem estrutura de bloco definida.

4. Limitações da classificação
   - Existem dispositivos que não se enquadram nas categorias de blocos ou caracteres.
   - Exemplos incluem relógios e telas mapeadas na memória.

5. Uso do modelo na criação de software de sistema operacional
   - O sistema de arquivos lida apenas com dispositivos de blocos abstratos.
   - As especificidades dos dispositivos são tratadas pelos softwares de nível mais baixo.

6. Variação na velocidade dos dispositivos de E/S
   - Os dispositivos de E/S possuem diferentes taxas de transferência de dados.
   - O software deve ser capaz de lidar eficientemente com essas variações.

### 1.2 Controladores de dispositivos
Unidades de E/S consistem em componentes mecânicos e eletrônicos. O componente eletrônico, chamado de controlador ou adaptador, é responsável por gerenciar a interface entre o dispositivo e o computador. Geralmente, está presente como um chip na placa-mãe ou um cartão de circuito impresso. O componente mecânico é o próprio dispositivo físico.

O controlador e o dispositivo estão conectados por meio de um cabo, e um único controlador pode lidar com vários dispositivos semelhantes. Se a interface entre o controlador e o dispositivo seguir um padrão oficial, como SATA, SCSI, USB, Thunderbolt ou FireWire, diferentes empresas podem produzir controladores e dispositivos compatíveis com essa interface.

A interface entre o controlador e o dispositivo é de nível baixo. Por exemplo, em um disco rígido, os dados são armazenados em setores de 512 bytes, mas o que é transmitido da unidade é um fluxo serial de bits, que inclui um preâmbulo, os bits do setor e uma soma de verificação. O trabalho do controlador é converter esse fluxo serial de bits em blocos de bytes, realizar correção de erros, e em seguida, copiar o bloco para a memória principal.

O controlador de um monitor LCD também opera em um nível de bits. Ele lê os bytes da memória que contêm os caracteres a serem exibidos e gera os sinais necessários para modificar a polarização da retroiluminação dos pixels correspondentes, escrevendo-os na tela. Sem o controlador, o programador do sistema operacional teria que lidar diretamente com os campos elétricos de cada pixel. O controlador simplifica essa tarefa ao receber parâmetros iniciais do sistema operacional e cuidar da orientação dos campos elétricos.

As telas de LCD substituíram os antigos monitores CRT. Os monitores CRT utilizavam tubos de raios catódicos e campos magnéticos para gerar imagens, eram grandes, consumiam mais energia e eram menos duráveis. Em contraste, as telas de LCD oferecem alta resolução (como a tecnologia Retina) e são mais leves e eficientes. Os avanços nas telas de LCD tornaram os antigos laptops com monitores CRT obsoletos em termos de tamanho e peso.

Esses exemplos ilustram como os controladores e dispositivos de E/S trabalham juntos para permitir a comunicação entre o computador e os periféricos, seja por meio de blocos de bytes ou fluxos de caracteres, de acordo com suas características específicas.

### 1.3 E/S mapeado na memória
Controladores são dispositivos que se comunicam com a CPU e possuem registradores e buffers de dados. Esses registradores permitem ao sistema operacional enviar comandos e verificar o estado do dispositivo. Os computadores podem adotar duas abordagens para a comunicação com os controladores: E/S com porta dedicada ou E/S mapeada na memória.

No primeiro caso, cada registrador de controle possui um número de porta de E/S exclusivo. O sistema operacional utiliza instruções especiais de E/S para ler ou escrever nesses registradores, protegendo o acesso ao espaço de E/S. Já na E/S mapeada na memória, os registradores de controle são tratados como variáveis na memória, permitindo que sejam acessados diretamente em C. Essa abordagem simplifica o desenvolvimento de drivers de dispositivos, mas requer mecanismos adicionais para gerenciar o cache e a proteção de acesso.

A escolha entre os dois esquemas envolve considerações de custo e benefício. A E/S mapeada na memória facilita o acesso aos registradores e não requer proteção especial contra acessos indevidos. Além disso, as instruções que referenciam a memória também podem ser usadas para acessar os registradores de controle. Por outro lado, a E/S mapeada na memória pode apresentar problemas com o uso de cache e requer um gerenciamento complexo em sistemas com múltiplos barramentos.

Em resumo, os controladores são dispositivos que se comunicam com a CPU por meio de registradores e buffers de dados. A forma como a CPU acessa esses registradores pode ser feita por meio de E/S com porta dedicada ou E/S mapeada na memória, cada uma com vantagens e desvantagens específicas.

Tópico: Comunicação entre Controladores e CPU

* Controladores possuem registradores para comunicação com a CPU.
* Sistema operacional escreve nesses registradores para comandar dispositivos.
* Leitura dos registradores permite ao sistema operacional verificar o estado do dispositivo.
* Além dos registradores de controle, muitos dispositivos possuem buffers de dados.
* Espaço de endereçamento para memória e E/S pode ser diferente.

Dois esquemas de comunicação:

1. Portas de E/S separadas:
* Cada registrador de controle possui um número de porta de E/S.
* Instruções de E/S especiais (IN e OUT) são usadas para ler/escrever nos registradores.
* Espaços de endereçamento para memória e E/S são diferentes.

>Nesse método, os dispositivos de E/S possuem endereços de portas dedicados, separados da memória principal. <br>
Imagine as portas de E/S como janelas pelas quais o computador pode "ver" e "falar" com os dispositivos externos. Cada janela tem um número de identificação, que corresponde a um endereço de memória específico. Quando a CPU deseja interagir com um dispositivo, ela envia ou recebe informações por meio dessas janelas, usando os endereços de memória correspondentes.

2. E/S mapeada na memória:
* Registradores de controle são mapeados no espaço de memória.
* Endereços de memória são usados para acessar os registradores.
* Vantagens: acesso em linguagem de alto nível, proteção fácil contra acesso de usuário, instruções de memória podem ser usadas para acessar os registradores.
* Desvantagens: problemas com o uso de cache, necessidade de mecanismos especiais para evitar conflitos com memória, complexidade adicional do hardware em sistemas com múltiplos barramentos

>Nesse método, os dispositivos de E/S são mapeados diretamente na memória principal do computador.


### 1.4 Acesso direto à memória (DMA)
A CPU precisa endereçar os controladores de dispositivos para trocar dados, mesmo que a E/S esteja mapeada na memória. O acesso direto à memória (DMA) é um esquema usado para transferências eficientes de dados. Um controlador de DMA é programado pela CPU e possui registradores para controlar a transferência de dados. O DMA permite que o controlador de dispositivo transfira diretamente os dados para a memória, sem envolver a CPU em cada byte transferido. Isso economiza tempo e permite transferências simultâneas. O DMA pode operar no modo palavra por palavra ou em modo bloco, sendo mais eficiente, mas pode bloquear a CPU e outros dispositivos por períodos longos. Alguns controladores de DMA usam endereços físicos de memória, enquanto outros usam endereços virtuais com a ajuda da MMU. O uso de um buffer interno no controlador de disco permite verificar erros de leitura e controlar o fluxo constante de dados do disco. Nem todos os computadores usam DMA, pois em alguns casos a CPU pode realizar as transferências de forma mais rápida e econômica.

1. CPU e endereçamento de controladores de dispositivos
2. Acesso direto à memória (DMA)
3. Funcionamento do controlador de DMA
4. Transferências de dados usando DMA
5. Modos de operação do DMA: palavra por palavra e modo bloco
6. Uso de endereços físicos e virtuais na DMA
7. Uso de buffer interno no controlador de disco
8. Comparação entre DMA e operações realizadas pela CPU
9. Considerações sobre o uso de DMA em diferentes computadores.

### 1.5 Interrupções revisitadas
 Interrupções são geradas por dispositivos de entrada/saída quando eles concluem uma tarefa e são detectadas por um chip controlador na placa-mãe. Um número é usado para identificar o dispositivo e encontrar a rotina de tratamento correspondente em uma tabela chamada vetor de interrupções. As informações relevantes são armazenadas antes do serviço de interrupção, geralmente em uma pilha.

Existem interrupções precisas, que possuem propriedades bem definidas, e interrupções imprecisas, que dificultam o trabalho do sistema operacional. Máquinas superescalares podem ter interrupções precisas ou imprecisas, afetando o desempenho e a complexidade do projeto.

Principais pontos:
1. Interrupções são geradas por dispositivos de entrada/saída.
2. Um chip controlador detecta as interrupções e as direciona para o tratamento.
3. Um vetor de interrupções é usado para encontrar a rotina correspondente.
4. As informações relevantes são armazenadas antes do serviço de interrupção.
5. Existem interrupções precisas e imprecisas.
6. Interrupções imprecisas dificultam o trabalho do sistema operacional.
7. Máquinas superescalares podem ter interrupções precisas ou imprecisas, afetando o desempenho e a complexidade do projeto.


## 2. Princípios do software de E/S
Vamos agora deixar de lado por enquanto o hardware de E/S e examinar o software de E/S. Primeiro analisaremos suas metas e então as diferentes maneiras que a E/S pode ser feita do ponto de vista do sistema operacional.

### 2.1 Objetivos do software de E/S
A independência de dispositivo é um conceito fundamental no projeto de software de E/S. Significa que os programas devem ser capazes de acessar qualquer dispositivo de E/S sem a necessidade de especificá-lo previamente. Por exemplo, um programa que lê um arquivo como entrada deve ser capaz de ler arquivos em discos rígidos, DVDs ou pen drives sem precisar ser modificado para cada dispositivo diferente. O sistema operacional é responsável por lidar com as diferenças entre os dispositivos.

A nomeação uniforme também é um objetivo importante no software de E/S. Isso significa que o nome de um arquivo ou dispositivo deve ser uma cadeia de caracteres ou um número inteiro e não depender do dispositivo em si. No UNIX, por exemplo, todos os discos podem ser integrados na hierarquia do sistema de arquivos de maneiras arbitrárias, permitindo que os usuários acessem os dispositivos sem precisar saber qual nome corresponde a qual dispositivo.

>Imagine que você esteja trabalhando em um sistema operacional e precisa realizar a E/S em um dispositivo específico, como uma impressora. Em vez de se preocupar com detalhes técnicos, como endereços físicos ou portas de comunicação, você pode usar a nomeação uniforme para identificar e acessar esse dispositivo.

O tratamento de erros é outra questão essencial na E/S. Erros devem ser tratados o mais próximo possível do hardware. Se ocorrer um erro de leitura, por exemplo, o controlador deve tentar corrigi-lo ou o driver do dispositivo deve lidar com a situação, como tentar ler o bloco novamente. A recuperação de erros pode ser feita de forma transparente em níveis baixos sem que os níveis superiores sejam informados sobre o erro.

A escolha entre transferências síncronas (bloqueantes) e assíncronas (orientadas à interrupção) também é relevante. A maioria das E/S físicas é assíncrona, o que significa que a CPU inicia a transferência e continua a executar outras tarefas até que a interrupção ocorra. Porém, para facilitar a programação do usuário, as operações de E/S podem parecer bloqueantes, fazendo com que o programa seja suspenso até que os dados estejam disponíveis no buffer. No entanto, algumas aplicações de alto desempenho exigem controle detalhado da E/S e podem usar operações assíncronas.

A utilização de buffer é uma consideração importante na E/S. Muitas vezes, os dados não podem ser armazenados diretamente em seu destino final, exigindo o uso de buffers temporários. Isso é especialmente necessário quando há restrições de tempo real ou quando os dados precisam ser examinados antes de serem armazenados permanentemente. O uso de buffer envolve operações de cópia e pode afetar o desempenho da E/S.

Por fim, há a distinção entre dispositivos compartilhados e dedicados. Alguns dispositivos, como discos, podem ser usados por vários usuários simultaneamente, enquanto outros, como impressoras, devem ser dedicados a um único usuário até que a operação seja concluída. O sistema operacional deve lidar com dispositivos compartilhados e dedicados

1. Independência de dispositivo:
   - Os programas podem acessar diferentes dispositivos de E/S sem precisar serem modificados.
   - Exemplo: Um programa que lê arquivos pode funcionar em discos rígidos, DVDs ou pen drives sem precisar de alterações.

2. Nomeação uniforme:
   - Os nomes de arquivos ou dispositivos não dependem do dispositivo em si.
   - No UNIX, por exemplo, os discos podem ser integrados na hierarquia do sistema de arquivos de maneiras arbitrárias, facilitando o acesso.

3. Tratamento de erros:
   - Os erros devem ser tratados o mais próximo possível do hardware.
   - Os controladores de dispositivos tentam corrigir erros temporários, como falhas na leitura devido a poeira no cabeçote.

4. Transferências síncronas versus assíncronas:
   - As transferências síncronas bloqueiam o programa até que os dados estejam disponíveis.
   - As transferências assíncronas permitem que o programa continue executando outras tarefas enquanto aguarda os dados.

5. Uso de buffer:
   - Os dados de um dispositivo são temporariamente armazenados em um buffer antes de serem processados ou enviados ao destino final.
   - Isso permite que o sistema operacional realize operações adicionais nos dados antes de enviá-los.

6. Dispositivos compartilhados versus dedicados:
   - Alguns dispositivos podem ser usados por vários usuários simultaneamente, como discos.
   - Outros dispositivos, como impressoras, são dedicados a um único usuário por vez para evitar conflitos.

### 2.2 E/S Programada
A E/S programada é uma forma simples de realizar operações de entrada e saída, na qual a CPU realiza todo o trabalho. Nesse método, a CPU verifica repetidamente se o dispositivo está pronto para aceitar mais dados, o que é conhecido como espera ocupada ou polling. Embora seja fácil de entender e implementar, a E/S programada tem a desvantagem de manter a CPU ocupada o tempo todo, o que pode ser ineficiente em sistemas mais complexos nos quais a CPU tem outras tarefas a executar.

1. E/S programada:
   - A CPU realiza todo o trabalho de entrada e saída.
   - Exemplo: Um processo de usuário quer imprimir uma cadeia de caracteres em uma impressora serial.

2. Espera ocupada (Busy waiting) ou Polling:
   - A CPU verifica repetidamente se o dispositivo está pronto para aceitar mais dados.
   - Nesse método, a CPU fica ocupada esperando o dispositivo.

3. Desvantagens da E/S programada:
   - A CPU fica ocupada o tempo todo até que a E/S seja concluída.
   - Ineficiente quando a CPU tem outras tarefas a realizar.

A E/S programada é uma forma simples de realizar operações de entrada e saída, onde a CPU executa todas as etapas. No entanto, essa abordagem tem a desvantagem de segurar a CPU e ser ineficiente em sistemas mais complexos.

### 2.3 E/S orienteda a interrupções
A E/S orientada à interrupção é uma abordagem que permite que a CPU execute outras tarefas enquanto espera a conclusão das operações de E/S. Nesse método, as interrupções são utilizadas para notificar a CPU quando o dispositivo está pronto para aceitar mais dados. Isso evita a espera ocupada da CPU, tornando o sistema mais eficiente.

Tópicos:
1. Problemas da E/S programada
   - Espera ocupada (polling) mantém a CPU ocupada o tempo todo.
   - Ineficiente em sistemas complexos com outras tarefas para a CPU.

2. Introdução às interrupções
   - Impressora imprime caracteres à medida que chegam.
   - Tempo de impressão por caractere é maior que o tempo necessário para executar outras tarefas.

3. Funcionamento da E/S orientada à interrupção
   - Chamada de sistema para imprimir copia o buffer para o espaço do núcleo.
   - O primeiro caractere é copiado para a impressora e a CPU pode executar outro processo.
   - A impressora gera uma interrupção quando está pronta para o próximo caractere.
   - O tratador de interrupção é executado, desbloqueando o processo do usuário e continuando a impressão.

4. Vantagens da E/S orientada à interrupção
   - Permite que a CPU execute outras tarefas enquanto espera a conclusão da E/S.
   - Evita a espera ocupada da CPU, tornando o sistema mais eficiente.

### 2.4 E/S usando DMA
Resumo:
A E/S utilizando DMA (Acesso Direto à Memória) é uma estratégia que permite que o controlador de DMA realize a transferência de dados para o dispositivo de E/S, liberando a CPU para outras tarefas. Isso reduz o número de interrupções necessárias, tornando o sistema mais eficiente. No entanto, o DMA pode ser mais lento que a CPU, e em alguns casos a E/S orientada à interrupção ou programada pode ser mais adequada.

Tópicos:
1. Desvantagens da E/S orientada a interrupções
   - Interrupções ocorrem a cada caractere, desperdiçando tempo da CPU.

2. Introdução ao DMA
   - Controlador de DMA alimenta os caracteres para o dispositivo de E/S.
   - DMA executa E/S programada, com o controlador de DMA realizando o trabalho.

3. Funcionamento da E/S utilizando DMA
   - Controlador de DMA transfere os caracteres um de cada vez, sem envolver a CPU.
   - Redução do número de interrupções para uma por buffer impresso.

4. Considerações sobre o DMA
   - O controlador de DMA pode ser mais lento que a CPU principal.
   - Se o controlador de DMA não consegue operar em velocidade máxima ou a CPU está ociosa durante a espera, outras estratégias de E/S podem ser mais eficientes.
   - No geral, o DMA é vantajoso na maioria dos casos, reduzindo o tempo de interrupções e melhorando o desempenho do sistema.

## 3. Camadas do Software E/S
Resumo:
O software de E/S é organizado em quatro camadas, cada uma com uma função específica e uma interface definida para se comunicar com as camadas adjacentes. Essas camadas desempenham um papel importante no gerenciamento e controle dos dispositivos de E/S. Embora a funcionalidade e as interfaces possam variar de sistema para sistema, a discussão a seguir aborda as camadas de forma geral, começando pela camada mais baixa.

Tópicos:
1. Organização em camadas do software de E/S
   - O software de E/S é dividido em quatro camadas.
   - Cada camada possui uma função específica e interface definida.

2. Camada de controladores de dispositivo
   - Gerencia os controladores de dispositivos físicos.
   - Fornece uma interface de programação para as camadas superiores.

3. Camada de drivers de dispositivo
   - Traduz os comandos da camada superior em comandos específicos do dispositivo.
   - Controla o acesso direto aos dispositivos de E/S.

4. Camada de software de E/S do sistema operacional
   - Gerencia e coordena as operações de E/S.
   - Realiza a alocação de recursos e o agendamento de E/S.

5. Camada de software de usuário
   - Fornece interfaces e bibliotecas para que os aplicativos possam realizar operações de E/S.
   - Simplifica a interação entre os aplicativos e as camadas inferiores.

6. Variações entre sistemas
   - A funcionalidade e as interfaces podem variar de acordo com o sistema operacional e a arquitetura de hardware.
   - A discussão aborda as camadas de forma geral, sem ser específica para uma máquina.

### 3.1 Tratadores de interrupção
Resumo:
A E/S programada é útil em casos específicos, mas, na maioria das vezes, as interrupções são inevitáveis. Para minimizar o conhecimento do sistema operacional sobre as interrupções, os drivers bloqueiam-se até que a operação de E/S seja concluída e a interrupção ocorra. Quando a interrupção é disparada, a rotina de tratamento correspondente lida com ela e desbloqueia o driver. Esse modelo funciona melhor quando os drivers são estruturados como processos do núcleo, com seus próprios estados, pilhas e contadores de programa. O processamento de uma interrupção envolve uma série de passos no software após a conclusão da interrupção de hardware. Esses passos incluem salvar registros, estabelecer contexto para a rotina de tratamento da interrupção, sinalizar o controlador de interrupções, copiar registros para a tabela de processos, executar a rotina de tratamento da interrupção e escolher o próximo processo a ser executado. O processamento da interrupção não é trivial e requer uma quantidade considerável de instruções da CPU, especialmente em sistemas com memória virtual e gerenciamento de TLB e cache da CPU durante a troca entre modos núcleo e usuário.

Tópicos:
1. Uso das interrupções na maioria das E/S
   - Interrupções devem ser escondidas do sistema operacional.
   - Os drivers são bloqueados até que a interrupção ocorra.

2. Desbloqueio do driver após a interrupção
   - O driver bloqueado pode ser desbloqueado pela rotina de interrupção.
   - O modelo funciona melhor quando os drivers são processos do núcleo.

3. Processamento da interrupção
   - Passos envolvidos no processamento da interrupção:
     a. Salvamento de registros<br/>
     b. Estabelecimento de contexto para a rotina de tratamento<br/>
     c. Estabelecimento de pilha para a rotina de tratamento<br/>
     d. Sinalização do controlador de interrupções<br/>
     e. Cópia dos registros para a tabela de processos<br/>
     f. Execução da rotina de tratamento de interrupção<br/>
     g. Escolha do próximo processo a ser executado<br/>
     h. Escolha do contexto de MMU para o próximo processo<br/>
     i. Carregamento dos registradores do novo processo<br/>
     j. Início da execução do novo processo<br/>

4. Complexidade do processamento de interrupção
   - Requer um número considerável de instruções da CPU.
   - Mais instruções são necessárias em sistemas com memória virtual.
   - TLB e cache da CPU podem precisar ser gerenciados durante a troca de modos.

### 3.2 Drivers dos dispositivos
Resumo:
Neste capítulo, é discutido o papel dos controladores de dispositivos e a importância dos drivers de dispositivo para controlar a comunicação entre o sistema operacional e os dispositivos de entrada/saída (E/S). Os controladores possuem registradores que são usados para enviar comandos e ler o estado do dispositivo. Cada dispositivo de E/S requer um driver específico, que geralmente é fornecido pelo fabricante. Os drivers podem ser divididos em categorias, como dispositivos de bloco e caracteres, e devem fornecer interfaces padrão para o sistema operacional utilizar. Os drivers lidam com tarefas como inicialização do dispositivo, gerenciamento de energia, emissão de comandos e tratamento de interrupções. Além disso, é explicado o modelo de pilha de drivers, onde os drivers USB são empilhados para permitir o compartilhamento de funcionalidades comuns.

1. Papel dos controladores de dispositivos
   - Registradores do controlador: são usados para enviar comandos e ler o estado do dispositivo.

2. Importância dos drivers de dispositivo
   - Necessidade de código específico para controlar cada dispositivo.
   - Drivers fornecidos pelos fabricantes para compatibilidade com diferentes sistemas operacionais.

3. Categorias de drivers
   - Dispositivos de bloco: gerenciam acesso a dispositivos de armazenamento em bloco, como discos rígidos.
   - Dispositivos de caracteres: gerenciam acesso a dispositivos que lidam com fluxo de caracteres, como teclados e mouses.

4. Funcionalidades dos drivers de dispositivo
   - Aceitação de solicitações de leitura e escrita.
   - Inicialização do dispositivo.
   - Gerenciamento de energia e eventos.

5. Modelo de pilha de drivers
   - Pilha de drivers USB: permite o compartilhamento de funcionalidades comuns entre drivers USB.

6. Acesso ao hardware do dispositivo
   - Drivers no espaço do usuário vs. no núcleo do sistema operacional.
   - Construção de sistemas altamente confiáveis.

7. Modelos de carregamento de drivers
   - Modelos estáticos em sistemas UNIX.
   - Carregamento dinâmico de drivers em sistemas modernos.

8. Tarefas executadas pelos drivers de dispositivo
   - Verificação de parâmetros de entrada.
   - Tradução de termos abstratos para concretos.
   - Verificação de disponibilidade do dispositivo.
   - Emissão de comandos e escrita nos registradores do controlador.
   - Tratamento de interrupções e bloqueio do driver.
   - Verificação de erros e retorno de informações.

9. Desafios e considerações adicionais
   - Reentrância dos drivers.
   - Remoção e adição de dispositivos durante a execução.
   - Interação dos drivers com o núcleo do sistema operacional.

### 3.3 Software de E/S independente de dispositivo
Resumo:

O texto aborda a questão do software de Entrada/Saída (E/S) em um sistema operacional. Ele destaca que parte do software de E/S é específico para cada dispositivo, enquanto outras partes são independentes. O limite entre os drivers e o software independente do dispositivo depende do sistema e do dispositivo, e algumas funções podem ser realizadas tanto de forma independente quanto nos drivers. O software independente do dispositivo tem a função de realizar as funções de E/S comuns a todos os dispositivos e fornecer uma interface uniforme para o software no nível do usuário.

>Imagine que você esteja desenvolvendo um sistema operacional que precisa suportar diferentes tipos de dispositivos de armazenamento, como discos rígidos e pen drives. Cada dispositivo tem suas próprias peculiaridades e detalhes técnicos, mas há algumas operações básicas que são comuns a todos eles, como ler e gravar dados.

Nesse caso, o software de E/S independente de dispositivo entra em ação. Ele é responsável por implementar as funções de leitura e gravação genéricas, que podem ser aplicadas a qualquer dispositivo de armazenamento, independentemente das diferenças técnicas entre eles.

Tópicos do texto:
1. Interface uniforme para os drivers dos dispositivos
   - Necessidade de ter interfaces semelhantes para todos os dispositivos
   - Diferença entre interfaces diferentes e interfaces iguais para os drivers
   - Definição de funções que o driver deve fornecer para cada classe de dispositivo

2. Utilização de buffer
   - Estratégias de utilização de buffer para lidar com a entrada e saída de dados
   - Problemas relacionados à utilização de buffer, como a paginação para o disco e o uso excessivo de memória
   - Técnicas como buffer duplo e buffer circular para otimizar a utilização de buffer

3. Nomeação e proteção de dispositivos de E/S
   - Mapeamento de nomes de dispositivos simbólicos para os drivers apropriados
   - Utilização de objetos nomeados e regras de proteção para os dispositivos de E/S

4. Relatório de erros
   - Tratamento de erros de E/S, tanto específicos de dispositivos quanto erros de programação
   - Modelo independente de dispositivo para o tratamento de erros de E/S.

### 3.4 Software de E/S do espaço do usuário
Resumo:

A maior parte do software de entrada/saída (E/S) está dentro do sistema operacional, mas uma pequena parte dele consiste em bibliotecas vinculadas aos programas do usuário, e alguns programas inteiros são executados fora do núcleo. As chamadas de sistema, incluindo as chamadas de E/S, geralmente são feitas por meio de rotinas de biblioteca. Essas rotinas de biblioteca são responsáveis por organizar os parâmetros corretamente para as chamadas de sistema. Além disso, existem outras rotinas de E/S que realizam o trabalho real, como a formatação de entrada e saída, e essas são executadas pelas rotinas de biblioteca.

Um exemplo de rotina de biblioteca é o printf em linguagem C, que recebe uma cadeia de caracteres e variáveis como entrada, constrói uma cadeia de caracteres formatada e, em seguida, chama a função write para enviar essa cadeia para a saída. Da mesma forma, a função scanf realiza uma leitura de entrada e armazena os valores lidos nas variáveis correspondentes, seguindo um formato especificado.

No entanto, nem todo o software de E/S no nível do usuário consiste apenas em rotinas de biblioteca. Outra categoria importante é o sistema de spooling, que é usado para lidar com dispositivos de E/S dedicados em sistemas de multiprogramação. O spooling evita que um dispositivo seja monopolizado por um processo, permitindo que outros processos usem o dispositivo. Por exemplo, para imprimir um arquivo em uma impressora, o arquivo é colocado em um diretório de spooling e um processo especial chamado "daemon" é responsável por imprimir os arquivos desse diretório. Assim, outros processos podem enviar arquivos para impressão sem bloquear o uso da impressora.

O spooling também é utilizado em outras situações de E/S, como transferência de arquivos por rede. Nesses casos, um daemon de rede é responsável por retirar arquivos de um diretório de spooling e transmiti-los pela rede. Um exemplo famoso de transmissão "spooled" de arquivos é o sistema de notícias USENET, onde os usuários podem postar mensagens que são colocadas em um diretório de spooling para serem transmitidas para outras máquinas posteriormente.

A Figura 5.17 apresenta um resumo do sistema de E/S, mostrando as diferentes camadas e suas funções principais. Essas camadas incluem o hardware, tratadores de interrupção, drivers de dispositivo, software independente de dispositivos e processos do usuário. O fluxo de controle ocorre quando um programa do usuário solicita uma operação de E/S, e o sistema operacional busca o recurso necessário, seja na cache do buffer ou através do driver do dispositivo. O processo é bloqueado até que a operação seja concluída e os dados estejam disponíveis. Quando o dispositivo de E/S termina, uma interrupção é gerada, o tratador de interrupção identifica o dispositivo e desperta o processo para continuar sua execução.

Tópicos do texto:
1. O papel das bibliotecas de E/S vinculadas aos programas do usuário.
2. A formatação de entrada e saída realizada pelas rotinas de biblioteca.
3. Exemplos de rotinas de biblioteca, como printf e scanf.
4. O uso do spooling para lidar com dispositivos dedicados, como impressoras.
5. O papel dos daemons e diretórios de spooling no spooling.
6. O uso do spooling em transferência de arquivos por rede e no sistema de notícias USENET.
7. A estrutura do sistema de E/S, incluindo camadas como hardware, tratadores de interrupção, drivers de dispositivo, software independente de dispositivos e processos do usuário.
8. O fluxo de controle durante as operações de E/S.<br/>

disclaimer: text genereate by IA and revised and corrected by human
