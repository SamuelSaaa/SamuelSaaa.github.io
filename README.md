<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>.</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background: linear-gradient(135deg, #0f2b3d, #1a4a6f, #0f2b3d);
            background-size: 200% 200%;
            animation: gradientShift 12s ease infinite;
            min-height: 100vh;
            padding: 12px;
        }
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .container { max-width: 800px; margin: 0 auto; }
        
        .auth-screen {
            background: rgba(15, 35, 55, 0.95);
            backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 30px 20px;
            text-align: center;
            border: 1px solid rgba(255, 217, 102, 0.3);
        }
        .auth-screen h2 { color: #ffd966; margin-bottom: 20px; font-size: 1.8rem; }
        .auth-screen input {
            width: 100%;
            padding: 14px;
            margin: 10px 0;
            border-radius: 30px;
            border: 1px solid rgba(255,217,102,0.3);
            background: rgba(0,0,0,0.4);
            color: white;
            font-size: 16px;
        }
        .auth-screen button {
            background: linear-gradient(95deg, #ffd966, #ffb347);
            border: none;
            padding: 14px 24px;
            border-radius: 40px;
            font-weight: bold;
            font-size: 16px;
            cursor: pointer;
            margin: 8px 5px;
        }
        .auth-screen .switch-btn {
            background: transparent;
            border: 1px solid #ffd966;
            color: #ffd966;
        }
        .error-msg { color: #ff8888; margin: 10px 0; }
        
        .user-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(15, 35, 55, 0.8);
            backdrop-filter: blur(8px);
            border-radius: 60px;
            padding: 10px 16px;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }
        .user-info { color: #ffd966; font-weight: bold; font-size: 14px; }
        .admin-badge { background: #ff4444; color: white; padding: 4px 10px; border-radius: 20px; font-size: 0.7rem; margin-left: 8px; }
        .logout-btn { background: rgba(255,100,100,0.3); border: 1px solid #ff6666; padding: 8px 16px; border-radius: 30px; cursor: pointer; color: #ffaaaa; font-size: 14px; }
        
        .tabs {
            display: flex;
            gap: 8px;
            justify-content: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        .tab-btn {
            background: rgba(15, 35, 55, 0.8);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 217, 102, 0.3);
            padding: 10px 20px;
            font-size: 14px;
            font-weight: bold;
            border-radius: 40px;
            cursor: pointer;
            color: #cbd5e6;
            transition: all 0.2s;
        }
        .tab-btn.active { background: #ffd966; color: #1a2a3a; border-color: #ffd966; }

        .panel {
            display: none;
            background: rgba(15, 35, 55, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 28px;
            padding: 20px;
            border: 1px solid rgba(255, 217, 102, 0.2);
        }
        .panel.active-panel { display: block; }

        .aviso-examen {
            background: rgba(255, 100, 100, 0.2);
            border: 1px solid #ff6666;
            border-radius: 20px;
            padding: 12px;
            margin: 15px 0;
            text-align: center;
            color: #ffaaaa;
            font-size: 14px;
        }

        .examenes-lista { margin-top: 20px; border-top: 2px solid rgba(255,217,102,0.3); padding-top: 15px; }
        .examen-card {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 12px;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 10px;
            border: 1px solid rgba(255,217,102,0.2);
        }
        .examen-info { flex: 1; }
        .examen-titulo { font-weight: bold; color: #ffd966; font-size: 14px; }
        .examen-fecha { font-size: 0.7rem; color: #8aaec0; }
        .examen-usuario { font-size: 0.65rem; color: #ffaa66; }
        .btn-icon {
            background: rgba(255,255,255,0.1);
            border: none;
            padding: 8px 14px;
            border-radius: 30px;
            cursor: pointer;
            color: #ffd966;
            font-size: 13px;
        }
        .btn-icon.admin-delete { color: #ff8888; border: 1px solid #ff8888; }
        .empty-message { color: #8aaec0; text-align: center; padding: 30px; }

        .file-drop-area {
            border: 2px dashed rgba(255, 217, 102, 0.4);
            border-radius: 24px;
            padding: 20px;
            text-align: center;
            background: rgba(10, 30, 50, 0.5);
            margin-bottom: 15px;
            cursor: pointer;
        }
        .file-drop-area.drag-over { border-color: #ffd966; background: rgba(255, 217, 102, 0.1); }
        .file-icon { font-size: 2.5rem; }
        .file-info { display: none; align-items: center; justify-content: center; gap: 10px; margin-top: 10px; flex-wrap: wrap; }
        .file-info.show { display: flex; }
        .file-name { background: rgba(0,0,0,0.4); padding: 5px 12px; border-radius: 30px; color: #4caf50; font-size: 12px; }
        .file-clear-btn { background: rgba(255,100,100,0.8); border: none; padding: 5px 12px; border-radius: 30px; cursor: pointer; color: white; font-size: 12px; }
        .file-select-btn { background: rgba(255,217,102,0.2); border: 1px solid #ffd966; padding: 8px 18px; border-radius: 40px; display: inline-block; margin-top: 8px; cursor: pointer; color: #ffd966; font-size: 14px; }
        .file-input-hidden { display: none; }

        textarea { 
            width: 100%; 
            padding: 12px; 
            border-radius: 20px; 
            border: 1px solid rgba(255,217,102,0.3); 
            background: rgba(0,0,0,0.4); 
            color: white; 
            margin: 10px 0; 
            font-size: 14px;
        }
        button.primary { 
            background: linear-gradient(95deg, #ffd966, #ffb347); 
            border: none; 
            padding: 12px 20px; 
            border-radius: 40px; 
            font-weight: bold; 
            font-size: 16px;
            cursor: pointer; 
            margin: 5px; 
            color: #1a2a3a; 
            width: 100%;
        }
        button.secondary { 
            background: rgba(255,255,255,0.1); 
            border: 1px solid #ffd966; 
            color: #ffd966; 
            padding: 10px 20px; 
            border-radius: 40px; 
            font-size: 14px;
            cursor: pointer; 
            width: 100%;
        }
        .result-box { background: rgba(10,30,50,0.6); border-radius: 20px; padding: 15px; margin-top: 15px; color: white; font-size: 14px; }
        .loading::before { content: ""; display: inline-block; width: 18px; height: 18px; border: 2px solid #ffd966; border-top-color: transparent; border-radius: 50%; animation: spin 0.8s linear infinite; margin-right: 8px; }
        @keyframes spin { to { transform: rotate(360deg); } }

        .question-card { background: rgba(0,0,0,0.3); border-radius: 24px; padding: 20px; margin: 15px 0; }
        .question-text { font-size: 1.2rem; margin-bottom: 20px; color: white; line-height: 1.4; }
        .option-btn {
            background: rgba(25,55,80,0.9);
            border: 1px solid #ffd966;
            border-radius: 50px;
            padding: 14px 16px;
            margin: 8px 0;
            cursor: pointer;
            display: block;
            width: 100%;
            text-align: left;
            color: white;
            font-weight: 500;
            font-size: 15px;
            transition: 0.2s;
        }
        .option-btn:active { background: rgba(255,217,102,0.3); transform: scale(0.98); }
        .feedback-correct { background: #2e7d32; padding: 12px; border-radius: 30px; margin-top: 15px; text-align: center; color: white; font-size: 14px; }
        .feedback-wrong { background: #c62828; padding: 12px; border-radius: 30px; margin-top: 15px; text-align: center; color: white; font-size: 14px; }
        .score-area { background: #1a3a4a; padding: 20px; border-radius: 28px; margin: 20px 0; text-align: center; color: white; }
        .score-number { font-size: 2.2rem; font-weight: bold; color: #ffd966; }
        .timer { font-size: 1.3rem; font-weight: bold; color: #ffaa66; font-family: monospace; }
        .timer.warning { color: #ff4444; animation: pulse 0.5s infinite; }
        @keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: 0.5; } }
        
        @media (max-width: 500px) {
            .question-text { font-size: 1rem; }
            .option-btn { padding: 12px 14px; font-size: 14px; }
            .tab-btn { padding: 8px 14px; font-size: 12px; }
        }
    </style>
</head>
<body>
<div class="container" id="app"></div>

<script>
    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';
    
    // ========== CONFIGURACIÓN SUPABASE ==========
    const SUPABASE_URL = "https://eiaojqfnqvcpvfywhdle.supabase.co";
    const SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImVpYW9qcWZucXZjcHZmeXdoZGxlIiwicm9sZSI6ImFub24iLCJpYXQiOjE3Nzk5OTEzNTAsImV4cCI6MjA5NTU2NzM1MH0.zHZSVPXgDoeN1-hWIG6l0PunUOZ1YYTxC103aP2c1M4";
    
    // ========== CONFIGURACIÓN APIS IA ==========
    const GROQ_API_KEY = "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";
    const CEREBRAS_API_KEY = "csk-mmcetp8ndr55em9v83833hd8xvx495vwkpcvkmm8h2xcp8nc";
    
    let proveedorActual = 0;
    let usuarioActual = null;
    let esAdmin = false;
    let examenActualEsNuevo = false;
    
    // ========== FUNCIONES SUPABASE ==========
    async function obtenerExamenesCloud() {
        try {
            const response = await fetch(`${SUPABASE_URL}/rest/v1/examenes?select=*&order=created_at.desc`, {
                headers: { "apikey": SUPABASE_ANON_KEY, "Authorization": `Bearer ${SUPABASE_ANON_KEY}` }
            });
            if (!response.ok) return [];
            return await response.json();
        } catch(e) { return []; }
    }
    
    async function guardarExamenCloud(titulo, resumen, preguntas, usuario) {
        try {
            await fetch(`${SUPABASE_URL}/rest/v1/examenes`, {
                method: "POST",
                headers: { "Content-Type": "application/json", "apikey": SUPABASE_ANON_KEY, "Authorization": `Bearer ${SUPABASE_ANON_KEY}` },
                body: JSON.stringify({ titulo: titulo.substring(0, 50), resumen, preguntas, usuario, fecha: new Date().toLocaleString() })
            });
        } catch(e) {}
    }
    
    async function eliminarExamenCloud(id) {
        try {
            await fetch(`${SUPABASE_URL}/rest/v1/examenes?id=eq.${id}`, {
                method: "DELETE",
                headers: { "apikey": SUPABASE_ANON_KEY, "Authorization": `Bearer ${SUPABASE_ANON_KEY}` }
            });
        } catch(e) {}
    }
    
    // ========== FUNCIONES PDF MEJORADAS ==========
    async function generarPDF(contenidoHTML, nombreArchivo) {
        if (!contenidoHTML || contenidoHTML.trim() === '') {
            alert("No hay contenido para generar el PDF");
            return false;
        }
        
        // Crear un elemento temporal
        const tempDiv = document.createElement('div');
        tempDiv.innerHTML = contenidoHTML;
        tempDiv.style.position = 'absolute';
        tempDiv.style.left = '-9999px';
        tempDiv.style.top = '0';
        tempDiv.style.width = '800px';
        tempDiv.style.backgroundColor = 'white';
        tempDiv.style.padding = '20px';
        tempDiv.style.fontFamily = 'Arial, sans-serif';
        document.body.appendChild(tempDiv);
        
        try {
            await html2pdf().from(tempDiv).set({
                margin: 0.5,
                filename: nombreArchivo,
                image: { type: 'jpeg', quality: 0.98 },
                html2canvas: { scale: 2, useCORS: true, logging: false },
                jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
            }).save();
            return true;
        } catch(e) {
            console.error("Error PDF:", e);
            alert("Error al generar el PDF: " + e.message);
            return false;
        } finally {
            document.body.removeChild(tempDiv);
        }
    }
    
    // ========== SISTEMA DE USUARIOS ==========
    async function hashPassword(password) {
        const encoder = new TextEncoder();
        const data = encoder.encode(password);
        const hash = await crypto.subtle.digest('SHA-256', data);
        return Array.from(new Uint8Array(hash)).map(b => b.toString(16).padStart(2, '0')).join('');
    }
    
    async function registrarUsuario(username, password) {
        const usernameLimpio = username.replace(/[^a-zA-Z0-9_]/g, '');
        if (usernameLimpio !== username) return { success: false, error: "Usuario solo letras, números y _" };
        if (password.length < 4) return { success: false, error: "Mínimo 4 caracteres" };
        const usuarios = JSON.parse(localStorage.getItem('usuarios') || '{}');
        if (usuarios[usernameLimpio]) return { success: false, error: "Usuario ya existe" };
        usuarios[usernameLimpio] = { hash: await hashPassword(password), esAdmin: false };
        localStorage.setItem('usuarios', JSON.stringify(usuarios));
        return { success: true };
    }
    
    async function loginUsuario(username, password) {
        const usernameLimpio = username.replace(/[^a-zA-Z0-9_]/g, '');
        if (usernameLimpio !== username) return { success: false, error: "Usuario no válido" };
        const usuarios = JSON.parse(localStorage.getItem('usuarios') || '{}');
        const user = usuarios[usernameLimpio];
        if (!user) return { success: false, error: "Usuario no existe" };
        if (user.hash !== await hashPassword(password)) return { success: false, error: "Contraseña incorrecta" };
        return { success: true, esAdmin: user.esAdmin || false };
    }
    
    // ========== FUNCIONES IA ==========
    async function llamarGroq(prompt) {
        const response = await fetch("https://api.groq.com/openai/v1/chat/completions", {
            method: "POST", headers: { "Content-Type": "application/json", "Authorization": `Bearer ${GROQ_API_KEY}` },
            body: JSON.stringify({ model: "llama-3.1-8b-instant", messages: [{ role: "system", content: "Eres un asistente educativo útil." }, { role: "user", content: prompt }], temperature: 0.5, max_tokens: 1500 })
        });
        if (response.status === 429) throw new Error("Rate limit");
        const data = await response.json();
        return data.choices[0].message.content;
    }
    
    async function llamarCerebras(prompt) {
        const response = await fetch("https://api.cerebras.ai/v1/chat/completions", {
            method: "POST", headers: { "Content-Type": "application/json", "Authorization": `Bearer ${CEREBRAS_API_KEY}` },
            body: JSON.stringify({ model: "llama3.1-8b", messages: [{ role: "system", content: "Eres un asistente educativo útil." }, { role: "user", content: prompt }], temperature: 0.5, max_tokens: 1500 })
        });
        if (response.status === 429) throw new Error("Rate limit");
        const data = await response.json();
        return data.choices[0].message.content;
    }
    
    async function llamarIA(prompt) {
        const proveedores = [llamarGroq, llamarCerebras];
        for (let i = 0; i < proveedores.length; i++) {
            try { return await proveedores[(proveedorActual + i) % proveedores.length](prompt); } catch(e) { continue; }
        }
        throw new Error("Todos los proveedores fallaron");
    }
    
    // ========== VARIABLES GLOBALES ==========
    let ultimoResumen = "", preguntasExamen = [], respuestasUsuario = [], examenEnCurso = false, preguntaActualIndex = 0;
    let culturaPreguntas = [], culturaRespuestas = [], culturaIndex = 0, culturaEnCurso = false, tiempoRestante = 300, temporizador = null;
    let examenesCloud = [];
    const app = document.getElementById('app');
    
    // ========== RENDERIZADO ==========
    function mostrarPantallaLogin() {
        app.innerHTML = `
            <div class="auth-screen">
                <h2>📚 Bienvenido</h2>
                <div id="loginForm">
                    <input type="text" id="loginUser" placeholder="Usuario">
                    <input type="password" id="loginPass" placeholder="Contraseña">
                    <div id="loginError" class="error-msg"></div>
                    <button id="btnLogin">🔓 Iniciar sesión</button>
                    <button id="btnMostrarRegistro" class="switch-btn">📝 Crear cuenta</button>
                </div>
                <div id="registerForm" style="display:none;">
                    <input type="text" id="regUser" placeholder="Nuevo usuario">
                    <input type="password" id="regPass" placeholder="Contraseña (mínimo 4)">
                    <div id="regError" class="error-msg"></div>
                    <button id="btnRegister">✅ Registrarse</button>
                    <button id="btnMostrarLogin" class="switch-btn">◀ Volver</button>
                </div>
            </div>
        `;
        document.getElementById('btnLogin')?.addEventListener('click', async () => {
            const user = document.getElementById('loginUser').value.trim();
            const pass = document.getElementById('loginPass').value;
            const result = await loginUsuario(user, pass);
            if (result.success) { usuarioActual = user; esAdmin = result.esAdmin; mostrarPantallaPrincipal(); }
            else document.getElementById('loginError').innerText = result.error;
        });
        document.getElementById('btnMostrarRegistro')?.addEventListener('click', () => { document.getElementById('loginForm').style.display = 'none'; document.getElementById('registerForm').style.display = 'block'; });
        document.getElementById('btnMostrarLogin')?.addEventListener('click', () => { document.getElementById('registerForm').style.display = 'none'; document.getElementById('loginForm').style.display = 'block'; });
        document.getElementById('btnRegister')?.addEventListener('click', async () => {
            const user = document.getElementById('regUser').value.trim();
            const pass = document.getElementById('regPass').value;
            const result = await registrarUsuario(user, pass);
            if (result.success) {
                document.getElementById('registerForm').style.display = 'none';
                document.getElementById('loginForm').style.display = 'block';
                document.getElementById('loginUser').value = user;
                document.getElementById('loginError').innerText = "✅ Registrado. Inicia sesión.";
            } else document.getElementById('regError').innerText = result.error;
        });
    }
    
    async function mostrarPantallaPrincipal() {
        examenesCloud = await obtenerExamenesCloud();
        app.innerHTML = `
            <div class="user-header">
                <div class="user-info">👤 ${usuarioActual} ${esAdmin ? '<span class="admin-badge">👑 ADMIN</span>' : ''}</div>
                <button class="logout-btn" id="logoutBtn">🚪 Cerrar sesión</button>
            </div>
            <div class="tabs">
                <button class="tab-btn active" data-tab="resumen">📄 Resumen</button>
                <button class="tab-btn" data-tab="historial">📋 Historial</button>
                <button class="tab-btn" data-tab="cultura">🌍 Cultura</button>
            </div>
            <div id="resumenPanel" class="panel active-panel">
                <div id="resumenSection">
                    <h3>📎 Sube un PDF o pega texto</h3>
                    <div class="file-drop-area" id="fileDropArea">
                        <div class="file-icon">📄</div>
                        <div class="file-drop-text"><strong>📂 Toca para seleccionar</strong><br>o arrastra un PDF</div>
                        <div class="file-info" id="fileInfo"><span class="file-name" id="fileName"></span><button class="file-clear-btn" id="clearFileBtn">✖</button></div>
                        <div class="file-select-btn" id="selectFileBtn">🔍 Seleccionar archivo</div>
                        <input type="file" id="pdfInput" accept="application/pdf" class="file-input-hidden">
                    </div>
                    <textarea id="rawTextInput" rows="4" placeholder="O pega texto..."></textarea>
                    <button class="primary" id="generateSummaryBtn">✨ Generar Resumen</button>
                    <div id="summaryResult" class="result-box" style="display:none;">
                        <strong>📌 Resumen:</strong><div id="summaryText"></div>
                        <div id="examFromSummaryContainer" style="display:none; text-align:center; margin-top:15px;">
                            <button class="secondary" id="examFromSummaryBtn">📝 Tomar examen</button>
                        </div>
                    </div>
                </div>
                <div id="avisoExamen" class="aviso-examen" style="display:none;">📖 El resumen se ha ocultado</div>
                <div id="examArea" style="margin-top:20px;"></div>
            </div>
            <div id="historialPanel" class="panel"><div id="listaExamenes" class="examenes-lista"></div></div>
            <div id="culturaPanel" class="panel"><div id="culturaApp"></div></div>
        `;
        document.getElementById('logoutBtn')?.addEventListener('click', () => { usuarioActual = null; mostrarPantallaLogin(); });
        inicializarSubidaArchivos();
        inicializarResumen();
        actualizarListaExamenes();
        iniciarCulturaGeneral();
        document.querySelectorAll('.tab-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                const tab = btn.getAttribute('data-tab');
                document.querySelectorAll('.panel').forEach(p => p.classList.remove('active-panel'));
                document.getElementById(`${tab}Panel`).classList.add('active-panel');
                if (tab === 'historial') actualizarListaExamenes();
                if (tab === 'cultura' && !culturaEnCurso && document.getElementById('culturaApp').innerHTML === '') iniciarCulturaGeneral();
            });
        });
    }
    
    async function actualizarListaExamenes() {
        const contenedor = document.getElementById('listaExamenes');
        if (!contenedor) return;
        examenesCloud = await obtenerExamenesCloud();
        if (examenesCloud.length === 0) { contenedor.innerHTML = '<div class="empty-message">📭 No hay exámenes</div>'; return; }
        contenedor.innerHTML = examenesCloud.map(examen => `
            <div class="examen-card">
                <div class="examen-info">
                    <div class="examen-titulo">📖 ${escapeHtml(examen.titulo)}</div>
                    <div class="examen-fecha">📅 ${examen.fecha}</div>
                    <div class="examen-usuario">👤 ${escapeHtml(examen.usuario)}</div>
                </div>
                <div class="examen-actions">
                    <button class="btn-icon tomar-examen" data-id="${examen.id}">📝 Tomar</button>
                    ${esAdmin ? `<button class="btn-icon admin-delete eliminar-examen" data-id="${examen.id}">🗑️ Eliminar</button>` : ''}
                </div>
            </div>
        `).join('');
        document.querySelectorAll('.tomar-examen').forEach(btn => {
            btn.addEventListener('click', () => { const examen = examenesCloud.find(e => e.id == btn.dataset.id); if(examen) iniciarExamenGuardado(examen); });
        });
        if(esAdmin) document.querySelectorAll('.eliminar-examen').forEach(btn => {
            btn.addEventListener('click', async () => { await eliminarExamenCloud(btn.dataset.id); actualizarListaExamenes(); });
        });
    }
    
    function iniciarExamenGuardado(examen) {
        preguntasExamen = examen.preguntas;
        respuestasUsuario = [];
        examenEnCurso = true;
        preguntaActualIndex = 0;
        ultimoResumen = examen.resumen || "";
        examenActualEsNuevo = false;
        ocultarResumen();
        mostrarPreguntaNormal();
        document.querySelector('.tab-btn[data-tab="resumen"]').click();
    }
    
    function ocultarResumen() { let rs = document.getElementById('resumenSection'); if(rs) rs.style.display = 'none'; let av = document.getElementById('avisoExamen'); if(av) av.style.display = 'block'; }
    function mostrarResumen() { let rs = document.getElementById('resumenSection'); if(rs) rs.style.display = 'block'; let av = document.getElementById('avisoExamen'); if(av) av.style.display = 'none'; }
    
    function parsearPreguntas(respuesta) {
        const preguntas = []; const lineas = respuesta.split('\n'); let actual = null;
        for (let linea of lineas) {
            linea = linea.trim();
            if (linea.match(/^Pregunta \d+:/i)) {
                if (actual && actual.opciones?.length === 4) preguntas.push(actual);
                actual = { texto: linea.substring(linea.indexOf(':') + 1).trim(), opciones: [], respuesta: "" };
            } else if (actual && /^[A-D]\)/.test(linea)) actual.opciones.push(linea.substring(2).trim());
            else if (actual && linea.toLowerCase().startsWith('respuesta:')) actual.respuesta = linea.substring('respuesta:'.length).trim().toUpperCase()[0] || '';
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
    
    function inicializarSubidaArchivos() {
        const drop = document.getElementById('fileDropArea'), input = document.getElementById('pdfInput'), info = document.getElementById('fileInfo'), nameSpan = document.getElementById('fileName'), selectBtn = document.getElementById('selectFileBtn'), clearBtn = document.getElementById('clearFileBtn');
        function update(file) {
            if(file) { nameSpan.textContent = `📄 ${file.name} (${(file.size/1024).toFixed(1)} KB)`; info.classList.add('show'); drop.classList.add('has-file'); input.files = file; }
            else { info.classList.remove('show'); drop.classList.remove('has-file'); input.value = ''; }
        }
        selectBtn?.addEventListener('click', () => input.click());
        input?.addEventListener('change', (e) => update(e.target.files[0]));
        clearBtn?.addEventListener('click', () => update(null));
        drop?.addEventListener('dragover', (e) => { e.preventDefault(); drop.classList.add('drag-over'); });
        drop?.addEventListener('dragleave', () => drop.classList.remove('drag-over'));
        drop?.addEventListener('drop', (e) => {
            e.preventDefault(); drop.classList.remove('drag-over');
            const file = e.dataTransfer.files[0];
            if(file && file.type === 'application/pdf') update(file);
            else alert('Solo PDF');
        });
        drop?.addEventListener('click', () => input.click());
    }
    
    function inicializarResumen() {
        document.getElementById('generateSummaryBtn')?.addEventListener('click', async () => {
            const pdfFile = document.getElementById('pdfInput').files[0], manualText = document.getElementById('rawTextInput').value;
            let textoFuente = "";
            if(pdfFile) { 
                try { textoFuente = await extraerTextoPDF(pdfFile); } 
                catch(e) { alert("Error al leer el PDF: " + e.message); return; }
            }
            else if(manualText.trim()) textoFuente = manualText;
            else { alert("Sube un PDF o texto"); return; }
            
            if(textoFuente.trim().length < 50) {
                alert("⚠️ El PDF no contiene texto legible. Puede ser una imagen escaneada o estar protegido.");
                return;
            }
            
            const summaryDiv = document.getElementById('summaryResult'), summaryTextDiv = document.getElementById('summaryText'), examBtnCont = document.getElementById('examFromSummaryContainer');
            summaryDiv.style.display = 'block'; summaryTextDiv.innerHTML = '<span class="loading"></span> Generando resumen...'; examBtnCont.style.display = 'none';
            try {
                const resumen = await llamarIA(`Resume el siguiente texto de forma clara y concisa en español (máximo 300 palabras):\n\n${textoFuente.substring(0,8000)}`);
                summaryTextDiv.innerHTML = resumen.replace(/\n/g,'<br>'); ultimoResumen = resumen; examBtnCont.style.display = 'block';
            } catch(e) { summaryTextDiv.innerHTML = `❌ Error: ${e.message}`; }
        });
        document.getElementById('examFromSummaryBtn')?.addEventListener('click', async () => { if(!ultimoResumen) alert("Genera un resumen"); else await iniciarExamenNormal(ultimoResumen); });
    }
    
    async function iniciarExamenNormal(textoBase) {
        const area = document.getElementById('examArea');
        if(!area) return;
        area.innerHTML = '<div class="loading"></div> Generando examen...';
        examenActualEsNuevo = true;
        try {
            const respuesta = await llamarIA(`Genera un examen de 10 preguntas de opción múltiple (A,B,C,D) basado en este texto. Formato:\nPregunta 1: [texto]\nA) [opcion]\nB) [opcion]\nC) [opcion]\nD) [opcion]\nRespuesta: X\n(Repite 10 veces)\nTexto: ${textoBase.substring(0,6000)}`);
            preguntasExamen = parsearPreguntas(respuesta);
            if(preguntasExamen.length < 8) throw new Error("No se generaron suficientes preguntas");
            preguntasExamen = preguntasExamen.slice(0,10);
            respuestasUsuario = [];
            examenEnCurso = true;
            preguntaActualIndex = 0;
            ocultarResumen();
            mostrarPreguntaNormal();
        } catch(e) { area.innerHTML = `<div style="color:#ffaaaa;">❌ Error: ${e.message}</div>`; }
    }
    
    function mostrarPreguntaNormal() {
        const area = document.getElementById('examArea');
        if(!area) return;
        if(preguntaActualIndex >= preguntasExamen.length) { mostrarResultadosNormal(); return; }
        const q = preguntasExamen[preguntaActualIndex];
        area.innerHTML = `
            <div class="question-card">
                <div style="margin-bottom:12px; color:#ffd966;">📋 Pregunta ${preguntaActualIndex+1} de ${preguntasExamen.length}</div>
                <div class="question-text">${escapeHtml(q.texto)}</div>
                <div id="opcionesNormal">${q.opciones.map((opt,idx) => `<button class="option-btn" data-letra="${['A','B','C','D'][idx]}">${['A','B','C','D'][idx]}) ${escapeHtml(opt)}</button>`).join('')}</div>
                <div id="feedbackNormal"></div>
            </div>`;
        let bloqueado = false;
        document.querySelectorAll('#opcionesNormal .option-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                if(bloqueado || !examenEnCurso) return;
                bloqueado = true;
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
                if(letraSel === correcta) fb.innerHTML = '<div class="feedback-correct">✅ ¡Correcta! Avanzando...</div>';
                else fb.innerHTML = `<div class="feedback-wrong">❌ Incorrecta. Era ${correcta}: ${escapeHtml(textoCorrecto)}. Avanzando...</div>`;
                setTimeout(() => { preguntaActualIndex++; mostrarPreguntaNormal(); }, 1200);
            });
        });
    }
    
    async function mostrarResultadosNormal() {
        const area = document.getElementById('examArea');
        if(!area) return;
        const total = respuestasUsuario.length;
        const correctas = respuestasUsuario.filter(r => r.seleccionada === r.correcta).length;
        const nota = (correctas / total) * 5;
        examenEnCurso = false;
        
        if(examenActualEsNuevo) {
            await guardarExamenCloud(ultimoResumen, ultimoResumen, preguntasExamen, usuarioActual);
            actualizarListaExamenes();
        }
        
        // Generar HTML para el PDF
        let preguntasHtml = `<div style="font-family: Arial, sans-serif; padding: 20px;">
            <h1 style="color: #1a4a6f;">📚 Examen - Repaso</h1>
            <p style="color: #666;">Fecha: ${new Date().toLocaleString()}</p>
            <p style="color: #666;">Usuario: ${usuarioActual}</p>
            <hr>`;
        
        respuestasUsuario.forEach((r,i) => {
            preguntasHtml += `<div style="margin-bottom: 25px; page-break-inside: avoid;">
                <p><strong>${i+1}. ${escapeHtml(r.pregunta)}</strong></p>
                <p>${r.opciones.map((opt,idx) => `${['A','B','C','D'][idx]}) ${escapeHtml(opt)}`).join(' | ')}</p>
                <p style="color: green;"><strong>✅ Respuesta correcta: ${r.correcta}) ${escapeHtml(r.textoCorrecto)}</strong></p>
                <p>Tu respuesta: ${r.seleccionada}) ${escapeHtml(r.opciones[r.seleccionada.charCodeAt(0)-65] || 'No seleccionada')}</p>
            </div>`;
        });
        preguntasHtml += `<hr><p><strong>Calificación: ${nota.toFixed(1)} / 5.0 (${correctas}/${total} correctas)</strong></p></div>`;
        
        area.innerHTML = `
            <div class="score-area">
                <div class="score-number">${nota.toFixed(1)} / 5.0</div>
                <div>✅ Correctas: ${correctas} | ❌ Incorrectas: ${total - correctas}</div>
                <button class="primary" id="pdfBtn">📄 Generar PDF</button>
                <button class="secondary" id="newExamBtn">📝 Nuevo examen</button>
                ${examenActualEsNuevo ? '<div style="margin-top:10px; color:#ffd966;">✅ Examen guardado en la nube</div>' : ''}
            </div>`;
        
        mostrarResumen();
        
        // Evento para generar PDF
        document.getElementById('pdfBtn')?.addEventListener('click', async () => {
            await generarPDF(preguntasHtml, `examen_repaso_${Date.now()}.pdf`);
        });
        document.getElementById('newExamBtn')?.addEventListener('click', () => iniciarExamenNormal(ultimoResumen));
    }
    
    // ========== CULTURA GENERAL ==========
    async function iniciarCulturaGeneral() {
        const appCultura = document.getElementById('culturaApp');
        if(!appCultura) return;
        appCultura.innerHTML = '<div class="loading"></div> Generando preguntas...';
        try {
            const respuesta = await llamarIA(`Genera 10 preguntas de cultura general variadas (historia, ciencia, arte, geografía). Formato:\nPregunta 1: [texto]\nA) [opcion]\nB) [opcion]\nC) [opcion]\nD) [opcion]\nRespuesta: X\n(Repite 10 veces)`);
            culturaPreguntas = parsearPreguntas(respuesta);
            if(culturaPreguntas.length < 8) throw new Error("No se generaron suficientes");
            culturaPreguntas = culturaPreguntas.slice(0,10);
            culturaRespuestas = [];
            culturaIndex = 0;
            culturaEnCurso = true;
            tiempoRestante = 300;
            if(temporizador) clearInterval(temporizador);
            temporizador = setInterval(() => {
                if(!culturaEnCurso) return;
                if(tiempoRestante <= 0) { clearInterval(temporizador); finalizarCulturaGeneral(); }
                else { tiempoRestante--; const minutos = Math.floor(tiempoRestante/60), segundos = tiempoRestante%60; const td = document.getElementById('culturaTimer'); if(td) { td.innerHTML = `⏱️ ${minutos.toString().padStart(2,'0')}:${segundos.toString().padStart(2,'0')}`; if(tiempoRestante<60) td.classList.add('warning'); } }
            }, 1000);
            mostrarPreguntaCultura();
        } catch(e) { appCultura.innerHTML = `<div style="color:#ffaaaa;">❌ Error: ${e.message}<br><button class="primary" onclick="iniciarCulturaGeneral()">Reintentar</button></div>`; }
    }
    
    function mostrarPreguntaCultura() {
        const appCultura = document.getElementById('culturaApp');
        if(!appCultura) return;
        if(culturaIndex >= culturaPreguntas.length) { finalizarCulturaGeneral(); return; }
        const q = culturaPreguntas[culturaIndex];
        appCultura.innerHTML = `
            <div style="display:flex; justify-content:space-between; margin-bottom:15px; flex-wrap:wrap;"><div style="color:#ffd966;">📋 Pregunta ${culturaIndex+1} de ${culturaPreguntas.length}</div><div id="culturaTimer" class="timer">⏱️ 05:00</div></div>
            <div class="question-card"><div class="question-text">${escapeHtml(q.texto)}</div><div id="opcionesCultura">${q.opciones.map((opt,idx) => `<button class="option-btn" data-letra="${['A','B','C','D'][idx]}">${['A','B','C','D'][idx]}) ${escapeHtml(opt)}</button>`).join('')}</div><div id="feedbackCultura"></div></div>`;
        let bloqueado = false;
        document.querySelectorAll('#opcionesCultura .option-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                if(bloqueado || !culturaEnCurso) return;
                bloqueado = true;
                const letraSel = btn.getAttribute('data-letra');
                const correcta = culturaPreguntas[culturaIndex].respuesta;
                const idxCorrecta = correcta.charCodeAt(0)-65;
                const textoCorrecto = culturaPreguntas[culturaIndex].opciones[idxCorrecta];
                culturaRespuestas.push({ pregunta: culturaPreguntas[culturaIndex].texto, seleccionada: letraSel, correcta: correcta, textoCorrecto: textoCorrecto });
                const fb = document.getElementById('feedbackCultura');
                if(letraSel === correcta) fb.innerHTML = '<div class="feedback-correct">✅ ¡Correcta! Avanzando...</div>';
                else fb.innerHTML = `<div class="feedback-wrong">❌ Incorrecta. Era ${correcta}: ${escapeHtml(textoCorrecto)}. Avanzando...</div>`;
                setTimeout(() => { culturaIndex++; mostrarPreguntaCultura(); }, 1200);
            });
        });
    }
    
    function finalizarCulturaGeneral() {
        if(temporizador) clearInterval(temporizador);
        culturaEnCurso = false;
        const total = culturaRespuestas.length, correctas = culturaRespuestas.filter(r => r.seleccionada === r.correcta).length, nota = (correctas/total)*5;
        const appCultura = document.getElementById('culturaApp');
        if(appCultura) appCultura.innerHTML = `<div class="score-area"><div class="score-number">${nota.toFixed(1)} / 5.0</div><div>✅ Correctas: ${correctas} | ❌ Incorrectas: ${total-correctas}</div><button class="primary" id="nuevoCulturaBtn">🎲 Nuevo test</button></div>`;
        document.getElementById('nuevoCulturaBtn')?.addEventListener('click', () => iniciarCulturaGeneral());
    }
    
    function escapeHtml(str) { return str.replace(/[&<>]/g, m => ({ '&':'&amp;', '<':'&lt;', '>':'&gt;' }[m])); }
    
    (async () => {
        const usuarios = JSON.parse(localStorage.getItem('usuarios') || '{}');
        if(!usuarios.admin) {
            usuarios.admin = { hash: await hashPassword("admin123"), esAdmin: true };
            localStorage.setItem('usuarios', JSON.stringify(usuarios));
        }
    })();
    
    mostrarPantallaLogin();
</script>
</body>
</html>
