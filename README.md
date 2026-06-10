# Bússola Eletrônica com Microcontrolador 8051

## 📌 Descrição

Este projeto foi desenvolvido na disciplina **CE4411 - ARQUITETURA DE COMPUTADORES** (graduação) e implementa uma **bússola eletrônica interativa** utilizando um microcontrolador 8051 (simulado no **Edsim51**).  
O usuário pode selecionar um país (Brasil, Estados Unidos, Sérvia, Japão ou Vaticano) e, em seguida, pressionar teclas numéricas (1 a 9) para visualizar no display LCD os países ou oceanos adjacentes àquele país na direção correspondente (Norte, Sul, Leste, Oeste, etc.).

O sistema utiliza:
- **LCD 16x2** (controlado via porta P1) para exibição das informações.
- **Teclado matricial 4x3** (ligado à porta P0) para entrada das direções.
- **Botões de interrupção externa** (INT0 e INT1) para selecionar o país e reinicializar o mapeamento do teclado.

---

## 🎯 Funcionalidades

- Seleção cíclica de país através do botão associado à **INT1** (cada pressionamento avança para o próximo país).
- Exibição do país atual na primeira linha do LCD.
- Leitura do teclado numérico (1 a 9) para escolher uma direção (ex.: 1 = Noroeste, 2 = Norte, 3 = Nordeste, etc.).
- Exibição na segunda linha do LCD do país ou acidente geográfico vizinho correspondente à direção escolhida.
- Botão **INT0** para reinicializar o sistema (limpa o display e recarrega os símbolos do teclado).

---

## 🧠 Estrutura do Código

O código é escrito em **Assembly 8051** e está organizado nas seguintes seções principais:

| Seção          | Descrição                                                                                     |
|----------------|-----------------------------------------------------------------------------------------------|
| **Vetores de interrupção** | `INT0` (0003h) → chama `clearDisplay` e `memoria`.<br>`INT1` (0013h) → cicla pelos países. |
| **Tabelas de dados**       | Strings para cada país (Brasil, EUA, Sérvia, Japão, Vaticano) e seus respectivos vizinhos.   |
| **Inicialização** (`START`) | Habilita interrupções, configura borda de descida e chama `lcdInit`.                         |
| **Leitura do teclado**     | Rotinas `leituraTeclado` e `colScan` – varre linhas e colunas da matriz 4x3.                  |
| **Mapeamento de teclas**   | `memoria` preenche endereços 70h..7Bh com caracteres (`#`, `0`, `*`, `9`, ..., `1`).        |
| **Direções por país**      | Blocos `Brasil1`, `EUA1`, `Servia1`, `Japao1`, `Vaticano1` – comparam a tecla pressionada e exibem o vizinho correspondente. |
| **Rotinas do LCD**         | `lcdInit`, `sendCharacter`, `posicionaCursor`, `clearDisplay`, `escreveStringROM`.           |

### Mapeamento do teclado matricial

O teclado é interpretado pela seguinte relação (endereços 70h..7Bh):

| Índice (R0) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|-------------|---|---|---|---|---|---|---|---|---|---|----|----|
| Caractere   | # | 0 | * | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2  | 1  |

As teclas que produzem **'1' a '9'** (índices 11 a 3) são usadas para selecionar as direções.

---

## 🖥️ Como Executar

### Requisitos
- **Edsim51** (simulador 8051) – [baixar aqui](http://edsim51.com/)
- Código fonte (`Bussola.asm`)

### Passos
1. Abra o **Edsim51**.
2. Carregue o arquivo `Bussola.asm`.
3. Monte/compile o código (botão **Assemble**).
4. Execute a simulação (botão **Run**).
5. **Interaja**:
   - Pressione o botão ligado à **INT1** (no simulador, use os botões virtuais da placa) para selecionar o país desejado.
   - Após a escolha, pressione as teclas **1 a 9** do teclado matricial para ver os vizinhos.
   - Pressione **INT0** para reiniciar o mapeamento das teclas (útil se a leitura ficar inconsistente).

> ⚠️ No simulador, as interrupções externas são acionadas por borda de descida. O comportamento dos botões virtuais pode exigir cliques curtos.

---

## 🗺️ Países e Vizinhos (Exemplo)

### Brasil
| Tecla | Direção      | Vizinho                |
|-------|--------------|------------------------|
| 1     | Noroeste     | Colômbia               |
| 2     | Norte        | Guiana Francesa        |
| 3     | Nordeste     | Oceano Atlântico       |
| 4     | Oeste        | Bolívia                |
| 5     | Centro       | Brasil                 |
| 6     | Leste        | Oceano Atlântico       |
| 7     | Sudoeste     | Argentina              |
| 8     | Sul          | Uruguai                |
| 9     | Sudeste      | Oceano Atlântico       |

Os demais países seguem estrutura semelhante, com seus respectivos limites geográficos.

---

## 🛠️ Principais Rotinas Implementadas

- `lcdInit` – Inicializa o LCD no modo 4 bits.
- `escreveStringROM` – Lê uma string terminada em 0 da memória de programa e envia ao LCD.
- `leituraTeclado` – Varre as 4 linhas do teclado e chama `colScan`.
- `leituraDirecao` – Converte o índice da tecla no caractere armazenado em 70h..7Bh.
- `clearDisplay` – Limpa o LCD e retorna o cursor à posição inicial.

---

## 👨‍🎓 Autoria e Contexto

- **Disciplina:** CE4411 - ARQUITETURA DE COMPUTADORES  
- **Instituição:** Centro Universitário FEI
- **Período:** Graduação
- **Linguagem:** Assembly 8051  
- **Simulador:** Edsim51  
- **Ano:** 2023

Este código foi desenvolvido como trabalho prático para fixação dos conceitos de:
- Programação em baixo nível
- Manipulação de portas de I/O
- Interrupções externas
- Interface com LCD e teclado matricial
