# Clean Architecture: Validação Desacoplada na Entidade Product

Refatoração da validação da entidade `Product` para delegar as regras a uma classe validadora especializada (`ProductYupValidator`), em vez de validar com `if/else` soltos na própria entidade.

A entidade chama o validador via factory; o validador preenche a `Notification` (Notification Pattern) em caso de erros, sem acoplar o domínio ao Yup.

Base: repositório base do curso ([devfullcycle/fc-clean-architecture](https://github.com/devfullcycle/fc-clean-architecture.git)), evoluído a partir do desafio anterior de Notification Pattern.

## Como rodar os testes

Requer Node.js (testado com v22) e npm.

```bash
npm install
npm test
```

`npm test` roda a checagem de tipos (`tsc --noEmit`) e o Jest.

Para rodar só os testes de Product:

```bash
npx jest src/domain/product
```

## Validar no navegador (opcional)

```bash
npm run dev
```

Abrir http://localhost:3000. Form: deixe **Name vazio** e **Price negativo** → a página mostra **os dois erros juntos** (`product: Name is required` e `product: Price must be greater than zero`).

Endpoint: `POST /product` → `201` com o produto, ou `400` com `{ "errors": [...] }`.

## Arquitetura da validação

- `src/domain/@shared/validator/validator.interface.ts` — contrato `ValidatorInterface<T>`.
- `src/domain/product/validator/product.yup.validator.ts` — regras (`Id`, `Name`, `Price`), implementa `ValidatorInterface<ProductInterface>`.
- `src/domain/product/factory/product.validator.factory.ts` — factory que cria o validador (`ProductValidatorFactory.create()`).
- `src/domain/product/entity/product.ts` / `product-b.ts` — entidades: apenas chamam o validador e lançam `NotificationError` se a notificação tiver erros.

## Integridade / comportamento mantido

- `Product` continua **rejeitando dados inválidos**; mesmo comportamento externo, processo interno delegado.
- Erros **acumulados** na notificação (não interrompe no primeiro), lançados juntos via `NotificationError`.
- Regras mantidas: `Id` obrigatório, `Name` obrigatório, `Price` maior ou igual a zero.

## Teste de múltiplos erros

`Name` vazio **e** `Price` negativo ao mesmo tempo: a notificação contém **ambos** os erros (`product: Name is required` e `product: Price must be greater than zero`). Ver `src/domain/product/entity/product.spec.ts`.
