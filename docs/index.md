<h2><a href= "https://www.mackenzie.br">Universidade Presbiteriana Mackenzie</a></h2>
<h3><a href= "https://www.mackenzie.br/graduacao/sao-paulo-higienopolis/ciencia-da-computacao">Ciência da Computação</a></h3>


# Sistema de Monitoramento Agrícola com Drones


**Conteúdo**

- [Autores](#nome-alunos)
- [Descrição do Projeto](#introdução-do-projeto)
- [Diagrama de Classes](#diagrama-orientado-objetos)
- [Diagrama de Senquencia](#diagrama-de-ordem-interações)
- [Diagrama de Estados](#diagrama-estrutura-componente)
- [Diagrama de Implantação](#diagrama-de-hardware-software)
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
    }
    
    class OperadorDrone {
        +agendarMissao() Boolean
        +executarMissao() Boolean
    }

    class AreaAgricola {
        -String id
        -String nome
        -Double tamanho
        -String localizacao
        -String tipoCultivo
    }
    
    class Drone {
        -String id
        -String modelo
        -String status
        -Double nivelBateria
        +verificarBateria() Boolean
    }
    
    class Sensor {
        -String id
        -String tipo
        -String status
        +validarFuncionamento() Boolean
    }

    class MissaoVoo {
        -String id
        -DateTime dataAgendada
        -String status
        +validarSobreposicao() Boolean
        +executar() Boolean
    }
    
    class ChecklistSeguranca {
        -Boolean bateriaOk
        -Boolean sensoresOk
        +executarChecklist() Boolean
    }

    class DadoColetado {
        -String id
        -DateTime timestamp
        -Double temperatura
        -Double umidade
        -String pragas
        +validarDado() Boolean
    }
    
    class Imagem {
        -String id
        -DateTime timestamp
        -String caminhoArquivo
    }

    class RelatorioPlantacao {
        -String id
        -DateTime dataGeracao
        +gerarRelatorio() String
    }

    Usuario <|-- Administrador
    Usuario <|-- OperadorDrone

    Drone *-- Sensor
    MissaoVoo *-- ChecklistSeguranca
    
    MissaoVoo o-- AreaAgricola
    MissaoVoo o-- Drone
    MissaoVoo o-- DadoColetado
    MissaoVoo o-- Imagem
    
    Sensor o-- DadoColetado
```


# Diagrama de Sequência

*&lt;Diagrama de ordem e interação dos objetos&gt;*

# Diagrama de Estados

*&lt;Diagrama para permite modelar o comportamento interno de um determinado objeto, subsistema ou sistema global&gt;*

# Diagrama de Implantação

*&lt;Diagrama para exibir o relacionamento de hardware e software no projeto&gt;*

# Referências

*&lt;Lista de referências&gt;*
