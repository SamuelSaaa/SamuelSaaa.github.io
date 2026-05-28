<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>📚 Resúmenes, Exámenes y Cultura General</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f2b3d, #1a4a6f, #0f2b3d);
            background-size: 200% 200%;
            animation: gradientShift 12s ease infinite;
            min-height: 100vh;
            padding: 20px;
            color: #ffffff;
        }
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .container { max-width: 1200px; margin: 0 auto; }
        h1 { text-align: center; color: #ffd966; font-size: 2rem; margin-bottom: 10px; }
        .subtitle { text-align: center; color: #cbd5e6; margin-bottom: 25px; }

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
            color: #ffffff;
            transition: all 0.2s;
        }
        .tab-btn.active { background: #ffd966; color: #1a2a3a; border-color: #ffd966; }

        .panel {
            display: none;
            background: rgba(15, 35, 55, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 30px;
            animation: fadeIn 0.3s ease;
            border: 1px solid rgba(255, 217, 102, 0.2);
            color: #ffffff;
        }
        .panel.active-panel { display: block; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Sección del resumen */
        .resumen-section {
            transition: all 0.3s ease;
        }
        /* Texto del resumen en blanco durante el examen (invisible) */
        .resumen-texto-oculto {
            opacity: 0;
            pointer-events: none;
            user-select: none;
        }
        
        .aviso-examen {
            background: rgba(255, 100, 100, 0.2);
            border: 1px solid #ff6666;
            border-radius: 20px;
            padding: 15px;
            margin: 15px 0;
            text-align: center;
            color: #ffaaaa;
        }

        .file-drop-area {
            border: 2px dashed rgba(255, 217, 102, 0.4);
            border-radius: 28px;
            padding: 30px;
            text-align: center;
            background: rgba(10, 30, 50, 0.5);
            margin-bottom: 20px;
            cursor: pointer;
            transition: all 0.3s;
            color: #ffffff;
        }
        .file-drop-area.drag-over { border-color: #ffd966; background: rgba(255, 217, 102, 0.1); }
        .file-drop-area.has-file { border-color: #4caf50; background: rgba(76, 175, 80, 0.1); }
        .file-icon { font-size: 3rem; }
        .file-drop-text strong { color: #ffd966; }
        .file-info { display: none; align-items: center; justify-content: center; gap: 15px; margin-top: 15px; }
        .file-info.show { display: flex; }
        .file-name { background: rgba(0,0,0,0.4); padding: 5px 15px; border-radius: 60px; color: #4caf50; }
        .file-clear-btn { background: rgba(255,100,100,0.8); border: none; padding: 5px 12px; border-radius: 30px; cursor: pointer; color: white; }
        .file-select-btn { background: rgba(255,217,102,0.2); border: 1px solid #ffd966; padding: 8px 20px; border-radius: 40px; display: inline-block; margin-top: 10px; cursor: pointer; color: #ffd966; }

        textarea { width: 100%; padding: 12px; border-radius: 20px; border: 1px solid rgba(255,217,102,0.3); background: rgba(0,0,0,0.4); color: #ffffff; margin: 10px 0; }
        textarea::placeholder { color: #8aaec0; }
        
        button.primary { background: linear-gradient(95deg, #ffd966, #ffb347); border: none; padding: 12px 28px; border-radius: 40px; font-weight: bold; cursor: pointer; margin: 5px; color: #1a2a3a; }
        button.secondary { background: rgba(255,255,255,0.1); border: 1px solid #ffd966; color: #ffd966; padding: 10px 24px; border-radius: 40px; cursor: pointer; }
        .result-box { background: rgba(10,30,50,0.6); border-radius: 20px; padding: 20px; margin-top: 20px; color: #ffffff; }
        .loading::before { content: ""; display: inline-block; width: 18px; height: 18px; border: 2px solid #ffd966; border-top-color: transparent; border-radius: 50%; animation: spin 0.8s linear infinite; margin-right: 8px; }
        @keyframes spin { to { transform: rotate(360deg); } }

        .question-card { background: rgba(0,0,0,0.3); border-radius: 24px; padding: 25px; margin: 20px 0; }
        .question-text { font-size: 1.3rem; margin-bottom: 20px; color: #ffffff; }
        .option-btn { background: rgba(25,55,80,0.9); border: 1px solid #ffd966; border-radius: 60px; padding: 12px 18px; margin: 8px; cursor: pointer; display: inline-block; transition: 0.2s; color: #ffffff; }
        .option-btn:hover { background: rgba(255,217,102,0.2); transform: scale(1.02); color: #ffffff; }
        .feedback-correct { background: #2e7d32; padding: 10px; border-radius: 30px; margin-top: 15px; text-align: center; color: #ffffff; }
        .feedback-wrong { background: #c62828; padding: 10px; border-radius: 30px; margin-top: 15px; text-align: center; color: #ffffff; }
        .score-area { background: #1a3a4a; padding: 15px; border-radius: 30px; margin: 20px 0; text-align: center; color: #ffffff; }
        .score-number { font-size: 2rem; font-weight: bold; color: #ffd966; }
        .timer { font-size: 1.5rem; font-weight: bold; color: #ffaa66; font-family: monospace; }
        .timer.warning { color: #ff4444; animation: pulse 0.5s infinite; }
        @keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: 0.5; } }
        
        h3 { color: #ffd966; margin-bottom: 20px; }
        strong { color: #ffd966; }
        
        @media (max-width: 650px) { .panel { padding: 20px; } .question-text { font-size: 1.1rem; } .option-btn { padding: 8px 12px; } }
    </style>
</head>
<body>
<div class="container">
    <h1>📚 Resúmenes, Exámenes y Cultura General</h1>
    <div class="subtitle">🤖 Con IA - Estudia y pon a prueba tus conocimientos</div>

    <div class="tabs">
        <button class="tab-btn active" data-tab="resumen">📄 Resumen y Examen</button>
        <button class="tab-btn" data-tab="cultura">🌍 Cultura General</button>
    </div>

    <!-- Panel Resumen y Examen -->
    <div id="resumenPanel" class="panel active-panel">
        <!-- Sección que se oculta durante el examen -->
        <div id="resumenSection" class="resumen-section">
            <h3>📎 Sube un PDF o pega texto</h3>
            <div class="file-drop-area" id="fileDropArea">
                <div class="file-icon">📄</div>
                <div class="file-drop-text"><strong>📂 Arrastra y suelta tu PDF aquí</strong><br>o haz clic para seleccionar</div>
                <div class="file-info" id="fileInfo"><span class="file-name" id="fileName"></span><button class="file-clear-btn" id="clearFileBtn">✖ Cambiar</button></div>
                <div class="file-select-btn" id="selectFileBtn">🔍 Seleccionar archivo</div>
                <input type="file" id="pdfInput" accept="application/pdf" class="file-input-hidden">
            </div>
            <textarea id="rawTextInput" rows="4" placeholder="O pega aquí el texto que quieras resumir..."></textarea>
            <button class="primary" id="generateSummaryBtn">✨ Generar Resumen con IA</button>

            <div id="summaryResult" class="result-box" style="display:none;">
                <strong>📌 Resumen generado:</strong>
                <div id="summaryText"></div>
                <div class="exam-from-summary-btn" id="examFromSummaryContainer" style="display:none;">
                    <button class="secondary" id="examFromSummaryBtn">📝 Tomar examen sobre este resumen</button>
                </div>
            </div>
        </div>

        <!-- Aviso durante el examen -->
        <div id="avisoExamen" class="aviso-examen" style="display:none;">
            📖 El resumen se ha ocultado para mantener la honestidad del examen.<br>
            Termina el examen para volver a verlo.
        </div>

        <!-- Zona del examen normal -->
        <div id="examArea" style="margin-top: 20px;"></div>
    </div>

    <!-- Panel Cultura General -->
    <div id="culturaPanel" class="panel">
        <div id="culturaApp"></div>
    </div>
</div>

<script>
    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';
    
    const GROQ_API_KEY = "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";
    const MODELO = "llama-3.1-8b-instant";
    
    let ultimoResumen = "";
    let preguntasExamen = [];
    let respuestasUsuario = [];
    let examenEnCurso = false;
    let preguntaActualIndex = 0;
    
    let culturaPreguntas = [];
    let culturaRespuestas = [];
    let culturaIndex = 0;
    let culturaEnCurso = false;
    let tiempoRestante = 300;
    let temporizador = null;
    
    const examArea = document.getElementById('examArea');
    const culturaApp = document.getElementById('culturaApp');
    const resumenSection = document.getElementById('resumenSection');
    const summaryResult = document.getElementById('summaryResult');
    const avisoExamen = document.getElementById('avisoExamen');
    
    function ocultarResumen() {
        // Ocultar el resumen visualmente (opacidad 0)
        if (summaryResult) {
            summaryResult.classList.add('resumen-texto-oculto');
        }
        avisoExamen.style.display = 'block';
    }
    
    function mostrarResumen() {
        if (summaryResult) {
            summaryResult.classList.remove('resumen-texto-oculto');
        }
        avisoExamen.style.display = 'none';
    }
    
    async function llamarGroq(prompt) {
        const response = await fetch("https://api.groq.com/openai/v1/chat/completions", {
            method: "POST",
            headers: { "Content-Type": "application/json", "Authorization": `Bearer ${GROQ_API_KEY}` },
            body: JSON.stringify({
                model: MODELO,
                messages: [{ role: "system", content: "Eres un asistente educativo útil. Respondes en español de forma clara." }, { role: "user", content: prompt }],
                temperature: 0.7,
                max_tokens: 2000
            })
        });
        if (!response.ok) throw new Error(`Error ${response.status}`);
        const data = await response.json();
        return data.choices[0].message.content;
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
            } else if (actual && /^[A-D]\)/.test(linea)) {
                actual.opciones.push(linea.substring(2).trim());
            } else if (actual && linea.toLowerCase().startsWith('respuesta:')) {
                let letra = linea.substring('respuesta:'.length).trim().toUpperCase();
                actual.respuesta = letra[0] || '';
            }
        }
        if (actual && actual.opciones?.length === 4) preguntas.push(actual);
        return preguntas;
    }
    
    async function extraerTextoPDF(file) {
        const arrayBuffer = await file.arrayBuffer();
        const pdf = await pdfjsLib.getDocument({ data: arrayBuffer }).promise;
        let texto = "";
        for (let i = 1; i <= pdf.numPages; i++) {
            const pagina = await pdf.getPage(i);
            const contenido = await pagina.getTextContent();
            texto += contenido.items.map(item => item.str).join(" ") + "\n";
        }
        return texto;
    }
    
    // Interactividad archivo
    const fileDropArea = document.getElementById('fileDropArea');
    const pdfInput = document.getElementById('pdfInput');
    const fileInfo = document.getElementById('fileInfo');
    const fileNameSpan = document.getElementById('fileName');
    const selectFileBtn = document.getElementById('selectFileBtn');
    const clearFileBtn = document.getElementById('clearFileBtn');
    
    function updateFileUI(file) {
        if (file) {
            fileNameSpan.textContent = `📄 ${file.name} (${(file.size/1024).toFixed(1)} KB)`;
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
    fileDropArea.addEventListener('dragover', (e) => { e.preventDefault(); fileDropArea.classList.add('drag-over'); });
    fileDropArea.addEventListener('dragleave', () => fileDropArea.classList.remove('drag-over'));
    fileDropArea.addEventListener('drop', (e) => {
        e.preventDefault();
        fileDropArea.classList.remove('drag-over');
        const file = e.dataTransfer.files[0];
        if (file && file.type === 'application/pdf') updateFileUI(file);
        else alert('Solo PDF');
    });
    fileDropArea.addEventListener('click', () => pdfInput.click());
    
    // Generar resumen
    document.getElementById('generateSummaryBtn').addEventListener('click', async () => {
        const pdfFile = pdfInput.files[0];
        const manualText = document.getElementById('rawTextInput').value;
        let textoFuente = "";
        if (pdfFile) {
            try { textoFuente = await extraerTextoPDF(pdfFile); }
            catch(e) { alert("Error PDF: " + e.message); return; }
        } else if (manualText.trim()) { textoFuente = manualText; }
        else { alert("Sube un PDF o texto"); return; }
        
        const summaryDiv = document.getElementById('summaryResult');
        const summaryTextDiv = document.getElementById('summaryText');
        const examBtnContainer = document.getElementById('examFromSummaryContainer');
        summaryDiv.style.display = 'block';
        summaryTextDiv.innerHTML = '<span class="loading"></span> Generando resumen...';
        examBtnContainer.style.display = 'none';
        
        try {
            const prompt = `Resume el siguiente texto de forma clara y concisa en español (máximo 300 palabras):\n\n${textoFuente.substring(0,8000)}`;
            const resumen = await llamarGroq(prompt);
            summaryTextDiv.innerHTML = resumen.replace(/\n/g,'<br>');
            ultimoResumen = resumen;
            examBtnContainer.style.display = 'block';
        } catch(e) { summaryTextDiv.innerHTML = `❌ Error: ${e.message}`; }
    });
    
    document.getElementById('examFromSummaryBtn').addEventListener('click', async () => {
        if (!ultimoResumen) { alert("Genera un resumen primero"); return; }
        await iniciarExamenNormal(ultimoResumen);
    });
    
    async function iniciarExamenNormal(textoBase) {
        examArea.innerHTML = '<div class="loading"></div> Generando examen de 10 preguntas...';
        try {
            const prompt = `Genera un examen de 10 preguntas de opción múltiple (A,B,C,D) basado en este texto. Usa este formato exacto:
Pregunta 1: [texto]
A) [opcion]
B) [opcion]
C) [opcion]
D) [opcion]
Respuesta: X
(Repite hasta 10)
Texto: ${textoBase.substring(0,6000)}`;
            const respuesta = await llamarGroq(prompt);
            preguntasExamen = parsearPreguntas(respuesta);
            if (preguntasExamen.length < 8) throw new Error("No se generaron suficientes preguntas");
            preguntasExamen = preguntasExamen.slice(0,10);
            respuestasUsuario = [];
            examenEnCurso = true;
            preguntaActualIndex = 0;
            ocultarResumen();
            mostrarPreguntaNormal();
        } catch(e) { examArea.innerHTML = `<div class="error-msg">❌ Error: ${e.message}<br><button class="primary" onclick="location.reload()">Reintentar</button></div>`; }
    }
    
    function mostrarPreguntaNormal() {
        if (preguntaActualIndex >= preguntasExamen.length) {
            mostrarResultadosNormal();
            return;
        }
        const q = preguntasExamen[preguntaActualIndex];
        examArea.innerHTML = `
            <div class="question-card">
                <div style="margin-bottom:15px; color:#ffd966;">📋 Pregunta ${preguntaActualIndex+1} de ${preguntasExamen.length}</div>
                <div class="question-text">${escapeHtml(q.texto)}</div>
                <div id="opcionesNormal">${q.opciones.map((opt,idx) => `<div class="option-btn" data-letra="${['A','B','C','D'][idx]}">${['A','B','C','D'][idx]}) ${escapeHtml(opt)}</div>`).join('')}</div>
                <div id="feedbackNormal"></div>
            </div>`;
        document.querySelectorAll('#opcionesNormal .option-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                if (!examenEnCurso) return;
                const letraSel = btn.getAttribute('data-letra');
                const correcta = preguntasExamen[preguntaActualIndex].respuesta;
                const idxCorrecta = correcta.charCodeAt(0)-65;
                const textoCorrecto = preguntasExamen[preguntaActualIndex].opciones[idxCorrecta];
                respuestasUsuario.push({
                    pregunta: preguntasExamen[preguntaActualIndex].texto,
                    seleccionada: letraSel,
                    correcta: correcta,
                    textoCorrecto: textoCorrecto,
                    opciones: [...preguntasExamen[preguntaActualIndex].opciones]
                });
                const fb = document.getElementById('feedbackNormal');
                if (letraSel === correcta) {
                    fb.innerHTML = '<div class="feedback-correct">✅ ¡Correcta! Avanzando...</div>';
                } else {
                    fb.innerHTML = `<div class="feedback-wrong">❌ Incorrecta. Era ${correcta}: ${escapeHtml(textoCorrecto)}. Avanzando...</div>`;
                }
                setTimeout(() => {
                    preguntaActualIndex++;
                    mostrarPreguntaNormal();
                }, 1200);
            });
        });
    }
    
    function mostrarResultadosNormal() {
        const total = respuestasUsuario.length;
        const correctas = respuestasUsuario.filter(r => r.seleccionada === r.correcta).length;
        const nota = (correctas / total) * 5;
        examenEnCurso = false;
        
        let preguntasHtml = '<div style="font-family: Arial; padding: 20px;"><h1>📚 Examen - Repaso</h1><h2>Preguntas y Respuestas Correctas</h2>';
        respuestasUsuario.forEach((r, i) => {
            preguntasHtml += `<div style="margin-bottom: 25px; border-bottom: 1px solid #ccc; padding-bottom: 10px;">
                <p><strong>${i+1}. ${escapeHtml(r.pregunta)}</strong></p>
                <p>${r.opciones.map((opt,idx) => `${['A','B','C','D'][idx]}) ${escapeHtml(opt)}`).join(' | ')}</p>
                <p style="color: green;"><strong>✅ Respuesta correcta: ${r.correcta}) ${escapeHtml(r.textoCorrecto)}</strong></p>
                <p>Tu respuesta: ${r.seleccionada}) ${escapeHtml(r.opciones[r.seleccionada.charCodeAt(0)-65] || 'No seleccionada')}</p>
            </div>`;
        });
        preguntasHtml += `<p style="margin-top: 20px;"><strong>Calificación: ${nota.toFixed(1)} / 5.0 (${correctas}/${total} correctas)</strong></p></div>`;
        
        examArea.innerHTML = `
            <div class="score-area">
                <div style="font-size:1.2rem;">📊 Resultado del Examen</div>
                <div class="score-number">${nota.toFixed(1)} / 5.0</div>
                <div>✅ Correctas: ${correctas} | ❌ Incorrectas: ${total - correctas}</div>
                <div style="margin-top: 15px;">
                    <button class="primary" id="pdfBtn">📄 Generar PDF con todas las preguntas</button>
                    <button class="secondary" id="newExamBtn">📝 Nuevo examen (mismo texto)</button>
                </div>
            </div>
            <div id="pdfContent" style="display:none;">${preguntasHtml}</div>
        `;
        
        mostrarResumen();
        
        document.getElementById('pdfBtn')?.addEventListener('click', () => {
            const element = document.getElementById('pdfContent');
            html2pdf().from(element).set({ filename: `examen_repaso_${new Date().toISOString().slice(0,19)}.pdf`, margin: 1 }).save();
        });
        document.getElementById('newExamBtn')?.addEventListener('click', () => iniciarExamenNormal(ultimoResumen));
    }
    
    async function iniciarCulturaGeneral() {
        culturaApp.innerHTML = '<div class="loading"></div> Generando preguntas de cultura general...';
        try {
            const prompt = `Genera 10 preguntas de cultura general variadas (historia, ciencia, arte, geografía, entretenimiento). Cada pregunta debe ser de opción múltiple (A,B,C,D) con una respuesta correcta. Usa este formato exacto:
Pregunta 1: [texto]
A) [opcion]
B) [opcion]
C) [opcion]
D) [opcion]
Respuesta: X
(Repite hasta 10)
Las preguntas deben ser desafiantes pero justas.`;
            const respuesta = await llamarGroq(prompt);
            culturaPreguntas = parsearPreguntas(respuesta);
            if (culturaPreguntas.length < 8) throw new Error("No se generaron suficientes");
            culturaPreguntas = culturaPreguntas.slice(0,10);
            culturaRespuestas = [];
            culturaIndex = 0;
            culturaEnCurso = true;
            tiempoRestante = 300;
            iniciarTemporizador();
            mostrarPreguntaCultura();
        } catch(e) { culturaApp.innerHTML = `<div class="error-msg">❌ Error: ${e.message}<br><button class="primary" onclick="iniciarCulturaGeneral()">Reintentar</button></div>`; }
    }
    
    function iniciarTemporizador() {
        if (temporizador) clearInterval(temporizador);
        temporizador = setInterval(() => {
            if (!culturaEnCurso) return;
            if (tiempoRestante <= 0) {
                clearInterval(temporizador);
                finalizarCulturaGeneral();
            } else {
                tiempoRestante--;
                actualizarTimerCultura();
            }
        }, 1000);
    }
    
    function actualizarTimerCultura() {
        const minutos = Math.floor(tiempoRestante / 60);
        const segundos = tiempoRestante % 60;
        const timerDiv = document.getElementById('culturaTimer');
        if (timerDiv) {
            timerDiv.innerHTML = `⏱️ ${minutos.toString().padStart(2,'0')}:${segundos.toString().padStart(2,'0')}`;
            if (tiempoRestante < 60) timerDiv.classList.add('warning');
            else timerDiv.classList.remove('warning');
        }
    }
    
    function mostrarPreguntaCultura() {
        if (culturaIndex >= culturaPreguntas.length) {
            finalizarCulturaGeneral();
            return;
        }
        const q = culturaPreguntas[culturaIndex];
        culturaApp.innerHTML = `
            <div style="display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; margin-bottom:20px;">
                <div style="color:#ffd966;">📋 Pregunta ${culturaIndex+1} de ${culturaPreguntas.length}</div>
                <div id="culturaTimer" class="timer">⏱️ 05:00</div>
            </div>
            <div class="question-card">
                <div class="question-text">${escapeHtml(q.texto)}</div>
                <div id="opcionesCultura">${q.opciones.map((opt,idx) => `<div class="option-btn" data-letra="${['A','B','C','D'][idx]}">${['A','B','C','D'][idx]}) ${escapeHtml(opt)}</div>`).join('')}</div>
                <div id="feedbackCultura"></div>
            </div>
        `;
        actualizarTimerCultura();
        document.querySelectorAll('#opcionesCultura .option-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                if (!culturaEnCurso) return;
                const letraSel = btn.getAttribute('data-letra');
                const correcta = culturaPreguntas[culturaIndex].respuesta;
                const idxCorrecta = correcta.charCodeAt(0)-65;
                const textoCorrecto = culturaPreguntas[culturaIndex].opciones[idxCorrecta];
                culturaRespuestas.push({
                    pregunta: culturaPreguntas[culturaIndex].texto,
                    seleccionada: letraSel,
                    correcta: correcta,
                    textoCorrecto: textoCorrecto,
                    opciones: [...culturaPreguntas[culturaIndex].opciones]
                });
                const fb = document.getElementById('feedbackCultura');
                if (letraSel === correcta) {
                    fb.innerHTML = '<div class="feedback-correct">✅ ¡Correcta! Avanzando...</div>';
                } else {
                    fb.innerHTML = `<div class="feedback-wrong">❌ Incorrecta. Era ${correcta}: ${escapeHtml(textoCorrecto)}. Avanzando...</div>`;
                }
                setTimeout(() => {
                    culturaIndex++;
                    mostrarPreguntaCultura();
                }, 1200);
            });
        });
    }
    
    function finalizarCulturaGeneral() {
        if (temporizador) clearInterval(temporizador);
        culturaEnCurso = false;
        const total = culturaRespuestas.length;
        const correctas = culturaRespuestas.filter(r => r.seleccionada === r.correcta).length;
        const nota = (correctas / total) * 5;
        culturaApp.innerHTML = `
            <div class="score-area">
                <div style="font-size:1.2rem;">🌍 Resultado de Cultura General</div>
                <div class="score-number">${nota.toFixed(1)} / 5.0</div>
                <div>✅ Correctas: ${correctas} | ❌ Incorrectas: ${total - correctas}</div>
                <div style="margin-top: 15px;">
                    <button class="primary" id="nuevoCulturaBtn">🎲 Nuevo test de Cultura General</button>
                </div>
            </div>
        `;
        document.getElementById('nuevoCulturaBtn')?.addEventListener('click', () => iniciarCulturaGeneral());
    }
    
    function escapeHtml(str) { return str.replace(/[&<>]/g, m => ({ '&':'&amp;', '<':'&lt;', '>':'&gt;' }[m])); }
    
    // Pestañas
    const tabBtns = document.querySelectorAll('.tab-btn');
    const resumenPanel = document.getElementById('resumenPanel');
    const culturaPanel = document.getElementById('culturaPanel');
    
    tabBtns.forEach(btn => {
        btn.addEventListener('click', () => {
            tabBtns.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            const tab = btn.getAttribute('data-tab');
            if (tab === 'resumen') {
                resumenPanel.classList.add('active-panel');
                culturaPanel.classList.remove('active-panel');
                if (!examenEnCurso) mostrarResumen();
            } else {
                culturaPanel.classList.add('active-panel');
                resumenPanel.classList.remove('active-panel');
                if (!culturaEnCurso && culturaApp.innerHTML === '') iniciarCulturaGeneral();
            }
        });
    });
    
    iniciarCulturaGeneral();
</script>
</body>
</html>
