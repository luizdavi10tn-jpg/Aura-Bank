<!DOCTYPE html>
<html lang="pt-BR">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Aura Bank - Seu Banco Digital</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#f0f3ff',
              100: '#e0e7ff',
              500: '#6366f1',
              600: '#4f46e5',
              700: '#4338ca',
              900: '#1e1b4b',
            }
          }
        }
      }
    }
  </script>
  <style>
    /* Estilização da barra de rolagem */
    ::-webkit-scrollbar {
      width: 4px;
      height: 4px;
    }

    ::-webkit-scrollbar-track {
      background: rgba(0, 0, 0, 0.05);
    }

    ::-webkit-scrollbar-thumb {
      background: rgba(0, 0, 0, 0.2);
      border-radius: 4px;
    }

    .hide-scrollbar::-webkit-scrollbar {
      display: none;
    }

    .hide-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }

    /* Animação do Leitor Laser */
    @keyframes scanAnimation {
      0% {
        top: 5%;
      }

      50% {
        top: 90%;
      }

      100% {
        top: 5%;
      }
    }

    .scan-laser {
      animation: scanAnimation 2s infinite ease-in-out;
    }

    /* Animação do Scanner Facial */
    @keyframes faceScanPulse {

      0%,
      100% {
        opacity: 0.3;
        transform: scale(0.98);
      }

      50% {
        opacity: 0.8;
        transform: scale(1.02);
      }
    }

    .face-pulse {
      animation: faceScanPulse 2s infinite ease-in-out;
    }
  </style>
</head>

