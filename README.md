# Comandos OSPF

    # Recursos e características de OSPF
        OSPFv2 é usado para redes IPv4. 
        OSPFv3 é usado para redes IPv6.

    # Componentes do OSPF
            Pacote Hello
            Database Description
            Link-State Request
            Link-State Update
            Pacote de confirmação de link-state
            ![Alt text](image.png)

        As mensagens OSPF são usadas para criar e manter três bancos de dados OSPF, da seguinte forma:

        Banco de dados de adjacência - Isso cria a tabela de vizinhos.
            - Mostrar tabela de vizinhos
                show ip ospf neighbor

        Banco de dados de estado de link (LSDB) - cria a tabela de topologia.
            - Mostrar tabela de Topologia
                show ip ospf database

        Banco de dados de encaminhamento - Isso cria a tabela de roteamento.
            - Mostrar tabela de roteamento
                show ip route
            

    # Operação Link-State - etapas no processo de roteamento de estado de link

        - Estabelecer Adjacências de Vizinhos
            Os roteadores habilitados para OSPF devem se reconhecer na rede antes que possam compartilhar informações. Um roteador OSPF ativado envia pacotes Hello de todas as interfaces OSPF permitidas para determinar se os vizinhos estão presentes nesses links. Se um vizinho estiver presente, o roteador com OSPF ativado tenta estabelecer uma adjacência de vizinho com aquele vizinho
        
        - Trocar anúncios de estado de link
            Depois que as adjacências são estabelecidas, os roteadores trocam anúncios de estado de link (LSAs). Os LSAs contêm o estado e o custo de cada link diretamente conectado. Os roteadores inundam o LSAs para os vizinhos adjacentes. Os vizinhos adjacentes que recebem o LSA inundam imediatamente todos os outros vizinhos diretamente conectados, até que todos os roteadores da área tenham todos os LSAs.

        - Criar o Banco de Dados de Estado do Link
            Depois que os LSAs são recebidos, os roteadores habilitados para OSPF criam a tabela de topologia (LSDB) com base nos LSAs recebidos. Esse banco de dados contém todas as informações sobre a topologia da área.

        - Executar o algoritmo SPF
            Os roteadores, em seguida, executam o algoritmo SPF. As engrenagens na figura para esta etapa são usadas para indicar a execução do algoritmo SPF. O algoritmo SPF cria a árvore SPF.
            ![Alt text](image-1.png)

        - Escolha a melhor rota
           Após a construção da árvore SPF, os melhores caminhos para cada rede são oferecidos para a tabela de roteamento IP. A rota será inserida na tabela de roteamento, a menos que haja uma origem de rota para a mesma rede com uma distância administrativa menor, como uma rota estática. As decisões de roteamento são feitas com base nas entradas da tabela de roteamento. 

    
    # OSPF de área única e multi área
        Para tornar o OSPF mais eficiente e escalável, o OSPF suporta o roteamento hierárquico com o uso de áreas. Uma área OSPF é um grupo de roteadores que compartilham as mesmas informações de link-state nos LSDBs. O OSPF pode ser implementado de uma das duas maneiras a seguir:

        OSPF de área única - Todos os roteadores estão em uma área. A melhor prática é usar a área 0.
        ![Alt text](image-2.png)
        OSPF multi área - OSPF é implementado usando várias áreas, de maneira hierárquica. Todas as áreas devem se conectar à área de backbone (área 0). Os roteadores interconectando as áreas são denominados Area Border Routers (ABRs).
        ![Alt text](image-3.png)
        O foco deste módulo está no OSPFv2 de área única.

    # OSPF multi área
        Com o OSPF de várias áreas, um domínio de roteamento grande pode ser dividido em áreas menores, para oferecer suporte ao roteamento hierárquico. O roteamento ainda ocorre entre as áreas (roteamento entre áreas), enquanto muitas das operações de roteamento intensivas do processador, como recalcular o banco de dados, são mantidas em uma área.

        Por exemplo, sempre que um roteador recebe novas informações sobre mudanças de topologia na área (incluindo a adição, exclusão ou modificação de um link), o roteador deve executar novamente o algoritmo SPF, criar uma nova árvore SPF e atualizar a tabela de roteamento. O algoritmo SPF exige muito da CPU e o tempo necessário para o cálculo depende do tamanho da área.

        Observação: Os roteadores em outras áreas recebem atualizações sobre alterações na topologia, mas esses roteadores somente atualizam a tabela de roteamento, não executam novamente o algoritmo SPF.

        Muitos roteadores em uma área tornariam o LSDBs muito grande e aumentaria a carga da CPU. Portanto, organizar os roteadores em áreas divide efetivamente um banco de dados potencialmente grande em bancos de dados menores e mais gerenciáveis.

        As opções de design de topologia hierárquica com OSPF de várias áreas podem oferecer as seguintes vantagens:

            - Tabelas de roteamento menores - As tabelas são menores porque há menos entradas de tabela de roteamento. Isso ocorre porque os endereços de rede podem ser resumidos entre áreas. O resumo da rota não é ativado por padrão.
            - Sobrecarga de atualização do estado do link reduzida - Projetar OSPF de várias áreas com áreas menores minimiza os requisitos de processamento e memória.
            - Frequência reduzida de cálculos de SPF - O OSPF de várias áreas localiza o impacto de uma alteração de topologia em uma área. Por exemplo, minimiza o impacto de atualização de roteamento, pois a inundação LSA para na borda de área.
            ![Alt text](image-4.png)

    

    # OSPFv3
        OSPFv3 é o equivalente do OSPFv2 para trocar prefixos do IPv6. Lembre-se que no IPv6 o endereço de rede é conhecido como prefixo e a máscara de sub-rede é chamada de tamanho do prefixo.
        Semelhante à sua contraparte do IPv4, o OSPFv3 troca informações de roteamento para preencher a tabela de roteamento IPv6 com prefixos remotos.
        Observação: Com o recurso Famílias de Endereços OSPFv3, o OSPFv3 inclui suporte para IPv4 e IPv6. As linhas de endereço de OSPF não fazem parte do escopo deste currículo.
        O OSPFv2 funcionada na camada de rede do IPv4, comunicando com outros pares IPv4 do OSPF e anunciando somente as rotas IPv4.
        O OSPFv3 tem a mesma funcionalidade que OSPFv2, mas usa o IPv6 como transporte de camada de rede, comunicando com os pares do OSPFv3 e anunciando rotas do IPv6. O OSPFv3 também usa o algoritmo SPF como mecanismo de computação para determinar os melhores caminhos em todo o domínio de roteamento.
        O OSPFv3 possui processos separados de sua contraparte do IPv4. Os processos e as operações são basicamente iguais aos do protocolo de roteamento do IPv4, mas são executados independentemente. OSPFv2 e OSPFv3 têm tabelas de adjacência separadas, tabelas de topologia do OSPF e tabelas de roteamento IP, como mostrado na figura.
        Os comandos de configuração e verificação do OSPFv3 são semelhantes aos usados no OSPFv2.
        ![Alt text](image-5.png)


    # Pacotes OSPF
        Tipos de pacotes do OSPF
            Pacotes de estado de link são as ferramentas usadas pelo OSPF para ajudar a determinar a rota mais rápida disponível para um pacote. O OSPF usa os seguintes LSP (link-state packets) para estabelecer e manter adjacências vizinhas e trocar atualizações de roteamento. Cada pacote serve a uma finalidade específica no processo de roteamento OSPF, da seguinte maneira:

            Tipo 1:Pacote de Hello - Isso é usado para estabelecer e manter adjacência com outros roteadores OSPF.
            Tipo 2:Pacote Descrição do Banco de Dados (DBD) - Ele contém uma lista abreviada do LSDB do roteador de envio e é usado pelo recebimento de roteadores para verificar o LSDB local. O LSDB deve ser idêntico em todos os roteadores link-state em uma área para criar uma árvore SPF precisa.
            Tipo 3:Pacote Link-State Request (LSR) - Os roteadores receptores podem solicitar mais informações sobre qualquer entrada no DBD enviando um LSR.
            Tipo 4:Pacote Link-State Update (LSU) - Isso é usado para responder aos LSRs e anunciar novas informações. As LSUs contêm vários tipos diferentes de LSAs.
            Tipo 5:Pacote Link-State Acknowledgement (LSAck) - Quando uma LSU é recebida, o roteador envia um LSAck para confirmar o recebimento da LSU. O campo de dados de LSAck está vazio.
            A tabela resume os cinco tipos diferentes de LSPs usados pelo OSPFv2. O OSPFv3 tem tipos similares de pacote.

            Legenda da tabela
                Tipo	Nome do pacote	                                 Descrição
                1	        Hello	                     Descobre vizinhos e cria adjacências entre eles
                2	 Database Description (DBD)	         Verifica a sincronização de banco de dados entre roteadores
                3	 Link-State Request (LSR)	         Solicita registros específicos de link-state de roteador para roteador
                4	 Link-State Update (LSU)	         Envia registros de link-state especificamente solicitados
                5	 Link-State Acknowledgment (LSAck)	 Confirma os outros tipos de pacotes

    # Atualizações Link State
        Os roteadores trocam inicialmente pacotes DBD Tipo 2, que é uma lista abreviada do LSDB do roteador remetente. Ele é usado pelo recebimento de roteadores para verificar com o LSDB local.
        Um pacote de LSR tipo 3 é usado pelos roteadores de recepção para solicitar mais informações sobre uma entrada na DBD.
        O pacote de LSU tipo 4 é usado para responder a um pacote de LSR.
        Um pacote tipo 5 é usado para confirmar o recebimento de um LSU tipo 4.
        LSUs também são usados para encaminhar atualizações de roteamento do OSPF, como mudanças de link. Especificamente, um pacote LSU pode conter 11 tipos diferentes de LSAs OSPFv2, com alguns dos mais comuns mostrados na figura. O OSPFv3 renomeou vários LSAs e também contém dois LSAs adicionais.
        Observação: Às vezes, a diferença entre os termos LSU e LSA pode ser confusa porque esses termos são frequentemente usados \u200b\u200bde forma intercambiável. No entanto, uma LSU contém um ou mais LSAs.
        ![Alt text](image-6.png)

    # Pacote Hello
        O pacote OSPF tipo 1 é o pacote de Hello. Os pacotes Hello são usados para:
        Descobrir vizinhos do OSPF e estabelecer adjacências de vizinhos.
        Anunciar parâmetros nos quais dois roteadores devem concordar para se tornarem vizinhos.
        Escolha o roteador designado (DR) e o roteador designado para backup (BDR) em redes multiacesso como Ethernet. Os links de ponto a ponto não exigem DR ou BDR.    

        - Conteúdo do pacote Hello do OSPF
            ![Alt text](image-7.png)


    # Configuração OSPFv2 enm área única

        Objetivo do módulo: Implementar o OSPFv2 de área única nas redes de multiacesso broadcast e ponto a ponto.

                                 Legenda da tabela
            Título do Tópico	                            Objetivo do Tópico
            ID do roteador OSPF	                Configurar uma ID de roteador OSPF.
            Redes OSPF ponto a ponto	        Configurar OSPFv2 de área única em uma rede ponto a ponto.
            Redes OSPF de multiacesso	        Configurar a prioridade da interface OSPF para influenciar a eleição de DR/BDR em uma rede multiacesso.
            Modificar OSPFv2 de área única	    Implementar modificações para alterar a operação de OSPFv2 de área única.
            Propagação de rota padrão	        Configurar o OSPF para propagar uma rota padrão.
            Verificar o OSPFv2 de área única	Verificar uma implementação de OSPFv2 de área única.

    
    # Topologia de Referência de OSPF
       Para começar, este tópico discute a base na qual o OSPF baseia todo seu processo, o ID do roteador OSPF.
        A figura mostra a topologia usada para configurar o OSPFv2 nesse módulo. Os roteadores na topologia têm uma configuração inicial, incluindo endereços de interface. No momento, não há roteamento estático ou dinâmico configurado em qualquer um dos roteadores. Todas as interfaces em R1, R2 e R3 (exceto o loopback 1 no R2) estão dentro da área de backbone do OSPF. O roteador ISP é usado como gateway para a Internet do domínio de roteamento.
        Observação: Nesta topologia, a interface de loopback é usada para simular o link WAN à Internet e uma LAN conectada a cada roteador. Isso é feito para permitir que essa topologia seja duplicada para fins de demonstração em roteadores que têm apenas duas interfaces Gigabit Ethernet. 

        ![Alt text](image-8.png)
    
    # Modo de configuração do roteador para OSPF
        OSPFv2 é ativado usando o comando do modo de configuração global router ospf process-id, como mostrado na janela de comando para R1. O valor process-id representa um número entre 1 e 65.535 e é selecionado pelo administrador da rede. O valor process-id é localmente significativo, o que significa que ele não precisa ter o mesmo valor nos outros roteadores OSPF para estabelecer adjacências com esses vizinhos. É considerado prática recomendada usar o mesmo process-id em todos os roteadores OSPF.

        Depois de inserir o comando router ospf process-id, o roteador entra no modo de configuração do roteador, conforme indicado pelo R1(config-router)# prompt. Entre com o sintal de interrogação (? ), para ver todos os comandos disponíveis neste modo. A lista de comandos mostrada aqui foi alterada para exibir apenas os comandos relevantes para este módulo.

        R1(config)# router ospf 10
        R1(config-router)# ?
        area                   OSPF area parameters
        auto-cost              Calculate OSPF interface cost according to bandwidth
        default-information    Control distribution of default information
        distance               Define an administrative distance
        exit                   Exit from routing protocol configuration mode
        log-adjacency-changes  Log changes in adjacency state
        neighbor               Specify a neighbor router
        network                Enable routing on an IP network
        no                     Negate a command or set its defaults
        passive-interface      Suppress routing updates on an interface
        redistribute           Redistribute information from another routing protocol
        router-id              router-id for this OSPF process
        R1(config-router)#

    # IDs do roteador
        Um ID de roteador OSPF é um valor de 32 bits, representado como um endereço IPv4. O ID do roteador é usado para identificar exclusivamente um roteador OSPF. Todos os pacotes OSPF incluem o ID do roteador de origem. Cada roteador requer uma ID de roteador para participar de um domínio OSPF. A ID do roteador pode ser definida por um administrador ou atribuída automaticamente pelo roteador. O ID do roteador é usado por um roteador habilitado para OSPF para fazer o seguinte:

        Participar da sincronização de bancos de dados OSPF — Durante o estado do Exchange, o roteador com o ID de roteador mais alto enviará seus pacotes de descritor de banco de dados (DBD) primeiro.
        Participar da eleição do roteador designado (DR) - Em um ambiente LAN multiacesso, o roteador com o ID de roteador mais alto é eleito o DR. O dispositivo de roteamento com o segundo ID de roteador mais alto é eleito o roteador designado de backup (BDR).
        Observação: O processo eleitoral de DR e BDR é discutido com mais detalhes mais adiante neste módulo.

    # Ordem de precedência da ID do roteador
        Mas como o roteador determina a ID do roteador? Como ilustrado na figura, os roteadores Cisco derivam a ID do roteador com base em um dos três critérios, na seguinte ordem preferencial:

        - O ID do roteador é configurado explicitamente usando o comando do modo de configuração do roteador OSPF router-id rid . O valor rid é qualquer valor de 32 bits expresso como um endereço IPv4. Este é o método recomendado para atribuir uma ID do roteador.
        - Se a ID do roteador não for configurada explicitamente, o roteador escolhe o maior endereço IPv4 de qualquer interface de loopback configurada. Essa é a melhor alternativa para atribuir uma ID do roteador.
        - Se nenhuma interface de loopback estiver configurada, o roteador escolhe o maior endereço IPv4 ativo de algumas das suas interfaces físicas. Esse é o método menos recomendado, pois, para os administradores, isso dificulta a diferenciação entre roteadores específicos.

        ![Alt text](image-9.png)

    # Configurar uma interface de loopback como o ID do roteador
        Na topologia de referência, somente as interfaces físicas são configuradas e ativas. As interfaces de loopback não foram configuradas. Quando o roteamento OSPF está habilitado no roteador, os roteadores escolhem o seguinte endereço IPv4 mais ativo configurado como o ID do roteador.

        R1: 10.1.1.14 (G0/0/1)
        R2: 10.1.1.9 (G0/0/1)
        R3: 10.1.1.13 (G0/0/0)
        Observação:O OSPF não precisa ser habilitado em uma interface para que essa interface seja escolhida como o ID do roteador.
        Em vez de depender da interface física, o ID do roteador pode ser atribuído a uma interface de loopback. Normalmente, o endereço IPv4 para esse tipo de interface de loopback deve ser configurado usando uma máscara de sub-rede de 32 bits (255.255.255.255). Isso cria efetivamente uma rota de host. Uma rota de host de 32 bits não seria anunciada como uma rota para outros roteadores OSPF.
        O exemplo mostra como configurar uma interface de loopback em R1. Assumindo que o ID do roteador não foi explicitamente configurado ou aprendido anteriormente, o R1 usará o endereço IPv4 1.1.1.1 como ID do roteador. Suponha que R1 ainda não tenha aprendido um ID de roteador.

        R1(config-if)# interface Loopback 1
        R1(config-if)# ip address 1.1.1.1 255.255.255.255
        R1(config-if)# end
        R1# show ip protocols | include Router ID
        Router ID 1.1.1.1
        R1#

    # Configurar explicitamente um ID de roteador
        Na figura, a topologia foi atualizada para mostrar o ID do roteador para cada roteador:
        R1 usa o ID do roteador 1.1.1.1
        R2 usa o ID do roteador 2.2.2.2
        R3 usa o ID do roteador 3.3.3.3

        ![Alt text](image-10.png)

        Use o comando router-id rid router no modo de configuração, para atribuir manualmente um ID de roteador. No exemplo, o ID do roteador 1.1.1.1 é atribuído ao R1. Use o comando show ip protocols para verificar o ID do roteador.

        R1(config)# router ospf 10
        R1(config-router)# router-id 1.1.1.1
        R1(config-router)# end
        *May 23 19:33:42.689: %SYS-5-CONFIG_I: Configured from console by console
        R1# show ip protocols | include Router ID
        Router ID 1.1.1.1
        R1#

    # Modificar uma identificação de roteador
        Depois que um roteador seleciona um ID de roteador, um roteador OSPF ativo não permite que o ID do roteador seja alterado até que o roteador seja recarregado ou o processo OSPF seja redefinido.
        Por exemplo, para R1, o ID do roteador configurado foi removido e o roteador recarregado. Observe que o ID do roteador atual é 10.10.1.1, que é o endereço IPv4 Loopback 0. A ID do roteador deve ser 1.1.1.1. Portanto, R1 é configurado com o comando router-id 1.1.1.1.
        Observe como uma mensagem informativa aparece informando que o processo OSPF deve ser limpo ou que o roteador deve ser recarregado. O motivo é que o R1 já possui adjacências com outros vizinhos usando o ID do roteador 10.10.1.1. Essas adjacências devem ser renegociadas usando a nova ID do roteador 1.1.1.1. Use o comando clear ip ospf process para redefinir as adjacências. Em seguida, você pode verificar se o R1 está usando o novo comando ID do roteador com o comando show ip protocols canalizado para exibir somente a seção ID do roteador.
        O método preferencial é apagar o processo do OSPF para redefinir a ID do roteador.

        R1# show ip protocols | include Router ID
        Router ID 10.10.1.1
        R1# conf t
        Enter configuration commands, one per line.  End with CNTL/Z.
        R1(config)# router ospf 10 
        R1(config-router)# router-id 1.1.1.1
        % OSPF: Reload or use "clear ip ospf process" command, for this to take effect
        R1(config-router)# end
        R1# clear ip ospf process
        Reset ALL OSPF processes? [no]: y
        *Jun  6 01:09:46.975: %OSPF-5-ADJCHG: Process 10, Nbr 3.3.3.3 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached
        *Jun  6 01:09:46.975: %OSPF-5-ADJCHG: Process 10, Nbr 2.2.2.2 on GigabitEthernet0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached
        *Jun  6 01:09:46.981: %OSPF-5-ADJCHG: Process 10, Nbr 3.3.3.3 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done
        *Jun  6 01:09:46.981: %OSPF-5-ADJCHG: Process 10, Nbr 2.2.2.2 on GigabitEthernet0/0/0 from LOADING to FULL, Loading Done
        R1# show ip protocols | include Router ID
        Router ID 1.1.1.1
        R1#

        Observação: O comando router-id é o método preferido. No entanto, algumas versões mais antigas do IOS não reconhecem o comando router-id; portanto, a melhor maneira de definir o ID do roteador nesses roteadores é usando uma interface de loopback.

    
    # Redes OSPF ponto a ponto
        A sintaxe do comando de rede
        Um tipo de rede que usa OSPF é a rede ponto a ponto. Você pode especificar as interfaces que pertencem a uma rede ponto a ponto configurando o comando network. Você também pode configurar o OSPF diretamente na interface com o comando ip ospf, como veremos mais tarde.

        Ambos os comandos são usados para determinar quais interfaces participam do processo de roteamento para uma área OSPFv2. A sintaxe básica para o comando network é a seguinte:

        Router(config-router)# network network-address wildcard-mask area area-id

        A sintaxe de máscara curinga do endereço de rede é usada para ativar o OSPF nas interfaces. network comandos de estão habilitados para enviar e receber pacotes OSPF.
        A sintaxe da área de identificação de área refere-se à área OSPF. area o comando network deve ser configurado com o mesmo valor area-id em todos os roteadores. Embora qualquer ID de área possa ser usado, é recomendado usar uma ID de área 0 com o OSPFv2 de área única. Essa convenção facilita uma posterior alteração da rede para compatibilidade com o OSPFv2 multiáreas.

    # A máscara curinga
        A máscara curinga geralmente é o inverso da máscara de sub-rede configurada nessa interface. Em uma máscara de sub-rede, o binário 1 é igual a uma correspondência e o binário 0 não é uma correspondência. Em uma máscara curinga, o inverso é verdadeiro, como mostrado aqui:

        - Máscara curinga bit 0 - Corresponde ao valor de bit correspondente no endereço.
        - Máscara curinga bit 1 - Ignora o valor do bit correspondente no endereço.

        O método mais fácil para calcular uma máscara curinga é subtrair a máscara de sub-rede da rede de 255.255.255.255, conforme mostrado nas máscaras de sub-rede / 24 e / 26 da figura.
        O gráfico mostra os cálculos de máscara curinga para duas máscaras de sub-rede fornecidas. Mostrado primeiro é o cálculo de uma máscara curinga para / 24. Escrito no topo é 255.255.255.255. A máscara de sub-rede de 255.255.255.0 é subtraída abaixo, resultando em uma máscara curinga de 0.0.255. A seguir é mostrado o cálculo de uma máscara curinga para /26. Escrito no topo é 255.255.255.255. A máscara de sub-rede de 255.255.255.192 é subtraída abaixo, resultando em uma máscara curinga de 0.0.0.63.

        ![Alt text](image-11.png)


    # Configurar o OSPF Usando o comando network
        No modo de configuração de roteamento, há duas maneiras de identificar as interfaces que participarão do processo de roteamento OSPFv2. A figura mostra a topologia de referência.

        ![Alt text](image-12.png)

        No primeiro exemplo, a máscara curinga identifica a interface com base nos endereços de rede. Qualquer interface ativa configurada com um endereço IPv4 pertencente a essa rede participará do processo de roteamento OSPFv2.

        R1(config)# router ospf 10
        R1(config-router)# network 10.10.1.0 0.0.0.255 area 0
        R1(config-router)# network 10.1.1.4 0.0.0.3 area 0
        R1(config-router)# network 10.1.1.12 0.0.0.3 area 0
        R1(config-router)#
        Observação: Algumas versões do IOS permitem que a máscara de sub-rede seja inserida em vez da máscara curinga. O IOS converte a máscara de sub-rede para o formato da máscara curinga.

        Como alternativa, o segundo exemplo mostra como o OSPFv2 pode ser habilitado especificando o endereço IPv4 exato da interface usando uma máscara curinga quad zero. Inserir network 10.1.1.5 0.0.0.0 area 0 em R1 diz ao roteador para ativar a interface Gigabit Ethernet 0/0/0 para o processo de roteamento. Como resultado, o processo OSPFv2 anunciará a rede que está nessa interface (10.1.1.4/30).

        R1(config)# router ospf 10
        R1(config-router)# network 10.10.1.1 0.0.0.0 area 0
        R1(config-router)# network 10.1.1.5 0.0.0.0 area 0
        R1(config-router)# network 10.1.1.14 0.0.0.0 area 0
        R1(config-router)#
        A vantagem de especificar a interface é que o cálculo da máscara curinga não é necessário. Observe que em todos os casos, o area argumento especifica a área 0.