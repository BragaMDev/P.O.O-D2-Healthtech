# Diagrama de classes: Academia e Planos (AV2_D2_A)

Código Mermaid. O GitHub e o GitLab renderizam este bloco automaticamente; também dá para colar em https://mermaid.live para exportar PNG ou SVG.
Os tipos seguem o estilo Java; ajuste para a linguagem usada na disciplina.

```mermaid
classDiagram
  class Aluno {
    -Long id
    -String nome
    -String cpf
    -String email
  }
  class Plano {
    -Long id
    -String nome
    -BigDecimal precoMensal
    -int duracaoMeses
    -int limiteCongelamentoDias
    -int capacidade
    +permiteCongelar(int dias) boolean
    +temVaga(int ocupadas) boolean
  }
  class Matricula {
    -Long id
    -LocalDate dataInicio
    -LocalDate dataFim
    -EstadoMatricula estado
    +checkin() Checkin
    +congelar(Periodo periodo, String motivo) void
    +descongelar() void
    +cancelar() void
    +mudarEstado(EstadoMatricula novo) void
  }
  class EstadoMatricula {
    <<interface>>
    +checkin(Matricula m) Checkin
    +congelar(Matricula m, Periodo p, String motivo) void
    +descongelar(Matricula m) void
    +cancelar(Matricula m) void
  }
  class MatriculaAtiva
  class MatriculaCongelada
  class MatriculaCancelada
  class Checkin {
    -Long id
    -LocalDateTime dataHora
  }
  class Congelamento {
    -Long id
    -String motivo
  }
  class Periodo {
    <<record>>
    -LocalDate inicio
    -LocalDate fim
    +dias() int
  }
  class ListaEspera {
    +entrar(Aluno a) EntradaEspera
    +chamarProximo() Aluno
    +sair(Aluno a) void
  }
  class EntradaEspera {
    -int posicao
    -LocalDateTime dataEntrada
  }
  class OperacaoInvalidaException {
    <<exception>>
  }
  class MatriculaService {
    +matricular(Aluno a, Plano p) Matricula
    +registrarCheckin(Long matriculaId) Checkin
    +congelar(Long matriculaId, Periodo p, String motivo) void
  }
  class MatriculaRepository {
    <<interface>>
    +buscarPorId(Long id) Matricula
    +salvar(Matricula m) void
  }
  Aluno "1" --> "0..*" Matricula : possui
  Plano "1" --> "0..*" Matricula : define
  Matricula "1" *-- "0..*" Checkin : registra
  Matricula "1" *-- "0..*" Congelamento : congela
  Congelamento --> Periodo
  Matricula --> EstadoMatricula : estado atual
  EstadoMatricula <|.. MatriculaAtiva
  EstadoMatricula <|.. MatriculaCongelada
  EstadoMatricula <|.. MatriculaCancelada
  MatriculaCongelada ..> OperacaoInvalidaException : checkin lança
  Plano "1" --> "1" ListaEspera : tem
  ListaEspera "1" *-- "0..*" EntradaEspera
  EntradaEspera --> Aluno
  MatriculaService ..> MatriculaRepository
  MatriculaService ..> Matricula
```
