# Clean Architecture: Validação Desacoplada na Entidade Product

## Objetivo

Replicar o processo de validação desacoplado (já aplicado no `Customer`) para a entidade `Product`.

Retirar as regras de validação diretas da entidade e delegá-las a uma classe validadora especializada, minimizando o acoplamento no domínio.

## Tecnologias e Ferramentas

- **Linguagem:** TypeScript
- **Padrão:** Validator Pattern / Validation Factory
- **Testes:** Unitários (Jest ou similar)
- **Base do Projeto:** Repositório Template (obrigatório)

## Estrutura Base (Obrigatória)

Obrigatório continuar usando o código base do curso (ou a evolução do desafio anterior):

- https://github.com/devfullcycle/fc-clean-architecture.git

Projetos que não seguirem a estrutura base não serão aceitos.

## Requisitos Técnicos

### Criação do Validator

- Implementar uma classe de validação específica para a entidade `Product` (`ProductYupValidator`), seguindo o padrão de interfaces definido no módulo (`ValidatorInterface`).

### Desacoplamento

- Refatorar a entidade `Product` para que ela não valide os dados diretamente (sem `if/else` soltos), mas sim utilize o validador criado.
- A entidade deve chamar o validador, que por sua vez deve preencher a `Notification` (implementada no desafio anterior) em caso de erros.

### Integridade

- O comportamento externo da entidade não deve mudar. Ela deve continuar rejeitando dados inválidos, mas agora internamente o processo deve ser delegado.

## Critérios de Testes (Regressão — Obrigatório)

Trata-se de uma refatoração: é crucial garantir que **todos os testes existentes continuem passando**.

- **Testes Passando:** alterar a implementação (o "como") sem quebrar o comportamento (o "o quê").
- **Teste de Múltiplos Erros:** manter o teste que force **dois erros simultâneos** (ex.: `Name` vazio e `Price` negativo) e verifique se a notificação contém **ambos** os erros.

## Entregável

- **Link do Repositório:** GitHub com o código refatorado e os testes.
- **README:** instruções de como rodar os testes.

## Regras de Entrega

- **Repositório Único:** apenas um projeto por repositório.
- **Branch Principal:** todo o código na branch `main`.
- **Base Obrigatória:** uso obrigatório da estrutura do repositório base fornecido.
