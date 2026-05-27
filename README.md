# SamuelSaaa.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Test: Independencia Latinoamericana | Modo extremo</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e3c2c 0%, #2a4a35 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .test-container {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 32px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            max-width: 700px;
            width: 100%;
            padding: 30px 25px;
            transition: all 0.3s ease;
        }

        h1 {
            font-size: 1.8rem;
            color: #1e3c2c;
            text-align: center;
            border-bottom: 3px solid #d4a373;
            display: inline-block;
            width: 100%;
            margin-bottom: 20px;
            padding-bottom: 10px;
        }

        .status-bar {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            margin-bottom: 25px;
            flex-wrap: wrap;
            gap: 10px;
            background: #f5f0e6;
            padding: 12px 18px;
            border-radius: 60px;
        }

        .progress {
            font-weight: bold;
            background: #2a4a35;
            color: white;
            padding: 5px 14px;
            border-radius: 30px;
            font-size: 0.9rem;
        }

        .message-area {
            font-size: 0.85rem;
            color: #b23c1c;
            font-weight: 500;
        }

        .question-card {
            background: #fef9ef;
            border-radius: 28px;
            padding: 25px 20px;
            margin: 20px 0;
            box-shadow: 0 6px 14px rgba(0, 0, 0, 0.1);
        }

        .question-text {
            font-size: 1.5rem;
            font-weight: 600;
            color: #2c3e2f;
            margin-bottom: 28px;
            line-height: 1.3;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 14px;
        }

        .option-btn {
            background: white;
            border: 2px solid #d4a373;
            border-radius: 50px;
            padding: 12px 20px;
            font-size: 1rem;
            text-align: left;
            cursor: pointer;
            transition: all 0.2s ease;
            font-weight: 500;
            color: #2c3e2f;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .option-btn:hover:not(:disabled) {
            background: #d4a373;
            color: white;
            border-color: #b5835a;
            transform: scale(1.01);
        }

        .option-prefix {
            font-weight: bold;
            background: #e9e3d5;
            width: 32px;
            height: 32px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            border-radius: 30px;
            transition: 0.2s;
        }

        .option-btn:hover .option-prefix {
            background: #fff0e0;
        }

        .correct-feedback {
            margin-top: 15px;
            padding: 10px;
            background: #d1e7dd;
            color: #0f5132;
            border-radius: 30px;
            text-align: center;
            font-weight: bold;
        }

        .wrong-feedback {
            margin-top: 15px;
            padding: 10px;
            background: #f8d7da;
            color: #842029;
            border-radius: 30px;
            text-align: center;
            font-weight: bold;
        }

        .reset-notice {
            background: #fff3cd;
            padding: 12px;
            border-radius: 40px;
            text-align: center;
            margin-top: 20px;
            font-weight: 500;
            color: #856404;
            border-left: 6px solid #ffc107;
        }

        .congrats {
            background: #2a4a35;
            color: #fef9ef;
            text-align: center;
            padding: 25px;
            border-radius: 28px;
            margin-top: 20px;
        }

        .btn-restart {
            background: #d4a373;
            border: none;
            padding: 12px 28px;
            font-size: 1.1rem;
            font-weight: bold;
            border-radius: 60px;
            margin-top: 15px;
            cursor: pointer;
            transition: 0.2s;
            color: #1e3c2c;
        }

        .btn-restart:hover {
            background: #b5835a;
            transform: scale(1.02);
            color: white;
        }

        footer {
            text-align: center;
            font-size: 0.7rem;
            margin-top: 25px;
            color: #5a6e55;
        }

        @media (max-width: 550px) {
            .test-container {
                padding: 20px 15px;
            }
            .question-text {
                font-size: 1.2rem;
            }
            .option-btn {
                padding: 8px 15px;
                font-size: 0.9rem;
            }
        }
    </style>
</head>
<body>
<div class="test-container" id="app">
    <h1>📜 Independencia Latinoamericana</h1>
    <div class="status-bar">
        <span class="progress" id="progressDisplay">Progreso: 0 / 10</span>
        <span class="message-area" id="statusMessage">⚡ Una falla = reinicio total + preguntas nuevas</span>
    </div>

    <div id="quizPanel">
        <!-- Aquí se renderiza la pregunta o el final -->
    </div>
    <footer>Modo extremo: si respondes mal, el test se reinicia con 10 preguntas nuevas.</footer>
</div>

