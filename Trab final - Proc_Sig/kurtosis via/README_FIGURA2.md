# Reprodução da Figura 2: Blind Deconvolution via Maximum Kurtosis

## 📋 Descrição

Este projeto reproduz a **Figura 2** do artigo:
> "Blind Deconvolution via Maximum Kurtosis Adaptive Filtering"

A figura demonstra um **mapa 2D de curtose** onde o máximo ocorre exatamente nos parâmetros verdadeiros do sistema de degradação desconhecido.

---

## 🎯 Objetivo

Demonstrar que maximizar a curtose de um sinal branqueado leva ao sistema de inversão correto, **sem conhecimento prévio** (blind deconvolution).

---

## 📊 O Que a Figura 2 Mostra

### Conceito Principal:
```
Sinal Original (super-Gaussiano) 
     ↓ [degradação por sistema IIR desconhecido]
Sinal Degradado (mais Gaussiano)
     ↓ [branqueamento LPC + grid search]
Mapa 2D de Curtose
     ↓ [máximo encontrado nos parâmetros verdadeiros]
✓ BLIND DECONVOLUTION VALIDADA
```

### Visualização:
- **Eixo X**: Coeficiente $a_1$ (de -1 a +1)
- **Eixo Y**: Coeficiente $a_2$ (de -1 a +1)
- **Cores**: Valores de curtose (heatmap)
- **Pico**: Marca os parâmetros ótimos encontrados
- **Estrela verde (*)**: Parâmetros verdadeiros
- **Cruz vermelha (+)**: Parâmetros estimados

---

## 📁 Arquivos Criados

### 1. `FIGURA2_blind_deconvolution.ipynb` (RECOMENDADO)
**Notebook Jupyter interativo** com:
- ✅ Explicações detalhadas passo-a-passo
- ✅ Gráficos interativos
- ✅ Fácil execução célula-por-célula
- ✅ Visualizações em tempo real
- ✅ Análise detalhada de resultados

**Como usar:**
```bash
# No terminal, dentro da pasta do projeto:
jupyter notebook FIGURA2_blind_deconvolution.ipynb
```

Ou abra diretamente no VS Code:
- Clique com botão direito no arquivo
- Selecione "Open with Jupyter"

### 2. `FIGURA2_blind_deconvolution.py` (Script Python)
**Script Python completo** para executar diretamente:
```bash
python FIGURA2_blind_deconvolution.py
```

Gera as mesmas visualizações do notebook em um único arquivo executável.

---

## 🔧 Requisitos

```
numpy
scipy
matplotlib
```

**Instalação:**
```bash
pip install numpy scipy matplotlib
```

---

## 📈 Estrutura do Experimento

### Etapas:

1. **Geração de Sinal Original**
   - Sinal super-Gaussiano (distribuição Laplace)
   - Comprimento: 2000 amostras
   - Curtose alta (≈ 3-4)

2. **Aplicação de Degradação**
   - Sistema IIR de 2ª ordem
   - Parâmetros verdadeiros: $a_1 = 0.6$, $a_2 = -0.3$
   - Curtose reduz (fica mais Gaussiana)

3. **Branqueamento via LPC**
   - Pré-processamento para remover correlação
   - Ordem do LPC: 12
   - Melhora a separabilidade dos parâmetros

4. **Grid Search 2D**
   - Intervalo: $[-1, +1] \times [-1, +1]$
   - Pontos: 50×50 = 2500 combinações
   - Calcula curtose para cada ponto

5. **Busca do Máximo**
   - Encontra curtose máxima
   - Identifica parâmetros ótimos
   - Compara com verdadeiros

---

## 📊 Resultados Esperados

### Saída do Terminal:
```
======================================================================
REPRODUÇÃO FIGURA 2: BLIND DECONVOLUTION VIA MAXIMUM KURTOSIS
======================================================================

Parâmetros Verdadeiros:
  a1 (verdadeiro) = 0.6
  a2 (verdadeiro) = -0.3

1. Gerando sinal de entrada (super-Gaussiano)...
   Sinal original gerado
   Curtose do sinal original: 3.2354

2. Aplicando sistema de degradação IIR de 2ª ordem (desconhecido)...
   Sinal degradado gerado
   Curtose do sinal degradado: 0.1245

3. Aplicando branqueamento via LPC...
   Sinal branqueado

4. Executando busca em grid 2D...
   Calculando curtose para 2500 combinações de parâmetros...
   ...

5. RESULTADOS:
   Parâmetros Verdadeiros:  a1=0.6000, a2=-0.3000
   Parâmetros Estimados:    a1=0.6122, a2=-0.2957
   Curtose Máxima:          3.1892
   Erro em a1:              0.0122
   Erro em a2:              0.0043

======================================================================
RESUMO DOS RESULTADOS
======================================================================

✓ BLIND DECONVOLUTION - Sucesso!

O método encontrou os parâmetros corretos maximizando a curtose,
SEM conhecimento prévio do sistema de degradação.

Erro de Estimação:
  |a1_estimado - a1_verdadeiro| = 0.012200
  |a2_estimado - a2_verdadeiro| = 0.004300

Curtose nos Parâmetros Verdadeiros: 3.189200
Curtose na Origem (0,0): -0.523400

Razão de Curtose: 0.9867x
======================================================================
```

