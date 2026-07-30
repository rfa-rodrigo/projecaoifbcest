# Projeção de Carga Horária Docente — IFB Campus Estrutural

Painel para **planejar a lotação docente**: dada a grade de todos os cursos, mostra
quanta aula cada área de conhecimento precisa entregar por semestre e se essa demanda
cabe no número de professores que a área tem hoje.

O painel é uma **página única e autocontida** (`index.html`): os dados vão embutidos e
todo o cálculo roda no navegador — não há servidor nem dependências externas.

**Página no ar:** https://rfa-rodrigo.github.io/projecaoifbcest/
*(se não abrir, habilite o GitHub Pages em Settings → Pages → branch `master`, pasta `/root`).*

---

## Como usar a página

### Abas
- **Visão geral** — carga por área, carga por curso e a tabela de detalhamento.
- **Consulta por curso** — a grade de um curso, disciplina a disciplina, por ano e semestre.

### Filtro de cursos (Visão geral)
No topo, o card **"Cursos no cálculo"** liga/desliga cada curso. Botões **Todos** / **Nenhum**
para atalho.

- Com o filtro ativo, as **cargas (hora-aula)** e o **gráfico por curso** consideram apenas
  os cursos marcados.
- A **média por docente** e a **situação** ficam **indisponíveis** nesse modo — de propósito:
  o quadro de professores é do campus inteiro e não pode ser rateado por curso. Volte a marcar
  **Todos** para reativá-las.

### Métricas (botões acima do primeiro gráfico)
- **Média por docente** — carga da área ÷ (docentes × semanas).
- **Média c/ coordenação** — desconta as horas de quem coordena (ver fórmula abaixo).
- **Carga total** — hora-aula por semestre, sem dividir por professor.

### Outros controles
- **Semanas** — base do cálculo semanal (16 a 20 semanas por semestre; padrão 20).
- **Tema** — claro/escuro.
- **🧪 Simular com seus dados** — clique em *Professores.csv* ou *Disciplinas.csv* para carregar
  um CSV próprio e ver o resultado na hora, sem alterar nada online. **Restaurar originais** volta
  ao estado inicial.

---

## Como ler os números

| Termo | Cálculo |
|---|---|
| **Carga** | Σ (CH × turmas × professores), por semestre — em hora-aula |
| **Média por docente** | carga ÷ (docentes × semanas) ≈ horas de aula por semana por professor |
| **Média c/ coordenação** | (carga − coordenadores × 8 × semanas) ÷ ((docentes − coordenadores) × semanas) |

- Cada **coordenador** tem redução de **8 h/semana** de aula (constante `COORD_RED`).
- **Tipo** da disciplina: *semestral* conta nos dois semestres; *anual* conta só no semestre indicado.

### Situação (semáforo)
- **Folga** — menor que 12 h/semana
- **Adequado** — de 12 a 18 h/semana (inclusive nos dois limites)
- **Sobrecarga** — maior que 18 h/semana

A situação de cada área é definida pela **maior** média entre os dois semestres.

---

## Os dados de origem (ficam locais)

Dois CSVs alimentam o painel. Eles são **embutidos** no `index.html` na hora de gerar.

### `professores.csv` — quadro docente por área
```
area,docentes,coordenadores
Matemática,14,1
```
| Coluna | Significado |
|---|---|
| `area` | nome da área (chave que casa com a coluna `area` de `disciplinas.csv`) |
| `docentes` | total de docentes da área no campus |
| `coordenadores` | quantos desses são coordenadores (têm redução de aula) |

### `disciplinas.csv` — componentes curriculares de todos os cursos
```
curso,disciplina,area,ano,tipo,semestre,ch,turmas,professores
Téc. Meio Ambiente (EMI),Ecologia Geral,Biologia,1,anual,1,60,2,1
```
| Coluna | Significado |
|---|---|
| `curso` | nome do curso (define a lista do filtro e da consulta por curso) |
| `disciplina` | nome do componente |
| `area` | área de conhecimento — **precisa bater exatamente** com uma `area` de `professores.csv` |
| `ano` | ano da matriz (1, 2, 3…) |
| `tipo` | `anual` ou `semestral` |
| `semestre` | `1` ou `2` — para *anual*, indica em qual semestre; *semestral* conta nos dois |
| `ch` | carga horária do componente, em hora-aula |
| `turmas` | nº de turmas ofertadas |
| `professores` | nº de professores no componente (co-docência) |

> **Atenção ao join por área.** Se a `area` de uma disciplina não existir em `professores.csv`,
> a carga dela entra no total do curso, mas a área **não aparece** nos gráficos por área (sem
> docentes para dividir). Disciplinas com `area` em branco ficam de fora da visão por área.

---

## Como atualizar e publicar

Tudo é gerado por um script Python a partir dos dois CSVs.

```bash
# 1. edite professores.csv e/ou disciplinas.csv
# 2. regenere a página
python3 gerar_dashboard.py     # sobrescreve index.html

# 3. publique (só o index.html é versionado)
git add index.html
git commit -m "Atualiza projeção"
git push
```

O GitHub Pages atualiza a página no ar em seguida.

> Antes de publicar, dá para conferir localmente: é só abrir o `index.html` no navegador
> (duplo clique). Ele funciona offline, sem servidor.

---

## Estrutura do repositório

Só o **`index.html`** (e este README) são versionados. Os dados de origem, a planilha,
os PPCs e o gerador ficam **fora do controle de versão** (ver `.gitignore`) — são a base
de trabalho local, não o produto publicado.

| Arquivo | Papel | Versionado? |
|---|---|---|
| `index.html` | painel publicado (gerado) | ✅ sim |
| `gerar_dashboard.py` | gerador: CSVs → `index.html` | 🚫 local |
| `professores.csv`, `disciplinas.csv` | dados de origem | 🚫 local |
| `Projeções.xlsx` | planilha de trabalho das projeções | 🚫 local |
| `*.pdf` | PPCs dos cursos e grades de horário | 🚫 local |

### Ajustes rápidos no gerador
No topo de `gerar_dashboard.py`:
- `COORD_RED` — horas semanais de redução por coordenador (padrão 8).

Os limites do semáforo (12 e 18) estão na função `situ(...)` dentro do template, e o texto
de referência correspondente fica no rodapé (`<footer>`).