<body class="bg-slate-950 text-slate-100 font-sans min-h-screen flex items-center justify-center p-0 sm:p-4">

  <!-- Container Principal do Telemóvel -->
  <div
    class="w-full max-w-md h-screen sm:h-[844px] bg-slate-900 sm:rounded-[40px] shadow-2xl border-0 sm:border-[8px] border-slate-800 flex flex-col overflow-hidden relative">

    <!-- Barra de Estado do Telemóvel -->
    <div class="bg-slate-900 px-6 pt-3 pb-2 flex justify-between items-center text-xs text-slate-400 select-none z-50">
      <span id="clock" class="font-semibold">12:00</span>
      <div class="w-16 h-4 bg-slate-800 rounded-full hidden sm:block"></div>
      <div class="flex items-center space-x-1.5">
        <i data-lucide="wifi" class="w-3.5 h-3.5"></i>
        <i data-lucide="battery" class="w-4 h-4"></i>
      </div>
    </div>

    <!-- TELA DE LOGIN E CADASTRO -->
    <div id="view-login"
      class="flex-1 overflow-y-auto px-6 py-6 space-y-5 hide-scrollbar flex flex-col justify-between z-40 bg-slate-900">

      <!-- Cabeçalho do Login -->
      <div class="text-center space-y-2 pt-2">
        <div
          class="w-14 h-14 rounded-2xl bg-gradient-to-tr from-indigo-600 to-violet-500 flex items-center justify-center text-white mx-auto shadow-lg shadow-indigo-500/30">
          <i data-lucide="shield-check" class="w-8 h-8"></i>
        </div>
        <h1 class="text-xl font-bold text-white tracking-tight">Aura Bank</h1>
        <p id="login-header-subtitle" class="text-xs text-slate-400">Acesse a sua conta existente ou cadastre-se</p>
      </div>

      <!-- SELETOR DE MODO: CONTA EXISTENTE VS CRIAR CONTA -->
      <div class="grid grid-cols-2 bg-slate-800/80 p-1 rounded-xl border border-slate-700/60 text-xs">
        <button id="tab-btn-login" onclick="switchLoginMode('login')"
          class="py-2 rounded-lg font-semibold bg-indigo-600 text-white shadow transition">
          Conta Existente
        </button>
        <button id="tab-btn-register" onclick="switchLoginMode('register')"
          class="py-2 rounded-lg font-semibold text-slate-400 hover:text-white transition">
          Criar Nova Conta
        </button>
      </div>

      <!-- FORMULÁRIO 1: CONTA EXISTENTE (LOGIN RÁPIDO) -->
      <div id="mode-login-form" class="space-y-4">
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">E-mail Cadastrado</label>
          <input type="email" id="login-email" placeholder="seu@email.com"
            class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3.5 py-2.5 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
        </div>

        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">CPF / Identidade</label>
          <input type="text" id="login-doc" placeholder="000.000.000-00"
            class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3.5 py-2.5 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
        </div>

        <button onclick="processExistingAccountLogin()"
          class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
          Entrar na Conta <i data-lucide="log-in" class="w-4 h-4"></i>
        </button>
      </div>

      <!-- FORMULÁRIO 2: CRIAR NOVA CONTA (CADASTRO PASSO A PASSO) -->
      <div id="mode-register-form" class="hidden space-y-4">

        <!-- PASSO 1: DADOS PESSOAIS -->
        <div id="login-step-1" class="space-y-4">
          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">E-mail</label>
            <input type="email" id="input-email" placeholder="seu@email.com"
              class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3.5 py-2.5 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
          </div>

          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">CPF / Identidade</label>
            <input type="text" id="input-doc" placeholder="000.000.000-00"
              class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3.5 py-2.5 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
          </div>

          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">Data de Nascimento</label>
            <input type="date" id="input-dob" onchange="checkAgeRequirement()"
              class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3.5 py-2.5 text-xs text-white focus:outline-none focus:border-indigo-500" />
          </div>

          <!-- Autorização dos Pais (Caso seja menor de 15 anos) -->
          <div id="parent-permission-box"
            class="hidden p-3.5 bg-amber-500/10 border border-amber-500/30 rounded-2xl space-y-3">
            <div class="flex items-center gap-2 text-amber-400">
              <i data-lucide="users" class="w-4 h-4"></i>
              <span class="text-xs font-bold">Autorização dos Pais (Menor de 15 Anos)</span>
            </div>
            <p class="text-[11px] text-slate-300">Como você tem menos de 15 anos, informe os dados do seu responsável
              legal para continuar.</p>
            <div>
              <label class="block text-[11px] text-slate-300 mb-1">Nome do Responsável Legal</label>
              <input type="text" id="parent-name" placeholder="Nome Completo do Pai ou Mãe"
                class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white focus:outline-none focus:border-amber-500" />
            </div>
            <div>
              <label class="block text-[11px] text-slate-300 mb-1">CPF do Responsável</label>
              <input type="text" id="parent-cpf" placeholder="000.000.000-00"
                class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white focus:outline-none focus:border-amber-500" />
            </div>
          </div>

          <button onclick="goToVerificationStep()"
            class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
            Continuar Cadastro <i data-lucide="arrow-right" class="w-4 h-4"></i>
          </button>
        </div>

        <!-- PASSO 2: CÓDIGO DE VERIFICAÇÃO -->
        <div id="login-step-2" class="hidden space-y-4">
          <div class="text-center space-y-1">
            <h3 class="text-xs font-bold text-white">Verificação de Segurança</h3>
            <p class="text-[11px] text-slate-400">Escolha onde deseja receber o seu código:</p>
          </div>

          <div class="grid grid-cols-2 gap-3">
            <button onclick="selectAuthMethod('email')" id="btn-method-email"
              class="p-3 bg-indigo-600/20 border border-indigo-500 text-white rounded-xl text-center space-y-1">
              <i data-lucide="mail" class="w-5 h-5 mx-auto text-indigo-400"></i>
              <p class="text-[11px] font-semibold">E-mail</p>
            </button>
            <button onclick="selectAuthMethod('sms')" id="btn-method-sms"
              class="p-3 bg-slate-800 border border-slate-700 text-slate-400 rounded-xl text-center space-y-1">
              <i data-lucide="smartphone" class="w-5 h-5 mx-auto"></i>
              <p class="text-[11px] font-semibold">SMS / Telemóvel</p>
            </button>
          </div>

          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">Código de 6 dígitos</label>
            <input type="text" id="input-code" maxlength="6" placeholder="123456"
              class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3.5 py-2.5 text-center text-sm tracking-widest text-white font-mono focus:outline-none focus:border-indigo-500" />
          </div>

          <button onclick="goToFacialStep()"
            class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
            Validar Código <i data-lucide="check-circle" class="w-4 h-4"></i>
          </button>
        </div>

        <!-- PASSO 3: VERIFICAÇÃO FACIAL -->
        <div id="login-step-3" class="hidden space-y-4 text-center">
          <div class="space-y-1">
            <h3 class="text-xs font-bold text-white">Reconhecimento Facial</h3>
            <p class="text-[11px] text-slate-400">Enquadre o seu rosto no círculo para validar</p>
          </div>

          <div
            class="relative w-44 h-44 mx-auto rounded-full border-4 border-indigo-500/60 overflow-hidden bg-slate-950 flex items-center justify-center shadow-xl">
            <div
              class="absolute inset-0 bg-gradient-to-tr from-indigo-950/80 via-slate-900 to-indigo-900/60 flex flex-col items-center justify-center text-indigo-400">
              <i data-lucide="user" class="w-24 h-24 text-indigo-400/80 face-pulse"></i>
            </div>
            <div class="absolute inset-2 border-2 border-dashed border-indigo-400/60 rounded-full animate-spin"
              style="animation-duration: 8s;"></div>
          </div>

          <button onclick="completeLogin()"
            class="w-full py-3 bg-emerald-600 hover:bg-emerald-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
            <i data-lucide="scan-face" class="w-4 h-4"></i> Confirmar Biometria Facial
          </button>
        </div>

      </div>

      <p class="text-[10px] text-slate-500 text-center pb-2">Protegido por Criptografia Aura 256-bit</p>
    </div>

    <!-- PAINEL PRINCIPAL DA APLICAÇÃO -->
    <div id="app-content" class="hidden flex-1 flex flex-col overflow-hidden relative">

      <!-- Header Principal -->
      <header class="bg-slate-900 px-5 py-3 border-b border-slate-800/80 flex items-center justify-between z-40">
        <div class="flex items-center space-x-3">
          <div
            class="w-10 h-10 rounded-full bg-gradient-to-tr from-indigo-600 to-violet-500 flex items-center justify-center font-bold text-white shadow-md text-sm">
            JS
          </div>
          <div>
            <p class="text-xs text-slate-400">Olá,</p>
            <h2 id="user-display-name" class="text-sm font-semibold text-white flex items-center gap-1">
              João Silva <i data-lucide="badge-check" class="w-3.5 h-3.5 text-indigo-400"></i>
            </h2>
          </div>
        </div>
        <div class="flex items-center space-x-2">
          <button onclick="toggleBalanceVisibility()"
            class="p-2 rounded-full hover:bg-slate-800 text-slate-400 hover:text-white transition">
            <i id="eye-icon" data-lucide="eye" class="w-5 h-5"></i>
          </button>
          <button onclick="openChatModal()"
            class="p-2 rounded-full hover:bg-slate-800 text-slate-400 hover:text-white transition relative">
            <i data-lucide="message-square" class="w-5 h-5"></i>
            <span class="absolute top-1.5 right-1.5 w-2 h-2 bg-indigo-500 rounded-full"></span>
          </button>
        </div>
      </header>

      <!-- CONTEÚDO DAS ABAS -->
      <main class="flex-1 overflow-y-auto px-5 py-4 space-y-5 hide-scrollbar pb-24" id="main-content">

        <!-- TELA 1: INÍCIO (DASHBOARD) -->
        <div id="view-dashboard" class="space-y-5">
          <!-- Card do Saldo -->
          <div
            class="bg-gradient-to-br from-indigo-900/60 via-slate-900 to-slate-900 border border-indigo-500/20 rounded-2xl p-5 shadow-lg relative overflow-hidden">
            <div class="absolute -right-6 -bottom-6 w-32 h-32 bg-indigo-500/10 rounded-full blur-2xl"></div>
            <div class="flex justify-between items-center text-slate-400 text-xs mb-1">
              <span>Saldo Disponível</span>
              <span id="account-type-tag"
                class="bg-indigo-500/10 text-indigo-400 px-2 py-0.5 rounded-full text-[10px] font-medium border border-indigo-500/20">Conta
                Corrente</span>
            </div>
            <div class="text-3xl font-bold text-white tracking-tight my-1">
              <span id="balance-display">R$ 4.850,30</span>
            </div>
            <div
              class="flex items-center justify-between pt-2 text-xs text-slate-400 border-t border-slate-800/80 mt-3">
              <span class="flex items-center gap-1">
                <i data-lucide="trending-up" class="w-3.5 h-3.5 text-emerald-400"></i> Rendimento este mês: <strong
                  class="text-emerald-400 font-medium">+R$ 38,40</strong>
              </span>
            </div>
          </div>

          <!-- Botões de Ação Rápida -->
          <div class="grid grid-cols-4 gap-3">
            <button onclick="switchTab('pix')" class="flex flex-col items-center gap-1.5 group">
              <div
                class="w-full aspect-square rounded-2xl bg-slate-800 group-hover:bg-indigo-600/20 group-hover:border-indigo-500/50 border border-slate-700/60 flex items-center justify-center text-indigo-400 group-hover:text-indigo-300 transition-all shadow-sm">
                <i data-lucide="qr-code" class="w-6 h-6"></i>
              </div>
              <span class="text-xs text-slate-300 group-hover:text-white font-medium">Pix</span>
            </button>

            <button onclick="switchTab('boletos')" class="flex flex-col items-center gap-1.5 group">
              <div
                class="w-full aspect-square rounded-2xl bg-slate-800 group-hover:bg-indigo-600/20 group-hover:border-indigo-500/50 border border-slate-700/60 flex items-center justify-center text-indigo-400 group-hover:text-indigo-300 transition-all shadow-sm">
                <i data-lucide="barcode" class="w-6 h-6"></i>
              </div>
              <span class="text-xs text-slate-300 group-hover:text-white font-medium">Pagar</span>
            </button>

            <button onclick="switchTab('cards')" class="flex flex-col items-center gap-1.5 group">
              <div
                class="w-full aspect-square rounded-2xl bg-slate-800 group-hover:bg-indigo-600/20 group-hover:border-indigo-500/50 border border-slate-700/60 flex items-center justify-center text-indigo-400 group-hover:text-indigo-300 transition-all shadow-sm">
                <i data-lucide="credit-card" class="w-6 h-6"></i>
              </div>
              <span class="text-xs text-slate-300 group-hover:text-white font-medium">Cartão</span>
            </button>

            <button onclick="switchTab('invest')" class="flex flex-col items-center gap-1.5 group">
              <div
                class="w-full aspect-square rounded-2xl bg-slate-800 group-hover:bg-indigo-600/20 group-hover:border-indigo-500/50 border border-slate-700/60 flex items-center justify-center text-indigo-400 group-hover:text-indigo-300 transition-all shadow-sm">
                <i data-lucide="line-chart" class="w-6 h-6"></i>
              </div>
              <span class="text-xs text-slate-300 group-hover:text-white font-medium">Investir</span>
            </button>
          </div>

          <!-- Banner Informativo -->
          <div
            class="bg-gradient-to-r from-violet-900/40 to-indigo-900/40 border border-violet-500/30 rounded-2xl p-4 flex items-center justify-between">
            <div class="space-y-1">
              <span
                class="text-[10px] bg-violet-500/20 text-violet-300 px-2 py-0.5 rounded-full font-semibold uppercase tracking-wider">Aura
                Black</span>
              <h4 class="text-xs font-semibold text-white">Seu limite de crédito aumentou!</h4>
              <p class="text-[11px] text-slate-300">Ajuste o valor diretamente pelo app.</p>
            </div>
            <button onclick="switchTab('cards')"
              class="px-3 py-1.5 bg-violet-600 hover:bg-violet-500 text-white text-xs font-semibold rounded-xl transition shadow">
              Ajustar
            </button>
          </div>

          <!-- Extrato das Transações -->
          <div class="space-y-3">
            <div class="flex items-center justify-between">
              <h3 class="text-sm font-semibold text-white flex items-center gap-2">
                <i data-lucide="arrow-left-right" class="w-4 h-4 text-indigo-400"></i> Últimas Movimentações
              </h3>
              <button onclick="clearTransactions()" class="text-xs text-indigo-400 hover:underline">Limpar</button>
            </div>

            <div id="transactions-list" class="space-y-2">
              <!-- Renderizado dinamicamente -->
            </div>
          </div>
        </div>

        <!-- TELA 2: ÁREA PIX -->
        <div id="view-pix" class="hidden space-y-5">
          <div class="flex items-center justify-between border-b border-slate-800 pb-3">
            <h2 class="text-base font-semibold text-white flex items-center gap-2">
              <i data-lucide="qr-code" class="w-5 h-5 text-indigo-400"></i> Área Pix
            </h2>
            <span class="text-xs text-slate-400">Envio Instantâneo</span>
          </div>

          <!-- Botão Leitor de QR Code Pix -->
          <div
            class="bg-gradient-to-br from-indigo-900/40 to-slate-800/80 border border-indigo-500/30 rounded-2xl p-4 text-center space-y-3">
            <div
              class="w-12 h-12 rounded-full bg-indigo-600/20 text-indigo-400 flex items-center justify-center mx-auto border border-indigo-500/30">
              <i data-lucide="camera" class="w-6 h-6"></i>
            </div>
            <div>
              <h3 class="text-xs font-bold text-white">Escanear QR Code Pix</h3>
              <p class="text-[11px] text-slate-400">Aproxime o código para ler os dados do pagamento</p>
            </div>
            <button onclick="openCameraModal('pix')"
              class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
              <i data-lucide="qr-code" class="w-4 h-4"></i> Abrir Leitor QR Code
            </button>
          </div>

          <div class="relative flex py-1 items-center">
            <div class="flex-grow border-t border-slate-800"></div>
            <span class="flex-shrink mx-3 text-[10px] text-slate-500 uppercase font-semibold">Ou insira os dados</span>
            <div class="flex-grow border-t border-slate-800"></div>
          </div>

          <!-- Formulário Pix -->
          <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-4 space-y-4">
            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Tipo de Chave</label>
              <select id="pix-type"
                class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white focus:outline-none focus:border-indigo-500">
                <option value="cpf">CPF / CNPJ</option>
                <option value="email">E-mail</option>
                <option value="phone">Telemóvel</option>
                <option value="random">Chave Aleatória</option>
              </select>
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Chave Pix do Destinatário</label>
              <input type="text" id="pix-key" placeholder="Insira a chave aqui..."
                class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Valor (R$)</label>
              <input type="number" id="pix-amount" placeholder="0,00" step="0.01"
                class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white font-semibold placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Descrição (Opcional)</label>
              <input type="text" id="pix-desc" placeholder="Ex: Almoço, Presente..."
                class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
            </div>

            <button onclick="processPixTransfer()"
              class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
              <i data-lucide="send" class="w-4 h-4"></i> Transferir Agora
            </button>
          </div>

          <!-- Contatos Frequentes -->
          <div class="space-y-2">
            <h3 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Contatos Frequentes</h3>
            <div class="flex space-x-3 overflow-x-auto pb-2 hide-scrollbar">
              <button onclick="fillPixContact('Maria Oliveira', 'maria@email.com')"
                class="flex flex-col items-center bg-slate-800/40 p-2.5 rounded-xl border border-slate-700/50 min-w-[80px]">
                <div
                  class="w-9 h-9 rounded-full bg-pink-600/30 text-pink-400 border border-pink-500/30 flex items-center justify-center text-xs font-bold mb-1">
                  MO</div>
                <span class="text-[11px] text-slate-200 font-medium">Maria</span>
              </button>
              <button onclick="fillPixContact('Carlos Eduardo', '11988887777')"
                class="flex flex-col items-center bg-slate-800/40 p-2.5 rounded-xl border border-slate-700/50 min-w-[80px]">
                <div
                  class="w-9 h-9 rounded-full bg-amber-600/30 text-amber-400 border border-amber-500/30 flex items-center justify-center text-xs font-bold mb-1">
                  CE</div>
                <span class="text-[11px] text-slate-200 font-medium">Carlos</span>
              </button>
              <button onclick="fillPixContact('Livraria Central', 'contato@livraria.com')"
                class="flex flex-col items-center bg-slate-800/40 p-2.5 rounded-xl border border-slate-700/50 min-w-[80px]">
                <div
                  class="w-9 h-9 rounded-full bg-emerald-600/30 text-emerald-400 border border-emerald-500/30 flex items-center justify-center text-xs font-bold mb-1">
                  LC</div>
                <span class="text-[11px] text-slate-200 font-medium">Livraria</span>
              </button>
            </div>
          </div>
        </div>

        <!-- TELA 3: PAGAMENTO DE BOLETOS -->
        <div id="view-boletos" class="hidden space-y-5">
          <div class="flex items-center justify-between border-b border-slate-800 pb-3">
            <h2 class="text-base font-semibold text-white flex items-center gap-2">
              <i data-lucide="barcode" class="w-5 h-5 text-indigo-400"></i> Pagamento de Contas
            </h2>
            <span class="text-xs text-slate-400">Boletos e Faturas</span>
          </div>

          <div
            class="bg-gradient-to-br from-indigo-900/40 to-slate-800/80 border border-indigo-500/30 rounded-2xl p-4 text-center space-y-3">
            <div
              class="w-12 h-12 rounded-full bg-indigo-600/20 text-indigo-400 flex items-center justify-center mx-auto border border-indigo-500/30">
              <i data-lucide="barcode" class="w-6 h-6"></i>
            </div>
            <div>
              <h3 class="text-xs font-bold text-white">Escanear Boleto Impresso</h3>
              <p class="text-[11px] text-slate-400">Aproxime o código impresso da câmera</p>
            </div>
            <button onclick="openCameraModal('boleto')"
              class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
              <i data-lucide="scan" class="w-4 h-4"></i> Abrir Leitor de Boletos
            </button>
          </div>

          <div class="relative flex py-1 items-center">
            <div class="flex-grow border-t border-slate-800"></div>
            <span class="flex-shrink mx-3 text-[10px] text-slate-500 uppercase font-semibold">Ou digite a linha
              digitável</span>
            <div class="flex-grow border-t border-slate-800"></div>
          </div>

          <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-4 space-y-4">
            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Código de Barras</label>
              <div class="relative">
                <input type="text" id="barcode-input" placeholder="00090.00005 00000.000000 00000.000000 0 0000000000"
                  class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500 pr-10" />
                <button onclick="simulateScanBarcode()"
                  class="absolute right-2 top-2 text-indigo-400 hover:text-indigo-300" title="Simulação Rápida">
                  <i data-lucide="sparkles" class="w-4 h-4"></i>
                </button>
              </div>
            </div>

            <div id="bill-details"
              class="hidden bg-slate-900/80 p-3 rounded-xl border border-slate-700 space-y-1 text-xs">
              <div class="flex justify-between text-slate-400">
                <span>Beneficiário:</span>
                <span id="bill-payee" class="text-white font-medium">Companhia de Energia S.A.</span>
              </div>
              <div class="flex justify-between text-slate-400">
                <span>Vencimento:</span>
                <span id="bill-due" class="text-white font-medium">28/07/2026</span>
              </div>
              <div class="flex justify-between text-slate-400">
                <span>Valor:</span>
                <span id="bill-value" class="text-indigo-400 font-bold">R$ 184,90</span>
              </div>
            </div>

            <button id="btn-pay-bill" onclick="processBillPayment()"
              class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
              <i data-lucide="check-circle" class="w-4 h-4"></i> Confirmar Pagamento
            </button>
          </div>
        </div>

        <!-- TELA 4: CARTÕES -->
        <div id="view-cards" class="hidden space-y-5">
          <div class="flex items-center justify-between border-b border-slate-800 pb-3">
            <h2 class="text-base font-semibold text-white flex items-center gap-2">
              <i data-lucide="credit-card" class="w-5 h-5 text-indigo-400"></i> Meus Cartões
            </h2>
            <span id="card-status-badge"
              class="text-[10px] bg-emerald-500/20 text-emerald-400 px-2 py-0.5 rounded-full font-semibold">ATIVO</span>
          </div>

          <!-- Cartão de Crédito Interativo -->
          <div id="virtual-card"
            class="bg-gradient-to-tr from-indigo-700 via-violet-600 to-indigo-900 rounded-2xl p-5 text-white shadow-xl space-y-6 relative overflow-hidden transition-all duration-300">
            <div class="flex justify-between items-center">
              <span class="font-bold tracking-wider text-sm italic">AURA</span>
              <i data-lucide="contactless" class="w-5 h-5 opacity-80"></i>
            </div>

            <div class="space-y-1">
              <p class="text-[10px] opacity-75 uppercase tracking-widest">Número do Cartão</p>
              <p id="card-number" class="font-mono text-base tracking-widest">•••• •••• •••• 8842</p>
            </div>

            <div class="flex justify-between items-end text-xs">
              <div>
                <p class="text-[9px] opacity-75 uppercase">Titular</p>
                <p id="card-holder-name" class="font-semibold tracking-wide">JOÃO SILVA</p>
              </div>
              <div>
                <p class="text-[9px] opacity-75 uppercase">Validade</p>
                <p class="font-semibold">08/30</p>
              </div>
              <div>
                <p class="text-[9px] opacity-75 uppercase">CVV</p>
                <p id="card-cvv" class="font-semibold">•••</p>
              </div>
            </div>
          </div>

          <!-- Ajustes do Cartão -->
          <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-4 space-y-4">
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-3">
                <i data-lucide="lock" class="w-5 h-5 text-indigo-400"></i>
                <div>
                  <p class="text-xs font-semibold text-white">Bloqueio Temporário</p>
                  <p class="text-[10px] text-slate-400">Bloquear compras presenciais e virtuais</p>
                </div>
              </div>
              <label class="relative inline-flex items-center cursor-pointer">
                <input type="checkbox" id="toggle-block-card" onchange="toggleCardBlock()" class="sr-only peer">
                <div
                  class="w-9 h-5 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-indigo-600">
                </div>
              </label>
            </div>

            <div class="flex items-center justify-between border-t border-slate-700/50 pt-3">
              <div class="flex items-center gap-3">
                <i data-lucide="eye" class="w-5 h-5 text-indigo-400"></i>
                <div>
                  <p class="text-xs font-semibold text-white">Exibir Dados do Cartão</p>
                  <p class="text-[10px] text-slate-400">Mostra o número e código CVV</p>
                </div>
              </div>
              <button onclick="toggleCardDetails()" class="text-xs text-indigo-400 hover:underline font-medium">
                <span id="card-details-btn-text">Mostrar</span>
              </button>
            </div>

            <div class="border-t border-slate-700/50 pt-3 space-y-2">
              <div class="flex justify-between text-xs">
                <span class="text-slate-300">Ajustar Limite</span>
                <span id="limit-val" class="font-bold text-indigo-400">R$ 5.000,00</span>
              </div>
              <input type="range" id="limit-range" min="1000" max="15000" step="500" value="5000"
                oninput="updateCardLimit(this.value)"
                class="w-full accent-indigo-500 h-1.5 bg-slate-700 rounded-lg appearance-none cursor-pointer" />
            </div>
          </div>
        </div>

        <!-- TELA 5: INVESTIMENTOS -->
        <div id="view-invest" class="hidden space-y-5">
          <div class="flex items-center justify-between border-b border-slate-800 pb-3">
            <h2 class="text-base font-semibold text-white flex items-center gap-2">
              <i data-lucide="line-chart" class="w-5 h-5 text-indigo-400"></i> Investimentos
            </h2>
            <span class="text-xs text-emerald-400 flex items-center gap-1 font-semibold">
              <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Renda Fixa
            </span>
          </div>

          <div class="bg-slate-800/80 border border-slate-700/60 rounded-2xl p-4 flex items-center justify-between">
            <div>
              <p class="text-xs text-slate-400">Total Aplicado</p>
              <p id="invested-amount" class="text-2xl font-bold text-white my-0.5">R$ 12.450,00</p>
              <p class="text-[11px] text-emerald-400">Rendimento acumulado: +R$ 1.120,50</p>
            </div>
            <button onclick="applyInvestment()"
              class="px-3 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold shadow transition">
              Guardar Mais
            </button>
          </div>

          <div class="space-y-3">
            <h3 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Opções Seguras</h3>

            <div
              class="bg-slate-800/40 border border-slate-700/50 p-3.5 rounded-xl flex items-center justify-between hover:border-indigo-500/50 transition">
              <div class="flex items-center gap-3">
                <div class="p-2 bg-indigo-500/10 rounded-lg text-indigo-400">
                  <i data-lucide="shield-check" class="w-5 h-5"></i>
                </div>
                <div>
                  <p class="text-xs font-semibold text-white">CDB Aura 110% CDI</p>
                  <p class="text-[10px] text-slate-400">Liquidez diária • Garantia FGC</p>
                </div>
              </div>
              <span class="text-xs font-bold text-indigo-400">110% CDI</span>
            </div>

            <div
              class="bg-slate-800/40 border border-slate-700/50 p-3.5 rounded-xl flex items-center justify-between hover:border-indigo-500/50 transition">
              <div class="flex items-center gap-3">
                <div class="p-2 bg-emerald-500/10 rounded-lg text-emerald-400">
                  <i data-lucide="landmark" class="w-5 h-5"></i>
                </div>
                <div>
                  <p class="text-xs font-semibold text-white">Tesouro Selic 2029</p>
                  <p class="text-[10px] text-slate-400">Garantido pelo Governo • Seguro</p>
                </div>
              </div>
              <span class="text-xs font-bold text-emerald-400">Selic + 0.15%</span>
            </div>
          </div>
        </div>

      </main>

      <!-- BARRA DE NAVEGAÇÃO INFERIOR -->
      <nav
        class="absolute bottom-0 left-0 right-0 bg-slate-900/95 backdrop-blur-md border-t border-slate-800 px-3 py-2 flex justify-around items-center z-40">
        <button onclick="switchTab('dashboard')" id="nav-dashboard"
          class="flex flex-col items-center gap-1 text-indigo-400 transition">
          <i data-lucide="home" class="w-5 h-5"></i>
          <span class="text-[10px] font-medium">Início</span>
        </button>

        <button onclick="switchTab('pix')" id="nav-pix"
          class="flex flex-col items-center gap-1 text-slate-400 hover:text-indigo-400 transition">
          <i data-lucide="qr-code" class="w-5 h-5"></i>
          <span class="text-[10px] font-medium">Pix</span>
        </button>

        <button onclick="switchTab('boletos')" id="nav-boletos"
          class="flex flex-col items-center gap-1 text-slate-400 hover:text-indigo-400 transition">
          <i data-lucide="barcode" class="w-5 h-5"></i>
          <span class="text-[10px] font-medium">Pagar</span>
        </button>

        <button onclick="switchTab('cards')" id="nav-cards"
          class="flex flex-col items-center gap-1 text-slate-400 hover:text-indigo-400 transition">
          <i data-lucide="credit-card" class="w-5 h-5"></i>
          <span class="text-[10px] font-medium">Cartões</span>
        </button>

        <button onclick="switchTab('invest')" id="nav-invest"
          class="flex flex-col items-center gap-1 text-slate-400 hover:text-indigo-400 transition">
          <i data-lucide="line-chart" class="w-5 h-5"></i>
          <span class="text-[10px] font-medium">Investir</span>
        </button>
      </nav>
    </div>

    <!-- MODAL DO LEITOR DE CÂMERA (COM CANVAS VISUAL GARANTIDO) -->
    <div id="camera-modal"
      class="fixed inset-0 bg-slate-950/95 backdrop-blur-md z-50 hidden flex flex-col justify-between p-4">
      <div class="flex items-center justify-between text-white border-b border-slate-800 pb-3">
        <div class="flex items-center gap-2">
          <i data-lucide="camera" class="w-5 h-5 text-indigo-400"></i>
          <span id="camera-modal-title" class="text-xs font-bold">Leitor Digital de Código</span>
        </div>
        <button onclick="closeCameraModal()"
          class="p-1 rounded-full text-slate-400 hover:text-white hover:bg-slate-800 transition">
          <i data-lucide="x" class="w-6 h-6"></i>
        </button>
      </div>

      <!-- Área Visual da Câmera (Com Canvas Interativo que previne tela preta) -->
      <div
        class="relative w-full aspect-square max-w-xs mx-auto rounded-3xl overflow-hidden border-2 border-indigo-500/60 bg-slate-950 flex items-center justify-center my-auto shadow-2xl">

        <!-- Video elemento real (Oculto se der erro de permissão/iframe) -->
        <video id="camera-feed" autoplay playsinline
          class="w-full h-full object-cover transform -scale-x-100 hidden"></video>

        <!-- Canvas Ativo com Animação da Câmera Simulada -->
        <canvas id="camera-canvas" class="w-full h-full block"></canvas>

        <!-- Moldura de Enquadramento -->
        <div
          class="absolute inset-6 border-2 border-indigo-400/60 rounded-2xl pointer-events-none flex items-center justify-center">
          <!-- Laser de escaneamento -->
          <div
            class="w-full h-0.5 bg-gradient-to-r from-transparent via-indigo-400 to-transparent shadow-[0_0_15px_#818cf8] scan-laser absolute">
          </div>

          <div class="absolute top-2 left-2 w-3 h-3 border-t-2 border-l-2 border-indigo-400"></div>
          <div class="absolute top-2 right-2 w-3 h-3 border-t-2 border-r-2 border-indigo-400"></div>
          <div class="absolute bottom-2 left-2 w-3 h-3 border-b-2 border-l-2 border-indigo-400"></div>
          <div class="absolute bottom-2 right-2 w-3 h-3 border-b-2 border-r-2 border-indigo-400"></div>
        </div>

        <div
          class="absolute bottom-3 bg-slate-900/80 px-3 py-1 rounded-full border border-slate-700 text-[10px] text-indigo-300 font-semibold flex items-center gap-1.5">
          <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
          Câmera Pronta
        </div>
      </div>

      <!-- Botão de Ação -->
      <div class="space-y-2 max-w-xs mx-auto w-full pb-4">
        <button onclick="captureFrontScan()"
          class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl text-xs transition shadow-lg flex items-center justify-center gap-2">
          <i data-lucide="zap" class="w-4 h-4"></i> Escanear Código Instantaneamente
        </button>
        <p id="camera-modal-desc" class="text-[10px] text-slate-400 text-center">Posicione o código no centro do visor
        </p>
      </div>
    </div>

    <!-- MODAL DE CHAT DE SUPORTE -->
    <div id="chat-modal"
      class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 hidden flex items-end sm:items-center justify-center">
      <div
        class="w-full max-w-md h-[80vh] bg-slate-900 rounded-t-3xl sm:rounded-2xl border border-slate-800 flex flex-col overflow-hidden shadow-2xl">
        <div class="p-4 bg-slate-800 border-b border-slate-700/60 flex items-center justify-between">
          <div class="flex items-center gap-2.5">
            <div class="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white">
              <i data-lucide="bot" class="w-4 h-4"></i>
            </div>
            <div>
              <h3 class="text-xs font-bold text-white">Assistente Aura AI</h3>
              <span class="text-[10px] text-emerald-400 flex items-center gap-1">
                <span class="w-1.5 h-1.5 bg-emerald-400 rounded-full animate-pulse"></span> Online
              </span>
            </div>
          </div>
          <button onclick="closeChatModal()" class="text-slate-400 hover:text-white p-1">
            <i data-lucide="x" class="w-5 h-5"></i>
          </button>
        </div>

        <div id="chat-messages" class="flex-1 p-4 overflow-y-auto space-y-3 text-xs">
          <div class="bg-slate-800 p-3 rounded-2xl rounded-tl-none max-w-[85%] text-slate-200">
            Olá! Sou o assistente do Aura Bank. Como posso ajudar com as suas dúvidas?
          </div>
        </div>

        <div class="p-3 bg-slate-800 border-t border-slate-700/60 flex gap-2">
          <input type="text" id="chat-input" placeholder="Escreva a sua mensagem..."
            onkeydown="if(event.key === 'Enter') sendChatMessage()"
            class="flex-1 bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:border-indigo-500" />
          <button onclick="sendChatMessage()"
            class="p-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl transition">
            <i data-lucide="send" class="w-4 h-4"></i>
          </button>
        </div>
      </div>
    </div>

    <!-- NOTIFICAÇÃO TOAST -->
    <div id="toast-notification"
      class="absolute top-14 left-4 right-4 bg-slate-800/95 border border-indigo-500/40 text-white px-4 py-3 rounded-2xl shadow-xl flex items-center gap-3 transform -translate-y-24 opacity-0 transition-all duration-300 z-50 pointer-events-none">
      <div class="p-2 bg-indigo-600/20 text-indigo-400 rounded-xl">
        <i id="toast-icon" data-lucide="bell" class="w-4 h-4"></i>
      </div>
      <div>
        <p id="toast-title" class="text-xs font-bold text-white">Notificação</p>
        <p id="toast-msg" class="text-[11px] text-slate-300">Operação concluída com sucesso.</p>
      </div>
    </div>

  </div>

  <script>
    // ESTADO DA APLICAÇÃO
    const apiKey = "";
    let userBalance = 4850.30;
    let cardLimit = 5000.00;
    let isBalanceVisible = true;
    let isCardBlocked = false;
    let isCardDetailsShown = false;
    let investedTotal = 12450.00;
    let cameraStream = null;
    let scanMode = 'boleto';
    let authMethod = 'email';
    let isUnderAge = false;
    let canvasAnimationId = null;

    let transactions = [
      { id: 1, title: 'Supermercado Express', type: 'out', amount: 142.80, date: 'Hoje, 14:20', category: 'shopping-bag' },
      { id: 2, title: 'Transferência Pix Recebida', type: 'in', amount: 500.00, date: 'Ontem, 09:15', category: 'arrow-down-left' },
      { id: 3, title: 'Assinatura Música/Filmes', type: 'out', amount: 39.90, date: '21 de Julho', category: 'tv' },
      { id: 4, title: 'Rendimento CDB', type: 'in', amount: 12.40, date: '20 de Julho', category: 'trending-up' }
    ];

    window.onload = function () {
      lucide.createIcons();
      updateClock();
      setInterval(updateClock, 10000);
      renderTransactions();
    };

    function updateClock() {
      const now = new Date();
      const hours = String(now.getHours()).padStart(2, '0');
      const minutes = String(now.getMinutes()).padStart(2, '0');
      document.getElementById('clock').textContent = `${hours}:${minutes}`;
    }

    // ALTERNADOR DO MODO DE ENTRADA (CONTA EXISTENTE OU CRIAR CONTA)
    function switchLoginMode(mode) {
      const btnLogin = document.getElementById('tab-btn-login');
      const btnRegister = document.getElementById('tab-btn-register');
      const loginForm = document.getElementById('mode-login-form');
      const registerForm = document.getElementById('mode-register-form');
      const subtitle = document.getElementById('login-header-subtitle');

      if (mode === 'login') {
        btnLogin.className = "py-2 rounded-lg font-semibold bg-indigo-600 text-white shadow transition";
        btnRegister.className = "py-2 rounded-lg font-semibold text-slate-400 hover:text-white transition";
        loginForm.classList.remove('hidden');
        registerForm.classList.add('hidden');
        subtitle.textContent = "Acesse a sua conta existente";
      } else {
        btnRegister.className = "py-2 rounded-lg font-semibold bg-indigo-600 text-white shadow transition";
        btnLogin.className = "py-2 rounded-lg font-semibold text-slate-400 hover:text-white transition";
        registerForm.classList.remove('hidden');
        loginForm.classList.add('hidden');
        subtitle.textContent = "Cadastre-se para abrir a sua nova conta";
      }
    }

    // LOGIN PARA CONTA EXISTENTE
    function processExistingAccountLogin() {
      const email = document.getElementById('login-email').value;
      const doc = document.getElementById('login-doc').value;

      if (!email || !doc) {
        showToast("Campos Obrigatórios", "Informe o seu e-mail e CPF cadastrado.");
        return;
      }

      const userName = email.split('@')[0] || 'Usuário';

      document.getElementById('user-display-name').innerHTML = `${userName} <i data-lucide="badge-check" class="w-3.5 h-3.5 text-indigo-400"></i>`;
      document.getElementById('card-holder-name').textContent = userName.toUpperCase();

      document.getElementById('view-login').classList.add('hidden');
      document.getElementById('app-content').classList.remove('hidden');

      lucide.createIcons();
      showToast("Bem-vindo de volta!", `Acesso liberado na conta de ${userName}.`);
    }

    // REGRA DE IDADE DO CADASTRO (MAIOR/MENOR QUE 15 ANOS)
    function checkAgeRequirement() {
      const dobInput = document.getElementById('input-dob').value;
      if (!dobInput) return;

      const birthDate = new Date(dobInput);
      const today = new Date();
      let age = today.getFullYear() - birthDate.getFullYear();
      const monthDiff = today.getMonth() - birthDate.getMonth();

      if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birthDate.getDate())) {
        age--;
      }

      const parentBox = document.getElementById('parent-permission-box');
      if (age < 15) {
        isUnderAge = true;
        parentBox.classList.remove('hidden');
        showToast("Autorização dos Pais", "Menores de 15 anos necessitam de indicação de um responsável legal.");
      } else {
        isUnderAge = false;
        parentBox.classList.add('hidden');
      }
    }

    // ETAPAS DE REGISTRO
    function goToVerificationStep() {
      const email = document.getElementById('input-email').value;
      const doc = document.getElementById('input-doc').value;
      const dob = document.getElementById('input-dob').value;

      if (!email || !doc || !dob) {
        showToast("Atenção", "Preencha e-mail, documento e data de nascimento.");
        return;
      }

      if (isUnderAge) {
        const parentName = document.getElementById('parent-name').value;
        const parentCpf = document.getElementById('parent-cpf').value;
        if (!parentName || !parentCpf) {
          showToast("Dados do Responsável", "Insira o nome e CPF do responsável legal.");
          return;
        }
      }

      document.getElementById('login-step-1').classList.add('hidden');
      document.getElementById('login-step-2').classList.remove('hidden');
      showToast("Código Requerido", "Insira o código de 6 dígitos enviado.");
    }

    function selectAuthMethod(method) {
      authMethod = method;
      const btnEmail = document.getElementById('btn-method-email');
      const btnSms = document.getElementById('btn-method-sms');

      if (method === 'email') {
        btnEmail.className = "p-3 bg-indigo-600/20 border border-indigo-500 text-white rounded-xl text-center space-y-1";
        btnSms.className = "p-3 bg-slate-800 border border-slate-700 text-slate-400 rounded-xl text-center space-y-1";
        showToast("Verificação por E-mail", "Código enviado para a sua caixa de entrada.");
      } else {
        btnSms.className = "p-3 bg-indigo-600/20 border border-indigo-500 text-white rounded-xl text-center space-y-1";
        btnEmail.className = "p-3 bg-slate-800 border border-slate-700 text-slate-400 rounded-xl text-center space-y-1";
        showToast("Verificação por SMS", "Código enviado via telemóvel/SMS.");
      }
    }

    function goToFacialStep() {
      const code = document.getElementById('input-code').value;
      if (!code || code.length < 6) {
        showToast("Código Inválido", "Digite os 6 dígitos de validação.");
        return;
      }

      document.getElementById('login-step-2').classList.add('hidden');
      document.getElementById('login-step-3').classList.remove('hidden');
    }

    function completeLogin() {
      const email = document.getElementById('input-email').value;
      const userName = email.split('@')[0] || 'Usuário';

      document.getElementById('user-display-name').innerHTML = `${userName} <i data-lucide="badge-check" class="w-3.5 h-3.5 text-indigo-400"></i>`;
      document.getElementById('card-holder-name').textContent = userName.toUpperCase();

      if (isUnderAge) {
        document.getElementById('account-type-tag').textContent = "Conta Jovem Aura";
      }

      document.getElementById('view-login').classList.add('hidden');
      document.getElementById('app-content').classList.remove('hidden');

      lucide.createIcons();
      showToast("Conta Criada!", "Seja bem-vindo ao Aura Bank.");
    }

    function switchTab(tabName) {
      const views = ['dashboard', 'pix', 'boletos', 'cards', 'invest'];
      views.forEach(v => {
        const viewEl = document.getElementById(`view-${v}`);
        const navBtn = document.getElementById(`nav-${v}`);

        if (v === tabName) {
          viewEl.classList.remove('hidden');
          navBtn.classList.remove('text-slate-400');
          navBtn.classList.add('text-indigo-400');
        } else {
          viewEl.classList.add('hidden');
          navBtn.classList.remove('text-indigo-400');
          navBtn.classList.add('text-slate-400');
        }
      });
    }

    function formatCurrency(value) {
      return value.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    }

    function toggleBalanceVisibility() {
      isBalanceVisible = !isBalanceVisible;
      const display = document.getElementById('balance-display');
      const eyeIcon = document.getElementById('eye-icon');

      if (isBalanceVisible) {
        display.textContent = formatCurrency(userBalance);
        eyeIcon.setAttribute('data-lucide', 'eye');
      } else {
        display.textContent = 'R$ ••••••';
        eyeIcon.setAttribute('data-lucide', 'eye-off');
      }
      lucide.createIcons();
    }

    function renderTransactions() {
      const container = document.getElementById('transactions-list');
      container.innerHTML = '';

      if (transactions.length === 0) {
        container.innerHTML = '<p class="text-xs text-slate-500 text-center py-4">Sem movimentações no momento.</p>';
        return;
      }

      transactions.forEach(tx => {
        const item = document.createElement('div');
        item.className = 'bg-slate-800/40 border border-slate-700/40 p-3 rounded-xl flex items-center justify-between hover:bg-slate-800/70 transition';

        const isIncome = tx.type === 'in';
        const amountClass = isIncome ? 'text-emerald-400 font-semibold' : 'text-slate-200 font-semibold';
        const prefix = isIncome ? '+' : '-';

        item.innerHTML = `
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 rounded-full ${isIncome ? 'bg-emerald-500/10 text-emerald-400' : 'bg-slate-700 text-slate-300'} flex items-center justify-center">
              <i data-lucide="${tx.category || 'circle'}" class="w-4 h-4"></i>
            </div>
            <div>
              <p class="text-xs font-medium text-white">${tx.title}</p>
              <p class="text-[10px] text-slate-400">${tx.date}</p>
            </div>
          </div>
          <div class="text-right">
            <p class="text-xs ${amountClass}">${prefix}${formatCurrency(tx.amount)}</p>
          </div>
        `;
        container.appendChild(item);
      });

      lucide.createIcons();
    }

    function clearTransactions() {
      transactions = [];
      renderTransactions();
      showToast("Histórico Limpo", "Extrato zerado com sucesso.");
    }

    // PIX
    function processPixTransfer() {
      const keyInput = document.getElementById('pix-key').value;
      const amountInput = parseFloat(document.getElementById('pix-amount').value);

      if (!keyInput) {
        showToast("Erro", "Por favor, indique uma chave Pix válida.");
        return;
      }
      if (isNaN(amountInput) || amountInput <= 0) {
        showToast("Erro", "Insira um valor superior a R$ 0,00.");
        return;
      }
      if (amountInput > userBalance) {
        showToast("Saldo Insuficiente", "O valor excede o seu saldo em conta.");
        return;
      }

      userBalance -= amountInput;
      document.getElementById('balance-display').textContent = formatCurrency(userBalance);

      transactions.unshift({
        id: Date.now(),
        title: `Pix p/ ${keyInput.slice(0, 15)}...`,
        type: 'out',
        amount: amountInput,
        date: 'Agora',
        category: 'send'
      });

      renderTransactions();

      document.getElementById('pix-key').value = '';
      document.getElementById('pix-amount').value = '';
      document.getElementById('pix-desc').value = '';

      showToast("Pix Concluído!", `Transferência de ${formatCurrency(amountInput)} efetuada.`);
      switchTab('dashboard');
    }

    function fillPixContact(name, key) {
      document.getElementById('pix-key').value = key;
      document.getElementById('pix-amount').focus();
    }

    // SISTEMA DE CÂMERA DINÂMICA COM CANVAS INTERATIVO
    async function openCameraModal(mode = 'boleto') {
      scanMode = mode;
      const modal = document.getElementById('camera-modal');
      const video = document.getElementById('camera-feed');
      const titleEl = document.getElementById('camera-modal-title');
      const descEl = document.getElementById('camera-modal-desc');

      if (mode === 'pix') {
        titleEl.textContent = "Leitor QR Code Pix";
        descEl.textContent = "Aponta o QR Code para a zona de leitura";
      } else {
        titleEl.textContent = "Leitor de Boletos";
        descEl.textContent = "Enquadre o código de barras impresso";
      }

      modal.classList.remove('hidden');
      lucide.createIcons();

      startCanvasCameraSimulation();

      try {
        cameraStream = await navigator.mediaDevices.getUserMedia({
          video: { facingMode: 'user' },
          audio: false
        });
        video.srcObject = cameraStream;
        video.classList.remove('hidden');
      } catch (err) {
        video.classList.add('hidden');
      }
    }

    function startCanvasCameraSimulation() {
      const canvas = document.getElementById('camera-canvas');
      const ctx = canvas.getContext('2d');
      canvas.width = 300;
      canvas.height = 300;

      let frameCount = 0;

      function renderFrame() {
        frameCount++;
        ctx.fillStyle = '#090d16';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        ctx.strokeStyle = 'rgba(99, 102, 241, 0.15)';
        ctx.lineWidth = 1;
        for (let x = 0; x < canvas.width; x += 20) {
          ctx.beginPath();
          ctx.moveTo(x, 0);
          ctx.lineTo(x, canvas.height);
          ctx.stroke();
        }
        for (let y = 0; y < canvas.height; y += 20) {
          ctx.beginPath();
          ctx.moveTo(0, y);
          ctx.lineTo(canvas.width, y);
          ctx.stroke();
        }

        const centerX = canvas.width / 2;
        const centerY = canvas.height / 2;

        if (scanMode === 'pix') {
          ctx.fillStyle = 'rgba(129, 140, 248, 0.8)';
          ctx.fillRect(centerX - 40, centerY - 40, 80, 80);
          ctx.fillStyle = '#090d16';
          ctx.fillRect(centerX - 30, centerY - 30, 60, 60);
          ctx.fillStyle = '#818cf8';
          ctx.fillRect(centerX - 20, centerY - 20, 40, 40);
        } else {
          ctx.fillStyle = 'rgba(129, 140, 248, 0.8)';
          for (let i = -50; i <= 50; i += 10) {
            ctx.fillRect(centerX + i, centerY - 30, 4, 60);
          }
        }

        canvasAnimationId = requestAnimationFrame(renderFrame);
      }

      renderFrame();
    }

    function closeCameraModal() {
      const modal = document.getElementById('camera-modal');
      const video = document.getElementById('camera-feed');

      if (cameraStream) {
        cameraStream.getTracks().forEach(track => track.stop());
        cameraStream = null;
      }
      if (canvasAnimationId) {
        cancelAnimationFrame(canvasAnimationId);
      }
      video.srcObject = null;
      modal.classList.add('hidden');
    }

    function captureFrontScan() {
      closeCameraModal();

      if (scanMode === 'pix') {
        document.getElementById('pix-key').value = "pagamento-loja@aurabank.com";
        document.getElementById('pix-amount').value = "85.00";
        document.getElementById('pix-desc').value = "Compra Lojas Aura";
        showToast("QR Code Pix Lido!", "Chave e valor de R$ 85,00 lidos com sucesso.");
      } else {
        simulateScanBarcode();
        showToast("Boleto Detectado!", "Linha digitável importada.");
      }
    }

    // BOLETOS
    function simulateScanBarcode() {
      const simulatedBarcode = "00090.00005 61234.567809 12345.678903 8 9800000018490";
      document.getElementById('barcode-input').value = simulatedBarcode;
      document.getElementById('bill-details').classList.remove('hidden');
      showToast("Código Válido", "Detalhes da fatura carregados.");
    }

    function processBillPayment() {
      const code = document.getElementById('barcode-input').value;
      if (!code) {
        showToast("Erro", "Insira o código de barras.");
        return;
      }

      const billValue = 184.90;
      if (billValue > userBalance) {
        showToast("Erro", "Saldo insuficiente.");
        return;
      }

      userBalance -= billValue;
      document.getElementById('balance-display').textContent = formatCurrency(userBalance);

      transactions.unshift({
        id: Date.now(),
        title: 'Pagamento de Fatura',
        type: 'out',
        amount: billValue,
        date: 'Agora',
        category: 'file-text'
      });

      renderTransactions();
      document.getElementById('barcode-input').value = '';
      document.getElementById('bill-details').classList.add('hidden');

      showToast("Pagamento Efetuado!", `Pagamento de ${formatCurrency(billValue)} realizado.`);
      switchTab('dashboard');
    }

    // CARTÃO DE CRÉDITO
    function toggleCardBlock() {
      isCardBlocked = !isCardBlocked;
      const card = document.getElementById('virtual-card');
      const badge = document.getElementById('card-status-badge');

      if (isCardBlocked) {
        card.classList.add('opacity-40', 'grayscale');
        badge.textContent = 'BLOQUEADO';
        badge.className = 'text-[10px] bg-rose-500/20 text-rose-400 px-2 py-0.5 rounded-full font-semibold';
        showToast("Cartão Bloqueado", "Utilização do cartão suspensa.");
      } else {
        card.classList.remove('opacity-40', 'grayscale');
        badge.textContent = 'ATIVO';
        badge.className = 'text-[10px] bg-emerald-500/20 text-emerald-400 px-2 py-0.5 rounded-full font-semibold';
        showToast("Cartão Desbloqueado", "Cartão ativo para pagamentos.");
      }
    }

    function toggleCardDetails() {
      isCardDetailsShown = !isCardDetailsShown;
      const numberEl = document.getElementById('card-number');
      const cvvEl = document.getElementById('card-cvv');
      const btnText = document.getElementById('card-details-btn-text');

      if (isCardDetailsShown) {
        numberEl.textContent = '4532 8812 9011 8842';
        cvvEl.textContent = '742';
        btnText.textContent = 'Ocultar';
      } else {
        numberEl.textContent = '•••• •••• •••• 8842';
        cvvEl.textContent = '•••';
        btnText.textContent = 'Mostrar';
      }
    }

    function updateCardLimit(val) {
      cardLimit = parseFloat(val);
      document.getElementById('limit-val').textContent = formatCurrency(cardLimit);
    }

    // INVESTIMENTOS
    function applyInvestment() {
      if (userBalance < 100) {
        showToast("Atenção", "O valor mínimo de aplicação é de R$ 100,00.");
        return;
      }

      userBalance -= 100;
      investedTotal += 100;

      document.getElementById('balance-display').textContent = formatCurrency(userBalance);
      document.getElementById('invested-amount').textContent = formatCurrency(investedTotal);

      transactions.unshift({
        id: Date.now(),
        title: 'Aplicação em Renda Fixa',
        type: 'out',
        amount: 100.00,
        date: 'Agora',
        category: 'line-chart'
      });

      renderTransactions();
      showToast("Aplicação Realizada", "R$ 100,00 adicionados com sucesso.");
    }

    // CHAT INTELIGENTE
    function openChatModal() {
      document.getElementById('chat-modal').classList.remove('hidden');
    }

    function closeChatModal() {
      document.getElementById('chat-modal').classList.add('hidden');
    }

    async function sendChatMessage() {
      const input = document.getElementById('chat-input');
      const userText = input.value.trim();
      if (!userText) return;

      const chatContainer = document.getElementById('chat-messages');

      const userBubble = document.createElement('div');
      userBubble.className = 'bg-indigo-600 p-3 rounded-2xl rounded-tr-none max-w-[85%] text-white ml-auto';
      userBubble.textContent = userText;
      chatContainer.appendChild(userBubble);

      input.value = '';
      chatContainer.scrollTop = chatContainer.scrollHeight;

      const loadingBubble = document.createElement('div');
      loadingBubble.className = 'bg-slate-800 p-3 rounded-2xl rounded-tl-none max-w-[85%] text-slate-400 flex items-center gap-2';
      loadingBubble.id = 'chat-loading';
      loadingBubble.innerHTML = '<i data-lucide="loader-2" class="w-4 h-4 animate-spin"></i> A responder...';
      chatContainer.appendChild(loadingBubble);
      lucide.createIcons();
      chatContainer.scrollTop = chatContainer.scrollHeight;

      const botResponse = await getGeminiChatResponse(userText);

      document.getElementById('chat-loading')?.remove();

      const botBubble = document.createElement('div');
      botBubble.className = 'bg-slate-800 p-3 rounded-2xl rounded-tl-none max-w-[85%] text-slate-200';
      botBubble.textContent = botResponse;
      chatContainer.appendChild(botBubble);
      chatContainer.scrollTop = chatContainer.scrollHeight;
    }

    async function getGeminiChatResponse(prompt) {
      const systemPrompt = "Você é o assistente virtual amigável do Aura Bank. Escreva em português de forma clara, segura, educada e sucinta.";
      const delays = [1000, 2000, 4000];

      for (let attempt = 0; attempt <= delays.length; attempt++) {
        try {
          const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              contents: [{ parts: [{ text: prompt }] }],
              systemInstruction: { parts: [{ text: systemPrompt }] }
            })
          });

          if (response.ok) {
            const data = await response.json();
            const reply = data.candidates?.[0]?.content?.parts?.[0]?.text;
            if (reply) return reply;
          }
        } catch (e) { }

        if (attempt < delays.length) {
          await new Promise(res => setTimeout(res, delays[attempt]));
        }
      }

      return "Estou à disposição para responder às suas dúvidas sobre saldo, cartões ou Pix!";
    }

    // TOAST NOTIFICATIONS
    function showToast(title, msg) {
      const toast = document.getElementById('toast-notification');
      document.getElementById('toast-title').textContent = title;
      document.getElementById('toast-msg').textContent = msg;

      toast.classList.remove('-translate-y-24', 'opacity-0');
      toast.classList.add('translate-y-0', 'opacity-100');

      setTimeout(() => {
        toast.classList.remove('translate-y-0', 'opacity-100');
        toast.classList.add('-translate-y-24', 'opacity-0');
      }, 3500);
    }
  </script>
</body>

</html>
