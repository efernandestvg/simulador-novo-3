<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Grade de Programação de TV - Editável</title>
    <style>
        :root {
            --bg-color: #f4f6f9;
            --card-bg: #ffffff;
            --primary: #1e293b;
            --accent-programa: #2563eb;
            --accent-comercial: #d97706;
            --accent-chamada: #059669;
            --accent-marco: #7c3aed;
            --text: #334155;
            --border: #e2e8f0;
        }

        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-color);
            color: var(--text);
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 1250px;
            margin: 0 auto;
        }

        header {
            background: var(--primary);
            color: white;
            padding: 20px 30px;
            border-radius: 10px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }

        header h1 {
            margin: 0;
            font-size: 1.5rem;
        }

        .start-time-group {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .start-time-group input {
            padding: 8px 12px;
            border-radius: 6px;
            border: 1px solid #475569;
            background: #334155;
            color: white;
            font-size: 1rem;
            font-weight: bold;
        }

        .controls-panel {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            align-items: end;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .form-group label {
            font-size: 0.85rem;
            font-weight: 600;
            color: #64748b;
        }

        input, select {
            padding: 9px 12px;
            border: 1px solid var(--border);
            border-radius: 6px;
            font-size: 0.95rem;
        }

        .inline-input {
            padding: 6px 10px;
            border: 1px solid #cbd5e1;
            border-radius: 6px;
            font-size: 0.9rem;
            width: 100%;
            box-sizing: border-box;
        }

        .btn {
            padding: 10px 16px;
            border: none;
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
        }

        .btn-add { background: #0f172a; color: white; }
        .btn-add:hover { background: #334155; }

        .grid-table {
            width: 100%;
            border-collapse: collapse;
            background: var(--card-bg);
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }

        .grid-table th {
            background: #f8fafc;
            padding: 14px;
            text-align: left;
            font-size: 0.85rem;
            text-transform: uppercase;
            color: #64748b;
            border-bottom: 2px solid var(--border);
        }

        .grid-table td {
            padding: 10px 14px;
            border-bottom: 1px solid var(--border);
            vertical-align: middle;
        }

        .badge {
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: bold;
            color: white;
            text-transform: uppercase;
            display: inline-block;
        }

        .badge-programa { background: var(--accent-programa); }
        .badge-comercial { background: var(--accent-comercial); }
        .badge-chamada { background: var(--accent-chamada); }
        .badge-marco { background: var(--accent-marco); }

        .row-marco {
            background-color: #f3e8ff !important;
            font-weight: bold;
        }

        .row-editing {
            background-color: #f0fdf4 !important;
        }

        .status-afinacao {
            font-size: 0.85rem;
            font-weight: bold;
            padding: 4px 8px;
            border-radius: 4px;
        }

        .afinado { background: #dcfce7; color: #166534; }
        .buraco { background: #fef3c7; color: #92400e; }
        .estouro { background: #fee2e2; color: #991b1b; }

        .btn-action {
            background: none;
            border: none;
            cursor: pointer;
            padding: 5px 8px;
            border-radius: 4px;
            font-size: 1.1rem;
        }

        .btn-action:hover { background: #e2e8f0; }
        .btn-delete:hover { background: #fee2e2; color: #dc2626; }
        .btn-save:hover { background: #dcfce7; color: #166534; }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>📺 SIMULADOR DE GRADE</h1>
        <div class="start-time-group">
            <label for="startTime">Início da Grade:</label>
            <input type="time" id="startTime" value="06:00:00" step="1" onchange="renderGrid()">
        </div>
    </header>

    <!-- Painel para adicionar novas linhas -->
    <div class="controls-panel">
        <div class="form-group">
            <label for="itemType">Tipo de Elemento</label>
            <select id="itemType" onchange="toggleInputs()">
                <option value="programa">📺 Programa</option>
                <option value="comercial">💰 Inserção Comercial</option>
                <option value="chamada">📢 Chamada / Promo</option>
                <option value="marco">🎯 Marco de Afinação</option>
            </select>
        </div>

        <div class="form-group">
            <label for="itemTitle">Título / Descrição</label>
            <input type="text" id="itemTitle" placeholder="Ex: Comercial Banco X / Entrada do Jornal">
        </div>

        <div class="form-group" id="durationGroup">
            <label for="itemDuration">Duração (HH:MM:SS ou MM:SS)</label>
            <input type="text" id="itemDuration" placeholder="00:30 ou 00:00:30" value="00:30">
        </div>

        <div class="form-group" id="targetTimeGroup" style="display: none;">
            <label for="targetTime">Horário Alvo Fixado</label>
            <input type="time" id="targetTime" value="06:30:00" step="1">
        </div>

        <button class="btn btn-add" onclick="addItem()">➕ Adicionar à Grade</button>
    </div>

    <!-- Tabela principal -->
    <table class="grid-table">
        <thead>
            <tr>
                <th style="width: 40px;">#</th>
                <th style="width: 140px;">Tipo</th>
                <th>Título / Elemento</th>
                <th style="width: 90px;">Início</th>
                <th style="width: 130px;">Duração / Alvo</th>
                <th style="width: 90px;">Término</th>
                <th style="width: 230px;">Afinação de Grade</th>
                <th style="width: 150px;">Ações</th>
            </tr>
        </thead>
        <tbody id="gridBody">
            <!-- Linhas geradas dinamicamente via JS -->
        </tbody>
    </table>
</div>

<script>
    // Estrutura de dados inicial
    let gridData = [
        { type: 'programa', title: 'Jornal da Manhã - Bloco 1', duration: '00:25:00' },
        { type: 'comercial', title: 'Comercial Banco X', duration: '00:00:30' },
        { type: 'chamada', title: 'Chamada Novela das 8', duration: '00:00:15' },
        { type: 'marco', title: 'MARCO: Entrada do Esporte', targetTime: '06:30:00' },
        { type: 'programa', title: 'Programa de Esportes - Bloco 1', duration: '00:20:00' }
    ];

    let editingIndex = -1; // Guarda o índice da linha em edição (-1 = nenhuma)

    function toggleInputs() {
        const type = document.getElementById('itemType').value;
        const durGroup = document.getElementById('durationGroup');
        const targetGroup = document.getElementById('targetTimeGroup');
        
        if (type === 'marco') {
            durGroup.style.display = 'none';
            targetGroup.style.display = 'flex';
        } else {
            durGroup.style.display = 'flex';
            targetGroup.style.display = 'none';
        }
    }

    function timeToSeconds(tStr) {
        if (!tStr) return 0;
        const parts = tStr.split(':').map(Number);
        if (parts.length === 3) return parts[0] * 3600 + parts[1] * 60 + parts[2];
        if (parts.length === 2) return parts[0] * 60 + parts[1];
        return 0;
    }

    function secondsToTime(totalSec) {
        const sign = totalSec < 0 ? "-" : "";
        const absSec = Math.abs(totalSec);
        const h = Math.floor(absSec / 3600).toString().padStart(2, '0');
        const m = Math.floor((absSec % 3600) / 60).toString().padStart(2, '0');
        const s = (absSec % 60).toString().padStart(2, '0');
        return `${sign}${h}:${m}:${s}`;
    }

    function addItem() {
        const type = document.getElementById('itemType').value;
        const title = document.getElementById('itemTitle').value.trim() || 'Sem Título';
        
        if (type === 'marco') {
            const targetTime = document.getElementById('targetTime').value;
            gridData.push({ type, title, targetTime });
        } else {
            let duration = document.getElementById('itemDuration').value.trim();
            if (duration.split(':').length === 2) duration = "00:" + duration;
            gridData.push({ type, title, duration });
        }

        document.getElementById('itemTitle').value = '';
        renderGrid();
    }

    function removeItem(index) {
        if (editingIndex === index) editingIndex = -1;
        gridData.splice(index, 1);
        renderGrid();
    }

    function moveItem(index, direction) {
        if (index + direction < 0 || index + direction >= gridData.length) return;
        const temp = gridData[index];
        gridData[index] = gridData[index + direction];
        gridData[index + direction] = temp;
        
        if (editingIndex === index) editingIndex = index + direction;
        else if (editingIndex === index + direction) editingIndex = index;
        
        renderGrid();
    }

    // --- FUNÇÕES DE EDIÇÃO ---
    function editItem(index) {
        editingIndex = index;
        renderGrid();
    }

    function cancelEdit() {
        editingIndex = -1;
        renderGrid();
    }

    function saveItem(index) {
        const editType = document.getElementById(`editType_${index}`).value;
        const editTitle = document.getElementById(`editTitle_${index}`).value.trim() || 'Sem Título';
        
        if (editType === 'marco') {
            const targetTime = document.getElementById(`editTargetTime_${index}`).value;
            gridData[index] = { type: editType, title: editTitle, targetTime };
        } else {
            let duration = document.getElementById(`editDuration_${index}`).value.trim();
            if (duration.split(':').length === 2) duration = "00:" + duration;
            gridData[index] = { type: editType, title: editTitle, duration };
        }

        editingIndex = -1;
        renderGrid();
    }

    function toggleEditRowInputs(index) {
        const type = document.getElementById(`editType_${index}`).value;
        const durContainer = document.getElementById(`editDurContainer_${index}`);
        const targetContainer = document.getElementById(`editTargetContainer_${index}`);

        if (type === 'marco') {
            durContainer.style.display = 'none';
            targetContainer.style.display = 'block';
        } else {
            durContainer.style.display = 'block';
            targetContainer.style.display = 'none';
        }
    }

    function renderGrid() {
        const tbody = document.getElementById('gridBody');
        tbody.innerHTML = '';

        let currentSec = timeToSeconds(document.getElementById('startTime').value);

        gridData.forEach((item, index) => {
            const tr = document.createElement('tr');
            const isEditing = (index === editingIndex);

            if (isEditing) {
                tr.classList.add('row-editing');
                
                // MODO EDIÇÃO
                const startStr = secondsToTime(currentSec);
                let durSec = (item.type !== 'marco') ? timeToSeconds(item.duration) : 0;
                const endStr = secondsToTime(currentSec + durSec);

                tr.innerHTML = `
                    <td>${index + 1}</td>
                    <td>
                        <select id="editType_${index}" class="inline-input" onchange="toggleEditRowInputs(${index})">
                            <option value="programa" ${item.type === 'programa' ? 'selected' : ''}>📺 Programa</option>
                            <option value="comercial" ${item.type === 'comercial' ? 'selected' : ''}>💰 Comercial</option>
                            <option value="chamada" ${item.type === 'chamada' ? 'selected' : ''}>📢 Chamada</option>
                            <option value="marco" ${item.type === 'marco' ? 'selected' : ''}>🎯 Marco</option>
                        </select>
                    </td>
                    <td>
                        <input type="text" id="editTitle_${index}" value="${item.title}" class="inline-input">
                    </td>
                    <td>${startStr}</td>
                    <td>
                        <div id="editDurContainer_${index}" style="display: ${item.type !== 'marco' ? 'block' : 'none'};">
                            <input type="text" id="editDuration_${index}" value="${item.duration || '00:00:30'}" class="inline-input" placeholder="00:00:30">
                        </div>
                        <div id="editTargetContainer_${index}" style="display: ${item.type === 'marco' ? 'block' : 'none'};">
                            <input type="time" id="editTargetTime_${index}" value="${item.targetTime || '06:30:00'}" step="1" class="inline-input">
                        </div>
                    </td>
                    <td>${item.type !== 'marco' ? endStr : startStr}</td>
                    <td><span style="font-size:0.8rem; color:#64748b;">(salve para recalcular)</span></td>
                    <td>
                        <button class="btn-action btn-save" title="Salvar" onclick="saveItem(${index})">💾</button>
                        <button class="btn-action" title="Cancelar" onclick="cancelEdit()">❌</button>
                    </td>
                `;

                // Avança o horário para manter o cálculo dos itens posteriores correto durante a edição
                if (item.type !== 'marco') currentSec += durSec;

            } else if (item.type === 'marco') {
                tr.classList.add('row-marco');
                const targetSec = timeToSeconds(item.targetTime);
                const diffSec = currentSec - targetSec;
                
                let statusHtml = '';
                if (diffSec === 0) {
                    statusHtml = `<span class="status-afinacao afinado">✅ AFINADO (00:00:00)</span>`;
                } else if (diffSec < 0) {
                    statusHtml = `<span class="status-afinacao buraco">⚠️ BURACO: +${secondsToTime(Math.abs(diffSec))} faltantes</span>`;
                } else {
                    statusHtml = `<span class="status-afinacao estouro">🚨 ESTOURO: -${secondsToTime(diffSec)} excedentes</span>`;
                }

                const timeStr = secondsToTime(currentSec);

                tr.innerHTML = `
                    <td>${index + 1}</td>
                    <td><span class="badge badge-marco">🎯 Marco</span></td>
                    <td>${item.title}</td>
                    <td><strong>${timeStr}</strong></td>
                    <td>Alvo: <strong>${item.targetTime}</strong></td>
                    <td><strong>${timeStr}</strong></td>
                    <td>${statusHtml}</td>
                    <td>
                        <button class="btn-action" title="Editar" onclick="editItem(${index})">✏️</button>
                        <button class="btn-action" title="Subir" onclick="moveItem(${index}, -1)">⬆️</button>
                        <button class="btn-action" title="Descer" onclick="moveItem(${index}, 1)">⬇️</button>
                        <button class="btn-action btn-delete" title="Excluir" onclick="removeItem(${index})">🗑️</button>
                    </td>
                `;
            } else {
                const startStr = secondsToTime(currentSec);
                const durSec = timeToSeconds(item.duration);
                currentSec += durSec;
                const endStr = secondsToTime(currentSec);

                const badgeMap = {
                    programa: '<span class="badge badge-programa">📺 Programa</span>',
                    comercial: '<span class="badge badge-comercial">💰 Comercial</span>',
                    chamada: '<span class="badge badge-chamada">📢 Chamada</span>'
                };

                tr.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${badgeMap[item.type]}</td>
                    <td>${item.title}</td>
                    <td>${startStr}</td>
                    <td>${item.duration}</td>
                    <td>${endStr}</td>
                    <td>-</td>
                    <td>
                        <button class="btn-action" title="Editar" onclick="editItem(${index})">✏️</button>
                        <button class="btn-action" title="Subir" onclick="moveItem(${index}, -1)">⬆️</button>
                        <button class="btn-action" title="Descer" onclick="moveItem(${index}, 1)">⬇️</button>
                        <button class="btn-action btn-delete" title="Excluir" onclick="removeItem(${index})">🗑️</button>
                    </td>
                `;
            }

            tbody.appendChild(tr);
        });
    }

    // Inicialização
    renderGrid();
</script>

</body>
</html>