<script>
    // ---------- BANCO DE 30 PREGUNTAS (basadas en el resumen detallado) ----------
    const QUESTION_BANK = [
        { text: "¿Cuál de las siguientes NO fue una medida de las Reformas Borbónicas en América?", options: ["Creación de nuevos impuestos y sistemas de recaudación", "Expulsión de los jesuitas en 1767", "Entrega del poder político a los criollos", "Limitación del poder de los virreyes"], correct: "C" },
        { text: "¿Qué consigna gritaban los comuneros durante la revuelta de 1781 en Nueva Granada?", options: ["'Independencia o muerte'", "'Libertad, fraternidad, igualdad'", "'Viva el Rey, muera el mal gobierno'", "'América para los americanos'"], correct: "C" },
        { text: "¿Quién fue el líder esclavo haitiano apodado 'Louverture' que significa 'El iniciador'?", options: ["Jean-Jacques Dessalines", "Henri Christophe", "Dutty Boukman", "François Dominique Toussaint"], correct: "D" },
        { text: "¿En qué batalla fue derrotado Bernardo O'Higgins en 1814, dando inicio al período de Reconquista en Chile?", options: ["Batalla de Chacabuco", "Batalla de Maipú", "Batalla de Rancagua", "Batalla de Tucumán"], correct: "C" },
        { text: "¿Qué proclamó el Plan de Iguala firmado por Iturbide y Guerrero en 1821?", options: ["La abolición de la esclavitud en México", "La independencia de México bajo una monarquía con Fernando VII como rey", "La creación de una república federal en Centroamérica", "La expulsión de todos los españoles nacidos en España"], correct: "B" },
        { text: "Según el resumen, ¿cuál fue la principal diferencia entre el proceso de independencia de Brasil y el de las colonias españolas?", options: ["Brasil siguió siendo colonia hasta 1889", "En Brasil no hubo enfrentamientos bélicos contra la metrópoli", "La independencia de Brasil fue liderada por indígenas", "Brasil se independizó primero que Haití"], correct: "B" },
        { text: "¿En qué fecha y lugar se declaró oficialmente la independencia de las Provincias Unidas del Río de la Plata?", options: ["25 de mayo de 1810 en Buenos Aires", "9 de julio de 1816 en el Congreso de Tucumán", "28 de julio de 1821 en Lima", "7 de septiembre de 1822 en Ipiranga"], correct: "B" },
        { text: "Según Salomón Kalmanovitz, ¿qué caracterizó a las excolonias ibéricas durante el siglo XIX frente a las anglosajonas?", options: ["Estabilidad política y crecimiento continuo", "Cuasi perpetua guerra civil y estancamiento económico", "Rápida industrialización desde 1820", "Ausencia total de conflictos internos"], correct: "B" },
        { text: "¿Cómo se llamó el movimiento iniciado por los sacerdotes José Matías Delgado y Nicolás Aguilar en 1811 en San Salvador?", options: ["Conspiración de Belén", "Grito de Dolores", "Revolución de los Comuneros", "Revolución de la provincia de San Salvador"], correct: "D" },
        { text: "¿Qué líder independentista mexicano convocó el Congreso de Chilpancingo en 1813?", options: ["Miguel Hidalgo", "Vicente Guerrero", "José María Morelos", "Agustín de Iturbide"], correct: "C" },
        { text: "¿Qué dos nuevos virreinatos fueron creados por los Borbones en América?", options: ["Nueva España y Perú", "Nueva Granada y Río de la Plata", "Nueva Granada y Nueva España", "Río de la Plata y Nueva España"], correct: "B" },
        { text: "¿Quién fue el emperador que proclamó la independencia de Brasil el 7 de septiembre de 1822?", options: ["Juan VI", "Pedro I", "Pedro II", "José de San Martín"], correct: "B" },
        { text: "¿En qué año y por quién fue liderada la gran rebelión indígena en Perú que buscaba expulsar a los españoles?", options: ["1713 por los Borbones", "1767 por los jesuitas", "1780 por Túpac Amaru", "1810 por Miguel Hidalgo"], correct: "C" },
        { text: "¿Cómo se llamó el movimiento de 1781 en Nueva Granada que reunió más de 20.000 hombres?", options: ["Rebelión de Túpac Amaru", "Revolución de los Comuneros", "Conspiración de Belén", "Rebelión de los Barrios de Quito"], correct: "B" },
        { text: "¿Qué país centroamericano no formó parte de las Provincias Unidas de Centroamérica?", options: ["Guatemala", "El Salvador", "Panamá", "Costa Rica"], correct: "C" },
        { text: "¿Qué líder independentista argentino obtuvo la victoria en la Batalla de Tucumán (1812)?", options: ["Cornelio Saavedra", "José de San Martín", "Manuel Belgrano", "Bernardo O'Higgins"], correct: "C" },
        { text: "¿Qué nombre recibió el movimiento de 1765 en Quito contra el estanco al aguardiente?", options: ["Revolución de los Comuneros", "Rebelión de Túpac Amaru", "Rebelión de los Barrios", "Conspiración de Belén"], correct: "C" },
        { text: "¿Quién fundó la Real Expedición Botánica del Nuevo Reino de Granada?", options: ["Túpac Amaru", "José Celestino Mutis", "Miguel Hidalgo", "Francisco de León"], correct: "B" },
        { text: "¿Qué líder haitiano venció a las tropas napoleónicas en 1804 y proclamó la independencia?", options: ["Henri Christophe", "Alexandre Pétion", "Jean-Jacques Dessalines", "Dutty Boukman"], correct: "C" },
        { text: "¿Qué dictador gobernó Paraguay desde 1814 hasta 1840 aislando el país?", options: ["Cornelio Saavedra", "Gaspar Rodríguez de Francia", "José de San Martín", "Manuel Belgrano"], correct: "B" },
        { text: "¿En qué ciudad y año se creó el Virreinato de Nueva Granada?", options: ["Santafé, 1717", "Buenos Aires, 1776", "Lima, 1542", "México, 1535"], correct: "A" },
        { text: "¿Qué documento firmado en 1824 selló la independencia del Perú y el abandono español?", options: ["Plan de Iguala", "Tratado de Córdoba", "Capitulaciones de Ayacucho", "Constitución de Cádiz"], correct: "C" },
        { text: "¿Quién asumió como Jefe Supremo de Chile tras las victorias de Chacabuco y Maipú?", options: ["José Miguel Carrera", "Bernardo O'Higgins", "José de San Martín", "Manuel Belgrano"], correct: "B" },
        { text: "¿Qué bergantín usó San Martín para abandonar Perú para siempre en 1822?", options: ["Bergantín Belgrano", "Fragata Libertad", "Goleta Ancud", "Bergantín Independencia"], correct: "A" },
        { text: "Haití se convirtió en la primera colonia independiente de América Latina en:", options: ["1791", "1804", "1810", "1822"], correct: "B" },
        { text: "Durante las Reformas Borbónicas, ¿qué institución fue controlada y perdió privilegios?", options: ["El ejército", "La Iglesia", "Los corregidores", "Los indígenas"], correct: "B" },
        { text: "¿Quién lideró la conspiración de Belén en Guatemala (1813)?", options: ["José Matías Delgado", "Gabino Gainza", "Juan de la Concepción", "Nicolás Aguilar"], correct: "C" },
        { text: "La independencia de Chile se consolidó definitivamente después de la batalla de:", options: ["Rancagua", "Chacabuco", "Maipú", "Tucumán"], correct: "C" },
        { text: "El sueño de José de San Martín para América era:", options: ["Una Gran Colombia republicana", "Una monarquía borbónica en cada país", "Países independientes gobernados por príncipes borbones", "Una federación única"], correct: "C" },
        { text: "¿Qué país nace como república independiente tras la intervención de Bolívar y Sucre (1824-1825)?", options: ["Perú", "Ecuador", "Bolivia", "Colombia"], correct: "C" }
    ];

    // Estado global del test
    let currentQuestions = [];       // Array de 10 preguntas seleccionadas aleatoriamente
    let currentIndex = 0;           // pregunta actual (0..9)
    let waitingResponse = false;     // bloqueo mientras se muestra feedback
    let timeoutId = null;

    // Elementos DOM
    const quizPanel = document.getElementById('quizPanel');
    const progressDisplay = document.getElementById('progressDisplay');
    const statusMessage = document.getElementById('statusMessage');

    // Función: obtener 10 preguntas aleatorias SIN repetición dentro del conjunto
    function getRandomQuestions() {
        // Copiar y desordenar (Fisher-Yates)
        const shuffled = [...QUESTION_BANK];
        for (let i = shuffled.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
        }
        return shuffled.slice(0, 10);
    }

    // Reinicio completo del test (nuevas preguntas, resetear índice)
    function fullReset() {
        if (timeoutId) clearTimeout(timeoutId);
        currentQuestions = getRandomQuestions();
        currentIndex = 0;
        waitingResponse = false;
        renderCurrentQuestion();
        statusMessage.innerHTML = "⚠️ Reinicio automático: nuevas preguntas. ¡Ánimo!";
        setTimeout(() => {
            if (statusMessage) statusMessage.innerHTML = "⚡ Una falla = reinicio total + preguntas nuevas";
        }, 1800);
    }

    // Manejar respuesta del usuario
    function handleAnswer(selectedLetter, selectedText, correctLetter) {
        if (waitingResponse) return;       // ya se eligió, esperando avance
        waitingResponse = true;

        const isCorrect = (selectedLetter === correctLetter);

        if (isCorrect) {
            // Respuesta correcta: avanzar o finalizar
            showFeedback(true, selectedLetter, correctLetter);
            // Avanzar después de breve pausa
            timeoutId = setTimeout(() => {
                // Si completó el test
                if (currentIndex + 1 >= currentQuestions.length) {
                    // Ganó el test
                    renderWinScreen();
                } else {
                    // pasar a siguiente pregunta
                    currentIndex++;
                    renderCurrentQuestion();
                }
                waitingResponse = false;
                timeoutId = null;
            }, 800);
        } else {
            // Respuesta incorrecta: mostrar mensaje de error y REINICIO TOTAL con nuevas preguntas
            showFeedback(false, selectedLetter, correctLetter);
            timeoutId = setTimeout(() => {
                fullReset();   // reinicia todo (nuevas 10 preguntas)
                waitingResponse = false;
                timeoutId = null;
            }, 1200);
        }
    }

    function showFeedback(isCorrect, selected, correctLetter) {
        const questionContainer = document.querySelector('.question-card');
        if (!questionContainer) return;
        let feedbackDiv = document.querySelector('.feedback-temp');
        if (feedbackDiv) feedbackDiv.remove();
        feedbackDiv = document.createElement('div');
        feedbackDiv.className = isCorrect ? 'correct-feedback feedback-temp' : 'wrong-feedback feedback-temp';
        if (isCorrect) {
            feedbackDiv.innerHTML = '✅ ¡Correcto! Avanzando...';
        } else {
            const correctOptionLetter = correctLetter;
            let correctText = "";
            const currentQ = currentQuestions[currentIndex];
            const idx = correctOptionLetter.charCodeAt(0) - 65;
            if (currentQ && currentQ.options[idx]) correctText = currentQ.options[idx];
            feedbackDiv.innerHTML = `❌ Incorrecto. La respuesta correcta era ${correctOptionLetter}: ${correctText}. <strong>🔁 Reiniciando test con preguntas nuevas...</strong>`;
        }
        questionContainer.appendChild(feedbackDiv);
    }

    function renderCurrentQuestion() {
        if (!currentQuestions.length || currentIndex >= currentQuestions.length) {
            fullReset();
            return;
        }
        const q = currentQuestions[currentIndex];
        // Actualizar progreso
        progressDisplay.innerText = `Progreso: ${currentIndex} / 10`;

        // Construir HTML
        const letters = ['A', 'B', 'C', 'D'];
        const optionsHtml = q.options.map((opt, idx) => {
            const letter = letters[idx];
            return `
                <div class="option-btn" data-letter="${letter}" data-opt="${opt.replace(/"/g, '&quot;')}">
                    <span class="option-prefix">${letter}</span>
                    <span>${opt}</span>
                </div>
            `;
        }).join('');

        const html = `
            <div class="question-card">
                <div class="question-text">${q.text}</div>
                <div class="options" id="optionsContainer">
                    ${optionsHtml}
                </div>
            </div>
        `;
        quizPanel.innerHTML = html;

        // Agregar eventos a los botones
        const optButtons = document.querySelectorAll('.option-btn');
        optButtons.forEach(btn => {
            btn.addEventListener('click', (e) => {
                if (waitingResponse) return;
                const letter = btn.getAttribute('data-letter');
                const optText = btn.getAttribute('data-opt');
                handleAnswer(letter, optText, q.correct);
            });
        });
    }

    function renderWinScreen() {
        progressDisplay.innerText = `Progreso: 10 / 10 🎉`;
        quizPanel.innerHTML = `
            <div class="congrats">
                <h2>🏆 ¡FELICIDADES! 🏆</h2>
                <p>Respondiste 10 preguntas correctas SEGUIDAS.</p>
                <p>Demostraste dominio sobre la Independencia de América Latina.</p>
                <button class="btn-restart" id="winRestartBtn">📚 Comenzar un nuevo test</button>
            </div>
        `;
        const restartBtn = document.getElementById('winRestartBtn');
        if (restartBtn) {
            restartBtn.addEventListener('click', () => {
                fullReset();
            });
        }
        waitingResponse = false; // ya no hay bloqueo por respuesta
        if (timeoutId) clearTimeout(timeoutId);
    }

    // Inicializar el test por primera vez
    function initTest() {
        currentQuestions = getRandomQuestions();
        currentIndex = 0;
        waitingResponse = false;
        renderCurrentQuestion();
    }

    initTest();
</script>
</body>
</html>
