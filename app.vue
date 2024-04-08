<template>
  <div class="container">
    <h1 class="title">Progresso de Entrega de Software: Previsão vs Realidade</h1>
    <p class="description">
      O gráfico em questão oferece uma visão crítica do progresso do projeto de software,
      estabelecendo uma comparação entre o planejamento inicial e o que foi efetivamente realizado
      até o momento. A tendência atual demonstra um atraso preocupante em relação ao cronograma
      estabelecido. Caso não sejam tomadas medidas corretivas e as estratégias não sejam ajustadas,
      existe o risco de o software não ser entregue dentro do prazo de 19 semanas. Essa situação
      pode gerar um impacto negativo no relacionamento com o cliente e comprometer acordos
      contratuais.
    </p>
    <div class="chartsContainer">
      <GraficoProjetoSoftware />
    </div>
    <p class="description">
      O gráfico funciona como uma ferramenta essencial de gestão visual, permitindo que a equipe
      identifique rapidamente quaisquer desvios entre o progresso planejado e o real. Sua utilização
      ocorre diariamente em reuniões de status e revisões de sprint, com o objetivo de garantir o
      alinhamento da equipe, a conscientização sobre os desafios e a prontidão para implementar os
      ajustes necessários. Essa abordagem ágil auxilia na resposta rápida a problemas e atrasos,
      mantendo a qualidade do software e assegurando entregas pontuais aos clientes.
    </p>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
  margin: 20px;
}

.title {
  font-size: 1.5em;
  max-width: 910px;
  margin: 0.5em auto;
}

.description {
  font-size: 1em;
  max-width: 910px;
  margin: 2em auto;
}

.chartsContainer {
  display: flex;
  justify-content: center;
  gap: 20px;
}

@media (max-width: 768px) {
  .chartsContainer {
    flex-direction: column;
    align-items: center;
  }
}
</style>
