<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>📚 Resúmenes y Exámenes - Grupo de Estudio</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* Fondo normal */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f2b3d, #1a4a6f, #0f2b3d);
            background-size: 200% 200%;
            animation: gradientShift 12s ease infinite;
            min-height: 100vh;
            padding: 20px;
            transition: background 0.5s;
        }

        /* Fondo de advertencia (trampa) */
        body.warning-mode {
            background: linear-gradient(135deg, #8B0000, #DC143C, #8B0000);
            background-size: 200% 200%;
            animation: warningGlow 0.8s infinite alternate;
        }

        @keyframes warningGlow {
            0% { background-position: 0% 50%; filter: brightness(1); }
            100% { background-position: 100% 50%; filter: brightness(1.2); }
        }

        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        h1 {
            text-align: center;
            color: #ffd966;
            font-size: 2rem;
            margin-bottom: 10px;
            text-shadow: 0 2px 10px rgba(0,0,0,0.3);
        }

        .subtitle {
            text-align: center;
            color: #cbd5e6;
            margin-bottom: 25px;
        }

        .tabs {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-bottom: 25px;
            flex-wrap: wrap;
        }

        .tab-btn {
            background: rgba(15, 35, 55, 0.8);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 217, 102, 0.3);
            padding: 12px 35px;
            font-size: 1rem;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            color: #cbd5e6;
            transition: all 0.2s;
        }

        .tab-btn.active {
            background: #ffd966;
            color: #1a2a3a;
            border-color: #ffd966;
        }

        .panel {
            display: none;
            background: rgba(15, 35, 55, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 30px;
            animation: fadeIn 0.3s ease;
            border: 1px solid rgba(255, 217, 102, 0.2);
        }

        .panel.active-panel {
            display: block;
        }

        /* Pantalla de advertencia (trampa) */
        .warning-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(8px);
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
            animation: fadeInWarning 0.3s ease;
        }

        @keyframes fadeInWarning {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .warning-card {
            background: linear-gradient(145deg, #2a0000, #4a0000);
            border-radius: 48px;
            padding: 40px;
            text-align: center;
            max-width: 450px;
            width: 90%;
            border: 3px solid #ff4444;
            box-shadow: 0 0 50px rgba(255,0,0,0.5);
            animation: shakeCard 0.5s ease;
        }

        @keyframes shakeCard {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }

        .warning-icon {
            font-size: 5rem;
            margin-bottom: 20px;
            animation: pulseWarning 0.8s infinite alternate;
        }

        @keyframes pulseWarning {
            from { transform: scale(1); text-shadow: 0 0 5px red; }
            to { transform: scale(1.1); text-shadow: 0 0 20px red; }
        }

        .warning-title {
            font-size: 2rem;
            color: #ff6666;
            margin-bottom: 15px;
            font-weight: bold;
        }

        .warning-text {
            color: #ffaaaa;
            margin-bottom: 30px;
            font-size: 1rem;
        }

        .warning-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .warning-btn-back {
            background: linear-gradient(95deg, #ffd966, #ffb347);
            border: none;
            padding: 14px 28px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.2s;
            color: #1a2a3a;
        }

        .warning-btn-back:hover {
            transform: scale(1.02);
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
        }

        .warning-btn-cheat {
            background: rgba(255, 50, 50, 0.8);
            border: 2px solid #ff8888;
            padding: 14px 28px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.2s;
            color: white;
        }

        .warning-btn-cheat:hover {
            background: #f00;
            transform: scale(1.02);
        }

        /* Resto de estilos... */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        h3 { color: #ffd966; margin-bottom: 20px; }

        .file-drop-area {
            border: 2px dashed rgba(255, 217, 102, 0.4);
            border-radius: 28px;
            padding: 30px;
            text-align: center;
            background: rgba(10, 30, 50, 0.5);
            margin-bottom: 20px;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .file-drop-area.drag-over {
            border-color: #ffd966;
            background: rgba(255, 217, 102, 0.1);
            transform: scale(1.01);
        }

        .file-drop-area.has-file {
            border-color: #4caf50;
            background: rgba(76, 175, 80, 0.1);
        }

        .file-icon { font-size: 3rem; margin-bottom: 12px; }
        .file-drop-text { color: #cbd5e6; margin-bottom: 12px; }
        .file-drop-text strong { color: #ffd966; }
        .file-input-hidden { display: none; }

        .file-info {
            display: none;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-top: 15px;
            padding: 12px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 60px;
            flex-wrap: wrap;
        }

        .file-info.show { display: flex; }
        .file-name { color: #4caf50; font-weight: bold; background: rgba(0, 0, 0, 0.4); padding: 5px 15px; border-radius: 60px; font-size: 0.9rem; }
        .file-clear-btn { background: rgba(255, 100, 100, 0.8); border: none; padding: 6px 15px; border-radius: 30px; color: white; cursor: pointer; font-size: 0.8rem; }
        .file-clear-btn:hover { background: #f44336; }
        .file-select-btn { background: rgba(255, 217, 102, 0.2); border: 1px solid #ffd966; padding: 10px 24px; border-radius: 40px; color: #ffd966; cursor: pointer; font-size: 0.9rem; margin-top: 10px; }
        .file-select-btn:hover { background: rgba(255, 217, 102, 0.3); }

        textarea { width: 100%; padding: 12px; border-radius: 20px; border: 1px solid rgba(255, 217, 102, 0.3); background: rgba(0, 0, 0, 0.4); color: #e2e8f0; margin: 10px 0; font-size: 0.9rem; }
        textarea::placeholder { color: #8aaec0; }

        button.primary { background: linear-gradient(95deg, #ffd966, #ffb347); color: #1a2a3a; border: none; padding: 12px 28px; border-radius: 40px; font-weight: bold; font-size: 1rem; cursor: pointer; transition: transform 0.1s; }
        button.primary:hover { transform: scale(1.02); }
        button.secondary { background: rgba(255, 255, 255, 0.1); border: 1px solid #ffd966; color: #ffd966; padding: 10px 24px; border-radius: 40px; font-weight: bold; cursor: pointer; transition: 0.2s; }
        button.secondary:hover { background: rgba(255, 217, 102, 0.15); }

        .result-box { background: rgba(10, 30, 50, 0.6); border-radius: 20px; padding: 20px; margin-top: 20px; color: #e2e8f0; line-height: 1.6; white-space: pre-wrap; }
        .loading { display: inline-flex; align-items: center; gap: 10px; }
        .loading::before { content: ""; width: 18px; height: 18px; border: 2px solid #ffd966; border-top-color: transparent; border-radius: 50%; animation: spin 0.8s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .error-msg { background: rgba(198, 40, 40, 0.8); padding: 12px; border-radius: 20px; text-align: center; }
        .exam-from-summary-btn { text-align: center; margin-top: 20px; }

        .exam-header { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; margin-bottom: 25px; padding-bottom: 15px; border-bottom: 2px solid rgba(255, 217, 102, 0.3); }
        .progress-area { background: rgba(0, 0, 0, 0.4); border-radius: 60px; padding: 8px 20px; display: flex; align-items: center; gap: 12px; }
        .progress-text { font-size: 1.2rem; font-weight: bold; color: #ffd966; }
        .progress-bar-bg { width: 150px; height: 8px; background: #2a3a5a; border-radius: 10px; overflow: hidden; }
        .progress-bar-fill { height: 100%; background: linear-gradient(90deg, #ffd966, #ffb347); width: 0%; transition: width 0.4s ease; }
        .streak-area { background: rgba(255, 200, 100, 0.15); border-radius: 60px; padding: 8px 18px; display: flex; align-items: center; gap: 8px; }
        .streak-count { font-weight: bold; color: #ffd966; }
        .question-card { background: linear-gradient(135deg, rgba(255,255,255,0.08), rgba(255,255,255,0.03)); border-radius: 32px; padding: 35px 25px; margin: 20px 0; text-align: center; }
        .question-text { font-size: 1.5rem; font-weight: 600; color: #ffffff; line-height: 1.4; margin-bottom: 30px; }
        .options-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px; margin-top: 15px; }
        .option-btn { background: rgba(25, 55, 80, 0.9); border: 2px solid rgba(255, 217, 102, 0.3); border-radius: 60px; padding: 14px 18px; font-size: 1rem; font-weight: 500; color: #e2e8f0; cursor: pointer; transition: all 0.2s ease; text-align: left; display: flex; align-items: center; gap: 15px; }
        .option-btn:hover { background: rgba(255, 217, 102, 0.15); border-color: #ffd966; transform: translateX(5px); }
        .option-prefix { background: #ffd966; color: #1a2a3a; width: 34px; height: 34px; display: inline-flex; align-items: center; justify-content: center; border-radius: 30px; font-weight: bold; }
        .feedback { margin-top: 25px; padding: 14px; border-radius: 60px; text-align: center; font-weight: bold; }
        .feedback-correct { background: rgba(46, 125, 50, 0.9); color: #c8e6c9; }
        .feedback-wrong { background: rgba(198, 40, 40, 0.9); color: #ffcdd2; }
        .success-screen { text-align: center; padding: 40px 20px; }
        .success-icon { font-size: 4rem; margin-bottom: 20px; }
        .success-title { font-size: 2rem; color: #ffd966; margin-bottom: 15px; }
        .footer-note { text-align: center; margin-top: 25px; font-size: 0.75rem; color: #8aaec0; }

        @media (max-width: 650px) {
            .panel { padding: 20px; }
            .question-text { font-size: 1.2rem; }
            .options-grid { grid-template-columns: 1fr; }
            .option-btn { padding: 12px 16px; }
        }
    </style>
</head>
<body>
<div class="container">
    <h1>📚 Resúmenes y Exámenes</h1>
    <div class="subtitle">🤖 Con IA - Sube un PDF o pega texto</div>

    <div class="tabs">
        <button class="tab-btn active" data-tab="resumen">📄 Generar Resumen</button>
        <button class="tab-btn" data-tab="examen">📝 Hacer Examen</button>
    </div>

    <!-- Panel Resumen -->
    <div id="resumenPanel" class="panel active-panel">
        <h3>📎 Sube un PDF o pega texto</h3>
        <div class="file-drop-area" id="fileDropArea">
            <div class="file-icon">📄</div>
            <div class="file-drop-text">
                <strong>📂 Arrastra y suelta tu PDF aquí</strong><br>
                o haz clic para seleccionar
            </div>
            <div class="file-info" id="fileInfo">
                <span class="file-name" id="fileName"></span>
                <button class="file-clear-btn" id="clearFileBtn">✖ Cambiar archivo</button>
            </div>
            <div class="file-select-btn" id="selectFileBtn">🔍 Seleccionar archivo</div>
            <input type="file" id="pdfInput" accept="application/pdf" class="file-input-hidden">
        </div>
        <textarea id="rawTextInput" rows="5" placeholder="O pega aquí el texto que quieras resumir..."></textarea>
        <button class="primary" id="generateSummaryBtn">✨ Generar Resumen con IA</button>

        <div id="summaryResult" class="result-box" style="display:none;">
            <strong>📌 Resumen generado:</strong>
            <div id="summaryText"></div>
            <div class="exam-from-summary-btn" id="examFromSummaryContainer" style="display:none;">
                <button class="secondary" id="examFromSummaryBtn">📝 Tomar examen sobre este resumen</button>
            </div>
        </div>
    </div>

    <!-- Panel Examen -->
    <div id="examenPanel" class="panel">
        <div id="examApp"></div>
    </div>
</div>

<script>
    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';
    
    // ========== CONFIGURACIÓN ==========
    const GROQ_API_KEY = "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";
    const MODELO = "llama-3.1-8b-instant";
    
    let ultimoResumen = "";
    let examActivo = false;  // ← Bandera para saber si el examen está en curso
    
    // Estado del examen
    let examPreguntas = [];
    let examIndice = 0;
    let examEsperando = false;
    let examTimeout = null;
    let aciertosConsecutivos = 0;
    let textoExamenActual = "";
    
    const examContainer = document.getElementById('examApp');
    
    // ========== FUNCIONES ANTI-TRAMPAS ==========
    function bloquearCambioATrampa() {
        if (!examActivo) return false;
        
        // Mostrar pantalla de advertencia
        const warningDiv = document.createElement('div');
        warningDiv.className = 'warning-overlay';
        warningDiv.innerHTML = `
            <div class="warning-card">
                <div class="warning-icon">⚠️🚨⚠️</div>
                <div class="warning-title">¡ESTÁS A PUNTO DE HACER TRAMPA!</div>
                <div class="warning-text">No puedes cambiar de pestaña mientras el examen está activo.<br>Elige una opción:</div>
                <div class="warning-buttons">
                    <button class="warning-btn-back" id="backToExamBtn">🔙 Volver al Examen</button>
                    <button class="warning-btn-cheat" id="cheatBtn">🎲 Hacer Trampa</button>
                </div>
            </div>
        `;
        document.body.appendChild(warningDiv);
        document.body.classList.add('warning-mode');
        
        document.getElementById('backToExamBtn')?.addEventListener('click', () => {
            warningDiv.remove();
            document.body.classList.remove('warning-mode');
        });
        
        document.getElementById('cheatBtn')?.addEventListener('click', () => {
            // Expulsar de la página (recargar)
            window.location.reload();
        });
        
        return true;
    }
    
    // Bloquear pestañas cuando el examen está activo
    function configurarAntiTrampas() {
        const tabBtns = document.querySelectorAll('.tab-btn');
        tabBtns.forEach(btn => {
            btn.addEventListener('click', (e) => {
                const tab = btn.getAttribute('data-tab');
                if (examActivo && tab !== 'examen') {
                    e.preventDefault();
                    e.stopPropagation();
                    bloquearCambioATrampa();
                    return false;
                }
            });
        });
        
        // También bloquear el botón de generar resumen si el examen está activo
        const generateBtn = document.getElementById('generateSummaryBtn');
        const originalClick = generateBtn.onclick;
        generateBtn.addEventListener('click', (e) => {
            if (examActivo) {
                e.preventDefault();
                e.stopPropagation();
                bloquearCambioATrampa();
                return false;
            }
        });
    }
    
    // ========== FUNCIONES DE RESUMEN ==========
    async function llamarGroq(prompt) {
        const response = await fetch("https://api.groq.com/openai/v1/chat/completions", {
            method: "POST",
            headers: { "Content-Type": "application/json", "Authorization": `Bearer ${GROQ_API_KEY}` },
            body: JSON.stringify({
                model: MODELO,
                messages: [{ role: "system", content: "Eres un asistente educativo útil. Respondes en español de forma clara y concisa." }, { role: "user", content: prompt }],
                temperature: 0.5,
                max_tokens: 1500
            })
        });
        if (!response.ok) throw new Error(`Error ${response.status}`);
        const data = await response.json();
        return data.choices[0].message.content;
    }
    
    async function extraerTextoPDF(file) {
        const arrayBuffer = await file.arrayBuffer();
        const pdf = await pdfjsLib.getDocument({ data: arrayBuffer }).promise;
        let textoCompleto = "";
        for (let i = 1; i <= pdf.numPages; i++) {
            const pagina = await pdf.getPage(i);
            const contenido = await pagina.getTextContent();
            textoCompleto += contenido.items.map(item => item.str).join(" ") + "\n";
        }
        return textoCompleto;
    }
    
    // ========== INTERACTIVIDAD DEL ARCHIVO ==========
    const fileDropArea = document.getElementById('fileDropArea');
    const pdfInput = document.getElementById('pdfInput');
    const fileInfo = document.getElementById('fileInfo');
    const fileNameSpan = document.getElementById('fileName');
    const selectFileBtn = document.getElementById('selectFileBtn');
    const clearFileBtn = document.getElementById('clearFileBtn');
    
    function updateFileUI(file) {
        if (file) {
            fileNameSpan.textContent = `📄 ${file.name} (${(file.size / 1024).toFixed(1)} KB)`;
            fileInfo.classList.add('show');
            fileDropArea.classList.add('has-file');
            pdfInput.files = file;
        } else {
            fileInfo.classList.remove('show');
            fileDropArea.classList.remove('has-file');
            pdfInput.value = '';
        }
    }
    
    selectFileBtn.addEventListener('click', () => pdfInput.click());
    pdfInput.addEventListener('change', (e) => updateFileUI(e.target.files[0]));
    clearFileBtn.addEventListener('click', () => updateFileUI(null));
    
    fileDropArea.addEventListener('dragover', (e) => {
        e.preventDefault();
        fileDropArea.classList.add('drag-over');
    });
    fileDropArea.addEventListener('dragleave', () => fileDropArea.classList.remove('drag-over'));
    fileDropArea.addEventListener('drop', (e) => {
        e.preventDefault();
        fileDropArea.classList.remove('drag-over');
        const file = e.dataTransfer.files[0];
        if (file && file.type === 'application/pdf') updateFileUI(file);
        else alert('Solo se permiten archivos PDF');
    });
    fileDropArea.addEventListener('click', () => pdfInput.click());
    
    document.getElementById('generateSummaryBtn').addEventListener('click', async () => {
        if (examActivo) return bloquearCambioATrampa();
        
        const pdfFile = pdfInput.files[0];
        const manualText = document.getElementById('rawTextInput').value;
        let textoFuente = "";
        
        if (pdfFile) {
            try { textoFuente = await extraerTextoPDF(pdfFile); }
            catch(e) { alert("Error al leer el PDF: " + e.message); return; }
        } else if (manualText.trim()) { textoFuente = manualText; }
        else { alert("Sube un PDF o ingresa texto manual."); return; }
        
        if (textoFuente.trim().length === 0) { alert("El texto está vacío."); return; }
        
        const summaryDiv = document.getElementById('summaryResult');
        const summaryTextDiv = document.getElementById('summaryText');
        const examBtnContainer = document.getElementById('examFromSummaryContainer');
        summaryDiv.style.display = 'block';
        summaryTextDiv.innerHTML = '<div class="loading">Generando resumen...</div>';
        examBtnContainer.style.display = 'none';
        
        try {
            const prompt = `Resume el siguiente texto de forma clara, concisa y en español, destacando las ideas principales (máximo 300 palabras):\n\n${textoFuente.substring(0, 8000)}`;
            const resumen = await llamarGroq(prompt);
            summaryTextDiv.innerHTML = resumen.replace(/\n/g, '<br>');
            ultimoResumen = resumen;
            examBtnContainer.style.display = 'block';
        } catch(e) { summaryTextDiv.innerHTML = `<div class="error-msg">❌ Error: ${e.message}</div>`; }
    });
    
    document.getElementById('examFromSummaryBtn').addEventListener('click', () => {
        if (!ultimoResumen) { alert("Primero genera un resumen."); return; }
        textoExamenActual = ultimoResumen;
        document.querySelector('.tab-btn[data-tab="examen"]').click();
        examActivo = true;
        generarExamen(textoExamenActual);
    });
    
    // ========== FUNCIONES DEL EXAMEN ==========
    async function generarExamen(textoBase) {
        if (!textoBase || textoBase.trim().length < 30) { mostrarPantallaInicialExamen(); return; }
        textoExamenActual = textoBase;
        mostrarCargandoExamen();
        try {
            const prompt = `Crea un examen de 10 preguntas de opción múltiple (cada una con A, B, C, D) basado en este texto. Usa exactamente este formato:
Pregunta 1: [texto pregunta]
A) [opcion]
B) [opcion]
C) [opcion]
D) [opcion]
Respuesta: X
(Repite hasta Pregunta 10)
Texto: ${textoBase.substring(0, 6000)}`;
            const respuesta = await llamarGroq(prompt);
            const preguntas = parsearPreguntas(respuesta);
            if (preguntas.length < 8) throw new Error("No se generaron suficientes preguntas");
            examPreguntas = preguntas.slice(0, 10);
            examIndice = 0;
            aciertosConsecutivos = 0;
            examEsperando = false;
            examActivo = true;
            if (examTimeout) clearTimeout(examTimeout);
            mostrarPreguntaExamen();
        } catch(e) { mostrarErrorExamen(); }
    }
    
    function parsearPreguntas(respuesta) {
        const preguntas = [];
        const lineas = respuesta.split('\n');
        let actual = null;
        for (let linea of lineas) {
            linea = linea.trim();
            if (linea.match(/^Pregunta \d+:/i)) {
                if (actual && actual.opciones?.length === 4) preguntas.push(actual);
                actual = { texto: linea.substring(linea.indexOf(':') + 1).trim(), opciones: [], respuesta: "" };
            } else if (actual && /^[A-D]\)/.test(linea)) { actual.opciones.push(linea.substring(2).trim()); }
            else if (actual && linea.toLowerCase().startsWith('respuesta:')) {
                let letra = linea.substring('respuesta:'.length).trim().toUpperCase();
                actual.respuesta = letra[0] || '';
            }
        }
        if (actual && actual.opciones?.length === 4) preguntas.push(actual);
        return preguntas;
    }
    
    function mostrarPantallaInicialExamen() {
        examContainer.innerHTML = `
            <div style="text-align:center; padding:25px;">
                <div style="font-size:3.5rem;">📝</div>
                <h3 style="color:#ffd966;">Examen interactivo</h3>
                <p style="color:#e2e8f0; margin-bottom:25px;">Pega el texto del que quieras estudiar</p>
                <textarea id="textoExamenManual" rows="5" style="width:100%; background:rgba(0,0,0,0.5); border:2px solid #ffd966; border-radius:24px; padding:15px; color:white; font-size:1rem;" placeholder="Ej: pega aquí tu resumen..."></textarea>
                <button class="primary" id="btnIniciarExamenManual" style="margin-top:20px;">🎯 Comenzar examen</button>
            </div>`;
        document.getElementById('btnIniciarExamenManual')?.addEventListener('click', () => {
            const texto = document.getElementById('textoExamenManual').value;
            if (texto.trim()) { examActivo = true; generarExamen(texto); }
            else alert("Pega un texto");
        });
    }
    
    function mostrarCargandoExamen() {
        examContainer.innerHTML = `<div style="text-align:center; padding:50px;"><div style="width:50px;height:50px;border:4px solid #ffd966;border-top-color:transparent;border-radius:50%;animation:spin 0.8s linear infinite;margin:0 auto 20px;"></div><p style="color:#ffd966;">📚 Generando tu examen...</p></div>`;
    }
    
    function mostrarErrorExamen() {
        examContainer.innerHTML = `<div style="text-align:center;padding:40px;"><div style="font-size:3rem;">📖</div><h3 style="color:#ffaa66;">No se pudo generar el examen</h3><button class="primary" onclick="location.reload()">Reintentar</button></div>`;
    }
    
    function mostrarPreguntaExamen() {
        if (examIndice >= examPreguntas.length) { mostrarExitoExamen(); return; }
        const q = examPreguntas[examIndice];
        const progreso = (examIndice / examPreguntas.length) * 100;
        examContainer.innerHTML = `
            <div class="exam-header">
                <div class="progress-area"><span class="progress-text">📋 ${examIndice+1}/${examPreguntas.length}</span><div class="progress-bar-bg"><div class="progress-bar-fill" style="width:${progreso}%;"></div></div></div>
                <div class="streak-area"><span>✨</span><span class="streak-count">Aciertos: ${aciertosConsecutivos}</span></div>
            </div>
            <div class="question-card"><div class="question-text">${escapeHtml(q.texto)}</div><div class="options-grid" id="opcionesGrid">${q.opciones.map((opt,idx) => `<div class="option-btn" data-letra="${['A','B','C','D'][idx]}"><span class="option-prefix">${['A','B','C','D'][idx]}</span><span>${escapeHtml(opt)}</span></div>`).join('')}</div><div id="feedbackArea"></div></div>
            <div class="footer-note">💡 Una respuesta incorrecta reinicia el examen con preguntas nuevas</div>`;
        document.querySelectorAll('.option-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                if (examEsperando) return;
                const letraSel = btn.getAttribute('data-letra');
                const correcta = examPreguntas[examIndice].respuesta;
                const fbDiv = document.getElementById('feedbackArea');
                if (letraSel === correcta) {
                    aciertosConsecutivos++;
                    fbDiv.innerHTML = `<div class="feedback feedback-correct">✅ ¡Correcto! ✨ Aciertos: ${aciertosConsecutivos}</div>`;
                    examEsperando = true;
                    examTimeout = setTimeout(() => { examIndice++; mostrarPreguntaExamen(); examEsperando = false; examTimeout = null; }, 800);
                } else {
                    const idxCorrecta = correcta.charCodeAt(0)-65;
                    const opCorrecta = examPreguntas[examIndice].opciones[idxCorrecta] || "No disponible";
                    fbDiv.innerHTML = `<div class="feedback feedback-wrong">❌ Incorrecto. Era ${correcta}: ${escapeHtml(opCorrecta)}.<br>🔄 Reiniciando examen...</div>`;
                    examEsperando = true;
                    examTimeout = setTimeout(() => { generarExamen(textoExamenActual); examEsperando = false; examTimeout = null; }, 2000);
                }
            });
        });
    }
    
    function mostrarExitoExamen() {
        examContainer.innerHTML = `<div class="success-screen"><div class="success-icon">🎉📚✨</div><div class="success-title">¡Felicidades!</div><p style="margin:15px 0;">Completaste las 10 preguntas sin errores</p><p style="color:#ffd966;">Aciertos: ${aciertosConsecutivos}</p><div style="display:flex;gap:15px;justify-content:center;margin-top:20px;"><button class="primary" id="newExamBtn">📚 Nuevo examen</button><button class="secondary" id="changeTextBtn">📝 Cambiar texto</button></div></div>`;
        document.getElementById('newExamBtn')?.addEventListener('click', () => { examActivo = true; generarExamen(textoExamenActual); });
        document.getElementById('changeTextBtn')?.addEventListener('click', () => { examActivo = false; mostrarPantallaInicialExamen(); });
    }
    
    function escapeHtml(str) { return str.replace(/[&<>]/g, m => ({ '&':'&amp;', '<':'&lt;', '>':'&gt;' }[m])); }
    
    // Inicializar anti-trampas
    configurarAntiTrampas();
    mostrarPantallaInicialExamen();
    
    // Cambio de pestañas manual
    const tabBtns = document.querySelectorAll('.tab-btn');
    const resumenPanel = document.getElementById('resumenPanel');
    const examenPanel = document.getElementById('examenPanel');
    tabBtns.forEach(btn => {
        btn.addEventListener('click', (e) => {
            const tab = btn.getAttribute('data-tab');
            if (examActivo && tab !== 'examen') { bloquearCambioATrampa(); return; }
            tabBtns.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            if (tab === 'resumen') { resumenPanel.classList.add('active-panel'); examenPanel.classList.remove('active-panel'); }
            else { examenPanel.classList.add('active-panel'); resumenPanel.classList.remove('active-panel'); if (!textoExamenActual && !examActivo) mostrarPantallaInicialExamen(); }
        });
    });
</script>
</body>
</html>