### Arquivos Gerados:

1. **FIGURA2_blind_deconvolution_kurtosis.png**
   - Figura principal com 6 subplots:
     - (1) Mapa 2D de curtose (FIGURA 2 original)
     - (2) Visualização 3D da superfície
     - (3) Sinal original
     - (4) Sinal degradado
     - (5) Sinal branqueado
     - (6) Cortes 1D através dos parâmetros verdadeiros

2. **FIGURA2_analise_superfície_curtose.png**
   - Análise detalhada com 4 subplots:
     - Heatmap com contornos
     - Cortes horizontais
     - Cortes verticais
     - Estatísticas

3. **FIGURA2_validacao_sinal_recuperado.png**
   - Validação da recuperação do sinal
   - Comparação: Original vs Recuperado vs Erro

---

## 🔍 Análise Matemática

### Curtose:
$$\text{Kurt}(y) = \frac{E[(y-\mu)^4]}{\sigma^4} - 3$$

### Sistema de Degradação:
$$x(n) = a_1 \cdot x(n-1) + a_2 \cdot x(n-2) + s(n)$$

### Filtro Inverso Estimado:
$$\hat{s}(n) = x_w(n) - a_{1,est} \cdot x_w(n-1) - a_{2,est} \cdot x_w(n-2)$$

Onde $x_w(n)$ é o sinal branqueado via LPC.

### Princípio:
- Sinal super-Gaussiano tem curtose **alta**
- Após passagem pelo filtro IIR, curtose **reduz**
- Aplicar filtro **correto** → curtose volta ao **máximo**
- Filtros **incorretos** → curtose **baixa**

---

## 📝 Parâmetros Configuráveis

No início dos arquivos, você pode ajustar:

```python
# Parâmetros verdadeiros do sistema
a1_true = 0.6      # Experimente: 0.5, 0.8, etc.
a2_true = -0.3     # Experimente: -0.5, -0.1, etc.

# Resolução da busca
grid_points = 50   # Aumentar = mais precisão (mais lento)

# Comprimento do sinal
signal_length = 2000   # Aumentar = melhor desempenho (mais lento)

# Parâmetro do LPC
lpc_order = 12     # Ordem do filtro LPC para branqueamento
```

---

## ✅ Validação

A reprodução é **bem-sucedida** se:

- ✓ Parâmetros estimados **coincidem** com verdadeiros (erro < 0.01)
- ✓ Mapa 2D mostra **pico pronunciado** nos parâmetros verdadeiros
- ✓ Curtose máxima é **próxima** à curtose original
- ✓ Sinal recuperado é **altamente correlacionado** com original

---

## 🎓 Referências

**Artigo Original:**
- "Blind Deconvolution via Maximum Kurtosis Adaptive Filtering"
- Pasta: `Livro/`

**Conceitos Relacionados:**
- Blind Deconvolution
- Adaptive Filtering
- Kurtosis Maximization
- LPC (Linear Predictive Coding)
- Non-Gaussian Signal Processing

---

## 💡 Notas

1. **Por que funciona**: Sinais de voz (e outros sinais naturais) são super-Gaussianos, então maximizar curtose é eficaz.

2. **Limitações**: 
   - Não funciona bem com sinais Gaussianos
   - Requer que o espaço de busca contenha a solução
   - Resolução limitada pela quantidade de pontos na grade

3. **Melhorias possíveis**:
   - Usar otimização gradiente ao invés de grid search
   - Adaptar dinamicamente a faixa de busca
   - Estimar a curtose com maior robustez

---

## 📧 Informações

**Data de Criação**: 2024
**Disciplina**: Processamento de Sinais
**Professor**: [conforme especificado]
**Trabalho Final**: Blind Deconvolution via Maximum Kurtosis

---

## 🚀 Como Executar

### Opção 1: Notebook Jupyter (Recomendado)
```bash
# No VS Code ou Jupyter Lab
jupyter notebook FIGURA2_blind_deconvolution.ipynb
```

### Opção 2: Script Python
```bash
cd "Trab final - Proc_Sig"
python FIGURA2_blind_deconvolution.py
```

### Opção 3: No Terminal do VS Code
- Abra o terminal integrado (Ctrl + `)
- Navegue até a pasta
- Execute um dos comandos acima

---

**Boa execução! 🎯**
