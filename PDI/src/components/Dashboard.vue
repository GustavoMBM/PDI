<template>
  <nav class="w-full text-white px-32 py-4 flex items-center justify-between shadow">
      <div class="text-lg font-medium text-gray-400 mb-8">PDI Manager</div>
        
        <div class="flex items-center gap-6 text-gray-500"> 
          <button @click="activeTab = 'dashboard'" 
                  :class="['w-full text-left p-3 rounded-lg transition-colors', 
                          activeTab === 'dashboard' ? 'text-white' : 'hover:text-gray-400']">
            Dashboard
          </button>
          
          <button @click="activeTab = 'objectives'" 
                  :class="['w-full text-left p-3 rounded-lg transition-colors', 
                          activeTab === 'objectives' ? 'text-white' : 'hover:text-gray-400']">
            Objetivos
          </button>
          
          <button @click="activeTab = 'skills'" 
                  :class="['w-full text-left p-3 rounded-lg transition-colors', 
                          activeTab === 'skills' ? 'text-white' : 'hover:text-gray-400']">
            Competências
          </button>
          
          <button @click="activeTab = 'feedback'" 
                  :class="['w-full text-left p-3 rounded-lg transition-colors', 
                          activeTab === 'feedback' ? 'text-white' : 'hover:text-gray-400']">
            Feedback
          </button>
          
          <button @click="activeTab = 'resources'" 
                  :class="['w-full text-left p-3 rounded-lg transition-colors', 
                          activeTab === 'resources' ? 'text-white' : 'hover:text-gray-400']">
            Recursos
          </button>
        </div>
    </nav>
  <div class="min-h-screen w-5/6 text-white flex bg-[#272C2F] rounded-3xl shadow-lg overflow-hidden">

    <main class="flex-1 p-6 md:p-10">
      <header class="flex justify-between items-center mb-10">
        <div class="flex flex-col gap-2">
          <h2 class="text-3xl font-bold">Se tornar um programador de sucesso</h2>
          <h4 class="text-2xl">Dashboard</h4>
        </div>
        
        <div class="flex gap-4 items-center w-2/5">
          <input type="text" placeholder="Buscar" class="w-full px-3 py-2 bg-black/20 border border-white/10 rounded-lg focus:outline-none focus:border-white/30" />
          <button class="px-4 py-2 bg-blue-600 rounded-lg hover:bg-blue-700 transition-colors">
            Novo Objetivo
          </button>
        </div>
      </header>

      <section v-if="activeTab === 'dashboard'" class="space-y-6">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div class="p-6 bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10">
            <h3 class="text-lg font-medium mb-3">Progresso Geral</h3>
            <p class="text-4xl font-bold">72%</p>
          </div>

          <div class="p-6 bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10">
            <h3 class="text-lg font-medium mb-3">Hábitos Ativos</h3>
            <p class="text-4xl font-bold">5</p>
          </div>

          <div class="p-6 bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10">
            <h3 class="text-lg font-medium mb-3">Metas em Andamento</h3>
            <p class="text-4xl font-bold">3</p>
          </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <div class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <h3 class="text-xl font-semibold mb-4">Resumo Semanal - AI</h3>
            <p class="opacity-80 mb-4">Com base no seu progresso, você está 72% no caminho para se tornar um desenvolvedor sênior. Continue focando em React e Node.js!</p>
            <div class="text-sm opacity-60">Atualizado há 2 horas</div>
          </div>

          <div class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <h3 class="text-xl font-semibold mb-4">Próximas Ações</h3>
            <div class="space-y-3">
              <div v-for="action in nextActions" :key="action.id" 
                   class="flex items-center justify-between p-3 bg-white/5 rounded-lg">
                <span>{{ action.text }}</span>
                <span class="text-sm opacity-60">{{ action.dueDate }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Aba de Objetivos -->
      <section v-if="activeTab === 'objectives'" class="space-y-6">
        <div class="flex justify-between items-center">
          <h3 class="text-2xl font-semibold">Meus Objetivos SMART</h3>
          <button class="px-4 py-2 bg-green-600 rounded-lg hover:bg-green-700 transition-colors">
            + Novo Objetivo
          </button>
        </div>

        <div class="grid grid-cols-1 gap-4">
          <div v-for="objective in objectives" :key="objective.id" 
               class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <div class="flex justify-between items-start mb-4">
              <h4 class="text-xl font-semibold">{{ objective.title }}</h4>
              <span class="px-3 py-1 bg-blue-500/20 rounded-full text-sm">{{ objective.progress }}%</span>
            </div>
            <p class="opacity-80 mb-4">{{ objective.description }}</p>
            
            <div class="grid grid-cols-2 gap-4 text-sm">
              <div>
                <strong>Data Limite:</strong> {{ objective.deadline }}
              </div>
              <div>
                <strong>Status:</strong> {{ objective.status }}
              </div>
            </div>

            <div class="mt-4 space-y-2">
              <div v-for="action in objective.actions" :key="action.id" 
                   class="flex items-center gap-3 p-2 bg-white/5 rounded">
                <input type="checkbox" v-model="action.completed" 
                       class="rounded border-white/20 bg-transparent">
                <span :class="{'line-through opacity-50': action.completed}">
                  {{ action.text }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!--  Competências -->
      <section v-if="activeTab === 'skills'" class="space-y-6">
        <h3 class="text-2xl font-semibold">Minhas Competências</h3>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <h4 class="text-lg font-semibold mb-4">Autoavaliação</h4>
            <div class="space-y-4">
              <div v-for="skill in skills" :key="skill.id" class="space-y-2">
                <div class="flex justify-between">
                  <span>{{ skill.name }}</span>
                  <span class="opacity-60">Nível {{ skill.currentLevel }}/5</span>
                </div>
                <div class="w-full bg-white/10 rounded-full h-2">
                  <div class="bg-blue-500 h-2 rounded-full" 
                       :style="{ width: `${(skill.currentLevel / 5) * 100}%` }"></div>
                </div>
              </div>
            </div>
          </div>

          <div class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <h4 class="text-lg font-semibold mb-4">Sugestões de Desenvolvimento</h4>
            <div class="space-y-3">
              <div v-for="suggestion in skillSuggestions" :key="suggestion.id" 
                   class="p-3 bg-white/5 rounded-lg">
                <div class="font-medium">{{ suggestion.skill }}</div>
                <div class="text-sm opacity-80 mt-1">{{ suggestion.suggestion }}</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section v-if="activeTab === 'feedback'" class="space-y-6">
        <div class="flex justify-between items-center">
          <h3 class="text-2xl font-semibold">Feedback e Acompanhamento</h3>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <div class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <h4 class="text-lg font-semibold mb-4">Feedback Recebido</h4>
            <div class="space-y-4">
              <div v-for="feedback in feedbacks" :key="feedback.id" 
                   class="p-4 bg-white/5 rounded-lg">
                <div class="flex justify-between items-start mb-2">
                  <span class="font-medium">{{ feedback.from }}</span>
                  <span class="text-sm opacity-60">{{ feedback.date }}</span>
                </div>
                <p class="opacity-80">{{ feedback.message }}</p>
              </div>
            </div>
          </div>

          <div class="bg-black/20 backdrop-blur-xl rounded-2xl border border-white/10 p-6">
            <h4 class="text-lg font-semibold mb-4">Solicitar Novo Feedback</h4>
            <div class="space-y-4">
              <select class="w-full px-3 py-2 bg-black/20 border border-white/10 rounded-lg">
                <option>Selecione o avaliador...</option>
                <option>Thiago Neps</option>
                <option>Bruno</option>
                <option>Tiago</option>
              </select>
              <textarea placeholder="Descreva o que gostaria de receber feedback..." 
                        class="w-full h-32 px-3 py-2 bg-black/20 border border-white/10 rounded-lg resize-none"></textarea>
              <button class="w-full px-4 py-2 bg-green-600 rounded-lg hover:bg-green-700 transition-colors">
                Enviar Solicitação
              </button>
            </div>
          </div>
        </div>
      </section>

      <section v-if="activeTab === 'resources'" class="space-y-6">
        <h3 class="text-2xl font-semibold">Recursos de Desenvolvimento</h3>

        
       
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

<style>

</style>