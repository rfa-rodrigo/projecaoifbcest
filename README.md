# Projeção de Carga Horária Docente — IFB Campus Estrutural

Esta é uma página de consulta para **planejar a lotação dos professores**. A partir das
grades de todos os cursos do campus, ela mostra **quanta aula cada área de conhecimento
precisa oferecer em cada semestre** e se essa demanda cabe no número de docentes que a
área tem hoje.

Serve para responder perguntas como: *a carga da área está equilibrada entre os dois
semestres? Alguma área está sobrecarregada? Uma mudança na grade de um curso pesa quanto
sobre cada área?*

👉 **Acesse a página:** https://rfa-rodrigo.github.io/projecaoifbcest/

A página abre direto no navegador (computador ou celular) e não precisa instalar nada.

---

## Navegando pela página

No alto há duas abas: **Visão geral** e **Consulta por curso**.

### Aba "Visão geral"

Mostra o retrato de todas as áreas de uma vez.

- **Cursos no cálculo** — no topo, você pode ligar/desligar cada curso para ver a carga
  apenas dos cursos que interessam (botões **Todos** / **Nenhum** para atalho).
- **Três formas de ver a carga** (botões acima do primeiro gráfico):
  - **Média por docente** — quantas horas de aula por semana cada professor da área teria.
  - **Média c/ coordenação** — a mesma conta, descontando as horas de quem coordena.
  - **Carga total** — o total de horas-aula da área no semestre, sem dividir por professor.
- **Semanas** — no canto superior, ajusta a base de semanas letivas do semestre (padrão: 20).
- **Gráfico por área** e **por curso** — barras do 1º semestre (azul) e do 2º (laranja).
  Passe o mouse sobre uma barra para ver o detalhe.
- **Tabela de detalhamento** — todas as áreas com suas médias, carga e a situação (ver adiante).

### Aba "Consulta por curso"

Escolha um curso no seletor **Curso:** e veja a grade dele, disciplina por disciplina,
organizada por ano e semestre — com a carga horária e o número de professores de cada
componente, além do total do curso.

### Simular cenários (opcional)

O painel **🧪 Simular com seus dados** permite testar hipóteses sem alterar a versão oficial:
você carrega uma planilha própria e os gráficos se recalculam na hora, só no seu navegador.
**Restaurar originais** volta aos dados atuais. *(O formato dos arquivos está no fim desta página.)*

### Tema claro/escuro

O botão **Tema** alterna a aparência.

---

## Como interpretar os números

Toda a carga é medida em **hora-aula por semestre**. As contas são:

| O que aparece | Como é calculado |
|---|---|
| **Carga** | carga horária da disciplina × nº de turmas × nº de professores |
| **Média por docente** | carga da área ÷ (nº de docentes × nº de semanas) ≈ horas de aula por semana por professor |
| **Média c/ coordenação** | a mesma média, descontando as horas de quem coordena (cada coordenador tem redução de 8 h/semana) |

Dois detalhes que afetam a leitura:

- Disciplinas **anuais** entram no semestre indicado; disciplinas **semestrais** contam nos dois.
- A **base de semanas** (16 a 20) pode ser ajustada no topo da Visão geral.

### Situação de cada área (o "semáforo")

Definida pela **maior** média entre os dois semestres:

- 🟢 **Folga** — menos de 12 h/semana por docente
- ⚪ **Adequado** — de 12 a 18 h/semana
- 🔴 **Sobrecarga** — mais de 18 h/semana

---

## Observações importantes

- **Os dados são agregados.** A página trabalha com contagens (nº de docentes, turmas, carga
  por área) — **não há nomes de professores** nem informações individuais.
- **A classificação de áreas ainda está em refinamento.** Alguns componentes podem estar em
  ajuste, então trate os números como uma **projeção de planejamento**, não como valor final.
- **O filtro de cursos desativa as médias por docente.** Isso é proposital: o quadro de
  professores é do campus inteiro e não pode ser dividido por um curso só. Ao filtrar cursos,
  a página mostra as **cargas** (que fazem sentido por curso) e oculta as médias e a situação,
  que só têm significado com todos os cursos somados.

---

## Para quem quiser testar cenários próprios (Simular)

O botão **Simular** aceita dois arquivos `.csv`. Dá para partir dos arquivos atuais, editar e
recarregar.

**`professores.csv`** — quadro docente por área:

| Coluna | Significado |
|---|---|
| `area` | nome da área |
| `docentes` | nº de docentes da área no campus |
| `coordenadores` | quantos deles coordenam (têm redução de aula) |

**`disciplinas.csv`** — componentes de todos os cursos:

| Coluna | Significado |
|---|---|
| `curso` | nome do curso |
| `disciplina` | nome do componente |
| `area` | área de conhecimento (deve coincidir com uma `area` de `professores.csv`) |
| `ano` | ano da matriz (1, 2, 3…) |
| `tipo` | `anual` ou `semestral` |
| `semestre` | `1` ou `2` |
| `ch` | carga horária do componente, em hora-aula |
| `turmas` | nº de turmas ofertadas |
| `professores` | nº de professores no componente |

A simulação acontece **apenas no seu navegador** — nada é enviado nem altera a página que
os outros veem.

---

## Dúvidas

Para esclarecimentos sobre os dados ou a metodologia, procure a **Coordenação Geral de
Ensino — IFB Campus Estrutural**.
