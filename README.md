<!doctype html>
<html lang="pt">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>KFC Kitchen Count Manager</title>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.31/jspdf.plugin.autotable.min.js"></script>
  <style>
        body {
            box-sizing: border-box;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html, body {
            height: 100%;
            font-family: 'Arial', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #e30613 0%, #8b0000 100%);
            color: #333;
        }

        .app-container {
            height: 100%;
            width: 100%;
            overflow: auto;
            background: #f5f5f5;
        }

        .header {
            background: #e30613;
            color: white;
            padding: 1.5rem 2rem;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }

        .header h1 {
            font-size: 1.8rem;
            display: flex;
            align-items: center;
            gap: 0.8rem;
        }

        .header-subtitle {
            font-size: 0.9rem;
            opacity: 0.9;
            margin-top: 0.3rem;
        }

        .nav-tabs {
            background: white;
            padding: 0 2rem;
            display: flex;
            gap: 0.5rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .nav-tab {
            padding: 1rem 1.5rem;
            background: none;
            border: none;
            border-bottom: 3px solid transparent;
            cursor: pointer;
            font-size: 1rem;
            color: #666;
            transition: all 0.3s;
        }

        .nav-tab:hover {
            color: #e30613;
            background: #f9f9f9;
        }

        .nav-tab.active {
            color: #e30613;
            border-bottom-color: #e30613;
            font-weight: bold;
        }

        .content {
            padding: 2rem;
            max-width: 1400px;
            margin: 0 auto;
        }

        .login-container {
            max-width: 400px;
            margin: 4rem auto;
            background: white;
            padding: 2.5rem;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
        }

        .login-header {
            text-align: center;
            margin-bottom: 2rem;
        }

        .login-header h2 {
            color: #e30613;
            font-size: 1.8rem;
            margin-bottom: 0.5rem;
        }

        .form-group {
            margin-bottom: 1.5rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            color: #333;
            font-weight: 500;
            font-size: 0.95rem;
        }

        .form-group input,
        .form-group select {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .form-group input:focus,
        .form-group select:focus {
            outline: none;
            border-color: #e30613;
        }

        .btn {
            padding: 0.8rem 1.5rem;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            transition: all 0.3s;
        }

        .btn-primary {
            background: #e30613;
            color: white;
            width: 100%;
        }

        .btn-primary:hover {
            background: #b8050f;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(227, 6, 19, 0.3);
        }

        .btn-primary:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
        }

        .btn-secondary {
            background: #666;
            color: white;
        }

        .btn-secondary:hover {
            background: #555;
        }

        .shift-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .shift-card {
            background: white;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            cursor: pointer;
            transition: all 0.3s;
            border: 3px solid transparent;
        }

        .shift-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 20px rgba(227, 6, 19, 0.2);
            border-color: #e30613;
        }

        .shift-card.selected {
            border-color: #e30613;
            background: #fff5f5;
        }

        .shift-card h3 {
            color: #e30613;
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
        }

        .shift-card p {
            color: #666;
            font-size: 0.95rem;
        }

        .products-table {
            background: white;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            overflow-x: auto;
            margin-bottom: 2rem;
        }

        .products-table table {
            width: 100%;
            border-collapse: collapse;
            min-width: 800px;
        }

        .products-table th {
            background: #e30613;
            color: white;
            padding: 1rem;
            text-align: left;
            font-weight: 600;
            white-space: nowrap;
        }

        .products-table td {
            padding: 1rem;
            border-bottom: 1px solid #eee;
        }

        .products-table tr:last-child td {
            border-bottom: none;
        }

        .products-table tbody tr:hover {
            background: #f9f9f9;
        }

        .input-group {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .input-group input {
            width: 80px;
            padding: 0.5rem;
            border: 2px solid #ddd;
            border-radius: 6px;
            text-align: center;
            font-size: 1rem;
        }

        .input-group input:focus {
            outline: none;
            border-color: #e30613;
        }

        .adjust-btn {
            width: 35px;
            height: 35px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1.2rem;
            font-weight: bold;
            transition: all 0.2s;
        }

        .adjust-btn.plus {
            background: #28a745;
            color: white;
        }

        .adjust-btn.plus:hover {
            background: #218838;
        }

        .adjust-btn.minus {
            background: #dc3545;
            color: white;
        }

        .adjust-btn.minus:hover {
            background: #c82333;
        }

        .total-display {
            font-weight: bold;
            color: #e30613;
            font-size: 1.1rem;
        }

        .summary-card {
            background: white;
            padding: 1.5rem;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            text-align: center;
        }

        .summary-card h3 {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 0.5rem;
            text-transform: uppercase;
        }

        .summary-card .value {
            color: #e30613;
            font-size: 2.5rem;
            font-weight: bold;
        }

        .action-buttons {
            display: flex;
            gap: 1rem;
            margin-top: 2rem;
            flex-wrap: wrap;
        }

        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .report-filters {
            background: white;
            padding: 1.5rem;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 2rem;
        }

        .filter-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1rem;
            align-items: end;
        }

        .report-table {
            background: white;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            overflow-x: auto;
            margin-bottom: 1.5rem;
        }

        .report-table table {
            width: 100%;
            border-collapse: collapse;
            min-width: 800px;
        }

        .report-table th {
            background: #e30613;
            color: white;
            padding: 1rem;
            text-align: left;
            font-weight: 600;
            white-space: nowrap;
        }

        .report-table td {
            padding: 0.8rem 1rem;
            border-bottom: 1px solid #eee;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 1rem;
            margin-left: auto;
        }

        .user-name {
            font-size: 0.9rem;
        }

        .logout-btn {
            background: rgba(255,255,255,0.2);
            color: white;
            padding: 0.5rem 1rem;
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
        }

        .logout-btn:hover {
            background: rgba(255,255,255,0.3);
        }

        .hidden {
            display: none;
        }

        .toast {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            background: #28a745;
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
            z-index: 1000;
            animation: slideIn 0.3s ease;
        }

        .toast.error {
            background: #dc3545;
        }

        @keyframes slideIn {
            from {
                transform: translateX(400px);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 0.8s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .header-top {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .empty-state {
            text-align: center;
            padding: 3rem;
            color: #999;
        }

        .empty-state h3 {
            font-size: 1.3rem;
            margin-bottom: 0.5rem;
        }

        .totals-row {
            background: #f5f5f5 !important;
            font-weight: bold;
        }

        .totals-row td {
            color: #e30613;
            font-size: 1.05rem;
        }

        .shift-badge {
            display: inline-block;
            padding: 0.3rem 0.8rem;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .shift-badge.tarde {
            background: #ffc107;
            color: #000;
        }

        .shift-badge.noite {
            background: #17a2b8;
            color: white;
        }

        .report-section {
            margin-bottom: 2rem;
        }

        .report-section h3 {
            color: #e30613;
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid #e30613;
        }

        .info-box {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 1rem;
            margin-bottom: 1.5rem;
            border-radius: 6px;
        }

        .info-box strong {
            color: #856404;
        }

        @media (max-width: 768px) {
            .header-top {
                flex-direction: column;
                gap: 1rem;
                align-items: flex-start;
            }

            .user-info {
                margin-left: 0;
                width: 100%;
                justify-content: space-between;
            }

            .content {
                padding: 1rem;
            }

            .action-buttons {
                flex-direction: column;
            }

            .action-buttons .btn {
                width: 100%;
            }
        }
    </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="app-container" id="appContainer"><!-- Login Screen -->
   <div id="loginScreen">
    <div class="login-container">
     <div class="login-header">
      <h2>🍗 <span id="systemTitle">KFC Kitchen Count Manager</span></h2>
      <p style="color: #666; margin-top: 0.5rem;">Sistema de Contagem de Produção</p>
     </div>
     <form id="loginForm">
      <div class="form-group"><label for="username">Nome de Utilizador</label> <input type="text" id="username" required placeholder="Digite seu nome">
      </div><button type="submit" class="btn btn-primary" id="loginBtn"> Entrar no Sistema </button>
     </form>
    </div>
   </div><!-- Main App -->
   <div id="mainApp" class="hidden">
    <header class="header">
     <div class="header-top">
      <div>
       <h1>🍗 <span id="mainSystemTitle">KFC Kitchen Count Manager</span></h1>
       <div class="header-subtitle">
        <span id="companyName">KFC Portugal</span>
       </div>
      </div>
      <div class="user-info"><span class="user-name">👤 <span id="currentUser"></span></span> <button class="logout-btn" id="logoutBtn">Sair</button>
      </div>
     </div>
    </header>
    <nav class="nav-tabs"><button class="nav-tab active" data-tab="register">📝 Registo</button> <button class="nav-tab" data-tab="reports">📊 Relatórios</button> <button class="nav-tab" data-tab="dashboard">📈 Dashboard</button>
    </nav>
    <main class="content"><!-- Register Tab -->
     <div id="registerTab" class="tab-content">
      <div class="shift-selector">
       <div class="shift-card" data-shift="Tarde">
        <h3>☀️ Turno da Tarde</h3>
        <p>Registo de produção do turno da tarde</p>
       </div>
       <div class="shift-card" data-shift="Noite">
        <h3>🌙 Turno da Noite</h3>
        <p>Registo de produção do turno da noite</p>
       </div>
      </div>
      <div id="registrationForm" class="hidden">
       <h2 style="margin-bottom: 1.5rem; color: #333;">Registo - Turno: <span id="selectedShift" style="color: #e30613;"></span></h2>
       <div class="info-box"><strong>📝 Atenção:</strong> Para <strong>Pedaços</strong>, insira o total de peças manualmente. Para Asas e Filetes Value, insira o número de <strong>caixas</strong>. O sistema calcula automaticamente.
       </div>
       <div class="products-table">
        <table>
         <thead>
          <tr>
           <th>Produto</th>
           <th>Configuração</th>
           <th>Sacos/Caixas</th>
           <th>Ajustes (unidades)</th>
           <th>Total Peças</th>
          </tr>
         </thead>
         <tbody id="productsTableBody"></tbody>
        </table>
       </div>
       <div class="dashboard-grid">
        <div class="summary-card">
         <h3>Total do Turno</h3>
         <div class="value" id="shiftTotal">
          0
         </div>
        </div>
       </div>
       <div class="action-buttons"><button class="btn btn-primary" id="saveBtn" style="flex: 1;"> 💾 Guardar Registo </button> <button class="btn btn-secondary" id="cancelBtn"> ❌ Cancelar </button>
       </div>
      </div>
     </div><!-- Reports Tab -->
     <div id="reportsTab" class="tab-content hidden">
      <div class="report-filters">
       <h2 style="margin-bottom: 1rem; color: #333;">Gerar Relatórios</h2>
       <div class="filter-row">
        <div class="form-group"><label for="reportType">Tipo de Relatório</label> <select id="reportType"> <option value="daily">Diário</option> <option value="shift">Por Turno</option> <option value="weekly">Semanal</option> <option value="monthly">Mensal</option> </select>
        </div>
        <div class="form-group" id="shiftFilterGroup"><label for="shiftFilter">Filtrar Turno</label> <select id="shiftFilter"> <option value="all">Todos</option> <option value="Tarde">Tarde</option> <option value="Noite">Noite</option> </select>
        </div>
        <div class="form-group"><label for="startDate">Data Início</label> <input type="date" id="startDate">
        </div>
        <div class="form-group"><label for="endDate">Data Fim</label> <input type="date" id="endDate">
        </div>
        <div class="form-group"><button class="btn btn-primary" id="generateReportBtn"> 📊 Gerar Relatório </button>
        </div>
       </div>
      </div>
      <div id="reportResults"></div>
      <div class="action-buttons" id="reportActions" style="display: none;"><button class="btn btn-primary" id="exportPdfBtn"> 📄 Exportar para PDF </button>
      </div>
     </div><!-- Dashboard Tab -->
     <div id="dashboardTab" class="tab-content hidden">
      <h2 style="margin-bottom: 1.5rem; color: #333;">Dashboard - Resumo Geral</h2>
      <div class="dashboard-grid" id="dashboardStats"></div>
      <div class="report-table" id="recentEntries"></div>
     </div>
    </main>
   </div>
  </div>
  <script>
        const PRODUCTS = [
            { id: 'filetes', name: 'Filetes', unitsPerBag: 10, inputType: 'bags', inputLabel: 'Sacos' },
            { id: 'minis', name: 'Minis/Tenders', unitsPerBag: 20, inputType: 'bags', inputLabel: 'Sacos' },
            { id: 'zingers', name: 'Zingers', unitsPerBag: 10, inputType: 'bags', inputLabel: 'Sacos' },
            { id: 'pedacos', name: 'Pedaços', inputType: 'manual', inputLabel: 'Total Peças' },
            { id: 'asas', name: 'Asas', unitsPerBag: 60, bagsPerBox: 3, inputType: 'boxes', inputLabel: 'Caixas', configText: '1 caixa = 3 sacos de 60 unid' },
            { id: 'filetes_value', name: 'Filetes Value', unitsPerBag: 24, bagsPerBox: 3, inputType: 'boxes', inputLabel: 'Caixas', configText: '1 caixa = 3 sacos de 24 unid' }
        ];

        let currentUser = '';
        let selectedShift = '';
        let currentData = {};
        let allRecords = [];
        let currentReportData = null;

        const defaultConfig = {
            system_title: 'KFC Kitchen Count Manager',
            company_name: 'KFC Portugal'
        };

        async function onConfigChange(config) {
            const systemTitle = config.system_title || defaultConfig.system_title;
            const companyName = config.company_name || defaultConfig.company_name;
            
            document.getElementById('systemTitle').textContent = systemTitle;
            document.getElementById('mainSystemTitle').textContent = systemTitle;
            document.getElementById('companyName').textContent = companyName;
        }

        const dataHandler = {
            onDataChanged(data) {
                allRecords = data.sort((a, b) => new Date(b.created_at) - new Date(a.created_at));
                
                if (!document.getElementById('dashboardTab').classList.contains('hidden')) {
                    renderDashboard();
                }
            }
        };

        async function initializeApp() {
            if (window.elementSdk) {
                window.elementSdk.init({
                    defaultConfig,
                    onConfigChange,
                    mapToCapabilities: (config) => ({
                        recolorables: [],
                        borderables: [],
                        fontEditable: undefined,
                        fontSizeable: undefined
                    }),
                    mapToEditPanelValues: (config) => new Map([
                        ['system_title', config.system_title || defaultConfig.system_title],
                        ['company_name', config.company_name || defaultConfig.company_name]
                    ])
                });
            }

            const result = await window.dataSdk.init(dataHandler);
            
            if (!result.isOk) {
                showToast('Erro ao inicializar sistema de dados', true);
            }
        }

        document.getElementById('loginForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const username = document.getElementById('username').value.trim();
            
            if (username) {
                currentUser = username;
                document.getElementById('currentUser').textContent = username;
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('mainApp').classList.remove('hidden');
                showToast(`Bem-vindo, ${username}!`);
            }
        });

        document.getElementById('logoutBtn').addEventListener('click', () => {
            currentUser = '';
            selectedShift = '';
            document.getElementById('username').value = '';
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('mainApp').classList.add('hidden');
            document.getElementById('registrationForm').classList.add('hidden');
        });

        document.querySelectorAll('.shift-card').forEach(card => {
            card.addEventListener('click', () => {
                selectedShift = card.dataset.shift;
                
                document.querySelectorAll('.shift-card').forEach(c => c.classList.remove('selected'));
                card.classList.add('selected');
                
                document.getElementById('selectedShift').textContent = selectedShift;
                document.getElementById('registrationForm').classList.remove('hidden');
                
                initializeProductsTable();
            });
        });

        function initializeProductsTable() {
            const tbody = document.getElementById('productsTableBody');
            tbody.innerHTML = '';
            
            currentData = {};
            
            PRODUCTS.forEach(product => {
                currentData[product.id] = {
                    input: 0,
                    adjust: 0,
                    total: 0,
                    numericTotal: 0
                };
                
                const row = document.createElement('tr');
                
                if (product.inputType === 'manual') {
                    row.innerHTML = `
                        <td><strong>${product.name}</strong></td>
                        <td style="font-size: 0.9rem; color: #666;">Inserir total de peças manualmente (texto livre)</td>
                        <td colspan="2">
                            <div class="input-group">
                                <input type="text" 
                                       id="${product.id}_manual" 
                                       value="" 
                                       data-product="${product.id}"
                                       placeholder="Ex: 8sacos + 1/2, 150, 20/30, etc."
                                       style="width: 250px;">
                            </div>
                        </td>
                        <td class="total-display" id="${product.id}_total">-</td>
                    `;
                } else {
                    const configText = product.configText || `1 saco = ${product.unitsPerBag} unidades`;
                    const fieldName = product.inputType === 'boxes' ? product.id + '_boxes' : product.id + '_bags';
                    
                    row.innerHTML = `
                        <td><strong>${product.name}</strong></td>
                        <td style="font-size: 0.9rem; color: #666;">${configText}</td>
                        <td>
                            <div class="input-group">
                                <input type="number" 
                                       id="${fieldName}" 
                                       min="0" 
                                       value="0" 
                                       data-product="${product.id}"
                                       placeholder="${product.inputLabel}">
                            </div>
                        </td>
                        <td>
                            <div class="input-group">
                                <button class="adjust-btn minus" data-product="${product.id}" data-action="minus">−</button>
                                <input type="number" 
                                       id="${product.id}_adjust" 
                                       value="0" 
                                       data-product="${product.id}"
                                       readonly>
                                <button class="adjust-btn plus" data-product="${product.id}" data-action="plus">+</button>
                            </div>
                        </td>
                        <td class="total-display" id="${product.id}_total">0</td>
                    `;
                }
                
                tbody.appendChild(row);
            });
            
            document.querySelectorAll('input[type="number"]:not([readonly])').forEach(input => {
                input.addEventListener('input', handleInputChange);
            });
            
            document.querySelectorAll('input[type="text"]').forEach(input => {
                input.addEventListener('input', handleInputChange);
            });
            
            document.querySelectorAll('.adjust-btn').forEach(btn => {
                btn.addEventListener('click', handleAdjustClick);
            });
        }

        function handleInputChange(e) {
            const productId = e.target.dataset.product;
            const product = PRODUCTS.find(p => p.id === productId);
            
            let total;
            
            if (product.inputType === 'manual') {
                const textValue = e.target.value.trim();
                
                // Tentar extrair número do texto para cálculos
                const numericValue = extractNumericValue(textValue);
                
                currentData[productId] = { 
                    input: textValue, 
                    adjust: 0, 
                    total: textValue,
                    numericTotal: numericValue 
                };
                
                // Mostrar o texto original no display
                document.getElementById(`${productId}_total`).textContent = textValue || '-';
            } else {
                const fieldName = product.inputType === 'boxes' ? productId + '_boxes' : productId + '_bags';
                const inputValue = parseInt(document.getElementById(fieldName).value) || 0;
                const adjust = parseInt(document.getElementById(`${productId}_adjust`).value) || 0;
                
                if (product.inputType === 'boxes') {
                    total = (inputValue * product.bagsPerBox * product.unitsPerBag) + adjust;
                } else {
                    total = (inputValue * product.unitsPerBag) + adjust;
                }
                
                currentData[productId] = { input: inputValue, adjust, total };
                document.getElementById(`${productId}_total`).textContent = total;
            }
            
            updateShiftTotal();
        }

        // Função para extrair valor numérico de texto livre
        function extractNumericValue(text) {
            if (!text || text === '-') return 0;
            
            // Se for um número simples, retornar diretamente
            const simpleNumber = parseFloat(text);
            if (!isNaN(simpleNumber)) return simpleNumber;
            
            // Tentar extrair e somar todos os números encontrados
            // Ex: "8sacos + 12" -> extrai [8, 12] -> soma = 20
            const numbers = text.match(/\d+/g);
            if (numbers && numbers.length > 0) {
                return numbers.reduce((sum, num) => sum + parseInt(num), 0);
            }
            
            return 0;
        }

        function handleAdjustClick(e) {
            const productId = e.target.dataset.product;
            const action = e.target.dataset.action;
            
            const adjustInput = document.getElementById(`${productId}_adjust`);
            let currentAdjust = parseInt(adjustInput.value) || 0;
            
            if (action === 'plus') {
                currentAdjust++;
            } else if (action === 'minus') {
                currentAdjust--;
            }
            
            adjustInput.value = currentAdjust;
            
            handleInputChange({ target: adjustInput });
        }

        function updateShiftTotal() {
            let total = 0;
            
            Object.keys(currentData).forEach(productId => {
                const product = PRODUCTS.find(p => p.id === productId);
                const item = currentData[productId];
                
                if (product.inputType === 'manual') {
                    // Usar valor numérico extraído para somar
                    total += item.numericTotal || 0;
                } else {
                    total += item.total;
                }
            });
            
            document.getElementById('shiftTotal').textContent = total;
        }

        document.getElementById('saveBtn').addEventListener('click', async () => {
            if (allRecords.length >= 999) {
                showToast('Limite máximo de 999 registos atingido. Por favor, elimine alguns registos primeiro.', true);
                return;
            }

            const btn = document.getElementById('saveBtn');
            btn.disabled = true;
            btn.innerHTML = '<span class="loading"></span> A guardar...';
            
            const record = {
                id: `${Date.now()}_${selectedShift}`,
                date: new Date().toLocaleDateString('pt-PT'),
                shift: selectedShift,
                username: currentUser,
                created_at: new Date().toISOString()
            };
            
            let totalShift = 0;
            
            PRODUCTS.forEach(product => {
                const data = currentData[product.id];
                
                if (product.inputType === 'manual') {
                    // Guardar o texto livre e o valor numérico
                    record[`${product.id}_manual`] = data.input;
                    record[`${product.id}_adjust`] = 0;
                    record[`${product.id}_total`] = data.input; // Guardar texto para exibição
                    
                    // Adicionar valor numérico ao total
                    totalShift += data.numericTotal || 0;
                } else {
                    if (product.inputType === 'boxes') {
                        record[`${product.id}_boxes`] = data.input;
                    } else {
                        record[`${product.id}_bags`] = data.input;
                    }
                    
                    record[`${product.id}_adjust`] = data.adjust;
                    record[`${product.id}_total`] = data.total;
                    
                    totalShift += data.total;
                }
            });
            
            record.total_shift = totalShift;
            
            const result = await window.dataSdk.create(record);
            
            btn.disabled = false;
            btn.textContent = '💾 Guardar Registo';
            
            if (result.isOk) {
                showToast('Registo guardado com sucesso!');
                document.getElementById('registrationForm').classList.add('hidden');
                document.querySelectorAll('.shift-card').forEach(c => c.classList.remove('selected'));
                selectedShift = '';
            } else {
                showToast('Erro ao guardar registo', true);
            }
        });

        document.getElementById('cancelBtn').addEventListener('click', () => {
            document.getElementById('registrationForm').classList.add('hidden');
            document.querySelectorAll('.shift-card').forEach(c => c.classList.remove('selected'));
            selectedShift = '';
        });

        document.querySelectorAll('.nav-tab').forEach(tab => {
            tab.addEventListener('click', () => {
                const tabName = tab.dataset.tab;
                
                document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
                tab.classList.add('active');
                
                document.querySelectorAll('.tab-content').forEach(content => {
                    content.classList.add('hidden');
                });
                
                document.getElementById(`${tabName}Tab`).classList.remove('hidden');
                
                if (tabName === 'dashboard') {
                    renderDashboard();
                }
            });
        });

        function renderDashboard() {
            const statsContainer = document.getElementById('dashboardStats');
            const recentContainer = document.getElementById('recentEntries');
            
            if (allRecords.length === 0) {
                statsContainer.innerHTML = '<div class="empty-state"><h3>Sem dados disponíveis</h3><p>Faça o primeiro registo para ver estat  sticas</p></div>';
                recentContainer.innerHTML = '';
                return;
            }
            
            const totalProduction = allRecords.reduce((sum, r) => sum + r.total_shift, 0);
            const tardeTotal = allRecords.filter(r => r.shift === 'Tarde').reduce((sum, r) => sum + r.total_shift, 0);
            const noiteTotal = allRecords.filter(r => r.shift === 'Noite').reduce((sum, r) => sum + r.total_shift, 0);
            
            statsContainer.innerHTML = `
                <div class="summary-card">
                    <h3>Total Produção</h3>
                    <div class="value">${totalProduction}</div>
                </div>
                <div class="summary-card">
                    <h3>☀️ Turno Tarde</h3>
                    <div class="value">${tardeTotal}</div>
                </div>
                <div class="summary-card">
                    <h3>🌙 Turno Noite</h3>
                    <div class="value">${noiteTotal}</div>
                </div>
                <div class="summary-card">
                    <h3>Total Registos</h3>
                    <div class="value">${allRecords.length}</div>
                </div>
            `;
            
            const recentRecords = allRecords.slice(0, 10);
            
            recentContainer.innerHTML = `
                <h3 style="padding: 1.5rem; background: white; margin: 0;">Registos Recentes</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Data</th>
                            <th>Turno</th>
                            <th>Utilizador</th>
                            <th>Total Peças</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${recentRecords.map(record => `
                            <tr>
                                <td>${record.date}</td>
                                <td><span class="shift-badge ${record.shift.toLowerCase()}">${record.shift}</span></td>
                                <td>${record.username}</td>
                                <td><strong>${record.total_shift}</strong></td>
                            </tr>
                        `).join('')}
                    </tbody>
                </table>
            `;
        }

        document.getElementById('generateReportBtn').addEventListener('click', () => {
            const reportType = document.getElementById('reportType').value;
            const shiftFilter = document.getElementById('shiftFilter').value;
            const startDate = document.getElementById('startDate').value;
            const endDate = document.getElementById('endDate').value;
            
            if (!startDate || !endDate) {
                showToast('Por favor, selecione as datas', true);
                return;
            }
            
            generateReport(reportType, shiftFilter, startDate, endDate);
        });

        function generateReport(reportType, shiftFilter, startDate, endDate) {
            const start = new Date(startDate);
            const end = new Date(endDate);
            
            let filteredRecords = allRecords.filter(record => {
                const recordDate = parsePortugueseDate(record.date);
                const dateMatch = recordDate >= start && recordDate <= end;
                const shiftMatch = shiftFilter === 'all' || record.shift === shiftFilter;
                return dateMatch && shiftMatch;
            });
            
            if (filteredRecords.length === 0) {
                document.getElementById('reportResults').innerHTML = `
                    <div class="empty-state">
                        <h3>Sem dados no período selecionado</h3>
                        <p>Tente outro intervalo de datas ou filtro de turno</p>
                    </div>
                `;
                document.getElementById('reportActions').style.display = 'none';
                return;
            }
            
            currentReportData = { reportType, shiftFilter, startDate, endDate, filteredRecords };
            
            let reportHtml = '';
            
            if (reportType === 'shift') {
                reportHtml = generateShiftReport(filteredRecords, shiftFilter);
            } else {
                reportHtml = generateStandardReport(filteredRecords);
            }
            
            document.getElementById('reportResults').innerHTML = reportHtml;
            document.getElementById('reportActions').style.display = 'flex';
        }

        function generateShiftReport(records, shiftFilter) {
            const shifts = shiftFilter === 'all' ? ['Tarde', 'Noite'] : [shiftFilter];
            
            let html = '<div class="report-section">';
            
            shifts.forEach(shift => {
                const shiftRecords = records.filter(r => r.shift === shift);
                
                if (shiftRecords.length === 0) return;
                
                html += `
                    <h3>Turno: ${shift} (${shiftRecords.length} registos)</h3>
                    <div class="report-table">
                        <table>
                            <thead>
                                <tr>
                                    <th>Data</th>
                                    <th>Utilizador</th>`;
                
                PRODUCTS.forEach(product => {
                    html += `<th>${product.name}</th>`;
                });
                
                html += `<th>Total Turno</th></tr></thead><tbody>`;
                
                shiftRecords.forEach(record => {
                    html += `<tr><td>${record.date}</td><td>${record.username}</td>`;
                    
                    PRODUCTS.forEach(product => {
                        const totalValue = record[`${product.id}_total`];
                        html += `<td>${totalValue || '-'}</td>`;
                    });
                    
                    html += `<td><strong>${record.total_shift}</strong></td></tr>`;
                });
                
                html += `<tr class="totals-row"><td colspan="2"><strong>TOTAL ${shift.toUpperCase()}</strong></td>`;
                
                PRODUCTS.forEach(product => {
                    const productRecords = shiftRecords.filter(r => {
                        const val = r[`${product.id}_total`];
                        return typeof val === 'number';
                    });
                    
                    if (productRecords.length > 0) {
                        const productTotal = productRecords.reduce((sum, r) => sum + r[`${product.id}_total`], 0);
                        html += `<td><strong>${productTotal}</strong></td>`;
                    } else {
                        html += `<td><strong>-</strong></td>`;
                    }
                });
                
                const numericTotal = shiftRecords.reduce((sum, r) => {
                    return sum + (typeof r.total_shift === 'number' ? r.total_shift : 0);
                }, 0);
                html += `<td><strong>${numericTotal}</strong></td></tr>`;
                
                html += '</tbody></table></div>';
            });
            
            html += '</div>';
            
            return html;
        }

        function generateStandardReport(records) {
            let html = '<div class="report-table"><table><thead><tr>';
            html += '<th>Data</th><th>Turno</th><th>Utilizador</th>';
            
            PRODUCTS.forEach(product => {
                html += `<th>${product.name}</th>`;
            });
            
            html += '<th>Total</th></tr></thead><tbody>';
            
            records.forEach(record => {
                html += `<tr><td>${record.date}</td>`;
                html += `<td><span class="shift-badge ${record.shift.toLowerCase()}">${record.shift}</span></td>`;
                html += `<td>${record.username}</td>`;
                
                PRODUCTS.forEach(product => {
                    const totalValue = record[`${product.id}_total`];
                    html += `<td>${totalValue || '-'}</td>`;
                });
                
                html += `<td><strong>${record.total_shift}</strong></td></tr>`;
            });
            
            html += `<tr class="totals-row"><td colspan="3"><strong>TOTAL GERAL</strong></td>`;
            
            PRODUCTS.forEach(product => {
                const productRecords = records.filter(r => {
                    const val = r[`${product.id}_total`];
                    return typeof val === 'number';
                });
                
                if (productRecords.length > 0) {
                    const productTotal = productRecords.reduce((sum, r) => sum + r[`${product.id}_total`], 0);
                    html += `<td><strong>${productTotal}</strong></td>`;
                } else {
                    html += `<td><strong>-</strong></td>`;
                }
            });
            
            const grandTotal = records.reduce((sum, r) => {
                return sum + (typeof r.total_shift === 'number' ? r.total_shift : 0);
            }, 0);
            html += `<td><strong>${grandTotal}</strong></td></tr>`;
            
            html += '</tbody></table></div>';
            
            return html;
        }

        function parsePortugueseDate(dateStr) {
            const parts = dateStr.split('/');
            return new Date(parts[2], parts[1] - 1, parts[0]);
        }

        document.getElementById('exportPdfBtn').addEventListener('click', () => {
            const btn = document.getElementById('exportPdfBtn');
            btn.disabled = true;
            btn.innerHTML = '<span class="loading"></span> A gerar PDF...';
            
            setTimeout(() => {
                exportToPDF();
                btn.disabled = false;
                btn.textContent = '📄 Exportar para PDF';
            }, 500);
        });

        function exportToPDF() {
            if (!currentReportData) return;
            
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF('l', 'mm', 'a4');
            
            const { reportType, shiftFilter, startDate, endDate, filteredRecords } = currentReportData;
            
            const reportTypeLabels = {
                daily: 'Diário',
                shift: 'Por Turno',
                weekly: 'Semanal',
                monthly: 'Mensal'
            };
            
            doc.setFontSize(18);
            doc.setTextColor(227, 6, 19);
            doc.text('KFC Kitchen Count Manager', 15, 15);
            
            doc.setFontSize(12);
            doc.setTextColor(0, 0, 0);
            doc.text(`Relatório ${reportTypeLabels[reportType]}`, 15, 25);
            
            if (reportType === 'shift' && shiftFilter !== 'all') {
                doc.text(`Turno: ${shiftFilter}`, 15, 32);
                doc.text(`Período: ${startDate} a ${endDate}`, 15, 39);
            } else {
                doc.text(`Período: ${startDate} a ${endDate}`, 15, 32);
            }
            
            doc.text(`Gerado em: ${new Date().toLocaleString('pt-PT')}`, 15, 46);
            
            if (reportType === 'shift') {
                exportShiftPDF(doc, filteredRecords, shiftFilter);
            } else {
                exportStandardPDF(doc, filteredRecords);
            }
            
            doc.save(`KFC_Relatorio_${reportType}_${startDate}_${endDate}.pdf`);
            showToast('PDF gerado com sucesso!');
        }

        function exportShiftPDF(doc, records, shiftFilter) {
            const shifts = shiftFilter === 'all' ? ['Tarde', 'Noite'] : [shiftFilter];
            let startY = 55;
            
            shifts.forEach((shift, index) => {
                const shiftRecords = records.filter(r => r.shift === shift);
                
                if (shiftRecords.length === 0) return;
                
                if (index > 0) {
                    doc.addPage();
                    startY = 20;
                }
                
                doc.setFontSize(14);
                doc.setTextColor(227, 6, 19);
                doc.text(`Turno: ${shift}`, 15, startY);
                
                const headers = [['Data', 'Utilizador', ...PRODUCTS.map(p => p.name), 'Total']];
                
                const data = shiftRecords.map(record => [
                    record.date,
                    record.username,
                    ...PRODUCTS.map(p => {
                        const val = record[`${p.id}_total`];
                        return val !== undefined && val !== null ? String(val) : '-';
                    }),
                    record.total_shift
                ]);
                
                const totalsRow = [
                    'TOTAL',
                    '',
                    ...PRODUCTS.map(p => {
                        const productRecords = shiftRecords.filter(r => {
                            const val = r[`${p.id}_total`];
                            return typeof val === 'number';
                        });
                        
                        if (productRecords.length > 0) {
                            return productRecords.reduce((sum, r) => sum + r[`${p.id}_total`], 0);
                        }
                        return '-';
                    }),
                    shiftRecords.reduce((sum, r) => {
                        return sum + (typeof r.total_shift === 'number' ? r.total_shift : 0);
                    }, 0)
                ];
                
                data.push(totalsRow);
                
                doc.autoTable({
                    startY: startY + 7,
                    head: headers,
                    body: data,
                    theme: 'grid',
                    headStyles: {
                        fillColor: [227, 6, 19],
                        textColor: 255,
                        fontStyle: 'bold'
                    },
                    styles: {
                        fontSize: 8,
                        cellPadding: 2
                    },
                    columnStyles: {
                        0: { cellWidth: 22 },
                        1: { cellWidth: 25 }
                    },
                    didParseCell: function(data) {
                        if (data.row.index === shiftRecords.length) {
                            data.cell.styles.fillColor = [245, 245, 245];
                            data.cell.styles.fontStyle = 'bold';
                            data.cell.styles.textColor = [227, 6, 19];
                        }
                    }
                });
            });
        }

        function exportStandardPDF(doc, records) {
            const headers = [['Data', 'Turno', 'Utilizador', ...PRODUCTS.map(p => p.name), 'Total']];
            
            const data = records.map(record => [
                record.date,
                record.shift,
                record.username,
                ...PRODUCTS.map(p => {
                    const val = record[`${p.id}_total`];
                    return val !== undefined && val !== null ? String(val) : '-';
                }),
                record.total_shift
            ]);
            
            const totalsRow = [
                'TOTAL',
                '',
                '',
                ...PRODUCTS.map(p => {
                    const productRecords = records.filter(r => {
                        const val = r[`${p.id}_total`];
                        return typeof val === 'number';
                    });
                    
                    if (productRecords.length > 0) {
                        return productRecords.reduce((sum, r) => sum + r[`${p.id}_total`], 0);
                    }
                    return '-';
                }),
                records.reduce((sum, r) => {
                    return sum + (typeof r.total_shift === 'number' ? r.total_shift : 0);
                }, 0)
            ];
            
            data.push(totalsRow);
            
            doc.autoTable({
                startY: 55,
                head: headers,
                body: data,
                theme: 'grid',
                headStyles: {
                    fillColor: [227, 6, 19],
                    textColor: 255,
                    fontStyle: 'bold'
                },
                styles: {
                    fontSize: 8,
                    cellPadding: 2
                },
                columnStyles: {
                    0: { cellWidth: 22 },
                    1: { cellWidth: 18 },
                    2: { cellWidth: 25 }
                },
                didParseCell: function(data) {
                    if (data.row.index === records.length) {
                        data.cell.styles.fillColor = [245, 245, 245];
                        data.cell.styles.fontStyle = 'bold';
                        data.cell.styles.textColor = [227, 6, 19];
                    }
                }
            });
        }

        function showToast(message, isError = false) {
            const toast = document.createElement('div');
            toast.className = `toast ${isError ? 'error' : ''}`;
            toast.textContent = message;
            
            document.body.appendChild(toast);
            
            setTimeout(() => {
                toast.remove();
            }, 3000);
        }

        document.getElementById('reportType').addEventListener('change', (e) => {
            const shiftFilterGroup = document.getElementById('shiftFilterGroup');
            if (e.target.value === 'shift') {
                shiftFilterGroup.style.display = 'block';
            } else {
                shiftFilterGroup.style.display = 'none';
            }
        });

        const today = new Date().toISOString().split('T')[0];
        document.getElementById('startDate').value = today;
        document.getElementById('endDate').value = today;

        initializeApp();
    </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9b3c952257396914',t:'MTc2NjcxMDIxMS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
