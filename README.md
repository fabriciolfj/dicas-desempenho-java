# Dicas desempenho java

## GC
```
Aqui estão dicas para escrever código Java que ajuda o garbage collector a trabalhar eficientemente:

1. **Reutilize objetos** quando possível, em vez de criar novos
   - Use object pools para objetos frequentemente criados e descartados
   - Considere objetos imutáveis para reduzir cópias desnecessárias

2. **Libere referências explicitamente** quando terminar de usar objetos
   - Atribua null a referências que não serão mais usadas
   - Especialmente importante em coleções de longa duração e caches

3. **Prefira tipos primitivos** a classes wrapper quando possível
   - `int` em vez de `Integer`, `long` em vez de `Long`, etc.

4. **Evite estruturas de dados aninhadas muito profundas**
   - Simplifique hierarquias de objetos complexas

5. **Minimize uso de finalizers e referências fracas/suaves**
   - Finalizers podem atrasar a coleta de lixo
   - Use `try-with-resources` para fechamento de recursos em vez de finalizers

6. **Escolha as coleções certas**
   - Dimensione coleções adequadamente para evitar redimensionamentos
   - Use `ArrayList` em vez de `LinkedList` para acesso sequencial

7. **Evite criação excessiva de Strings**
   - Use StringBuilder para concatenações em loops
   - Evite substring excessivo (antes do Java 7u6)

8. **Minimize uso de autoboxing/unboxing** em operações críticas
   - Particularmente em loops de alto volume

9. **Mantenha métodos pequenos e focados**
   - Ajuda o JIT a otimizar melhor o código

10. **Reduza uso de reflexão e clasloading dinâmico**
    - Podem manter classes na memória por mais tempo que o necessário

11. **Evite excessive try-catch em código de alto desempenho**
    - Exceções são pesadas para criação e processamento

12. **Considere uso de arrays** para estruturas fixas e críticas para desempenho

13. **Em aplicações de longa duração, evite vazamentos de memória**
    - Cuidado com listeners/observers que não são removidos
    - Tenha cuidado com caches que crescem indefinidamente

14. **Monitore o comportamento do GC** em produção
    - Use ferramentas como VisualVM, JProfiler ou flags da JVM para diagnóstico

Estas práticas ajudam a reduzir a pressão sobre o garbage collector, minimizando pausas e uso intensivo de CPU durante a coleta de lixo.
```

## Dicas de codigo que ajudam no gc
```
Métodos pequenos são uma boa prática que ajuda o garbage collector e o desempenho geral do Java. Aqui está por que e como implementá-los:

**Benefícios de métodos pequenos para o garbage collector:**

1. **Alocação de pilha mais eficiente** - Métodos pequenos têm frames de pilha menores, o que reduz a necessidade de memória durante a execução

2. **Melhor otimização pelo JIT** - O compilador Just-In-Time consegue otimizar métodos pequenos mais facilmente, incluindo possível inlining

3. **Escopo de variáveis reduzido** - Variáveis locais saem de escopo mais rapidamente, permitindo que seus objetos sejam coletados antes

4. **Menos variáveis temporárias** - Geralmente, métodos menores usam menos variáveis temporárias, reduzindo a pressão no heap

5. **Descarte mais rápido de objetos** - Referências em variáveis locais são liberadas assim que o método termina

**Dicas para criar métodos pequenos e eficientes:**

1. **Limite o tamanho** - Idealmente entre 10-30 linhas de código
   
2. **Responsabilidade única** - Cada método deve fazer apenas uma coisa

3. **Evite parâmetros excessivos** - Muitos parâmetros podem indicar que o método está fazendo demais

4. **Extraia lógica complexa** - Divida blocos de código complexos em métodos auxiliares menores

5. **Minimize variáveis locais** - Quanto menos variáveis locais, menos referências a objetos

6. **Separe loops aninhados** - Extraia loops internos para métodos separados quando possível

Esta abordagem não apenas ajuda o garbage collector, mas também torna seu código mais legível, testável e manutenível.
```

## Cuidado com profilers
```
O problema de sobrecarga na criação de profile, apontamento seguro, e tempo de pausa do kernel envolve diversos desafios técnicos relacionados ao desempenho de sistemas:

**Sobrecarga na criação de profile:**
- A geração de profiles de aplicações/sistemas pode consumir recursos significativos
- Instrumentação de código para coleta de métricas adiciona overhead de CPU e memória
- Samplers baseados em interrupções podem distorcer a medição real da aplicação
- Armazenamento e processamento de grandes volumes de dados de profile aumentam latência
- Métodos intrusivos de profiling podem alterar o comportamento que estão tentando medir

**Apontamento seguro (safe pointing):**
- Relacionado à identificação de pontos seguros para pausar a execução de threads
- Crucial para garbage collection e outras operações que precisam de consistência de estado
- Encontrar safepoints adequados pode ser complexo em código com loops longos ou nativos
- Pausas para safepoints mal distribuídas podem causar stuttering (engasgos) na aplicação
- Implementações inadequadas podem levar a atrasos significativos em sistemas de tempo real

**Tempo de pausa do kernel:**
- Pausas no kernel podem afetar toda a responsividade do sistema
- Operações de I/O síncronas, swapping, e grandes alocações de memória podem causar bloqueios
- Interrupções de hardware e seu processamento afetam o determinismo temporal
- Escalonamento de processos pode introduzir latências imprevisíveis
- Operações como copy-on-write, TLB flushes, e sincronização entre CPUs podem causar pausas perceptíveis

Estes problemas são particularmente críticos em:
- Sistemas de tempo real
- Aplicações de alta disponibilidade
- Ambientes de baixa latência como trading de alta frequência
- Sistemas embarcados com recursos limitados
- Aplicações de gaming ou multimídia onde stuttering é perceptível

A mitigação geralmente envolve técnicas como profiling não-intrusivo, escalonamento de tempo real, kernel com preempção, otimização de safepoints e garbage collectors concorrentes ou incrementais.
```

## Threads virtuais
- adote em contexto que existe bloqueio
