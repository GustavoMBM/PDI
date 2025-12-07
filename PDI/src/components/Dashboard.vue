<template>
  <nav class="navbar">
    <div class="brand">PDI Manager</div>

    <div class="nav-actions"> 
      <button @click="activeTab = 'dashboard'" :class="['nav-button', { active: activeTab === 'dashboard' }]">
        Dashboard
      </button>

      <button @click="activeTab = 'objectives'" :class="['nav-button', { active: activeTab === 'objectives' }]">
        Objetivos
      </button>

      <button @click="activeTab = 'skills'" :class="['nav-button', { active: activeTab === 'skills' }]">
        Competências
      </button>

      <button @click="activeTab = 'feedback'" :class="['nav-button', { active: activeTab === 'feedback' }]">
        Feedback
      </button>

      <button @click="activeTab = 'resources'" :class="['nav-button', { active: activeTab === 'resources' }]">
        Recursos
      </button>
    </div>
  </nav>

  <div class="dashboard-wrapper">
    <main class="main-content scroll-area">
      <header class="header">
        <div class="title-group">
          <h2 class="main-title">Se tornar um programador de sucesso</h2>
          <h4 class="subtitle">Dashboard</h4>
        </div>

        <div class="controls">
          <input type="text" placeholder="Buscar" class="search-input" />
          <button class="primary-btn btn3">Novo Objetivo</button>
        </div>
      </header>

      <section v-if="activeTab === 'dashboard'" class="section">
        <div class="grid grid-3">
          <div class="card">
            <h3 class="card-title">Progresso Geral</h3>
            <p class="big-number">72%</p>
          </div>

          <div class="card">
            <h3 class="card-title">Hábitos Ativos</h3>
            <p class="big-number">5</p>
          </div>

          <div class="card">
            <h3 class="card-title">Metas em Andamento</h3>
            <p class="big-number">3</p>
          </div>
        </div>

        <div class="grid grid-2">
          <div class="card">
            <h3 class="card-title">Resumo Semanal - AI</h3>
            <p class="muted">Com base no seu progresso, você está 72% no caminho para se tornar um desenvolvedor sênior. Continue focando em React e Node.js!</p>
            <div class="small-muted">Atualizado há 2 horas</div>
          </div>

          <div class="card">
            <h3 class="card-title">Próximas Ações</h3>
            <div class="actions-list">
              <div v-for="action in nextActions" :key="action.id" class="action-item">
                <span>{{ action.text }}</span>
                <span class="action-date">{{ action.dueDate }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Aba de Objetivos -->
      <section v-if="activeTab === 'objectives'" class="section">
        <div class="section-header">
          <h3 class="section-title">Meus Objetivos SMART</h3>
          <button class="success-btn btn2">+ Novo Objetivo</button>
        </div>

        <div class="objectives-list">
          <div v-for="objective in objectives" :key="objective.id" class="objective-card">
            <div class="objective-header">
              <h4 class="objective-title">{{ objective.title }}</h4>
              <span class="progress-badge">{{ objective.progress }}%</span>
            </div>
            <p class="muted">{{ objective.description }}</p>

            <div class="objective-meta">
              <div><strong>Data Limite:</strong> {{ objective.deadline }}</div>
              <div><strong>Status:</strong> {{ objective.status }}</div>
            </div>

            <div class="objective-actions">
              <div v-for="action in objective.actions" :key="action.id" class="objective-action-item">
                <input type="checkbox" v-model="action.completed" />
                <span :class="{ 'done': action.completed }">{{ action.text }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!--  Competências -->
      <section v-if="activeTab === 'skills'" class="section">
        <h3 class="section-title">Minhas Competências</h3>

        <div class="grid grid-2">
          <div class="card">
            <h4 class="card-title">Autoavaliação</h4>
            <div class="skills-list">
              <div v-for="skill in skills" :key="skill.id" class="skill-item">
                <div class="skill-row">
                  <span>{{ skill.name }}</span>
                  <span class="muted">Nível {{ skill.currentLevel }}/5</span>
                </div>
                <div class="progress-bar">
                  <div class="progress-fill" :style="{ width: `${(skill.currentLevel / 5) * 100}%` }"></div>
                </div>
              </div>
            </div>
          </div>

          <div class="card">
            <h4 class="card-title">Sugestões de Desenvolvimento</h4>
            <div class="suggestions-list">
              <div v-for="suggestion in skillSuggestions" :key="suggestion.id" class="suggestion-item">
                <div class="suggestion-skill">{{ suggestion.skill }}</div>
                <div class="muted small">{{ suggestion.suggestion }}</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section v-if="activeTab === 'feedback'" class="section">
        <div class="section-header">
          <h3 class="section-title">Feedback e Acompanhamento</h3>
        </div>

        <div class="grid grid-2">
          <div class="card">
            <h4 class="card-title">Feedback Recebido</h4>
            <div class="feedback-list">
              <div v-for="feedback in feedbacks" :key="feedback.id" class="feedback-item">
                <div class="feedback-header">
                  <span class="font-medium">{{ feedback.from }}</span>
                  <span class="muted small">{{ feedback.date }}</span>
                </div>
                <p class="muted">{{ feedback.message }}</p>
              </div>
            </div>
          </div>

          <div class="card">
            <h4 class="card-title">Solicitar Novo Feedback</h4>
            <div class="feedback-form">
              <select>
                <option>Selecione o avaliador...</option>
                <option>Thiago Neps</option>
                <option>Bruno</option>
                <option>Tiago</option>
              </select>
              <textarea placeholder="Descreva o que gostaria de receber feedback..."></textarea>
              <button class="success-btn btn1">Enviar Solicitação</button>
            </div>
          </div>
        </div>
      </section>

      <section v-if="activeTab === 'resources'" class="section">
        <h3 class="section-title">Recursos de Desenvolvimento</h3>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const activeTab = ref('dashboard')

//
// DADOS MOCADOS (mudar pra banco de dados real depois)

const nextActions = ref([
  { id: 1, text: 'Completar curso de Vue.js', dueDate: '15/12/2024' },
  { id: 2, text: 'Revisar projeto com mentor', dueDate: '18/12/2024' },
  { id: 3, text: 'Finalizar certificação C1', dueDate: '22/12/2024' }
])

const objectives = ref([
  {
    id: 1,
    title: 'Dominar Vue.js e Nuxt.js',
    description: 'Específico: Criar 3 projetos com Vue.js. Mensurável: 100% de cobertura de testes. Atingível: 6 meses de estudo. Relevante: Alinhado com vagas de frontend. Temporal: Finalizar até Junho/2024.',
    progress: 60,
    deadline: '30/06/2026',
    status: 'Em Andamento',
    actions: [
      { id: 1, text: 'Completar curso Vue Mastery', completed: true },
      { id: 2, text: 'Criar projeto e-commerce', completed: true },
      { id: 3, text: 'Implementar testes unitários', completed: false },
      { id: 4, text: 'Deploy na Vercel', completed: false }
    ]
  },
  {
    id: 2,
    title: 'Aprimorar habilidades de comunicação',
    description: 'Desenvolver habilidades de apresentação e escrita técnica para melhorar a comunicação com a equipe',
    progress: 30,
    deadline: '31/08/2026',
    status: 'Em Andamento',
    actions: [
      { id: 1, text: 'Fazer 5 apresentações', completed: true },
      { id: 2, text: 'Ler livro x', completed: false },
      { id: 3, text: 'Apresentar palestra', completed: false }
    ]
  }
])

const skills = ref([
  { id: 1, name: 'JavaScript', currentLevel: 4 },
  { id: 2, name: 'Vue.js', currentLevel: 3 },
  { id: 3, name: 'Flask', currentLevel: 3 },
  { id: 4, name: 'Flutter', currentLevel: 2 },
  { id: 5, name: 'Pandas', currentLevel: 1 }
])

const skillSuggestions = ref([
  { id: 1, skill: 'Javascript', suggestion: 'Curso: "Javascript"' },
  { id: 2, skill: 'Flask', suggestion: 'Curso: "backend developer"' },
  { id: 3, skill: 'Python', suggestion: 'Projeto: Forca' }
])

const feedbacks = ref([
  { id: 1, from: 'Thiago Neps', date: '10/12/2024', message: 'Muito progresso mas acho que vc ainda pode melhorar em ...' },
  { id: 2, from: 'Bruno', date: '05/12/2024', message: 'Acho otimo as decisoes que vc tomou! vamos marcar uma reunial' }
])

</script>
<!--Style em scss -->
<style scoped lang="scss">

body { font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; max-height: 100vh;}

.navbar{
  width: 100%;
  color: white;
  padding: 0 8rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 0.1px solid $border;
  margin-inline: 20px;
}

.navbar {
  position: relative;
}

.nav-actions{
  display: flex;
  gap: 1.5rem;
  color: #6B7280; /* gray-500 do tailwind*/
}

.nav-button{
  width: auto;
  text-align: left;
  padding: 0.6rem 0.9rem;
  border-radius: 0.75rem;
  background: transparent;
  border: none;
  color: inherit;
  cursor: pointer;
  transition: color 150ms ease, all 150ms ease;
}

.nav-button:hover{ color: #9CA3AF; }

.nav-button.active{ color: white; }

.dashboard-wrapper{
  max-height: 800px;
  min-height: 700px;
  width: 83.3333%; /* 5/6 */
  color: white;
  display: flex;
  background: $bg-dark;
  border-radius: 1.5rem;
  box-shadow: 0 10px 30px rgba(0,0,0,0.4);
  overflow: hidden;
  margin: 1.5rem auto;
}

.main-content{ 
  flex: 1; padding: 1.5rem; background-color: $card-bg; max-height: 700px;
  display: flex;
  flex-direction: column;
}

.scroll-area {
  flex: 1;
  overflow-y: auto; /* <-- scroll */
  overflow-x: hidden;
  scrollbar-width: thin;
  scrollbar-color: $primary $muted;
}


.header{ display:flex; justify-content:space-between; align-items:center; margin-bottom:2.5rem; }

.title-group{ display:flex; flex-direction:column; gap:0.5rem; }

.main-title{ font-size:1.875rem; font-weight:700; margin:0; }
.subtitle{ font-size:1.5rem; margin:0; }

.controls{ display:flex;align-items:center; width:40%}
.search-input{
  flex:1; padding:1rem 2rem; background: rgba(0,0,0,0.2); border:1px solid rgba(255,255,255,0.1); border-radius:0.5rem; color: white; outline:none;
}

.primary-btn{
  padding:0.5rem 1rem; background: $primary; border-radius:0.5rem; border:none; color:white; cursor:pointer; transition: all 150ms ease; -ms-flex-align: center; align-items: center;
}
.primary-btn:hover{ background: $primary-dark; }

.section{ margin-bottom:1.5rem; }

.grid{ display:grid; gap:1.25rem; }
.grid-3{ grid-template-columns: 1fr; margin-bottom: 20px;}
.grid-2{ grid-template-columns: 1fr; }

/* ===== RESPONSIVIDADE ===== */
@media (max-width: 1024px) {
  .navbar {
    padding: 0 2rem;
  }

  .dashboard-wrapper {
    width: 90%;
    max-height: 500px;
  }

  .controls {
    width: 60%;
  }
}

@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
  }

  .nav-actions {
    flex-wrap: wrap;
    gap: .6rem;
    justify-content: center;
  }

  .controls {
    flex-direction: column;
    width: 100%;
  }

  .primary-btn {
    width: 100%;
  }

  .header {
    flex-direction: column;
    align-items: flex-start;
    gap: 1rem;
  }

  .dashboard-wrapper {
    width: 100%;
    border-radius: 0;
  }
  .grid-3{ grid-template-columns: repeat(3, 1fr); }
  .grid-2{ grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 480px) {
  .main-title {
    font-size: 1.5rem;
  }

  .subtitle {
    font-size: 1rem;
  }

  .search-input {
    padding: .75rem 1rem;
  }

  .card {
    padding: 1rem;
  }
}


.card{
  padding:1.25rem; background: $card-bg; backdrop-filter: blur(6px); -webkit-backdrop-filter: blur(6px); border-radius:1rem; border:1px solid $border;
}

.card-title{ font-size:1.125rem; font-weight:500; margin:0 0 0.75rem 0; }
.big-number{ font-size:2rem; font-weight:700; margin:0; }
.muted{ opacity:0.85; margin:0 0 1rem 0; }
.small-muted{ font-size:0.875rem; opacity:0.6; }

.actions-list{ display:flex; flex-direction:column; gap:0.5rem; }
.action-item{ display:flex; justify-content:space-between; align-items:center; padding:0.6rem; background: rgba(255,255,255,0.03); border-radius:0.5rem; }
.action-date{ font-size:0.9rem; opacity:0.7; }

.section-header{ display:flex; justify-content:space-between; align-items:center; margin-bottom:1rem; }
.section-title{ font-size:1.25rem; font-weight:600; margin:0; }
.success-btn{ padding:0.5rem 1rem; background: $success; border-radius:0.5rem; color:white; border:none; cursor:pointer;}
.success-btn:hover{ background: $success-dark; }
.btn1{width: 100%;}
.btn3{margin-left: 10px; height: 54px;}

.objective-card{ background: $card-bg; border:1px solid $border; padding:1rem; border-radius:1rem; margin-bottom:0.75rem; }
.objective-header{ display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:0.5rem; }
.objective-title{ font-size:1.125rem; margin:0; }
.progress-badge{ padding:0.25rem 0.5rem; background: rgba(59,130,246,0.15); border-radius:9999px; font-size:0.875rem; }
.objective-meta{ display:grid; grid-template-columns: 1fr 1fr; gap:0.5rem; font-size:0.95rem; }
.objective-actions{ margin-top:0.75rem; display:flex; flex-direction:column; gap:0.4rem; }
.objective-action-item{ display:flex; align-items:center; gap:0.75rem; padding:0.4rem; background: rgba(255,255,255,0.03); border-radius:0.5rem; }
.objective-action-item input[type="checkbox"]{ width:1rem; height:1rem; }
.done{ text-decoration: line-through; opacity:0.5; }

.skills-list{ display:flex; flex-direction:column; gap:0.75rem; }
.skill-item{ display:flex; flex-direction:column; gap:0.4rem; }
.skill-row{ display:flex; justify-content:space-between; }
.progress-bar{ width:100%; background: rgba(255,255,255,0.08); height:0.5rem; border-radius:9999px; overflow:hidden; }
.progress-fill{ height:100%; background: $primary; border-radius:9999px; }

.suggestions-list{ display:flex; flex-direction:column; gap:0.5rem; }
.suggestion-item{ padding:0.6rem; background: rgba(255,255,255,0.03); border-radius:0.6rem;}
.suggestion-skill{ font-weight:500;font-size: 1.5rem; }

.feedback-list{ display:flex; flex-direction:column; gap:0.75rem; }
.feedback-item{ padding:0.75rem; background: rgba(255,255,255,0.03); border-radius:0.75rem; }
.feedback-header{ display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:0.5rem; }
.font-medium{ font-weight:500; }

.feedback-form select, .feedback-form textarea{ width:100%; padding:0.6rem; border-radius:0.5rem; background: rgba(0,0,0,0.2); border:1px solid rgba(255,255,255,0.1); color:white; }
.feedback-form textarea{ height:8rem; resize:none; }

input[type="checkbox"] {
  cursor: pointer;
}
option{
  border-radius: 20px;
  background-color: $bg-dark;
}
select {
  background-color: var(--card-bg);
  color: var(--text-color);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 6px 12px;
}

select:focus {
  outline: none;
  border-color: var(--primary);
}

</style>
