<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>📚 Exámenes IA - Con Usuarios</title>
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
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .container { max-width: 1200px; margin: 0 auto; }
        
        /* Pantallas de login y registro */
        .auth-screen {
            background: rgba(15, 35, 55, 0.95);
            backdrop-filter: blur(12px);
            border-radius: 48px;
            padding: 40px;
            max-width: 450px;
            margin: 50px auto;
            text-align: center;
            border: 1px solid rgba(255, 217, 102, 0.3);
            animation: fadeIn 0.5s ease;
        }
        
        .auth-screen h2 {
            color: #ffd966;
            margin-bottom: 25px;
        }
        
        .auth-screen input {
            width: 100%;
            padding: 14px;
            margin: 10px 0;
            border-radius: 30px;
            border: 1px solid rgba(255,217,102,0.3);
            background: rgba(0,0,0,0.4);
            color: white;
            font-size: 1rem;
        }
        
        .auth-screen button {
            background: linear-gradient(95deg, #ffd966, #ffb347);
            border: none;
            padding: 12px 28px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
            margin: 10px 5px;
        }
        
        .auth-screen .switch-btn {
            background: transparent;
            border: 1px solid #ffd966;
            color: #ffd966;
        }
        
        .error-msg {
            color: #ff8888;
            margin: 10px 0;
            font-size: 0.9rem;
        }
        
        /* Header de usuario */
        .user-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(15, 35, 55, 0.8);
            backdrop-filter: blur(8px);
            border-radius: 60px;
            padding: 10px 20px;
            margin-bottom: 25px;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .user-info {
            color: #ffd966;
            font-weight: bold;
        }
        
        .admin-badge {
            background: #ff4444;
            color: white;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.7rem;
            margin-left: 10px;
        }
        
        .logout-btn {
            background: rgba(255,100,100,0.3);
            border: 1px solid #ff6666;
            padding: 6px 16px;
            border-radius: 30px;
            cursor: pointer;
            color: #ffaaaa;
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
        
        .tab-btn.active { background: #ffd966; color: #1a2a3a; border-color: #ffd966; }

        .panel {
            display: none;
            background: rgba(15, 35, 55, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 30px;
            animation: fadeIn 0.3s ease;
            border: 1px solid rgba(255, 217, 102, 0.2);
        }
        
        .panel.active-panel { display: block; }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
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

        .examenes-lista {
            margin-top: 25px;
            border-top: 2px solid rgba(255,217,102,0.3);
            padding-top: 20px;
        }
        
        .examen-card {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 15px;
            margin-bottom: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 10px;
            border: 1px solid rgba(255,217,102,0.2);
        }
        
        .examen-info {
            flex: 1;
        }
        
        .examen-titulo {
            font-weight: bold;
            color: #ffd966;
            margin-bottom: 5px;
        }
        
        .examen-fecha {
            font-size: 0.75rem;
            color: #8aaec0;
        }
        
        .examen-usuario {
            font-size: 0.7rem;
            color: #ffaa66;
            margin-top: 3px;
        }
        
        .examen-actions {
            display: flex;
            gap: 8px;
        }
        
        .btn-icon {
            background: rgba(255,255,255,0.1);
            border: none;
            padding: 6px 12px;
            border-radius: 20px;
            cursor: pointer;
            color: #ffd966;
            font-size: 0.8rem;
        }
        
        .btn-icon.admin-delete {
            color: #ff8888;
            border: 1px solid #ff8888;
        }
        
        .btn-icon:hover { background: rgba(255,217,102,0.2); }
        
        .empty-message {
            color: #8aaec0;
            text-align: center;
            padding: 20px;
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
        }
        
        .file-drop-area.drag-over { border-color: #ffd966; background: rgba(255, 217, 102, 0.1); }
        .file-drop-area.has-file { border-color: #4caf50; background: rgba(76, 175, 80, 0.1); }
        .file-icon { font-size: 3rem; }
        .file-info { display: none; align-items: center; justify-content: center; gap: 15px; margin-top: 15px; }
        .file-info.show { display: flex; }
        .file-name { background: rgba(0,0,0,0.4); padding: 5px 15px; border-radius: 60px; color: #4caf50; }
        .file-clear-btn { background: rgba(255,100,100,0.8); border: none; padding: 5px 12px; border-radius: 30px; cursor: pointer; color: white; }
        .file-select-btn { background: rgba(255,217,102,0.2); border: 1px solid #ffd966; padding: 8px 20px; border-radius: 40px; display: inline-block; margin-top: 10px; cursor: pointer; color: #ffd966; }
        .file-input-hidden { display: none; }

        textarea { 
            width: 100%; 
            padding: 12px; 
            border-radius: 20px; 
            border: 1px solid rgba(255,217,102,0.3); 
            background: rgba(0,0,0,0.4); 
            color: #ffffff; 
            margin: 10px 0; 
        }
        
        textarea::placeholder { color: #8aaec0; }
        
        button.primary { 
            background: linear-gradient(95deg, #ffd966, #ffb347); 
            border: none; 
            padding: 12px 28px; 
            border-radius: 40px; 
            font-weight: bold; 
            cursor: pointer; 
            margin: 5px; 
            color: #1a2a3a; 
        }
        
        button.secondary { 
            background: rgba(255,255,255,0.1); 
            border: 1px solid #ffd966; 
            color: #ffd966; 
            padding: 10px 24px; 
            border-radius: 40px; 
            cursor: pointer; 
        }
        
        .result-box { 
            background: rgba(10,30,50,0.6); 
            border-radius: 20px; 
            padding: 20px; 
            margin-top: 20px; 
            color: #ffffff; 
        }
        
        .loading::before { 
            content: ""; 
            display: inline-block; 
            width: 18px; 
            height: 18px; 
            border: 2px solid #ffd966; 
            border-top-color: transparent; 
            border-radius: 50%; 
            animation: spin 0.8s linear infinite; 
            margin-right: 8px; 
        }
        
        @keyframes spin { to { transform: rotate(360deg); } }

        .question-card { 
            background: rgba(0,0,0,0.3); 
            border-radius: 24px; 
            padding: 25px; 
            margin: 20px 0; 
        }
        
        .question-text { 
            font-size: 1.3rem; 
            margin-bottom: 20px; 
            color: #ffffff; 
        }
        
        .option-btn { 
            background: rgba(25,55,80,0.9); 
            border: 1px solid #ffd966; 
            border-radius: 60px; 
            padding: 12px 18px; 
            margin: 8px; 
            cursor: pointer; 
            display: inline-block; 
            transition: 0.2s; 
            color: #ffffff;
            font-weight: 500;
        }
        
        .option-btn:hover { 
            background: rgba(255,217,102,0.2); 
            transform: scale(1.02); 
            color: #ffffff; 
        }
        
        .feedback-correct { 
            background: #2e7d32; 
            padding: 10px; 
            border-radius: 30px; 
            margin-top: 15px; 
            text-align: center; 
            color: #ffffff; 
        }
        
        .feedback-wrong { 
            background: #c62828; 
            padding: 10px; 
            border-radius: 30px; 
            margin-top: 15px; 
            text-align: center; 
            color: #ffffff; 
        }
        
        .score-area { 
            background: #1a3a4a; 
            padding: 15px; 
            border-radius: 30px; 
            margin: 20px 0; 
            text-align: center; 
            color: #ffffff; 
        }
        
        .score-number { 
            font-size: 2rem; 
            font-weight: bold; 
            color: #ffd966; 
        }
        
        .timer { 
            font-size: 1.5rem; 
            font-weight: bold; 
            color: #ffaa66; 
            font-family: monospace; 
        }
        
        .timer.warning { 
            color: #ff4444; 
            animation: pulse 0.5s infinite; 
        }
        
        @keyframes pulse { 
            0%,100% { opacity: 1; } 
            50% { opacity: 0.5; } 
        }
        
        @media (max-width: 650px) { 
            .panel { padding: 20px; } 
            .question-text { font-size: 1.1rem; } 
            .option-btn { padding: 8px 12px; font-size: 0.9rem; }
            .examen-card { flex-direction: column; align-items: flex-start; }
        }
    </style>
</head>
<body>
<div class="container" id="app">
    <!-- Aquí se renderiza todo dinámicamente -->
</div>

<script>
    // ========== CONFIGURACIÓN DE APIS ==========
    const GROQ_API_KEY = "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";
    const CEREBRAS_API_KEY = "csk-mmcetp8ndr55em9v83833hd8xvx495vwkpcvkmm8h2xcp8nc";
    
    let proveedorActual = 0;
    
    async function llamarGroq(prompt) {
        const response = await fetch("https://api.groq.com/openai/v1/chat/completions", {
            method: "POST",
            headers: { "Content-Type": "application/json", "Authorization": `Bearer ${GROQ_API_KEY}` },
            body: JSON.stringify({
                model: "llama-3.1-8b-instant",
                messages: [{ role: "system", content: "Eres un asistente educativo útil. Respondes en español." }, { role: "user", content: prompt }],
                temperature: 0.5,
                max_tokens: 1500
            })
        });
        if (response.status === 429) throw new Error("Rate limit");
        if (!response.ok) throw new Error(`Error ${response.status}`);
        const data = await response.json();
        return data.choices[0].message.content;
    }
    
    async function llamarCerebras(prompt) {
        const response = await fetch("https://api.cerebras.ai/v1/chat/completions", {
            method: "POST",
            headers: { "Content-Type": "application/json", "Authorization": `Bearer ${CEREBRAS_API_KEY}` },
            body: JSON.stringify({
                model: "llama3.1-8b",
                messages: [{ role: "system", content: "Eres un asistente educativo útil. Respondes en español." }, { role: "user", content: prompt }],
                temperature: 0.5,
                max_tokens: 1500
            })
        });
        if (response.status === 429) throw new Error("Rate limit");
        if (!response.ok) throw new Error(`Error ${response.status}`);
        const data = await response.json();
        return data.choices[0].message.content;
    }
    
    async function llamarIA(prompt) {
        const proveedores = [llamarGroq, llamarCerebras];
        for (let i = 0; i < proveedores.length; i++) {
            try {
                return await proveedores[(proveedorActual + i) % proveedores.length](prompt);
            } catch(e) { continue; }
        }
        throw new Error("Todos los proveedores fallaron. Espera unos segundos.");
    }
    
    // ========== SISTEMA DE USUARIOS ==========
    let usuarioActual = null;
    let esAdmin = false;
    
    // Usuario administrador predefinido (TÚ)
    const ADMIN_USER = "SamuelDJAdmin";
    const ADMIN_PASS = "1043690741SamuelDJADJA";  // ⚠️ Cámbiala después
    
    function cargarUsuarios() {
        const usuarios = localStorage.getItem('usuarios');
        if (!usuarios) {
            const usuariosIniciales = {};
            usuariosIniciales[ADMIN_USER] = { password: ADMIN_PASS, esAdmin: true };
            localStorage.setItem('usuarios', JSON.stringify(usuariosIniciales));
        }
        return JSON.parse(localStorage.getItem('usuarios'));
    }
    
    function guardarUsuarios(usuarios) {
        localStorage.setItem('usuarios', JSON.stringify(usuarios));
    }
    
    function registrarUsuario(username, password) {
        const usuarios = cargarUsuarios();
        if (usuarios[username]) return { success: false, error: "El usuario ya existe" };
        if (password.length < 4) return { success: false, error: "La contraseña debe tener al menos 4 caracteres" };
        
        usuarios[username] = { password: password, esAdmin: false };
        guardarUsuarios(usuarios);
        return { success: true };
    }
    
    function loginUsuario(username, password) {
        const usuarios = cargarUsuarios();
        const user = usuarios[username];
        if (!user) return { success: false, error: "Usuario no existe" };
        if (user.password !== password) return { success: false, error: "Contraseña incorrecta" };
        return { success: true, esAdmin: user.esAdmin || false };
    }
    
    // ========== EXÁMENES GUARDADOS ==========
    let examenesGuardados = [];
    
    function cargarExamenesGuardados() {
        const guardados = localStorage.getItem('examenes_guardados');
        examenesGuardados = guardados ? JSON.parse(guardados) : [];
    }
    
    function guardarExamenEnStorage(titulo, resumen, preguntas, usuario) {
        const nuevoExamen = {
            id: Date.now(),
            titulo: titulo.substring(0, 50) + (titulo.length > 50 ? '...' : ''),
            resumen: resumen,
            preguntas: preguntas,
            usuario: usuario,
            fecha: new Date().toLocaleString()
        };
        examenesGuardados.unshift(nuevoExamen);
        if (examenesGuardados.length > 50) examenesGuardados.pop();
        localStorage.setItem('examenes_guardados', JSON.stringify(examenesGuardados));
        return nuevoExamen.id;
    }
    
    function eliminarExamenStorage(id) {
        examenesGuardados = examenesGuardados.filter(e => e.id !== id);
        localStorage.setItem('examenes_guardados', JSON.stringify(examenesGuardados));
    }
    
    // ========== VARIABLES GLOBALES ==========
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
    
    const app = document.getElementById('app');
    
    // ========== RENDERIZADO DE PANTALLAS ==========
    function mostrarPantallaLogin() {
        app.innerHTML = `
            <div class="auth-screen">
                <h2>📚 Bienvenido</h2>
                <div id="loginForm">
                    <input type="text" id="loginUser" placeholder="Usuario" autocomplete="off">
                    <input type="password" id="loginPass" placeholder="Contraseña">
                    <div id="loginError" class="error-msg"></div>
                    <button id="btnLogin">🔓 Iniciar sesión</button>
                    <button id="btnMostrarRegistro" class="switch-btn">📝 Crear cuenta nueva</button>
                </div>
                <div id="registerForm" style="display:none;">
                    <input type="text" id="regUser" placeholder="Nuevo usuario">
                    <input type="password" id="regPass" placeholder="Contraseña (mínimo 4 caracteres)">
                    <div id="regError" class="error-msg"></div>
                    <button id="btnRegister">✅ Registrarse</button>
                    <button id="btnMostrarLogin" class="switch-btn">◀ Volver al inicio</button>
                </div>
            </div>
        `;
        
        document.getElementById('btnLogin')?.addEventListener('click', () => {
            const user = document.getElementById('loginUser').value.trim();
            const pass = document.getElementById('loginPass').value;
            const result = loginUsuario(user, pass);
            if (result.success) {
                usuarioActual = user;
                esAdmin = result.esAdmin;
                mostrarPantallaPrincipal();
            } else {
                document.getElementById('loginError').innerText = result.error;
            }
        });
        
        document.getElementById('btnMostrarRegistro')?.addEventListener('click', () => {
            document.getElementById('loginForm').style.display = 'none';
            document.getElementById('registerForm').style.display = 'block';
        });
        
        document.getElementById('btnMostrarLogin')?.addEventListener('click', () => {
            document.getElementById('registerForm').style.display = 'none';
            document.getElementById('loginForm').style.display = 'block';
        });
        
        document.getElementById('btnRegister')?.addEventListener('click', () => {
            const user = document.getElementById('regUser').value.trim();
            const pass = document.getElementById('regPass').value;
            const result = registrarUsuario(user, pass);
            if (result.success) {
                document.getElementById('registerForm').style.display = 'none';
                document.getElementById('loginForm').style.display = 'block';
                document.getElementById('loginUser').value = user;
                document.getElementById('loginPass').value = pass;
                document.getElementById('regError').innerText = "";
                document.getElementById('loginError').innerText = "✅ Usuario registrado. Inicia sesión.";
            } else {
                document.getElementById('regError').innerText = result.error;
            }
        });
    }
    
    function mostrarPantallaPrincipal() {
        cargarExamenesGuardados();
        
        app.innerHTML = `
            <div class="user-header">
                <div class="user-info">
                    👤 ${usuarioActual} ${esAdmin ? '<span class="admin-badge">👑 ADMINISTRADOR</span>' : ''}
                </div>
                <button class="logout-btn" id="logoutBtn">🚪 Cerrar sesión</button>
            </div>
            
            <div class="tabs">
                <button class="tab-btn active" data-tab="resumen">📄 Resumen y Examen</button>
                <button class="tab-btn" data-tab="historial">📋 Historial de Exámenes</button>
                <button class="tab-btn" data-tab="cultura">🌍 Cultura General</button>
            </div>
            
            <div id="resumenPanel" class="panel active-panel">
                <div id="resumenSection">
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
                <div id="avisoExamen" class="aviso-examen" style="display:none;">📖 El resumen se ha ocultado para mantener la honestidad del examen.<br>Termina el examen para volver a verlo.</div>
                <div id="examArea" style="margin-top: 20px;"></div>
            </div>
            
            <div id="historialPanel" class="panel">
                <h3>📋 Exámenes guardados</h3>
                <div id="listaExamenes" class="examenes-lista"></div>
            </div>
            
            <div id="culturaPanel" class="panel">
                <div id="culturaApp"></div>
            </div>
        `;
        
        // Eventos de logout
        document.getElementById('logoutBtn')?.addEventListener('click', () => {
            usuarioActual = null;
            esAdmin = false;
            mostrarPantallaLogin();
        });
        
        // Inicializar componentes
        inicializarSubidaArchivos();
        inicializarResumen();
        inicializarExamen();
        actualizarListaExamenes();
        inicializarCulturaGeneral();
        
        // Pestañas
        document.querySelectorAll('.tab-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                const tab = btn.getAttribute('data-tab');
                document.querySelectorAll('.panel').forEach(p => p.classList.remove('active-panel'));
                document.getElementById(`${tab}Panel`).classList.add('active-panel');
                if (tab === 'cultura' && !culturaEnCurso && document.getElementById('culturaApp').innerHTML === '') iniciarCulturaGeneral();
                if (tab === 'historial') actualizarListaExamenes();
            });
        });
    }
    
    function actualizarListaExamenes() {
        const contenedor = document.getElementById('listaExamenes');
        if (!contenedor) return;
        
        if (examenesGuardados.length === 0) {
            contenedor.innerHTML = '<div class="empty-message">📭 No hay exámenes guardados aún. Genera un examen y aparecerá aquí.</div>';
            return;
        }
        
        contenedor.innerHTML = examenesGuardados.map(examen => `
            <div class="examen-card">
                <div class="examen-info">
                    <div class="examen-titulo">📖 ${escapeHtml(examen.titulo)}</div>
                    <div class="examen-fecha">📅 ${examen.fecha}</div>
                    <div class="examen-usuario">👤 Creado por: ${escapeHtml(examen.usuario)}</div>
                    <div class="examen-preview">${examen.preguntas?.length || 0} preguntas</div>
                </div>
                <div class="examen-actions">
                    <button class="btn-icon tomar-examen" data-id="${examen.id}">📝 Tomar examen</button>
                    ${esAdmin ? `<button class="btn-icon admin-delete eliminar-examen-admin" data-id="${examen.id}">🗑️ Eliminar (Admin)</button>` : ''}
                </div>
            </div>
        `).join('');
        
        document.querySelectorAll('.tomar-examen').forEach(btn => {
            btn.addEventListener('click', () => {
                const id = parseInt(btn.getAttribute('data-id'));
                const examen = examenesGuardados.find(e => e.id === id);
                if (examen) iniciarExamenGuardado(examen);
            });
        });
        
        if (esAdmin) {
            document.querySelectorAll('.eliminar-examen-admin').forEach(btn => {
                btn.addEventListener('click', () => {
                    const id = parseInt(btn.getAttribute('data-id'));
                    eliminarExamenStorage(id);
                    actualizarListaExamenes();
                });
            });
        }
    }
    
    function iniciarExamenGuardado(examen) {
        preguntasExamen = examen.preguntas;
        respuestasUsuario = [];
        examenEnCurso = true;
        preguntaActualIndex = 0;
        ultimoResumen = examen.resumen;
        ocultarResumen();
        mostrarPreguntaNormal();
        document.querySelector('.tab-btn[data-tab="resumen"]').click();
    }
    
    // ========== FUNCIONES DEL EXAMEN ==========
    const examArea = () => document.getElementById('examArea');
    const culturaApp = () => document.getElementById('culturaApp');
    const resumenSection = () => document.getElementById('resumenSection');
    const avisoExamen = () => document.getElementById('avisoExamen');
    
    function ocultarResumen() {
        if (resumenSection()) resumenSection().style.display = 'none';
        if (avisoExamen()) avisoExamen().style.display = 'block';
    }
    
    function mostrarResumen() {
        if (resumenSection()) resumenSection().style.display = 'block';
        if (avisoExamen()) avisoExamen().style.display = 'none';
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
    
    function inicializarSubidaArchivos() {
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
        selectFileBtn?.addEventListener('click', () => pdfInput.click());
        pdfInput?.addEventListener('change', (e) => updateFileUI(e.target.files[0]));
        clearFileBtn?.addEventListener('click', () => updateFileUI(null));
        fileDropArea?.addEventListener('dragover', (e) => { e.preventDefault(); fileDropArea.classList.add('drag-over'); });
        fileDropArea?.addEventListener('dragleave', () => fileDropArea.classList.remove('drag-over'));
        fileDropArea?.addEventListener('drop', (e) => {
            e.preventDefault();
            fileDropArea.classList.remove('drag-over');
            const file = e.dataTransfer.files[0];
            if (file && file.type === 'application/pdf') updateFileUI(file);
            else alert('Solo PDF');
        });
        fileDropArea?.addEventListener('click', () => pdfInput.click());
    }
    
    function inicializarResumen() {
        document.getElementById('generateSummaryBtn')?.addEventListener('click', async () => {
            const pdfFile = document.getElementById('pdfInput').files[0];
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
                const resumen = await llamarIA(prompt);
                summaryTextDiv.innerHTML = resumen.replace(/\n/g,'<br>');
                ultimoResumen = resumen;
                examBtnContainer.style.display = 'block';
            } catch(e) { 
                summaryTextDiv.innerHTML = `<div style="color:#ffaaaa;">❌ Error: ${e.message}</div>`;
            }
        });
        
        document.getElementById('examFromSummaryBtn')?.addEventListener('click', async () => {
            if (!ultimoResumen) { alert("Genera un resumen primero"); return; }
            await iniciarExamenNormal(ultimoResumen);
        });
    }
    
    async function iniciarExamenNormal(textoBase) {
        const area = examArea();
        if (!area) return;
        area.innerHTML = '<div class="loading"></div> Generando examen de 10 preguntas...';
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
            const respuesta = await llamarIA(prompt);
            preguntasExamen = parsearPreguntas(respuesta);
            if (preguntasExamen.length < 8) throw new Error("No se generaron suficientes preguntas");
            preguntasExamen = preguntasExamen.slice(0,10);
            respuestasUsuario = [];
            examenEnCurso = true;
            preguntaActualIndex = 0;
            ocultarResumen();
            mostrarPreguntaNormal();
        } catch(e) { 
            area.innerHTML = `<div style="color:#ffaaaa; text-align:center;">❌ Error: ${e.message}<br><br>Espera unos segundos y reintenta.</div>`;
        }
    }
    
    function mostrarPreguntaNormal() {
        const area = examArea();
        if (!area) return;
        if (preguntaActualIndex >= preguntasExamen.length) {
            mostrarResultadosNormal();
            return;
        }
        const q = preguntasExamen[preguntaActualIndex];
        area.innerHTML = `
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
        const area = examArea();
        if (!area) return;
        const total = respuestasUsuario.length;
        const correctas = respuestasUsuario.filter(r => r.seleccionada === r.correcta).length;
        const nota = (correctas / total) * 5;
        examenEnCurso = false;
        
        guardarExamenEnStorage(ultimoResumen, ultimoResumen, preguntasExamen, usuarioActual);
        
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
        
        area.innerHTML = `
            <div class="score-area">
                <div style="font-size:1.2rem;">📊 Resultado del Examen</div>
                <div class="score-number">${nota.toFixed(1)} / 5.0</div>
                <div>✅ Correctas: ${correctas} | ❌ Incorrectas: ${total - correctas}</div>
                <div style="margin-top: 15px;">
                    <button class="primary" id="pdfBtn">📄 Generar PDF</button>
                    <button class="secondary" id="newExamBtn">📝 Nuevo examen</button>
                </div>
                <div style="margin-top: 15px; color:#ffd966;">✅ Examen guardado en el historial</div>
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
    
    // ========== CULTURA GENERAL ==========
    async function inicializarCulturaGeneral() {
        await iniciarCulturaGeneral();
    }
    
    async function iniciarCulturaGeneral() {
        const appCultura = document.getElementById('culturaApp');
        if (!appCultura) return;
        appCultura.innerHTML = '<div class="loading"></div> Generando preguntas de cultura general...';
        try {
            const prompt = `Genera 10 preguntas de cultura general variadas (historia, ciencia, arte, geografía). Cada pregunta debe ser de opción múltiple (A,B,C,D). Usa este formato:
Pregunta 1: [texto]
A) [opcion]
B) [opcion]
C) [opcion]
D) [opcion]
Respuesta: X
(Repite hasta 10)`;
            const respuesta = await llamarIA(prompt);
            culturaPreguntas = parsearPreguntas(respuesta);
            if (culturaPreguntas.length < 8) throw new Error("No se generaron suficientes");
            culturaPreguntas = culturaPreguntas.slice(0,10);
            culturaRespuestas = [];
            culturaIndex = 0;
            culturaEnCurso = true;
            tiempoRestante = 300;
            if (temporizador) clearInterval(temporizador);
            temporizador = setInterval(() => {
                if (!culturaEnCurso) return;
                if (tiempoRestante <= 0) {
                    clearInterval(temporizador);
                    finalizarCulturaGeneral();
                } else {
                    tiempoRestante--;
                    const minutos = Math.floor(tiempoRestante / 60);
                    const segundos = tiempoRestante % 60;
                    const timerDiv = document.getElementById('culturaTimer');
                    if (timerDiv) {
                        timerDiv.innerHTML = `⏱️ ${minutos.toString().padStart(2,'0')}:${segundos.toString().padStart(2,'0')}`;
                        if (tiempoRestante < 60) timerDiv.classList.add('warning');
                    }
                }
            }, 1000);
            mostrarPreguntaCultura();
        } catch(e) { 
            appCultura.innerHTML = `<div style="color:#ffaaaa; text-align:center;">❌ Error: ${e.message}<br><button class="primary" onclick="iniciarCulturaGeneral()">Reintentar</button></div>`;
        }
    }
    
    function mostrarPreguntaCultura() {
        const appCultura = document.getElementById('culturaApp');
        if (!appCultura) return;
        if (culturaIndex >= culturaPreguntas.length) {
            finalizarCulturaGeneral();
            return;
        }
        const q = culturaPreguntas[culturaIndex];
        appCultura.innerHTML = `
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
                    textoCorrecto: textoCorrecto
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
        const appCultura = document.getElementById('culturaApp');
        if (appCultura) {
            appCultura.innerHTML = `
                <div class="score-area">
                    <div style="font-size:1.2rem;">🌍 Resultado de Cultura General</div>
                    <div class="score-number">${nota.toFixed(1)} / 5.0</div>
                    <div>✅ Correctas: ${correctas} | ❌ Incorrectas: ${total - correctas}</div>
                    <button class="primary" id="nuevoCulturaBtn" style="margin-top:15px;">🎲 Nuevo test</button>
                </div>
            `;
            document.getElementById('nuevoCulturaBtn')?.addEventListener('click', () => iniciarCulturaGeneral());
        }
    }
    
    function escapeHtml(str) { return str.replace(/[&<>]/g, m => ({ '&':'&amp;', '<':'&lt;', '>':'&gt;' }[m])); }
    
    // Iniciar
    mostrarPantallaLogin();
</script>
</body>
</html>
