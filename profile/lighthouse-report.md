# Relatório Lighthouse — Acessibilidade

## Metodologia

- Chrome anônimo, sem extensões
- Categoria avaliada: **Acessibilidade**
- Dispositivo: **Desktop**
- 3 execuções consecutivas por página, considerada a **mediana**
- Servidor: Live Server (`http://127.0.0.1:5500`)

---

## Página Principal (Landing — `index.html`)

### Execução 1 — [preencher data]
Nota: 96/100

### Execução 2 — [preencher data]
Nota: 100/100

### Execução 3 — [preencher data]
Nota: 100/100

**Mediana: 100/100**

> Observação: uma execução anterior havia apontado nota 96, com o alerta "Os links não têm um nome compreensível". O problema foi corrigido (adição de texto/aria-label descritivo nos links afetados) antes das 3 execuções finais registradas acima.

---

## Página Interna — Carrinho (`cart/carrinho.html`)

### Execução 1 — [preencher data]
Nota: 100/100

### Execução 2 — [preencher data]
Nota: 100/100

### Execução 3 — [preencher data]
Nota: 100/100

**Mediana: 100/100**

---

## Página Interna — Perfil (`profile/index.html`)

### Execução 1 — [preencher data]
Nota: 91/100

### Execução 2 — [preencher data]
Nota: 100/100

### Execução 3 — [preencher data]
Nota: 100/100

**Mediana: 100/100**

---

## Resumo

| Página | Mediana |
|---|---|
| Landing (principal) | 100/100 |
| Carrinho | 100/100 |
| Perfil | 100/100 |

## Observações

- Todas as páginas testadas atingiram mediana ≥ 90, atendendo ao critério de acessibilidade da rubrica (item 10 da seção 5.1).
- O único ponto de atenção identificado durante os testes foi em links sem nome compreensível na Landing, já corrigido.
