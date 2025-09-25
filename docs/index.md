<h2><a href= "https://www.mackenzie.br">Universidade Presbiteriana Mackenzie</a></h2>
<h3><a href= "https://www.mackenzie.br/graduacao/sao-paulo-higienopolis/ciencia-da-computacao">Ciência da Computação</a></h3>


# Sistema de Monitoramento Agrícola com Drones


**Conteúdo**

- [Autores](#nome-alunos)
- [Descrição do Projeto](#introdução-do-projeto)
- [Diagrama de Classes](#diagrama-orientado-objetos)
- [Diagrama de Senquencia](#diagrama-de-ordem-interações)
- [Diagrama de Dados](#diagrama-estrutura-componente)
- [Integração Modelos](#diagrama-de-hardware-software)
- [Referências](#referências)


# Autores

* João Vitor de Araújo Trindade


# Descrição do Projeto

## Contexto
Uma cooperativa rural deseja monitorar plantações usando drones que realizam sobrevoos periódicos para coletar imagens e dados ambientais (temperatura, umidade e pragas).


## Funcionalidades mínimas a implementar
- **Cadastro de áreas agrícolas** (tamanho, localização e tipo de cultivo).
- **Cadastro de drones** (ID, sensores disponíveis e status).
- **Agendamento de missões de voo** (data, área e sensores a utilizar).
- **Registro de dados coletados** (imagens e valores de sensores simulados).
- **Relatórios básicos sobre condições da plantação** (últimas medições e voos realizados).



# Diagrama de Classes

``` mermaid
classDiagram
    class Usuario {
        -String id
        -String nome
        -String email
        -String tipoUsuario
        +autenticar() Boolean
    }
    
    class Administrador {
        +cadastrarArea() Boolean
        +cadastrarDrone() Boolean
        +configurarSistema() Boolean
    }
    
    class OperadorDrone {
        +agendarMissao() Boolean
        +executarMissao() Boolean
        +monitorarMissao() Boolean
    }
    
    class ControleCentral {
        -String id
        -List~MissaoVoo~ missoesAtivas
        +coordenarDrones() Boolean
        +otimizarRotas() Boolean
        +verificarColisoes() Boolean
        +sincronizarOperacoes() Boolean
    }
    
    class AreaAgricola {
        -String id
        -String nome
        -Double tamanho
        -String localizacao
        -String tipoCultivo
        +validarArea() Boolean
    }
    
    class Drone {
        -String id
        -String modelo
        -String status
        -Double nivelBateria
        -Coordenadas posicaoAtual
        +verificarBateria() Boolean
        +enviarTelemetria() Boolean
    }
    
    class Sensor {
        -String id
        -String tipo
        -String status
        -Double precisao
        +validarFuncionamento() Boolean
        +calibrar() Boolean
    }
    
    class MissaoVoo {
        -String id
        -DateTime dataAgendada
        -String status
        -String prioridade
        +validarSobreposicao() Boolean
        +executar() Boolean
        +pausar() Boolean
    }
    
    class ChecklistSeguranca {
        -Boolean bateriaOk
        -Boolean sensoresOk
        -Boolean condicaoMeteorologica
        +executarChecklist() Boolean
    }
    
    class DetectorEventos {
        -List~String~ tiposEventos
        -Map alertasAtivos
        +detectarEventoCritico() Boolean
        +processarAlerta() Boolean
        +notificarOperador() Boolean
        +acionarProtocoloEmergencia() Boolean
    }
    
    class DadoColetado {
        -String id
        -DateTime timestamp
        -Double temperatura
        -Double umidade
        -String pragas
        -String statusValidacao
        +criarDado() Boolean
    }
    
    class ValidadorDados {
        -Map~String,Range~ limitesEsperados
        -Double toleranciaRuido
        +validarDado(DadoColetado) Boolean
        +detectarOutliers() Boolean
        +aplicarFiltroRuido() Boolean
        +marcarDadoSuspeito() Boolean
    }
    
    class Imagem {
        -String id
        -DateTime timestamp
        -String caminhoArquivo
        -String qualidade
        +validarImagem() Boolean
    }
    
    class ServidorDados {
        -String id
        -String status
        -Double capacidadeArmazenamento
        +armazenarDados(DadoColetado) Boolean
        +armazenarImagens(Imagem) Boolean
        +realizarBackup() Boolean
        +sincronizarDados() Boolean
        +consultarHistorico() List~DadoColetado~
    }
    
    class RelatorioPlantacao {
        -String id
        -DateTime dataGeracao
        -String tipoRelatorio
        +gerarRelatorio() String
        +exportarRelatorio() Boolean
    }
    
    class SistemaMonitoramento {
        -List~Drone~ dronesAtivos
        -Map~String,String~ statusGeral
        +monitorarDrones() Boolean
        +atualizarStatus() Boolean
        +gerarDashboard() String
    }

    %% Herança
    Usuario <|-- Administrador
    Usuario <|-- OperadorDrone
    
    %% Composição
    Drone *-- Sensor
    MissaoVoo *-- ChecklistSeguranca
    
    %% Agregação e Associação
    ControleCentral o-- MissaoVoo
    ControleCentral o-- Drone
    ControleCentral o-- DetectorEventos
    ControleCentral o-- SistemaMonitoramento
    
    MissaoVoo o-- AreaAgricola
    MissaoVoo o-- Drone
    
    Sensor o-- DadoColetado
    ValidadorDados o-- DadoColetado
    
    ServidorDados o-- DadoColetado
    ServidorDados o-- Imagem
    
    DetectorEventos o-- Drone
    DetectorEventos o-- MissaoVoo
    
    SistemaMonitoramento o-- Drone
    SistemaMonitoramento o-- DetectorEventos
    
    RelatorioPlantacao o-- ServidorDados
```


# Diagrama de Sequência

``` mermaid
sequenceDiagram
    participant O as OperadorDrone
    participant CC as ControleCentral
    participant SM as SistemaMonitoramento
    participant A as AreaAgricola
    participant M as MissaoVoo
    participant C as ChecklistSeguranca
    participant D as Drone
    participant Sen as Sensor
    participant DE as DetectorEventos
    participant DC as DadoColetado
    participant VD as ValidadorDados
    participant I as Imagem
    participant SD as ServidorDados
    participant R as RelatorioPlantacao

    Note over O, R: Fase de Autenticação e Coordenação
    
    O->>+CC: login(credenciais)
    CC->>CC: autenticar()
    CC-->>O: autenticação OK
    
    O->>+CC: agendarMissao()
    CC->>+A: validarArea()
    A-->>-CC: área válida
    
    CC->>+CC: verificarColisoes()
    CC->>+CC: otimizarRotas()
    
    CC->>+M: criarMissao(dados)
    M->>M: validarSobreposicao()
    M-->>-CC: missão criada
    CC-->>-O: missão coordenada

    Note over O, R: Fase de Preparação e Monitoramento
    
    O->>+CC: executarMissao()
    CC->>+SM: iniciarMonitoramento()
    SM-->>-CC: monitoramento ativo
    
    CC->>+M: iniciarPreparacao()
    M->>+C: executarChecklist()
    
    C->>+D: verificarBateria()
    D-->>-C: bateria OK
    
    C->>+Sen: validarFuncionamento()
    Sen-->>-C: sensores OK
    
    C-->>-M: checklist aprovado
    
    CC->>+DE: ativarDeteccao()
    DE-->>-CC: detecção ativa
    
    M-->>-CC: pronto para execução

    Note over O, R: Fase de Execução com Validação
    
    CC->>+M: executar()
    M->>+D: ativarModoAutonomo()
    
    SM->>SM: monitorarDrones()
    
    loop Durante a missão
        D->>+Sen: coletarDados()
        Sen->>+DC: criarDadoColetado(temperatura, umidade, pragas)
        
        DC->>+VD: validarDado()
        VD->>VD: detectarOutliers()
        VD->>VD: aplicarFiltroRuido()
        
        alt Dado válido
            VD-->>DC: dados validados
            DC->>+SD: armazenarDados()
            SD-->>-DC: dados salvos
        else Dado suspeito
            VD->>VD: marcarDadoSuspeito()
            VD->>DE: reportarAnomalia()
        end
        
        VD-->>-DC: processamento concluído
        DC-->>-Sen: dados processados
        Sen-->>-D: coleta finalizada
        
        D->>+I: capturarImagem()
        I->>I: validarImagem()
        I->>+SD: armazenarImagens()
        SD-->>-I: imagem salva
        I-->>-D: imagem capturada
        
        D->>SM: enviarTelemetria()
        SM->>SM: atualizarStatus()
        
        DE->>DE: detectarEventoCritico()
        
        alt Evento crítico detectado
            DE->>DE: processarAlerta()
            DE->>CC: notificarControleCentral()
            DE->>O: notificarOperador()
            
            opt Emergência crítica
                DE->>DE: acionarProtocoloEmergencia()
                DE->>D: retornoEmergencia()
            end
        end
    end
    
    D-->>-M: missão executada

    Note over O, R: Fase de Finalização e Relatórios
    
    M->>+D: retornarPontoBase()
    D-->>-M: retorno concluído
    
    M->>+SD: registrarMissao()
    SD-->>-M: missão registrada
    
    CC->>+SM: finalizarMonitoramento()
    SM-->>-CC: monitoramento encerrado
    
    CC->>+R: gerarRelatorio()
    R->>+SD: consultarHistorico()
    SD-->>-R: dados históricos
    R->>R: processarInformacoes()
    R->>R: exportarRelatorio()
    R-->>-CC: relatório gerado
    
    CC-->>-O: missão finalizada

    O->>CC: encerrarSessao()
```

# Diagrama de Dados

``` mermaid
erDiagram
    USUARIO {
        VARCHAR(50) id PK
        VARCHAR(100) nome
        VARCHAR(100) email UK
        ENUM tipo_usuario
        VARCHAR(255) senha_hash
        TIMESTAMP data_cadastro
        BOOLEAN ativo
    }
    
    AREA_AGRICOLA {
        VARCHAR(50) id PK
        VARCHAR(100) nome
        DECIMAL tamanho
        VARCHAR(200) localizacao
        VARCHAR(100) tipo_cultivo
        TIMESTAMP data_cadastro
        BOOLEAN ativo
    }
    
    DRONE {
        VARCHAR(50) id PK
        VARCHAR(100) modelo
        ENUM status
        DECIMAL nivel_bateria
        DECIMAL posicao_lat
        DECIMAL posicao_lng
        TIMESTAMP data_cadastro
        BOOLEAN ativo
    }
    
    SENSOR {
        VARCHAR(50) id PK
        VARCHAR(50) drone_id FK
        ENUM tipo
        ENUM status
        DECIMAL precisao
        TIMESTAMP data_calibracao
    }
    
    MISSAO_VOO {
        VARCHAR(50) id PK
        VARCHAR(50) area_agricola_id FK
        VARCHAR(50) drone_id FK
        VARCHAR(50) operador_id FK
        TIMESTAMP data_agendada
        TIMESTAMP data_inicio
        TIMESTAMP data_fim
        ENUM status
        ENUM prioridade
        TEXT observacoes
    }
    
    CHECKLIST_SEGURANCA {
        VARCHAR(50) id PK
        VARCHAR(50) missao_id FK
        BOOLEAN bateria_ok
        BOOLEAN sensores_ok
        BOOLEAN condicao_meteorologica
        TIMESTAMP data_execucao
        TEXT observacoes
    }
    
    DADO_COLETADO {
        VARCHAR(50) id PK
        VARCHAR(50) missao_id FK
        VARCHAR(50) sensor_id FK
        TIMESTAMP timestamp_coleta
        DECIMAL temperatura
        DECIMAL umidade
        VARCHAR(200) pragas
        ENUM status_validacao
        TEXT observacoes_validacao
    }
    
    IMAGEM {
        VARCHAR(50) id PK
        VARCHAR(50) missao_id FK
        TIMESTAMP timestamp_captura
        VARCHAR(500) caminho_arquivo
        ENUM qualidade
        BIGINT tamanho_arquivo
    }
    
    EVENTO_CRITICO {
        VARCHAR(50) id PK
        VARCHAR(50) missao_id FK
        VARCHAR(50) drone_id FK
        ENUM tipo_evento
        TEXT descricao
        ENUM severidade
        TIMESTAMP timestamp_evento
        BOOLEAN resolvido
        TEXT acao_tomada
    }
    
    RELATORIO_PLANTACAO {
        VARCHAR(50) id PK
        VARCHAR(50) area_agricola_id FK
        TIMESTAMP data_geracao
        ENUM tipo_relatorio
        DATE periodo_inicio
        DATE periodo_fim
        VARCHAR(500) caminho_arquivo
    }
    
    LOG_MONITORAMENTO {
        VARCHAR(50) id PK
        VARCHAR(50) drone_id FK
        VARCHAR(50) missao_id FK
        TIMESTAMP timestamp_log
        ENUM tipo_evento
        JSON dados_json
    }

    %% Relacionamentos
    USUARIO ||--o{ MISSAO_VOO : "operador_id"
    AREA_AGRICOLA ||--o{ MISSAO_VOO : "area_agricola_id"
    DRONE ||--o{ MISSAO_VOO : "drone_id"
    DRONE ||--o{ SENSOR : "drone_id"
    DRONE ||--o{ EVENTO_CRITICO : "drone_id"
    DRONE ||--o{ LOG_MONITORAMENTO : "drone_id"
    
    MISSAO_VOO ||--|| CHECKLIST_SEGURANCA : "missao_id"
    MISSAO_VOO ||--o{ DADO_COLETADO : "missao_id"
    MISSAO_VOO ||--o{ IMAGEM : "missao_id"
    MISSAO_VOO ||--o{ EVENTO_CRITICO : "missao_id"
    MISSAO_VOO ||--o{ LOG_MONITORAMENTO : "missao_id"
    
    SENSOR ||--o{ DADO_COLETADO : "sensor_id"
    AREA_AGRICOLA ||--o{ RELATORIO_PLANTACAO : "area_agricola_id"

```

# Integração de modelos

*&lt;Diagrama para exibir o relacionamento de hardware e software no projeto&gt;*

# Referências

*&lt;Lista de referências&gt;*
