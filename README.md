Quiz Interativo: Vigilância Ambiental e Controle de Doenças ;)

📒 Descrição

Um quiz interativo e responsivo sobre Vigilância Ambiental e Controle de Doenças, criado para fins educacionais e de exposição.

O projeto consiste em um único arquivo HTML que funciona 100% offline. Ele seleciona aleatoriamente 10 perguntas de um banco total de 20, desafiando o usuário com um cronômetro de 15 segundos por questão. Ao final, exibe um feedback personalizado baseado no desempenho (acima ou abaixo de 50% de acertos).

🤖 Tecnologias Utilizadas

IA Generativa: Google Gemini (utilizado para geração de código, revisão, e brainstorming das perguntas).

Linguagens: HTML5, CSS3, JavaScript (ES6+).

Framework/Biblioteca: Tailwind CSS (para o design responsivo e moderno).

🧐 Processo de Criação

O projeto foi desenvolvido de forma iterativa, em colaboração direta com o Google Gemini.

Conceituação: A ideia inicial era um quiz sobre o tema, com um banco de 20 perguntas e seleção aleatória de 10 por rodada.

Features de Jogo: Adicionamos um cronômetro de 15 segundos por questão para tornar o jogo mais dinâmico.

Pivô (Offline): O projeto foi adaptado de uma versão inicial online (que usava Firebase para ranking) para uma versão 100% offline, garantindo o uso em qualquer ambiente, como exposições sem acesso à internet.

Refinamento de UX: Incluímos uma tela de início para melhorar o fluxo do usuário e uma tela de resultado com mensagens de feedback personalizadas (Parabéns/Motivação) baseadas na pontuação.

Design: O design foi construído com Tailwind CSS para garantir uma interface limpa, moderna (em tons de verde) e totalmente responsiva.

🚀 Resultados

O resultado final é um arquivo index.html único, leve (menos de 20KB sem o Tailwind) e totalmente autônomo (standalone).

O quiz é uma ferramenta educacional eficaz e portátil, ideal para eventos e exposições, rodando instantaneamente em qualquer navegador moderno, seja desktop ou mobile.

💭 Reflexão (Opcional)

O desafio foi utilizar a IA (Gemini) não apenas para gerar o código-base, mas para refinar iterativamente a lógica do JavaScript (como o timer, a seleção aleatória e as transições de tela) e o design (Tailwind CSS) em um único arquivo.

Conseguir um produto 'natty' (estiloso e funcional) 100% offline, que equilibra estética e performance, demonstra o poder da colaboração humano-IA no desenvolvimento rápido de protótipos funcionais.

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz: Vigilância Ambiental e Controle de Doenças</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;800&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --green-dark: #065F46; /* Emerald 800 */
            --green-primary: #059669; /* Emerald 600 */
            --green-light: #D1FAE5; /* Emerald 100 */
            --green-bg: #ECFDF5; /* Emerald 50 */
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--green-bg);
        }
        .btn-option {
            transition: all 0.2s;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
        }
        .btn-option:hover:not(.correct, .incorrect) {
            background-color: #047857; /* Emerald 700 */
        }
        .correct {
            background-color: #10B981 !important; /* Emerald 500 */
            border-color: #059669 !important;
            color: white !important;
        }
        .incorrect {
            background-color: #EF4444 !important; /* Red 500 */
            border-color: #DC2626 !important;
            color: white !important;
        }
        .timer-bar-container {
            width: 100%;
            height: 10px;
            background-color: #E5E7EB;
            border-radius: 9999px;
            overflow: hidden;
            margin-bottom: 1rem;
        }
        #timer-bar {
            height: 100%;
            background-color: var(--green-primary);
            transition: width 15s linear; /* Tempo total da barra */
        }

        /* Classes para controle de visibilidade */
        .hidden { display: none; }

        @keyframes pulse-red {
            0%, 100% { background-color: #EF4444; }
            50% { background-color: #F87171; }
        }
        .pulse-red {
            animation: pulse-red 1s infinite;
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-4xl mx-auto">
        
        <!-- Contêiner Principal do Quiz (Full Width) -->
        <div id="quiz-container" class="w-full bg-white p-6 md:p-10 rounded-xl shadow-2xl border-t-8 border-green-600">
            
            <h1 class="text-3xl font-extrabold text-center mb-6 text-green-800">
                Quiz da Vigilância Ambiental e Controle de Doenças
            </h1>

            <!-- 1. TELA DE INÍCIO -->
            <div id="start-screen" class="hidden text-center py-10">
                <p class="text-xl text-gray-700 mb-8">
                    Teste seus conhecimentos sobre Vigilância Ambiental e Controle de Doenças.<br>
                    Você responderá a 10 perguntas aleatórias e terá 15 segundos para cada uma.
                </p>
                <button 
                    class="px-8 py-3 bg-green-600 text-white font-bold text-lg rounded-xl hover:bg-green-700 transition duration-150 shadow-lg"
                    onclick="startQuiz()"
                >
                    Começar o Quiz
                </button>
            </div>

            <!-- 2. TELA DE PERGUNTAS -->
            <div id="question-screen" class="hidden">
                <p class="text-lg text-green-700 font-semibold mb-2">
                    Questão <span id="question-number">1</span> de 10
                </p>

                <div class="timer-bar-container">
                    <div id="timer-bar"></div>
                </div>

                <div id="question-text" class="text-2xl font-bold mb-6 text-gray-800 bg-green-50 p-4 rounded-lg border-l-4 border-green-400">
                    
                </div>
                
                <div id="options-container" class="space-y-4">
                    <!-- Opções serão inseridas aqui pelo JS -->
                </div>
            </div>

            <!-- 3. TELA DE RESULTADOS -->
            <div id="result-screen" class="hidden text-center py-10">
                <h2 class="text-4xl font-extrabold mb-4 text-green-800">Fim do Quiz!</h2>
                <div class="text-6xl font-extrabold mb-6 text-green-600" id="final-score"></div>
                
                <div id="feedback-message" class="text-xl font-semibold mb-8 h-10"></div>
                
                <div id="ranking-submission" class="flex flex-col items-center space-y-4">
                    <button 
                        class="px-8 py-3 bg-green-600 text-white font-bold text-lg rounded-xl hover:bg-green-700 transition duration-150 shadow-lg"
                        onclick="showStartScreen()"
                    >
                        Jogar Novamente
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const QUESTIONS_POOL = [
            {
                text: "Qual é o principal vetor de transmissão da Dengue, Zika e Chikungunya no Brasil?",
                options: ["Aedes albopictus", "Culex quinquefasciatus", "Aedes aegypti", "Anopheles darlingi"],
                correct: "Aedes aegypti",
                category: "Arboviroses"
            },
            {
                text: "O que caracteriza a Vigilância em Saúde Ambiental (VSA) de forma abrangente?",
                options: ["Foco exclusivo no tratamento de doenças infecciosas.", "Monitoramento e intervenção nos fatores ambientais que afetam a saúde humana.", "Controle de pragas urbanas somente em áreas rurais.", "Distribuição de medicamentos para a população."],
                correct: "Monitoramento e intervenção nos fatores ambientais que afetam a saúde humana.",
                category: "Geral"
            },
            {
                text: "A Leptospirose é uma zoonose tipicamente transmitida pelo contato com água ou lama contaminada pela urina de qual animal?",
                options: ["Gato doméstico", "Cão de rua", "Morcego", "Rato (roedores)"],
                correct: "Rato (roedores)",
                category: "Zoonoses"
            },
            {
                text: "Qual das seguintes ações é um foco primário da Vigilância da Qualidade do Ar?",
                options: ["Monitorar a temperatura ambiente.", "Medir a concentração de poluentes atmosféricos.", "Fiscalizar o uso de agrotóxicos.", "Controlar o nível de ruído em áreas urbanas."],
                correct: "Medir a concentração de poluentes atmosféricos.",
                category: "Riscos Não-biológicos"
            },
            {
                text: "O principal parasita causador da Malária no Brasil e na região amazônica é:",
                options: ["Plasmodium vivax", "Plasmodium falciparum", "Toxoplasma gondii", "Leishmania chagasi"],
                correct: "Plasmodium vivax",
                category: "Malária"
            },
            {
                text: "Qual é o vetor responsável pela transmissão da Malária?",
                options: ["Mosquito Aedes aegypti", "Mosquito Culex", "Mosquito Anopheles", "Mosquito Phlebotominae"],
                correct: "Mosquito Anopheles",
                category: "Malária"
            },
            {
                text: "A Doença de Chagas é causada pelo parasita *Trypanosoma cruzi* e é transmitida por qual inseto vetor?",
                options: ["Aedes aegypti", "Barbeiro (Triatomíneo)", "Pulga do rato", "Carrapato estrela"],
                correct: "Barbeiro (Triatomíneo)",
                category: "Zoonoses"
            },
            {
                text: "O que significa o termo 'riscos não-biológicos' dentro do contexto da Vigilância Ambiental?",
                options: ["Ameaças causadas por plantas e fungos.", "Riscos relacionados a vetores e parasitas.", "Perigos de origem física, química ou mecânica no ambiente.", "Doenças transmitidas exclusivamente pela água."],
                correct: "Perigos de origem física, química ou mecânica no ambiente.",
                category: "Riscos Não-biológicos"
            },
            {
                text: "Qual componente da VSA é responsável pelo monitoramento de agrotóxicos e outras substâncias químicas na água para consumo humano?",
                options: ["Vigidesastres", "Vigiagro", "Vigiágua", "Vigisolo"],
                correct: "Vigiágua",
                category: "Riscos Não-biológicos"
            },
            {
                text: "Qual sintoma NÃO é tipicamente associado à Dengue, Zika e Chikungunya?",
                options: ["Febre alta e dor de cabeça", "Manchas vermelhas no corpo", "Dores nas articulações", "Convulsões epilépticas"],
                correct: "Convulsões epilépticas",
                category: "Arboviroses"
            },
            {
                text: "A Febre Amarela Silvestre é uma zoonose que envolve quais hospedeiros e vetores no ciclo natural?",
                options: ["Ratos e pulgas", "Macacos e mosquitos Haemagogus", "Cães e carrapatos", "Morcegos e mosquitos Aedes"],
                correct: "Macacos e mosquitos Haemagogus",
                category: "Zoonoses"
            },
            {
                text: "Em caso de surtos de Malária, qual é a principal medida de controle do vetor?",
                options: ["Vacinação em massa", "Distribuição de água potável", "Controle químico de larvas e mosquitos adultos (uso de telas)", "Isolamento de todos os pacientes"],
                correct: "Controle químico de larvas e mosquitos adultos (uso de telas)",
                category: "Malária"
            },
            {
                text: "A vigilância de áreas contaminadas é uma ação focada em qual risco ambiental?",
                options: ["Risco biológico", "Risco físico e químico", "Risco de desastres naturais", "Risco social"],
                correct: "Risco físico e químico",
                category: "Riscos Não-biológicos"
            },
            {
                text: "O que são criadouros do Aedes aegypti?",
                options: ["Locais de desova de peixes", "Acúmulo de lixo seco", "Recipientes com água parada e limpa ou suja", "Locais de procriação de roedores"],
                correct: "Recipientes com água parada e limpa ou suja",
                category: "Arboviroses"
            },
            {
                text: "A raiva humana é uma zoonose grave. Qual é o principal animal transmissor em áreas urbanas?",
                options: ["Galinhas", "Cães e gatos", "Macacos", "Pássaros"],
                correct: "Cães e gatos",
                category: "Zoonoses"
            },
            {
                text: "Qual é o período de incubação típico da Malária (antes do aparecimento dos sintomas)?",
                options: ["1 a 3 dias", "2 a 4 semanas", "6 meses a 1 ano", "12 a 24 horas"],
                correct: "2 a 4 semanas",
                category: "Malária"
            },
            {
                text: "A qualidade da água para consumo humano é monitorada para prevenir doenças como:",
                options: ["Câncer de pele", "Diabetes", "Doenças de veiculação hídrica (ex: cólera, diarreias)", "Gripe suína"],
                correct: "Doenças de veiculação hídrica (ex: cólera, diarreias)",
                category: "Riscos Não-biológicos"
            },
            {
                text: "O que o indicador 'Índice de Breteau' mede no controle do Aedes aegypti?",
                options: ["Densidade de larvas por habitante", "Número de óbitos por Dengue", "Porcentagem de imóveis com presença de Aedes", "Nível de infestação de mosquitos adultos"],
                correct: "Porcentagem de imóveis com presença de Aedes",
                category: "Arboviroses"
            },
            {
                text: "A exposição prolongada a altos níveis de ruído pode ser classificada como qual tipo de risco ambiental à saúde?",
                options: ["Risco químico", "Risco biológico", "Risco físico", "Risco social"],
                correct: "Risco físico",
                category: "Riscos Não-biológicos"
            },
            {
                text: "O controle de animais sinantrópicos, como pombos, tem como objetivo principal prevenir doenças como:",
                options: ["Tuberculose", "Gripe H1N1", "Criptococose e Histoplasmose", "Febre amarela"],
                correct: "Criptococose e Histoplasmose",
                category: "Zoonoses"
            }
        ];

        let quizQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let timer = 15;
        let timerInterval;
        let isLocked = false;
        const TOTAL_QUESTIONS_PER_ROUND = 10;

        // Elementos do DOM
        const startScreen = document.getElementById('start-screen');
        const questionScreen = document.getElementById('question-screen');
        const resultScreen = document.getElementById('result-screen');
        const questionNumberEl = document.getElementById('question-number');
        const questionTextEl = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const finalScoreEl = document.getElementById('final-score');
        const feedbackMessageEl = document.getElementById('feedback-message');
        const timerBar = document.getElementById('timer-bar');

        // --- Funções de Controle de Tela ---
        
        const showScreen = (screenId) => {
            [startScreen, questionScreen, resultScreen].forEach(el => el.classList.add('hidden'));
            document.getElementById(screenId).classList.remove('hidden');
        };

        const showStartScreen = () => {
            showScreen('start-screen');
            questionScreen.classList.remove('flex', 'flex-col'); // Garante que a tela de pergunta não está visível
        };

        // --- Funções de Controle do Quiz ---

        const shuffleArray = (array) => {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        };

        window.startQuiz = () => {
            // Seleciona 10 perguntas aleatórias
            quizQuestions = shuffleArray([...QUESTIONS_POOL]).slice(0, TOTAL_QUESTIONS_PER_ROUND);
            currentQuestionIndex = 0;
            score = 0;
            showScreen('question-screen');
            questionScreen.classList.add('flex', 'flex-col');
            loadQuestion();
        };

        const loadQuestion = () => {
            if (currentQuestionIndex >= TOTAL_QUESTIONS_PER_ROUND) {
                endQuiz();
                return;
            }

            isLocked = false;
            clearInterval(timerInterval);
            timer = 15;

            const q = quizQuestions[currentQuestionIndex];
            questionNumberEl.textContent = currentQuestionIndex + 1;
            questionTextEl.textContent = q.text;
            optionsContainer.innerHTML = '';

            // Reinicia a barra do timer
            timerBar.style.width = '100%';
            timerBar.classList.remove('pulse-red');
            timerBar.style.transition = 'width 15s linear';
            
            // Inicia o intervalo de contagem regressiva
            timerInterval = setTimeout(() => advanceQuestion(null, true), 15000);
            
            // Cria as opções de resposta
            const shuffledOptions = shuffleArray([...q.options]);

            shuffledOptions.forEach(option => {
                const button = document.createElement('button');
                button.className = "btn-option w-full p-4 text-left bg-green-500 text-white rounded-lg font-semibold text-lg border-2 border-green-600";
                button.textContent = option;
                button.onclick = () => advanceQuestion(option);
                optionsContainer.appendChild(button);
            });
        };

        const advanceQuestion = (selectedOption, timedOut = false) => {
            if (isLocked) return;
            isLocked = true;
            clearTimeout(timerInterval);
            
            const q = quizQuestions[currentQuestionIndex];
            let isCorrect = false;

            if (selectedOption !== null) {
                isCorrect = selectedOption === q.correct;
                if (isCorrect) score++;
            }

            // Destaca a opção correta e incorreta (se houver)
            const optionButtons = optionsContainer.querySelectorAll('button');
            optionButtons.forEach(button => {
                button.onclick = null; // Desabilita cliques
                if (button.textContent === q.correct) {
                    button.classList.add('correct');
                } else if (button.textContent === selectedOption) {
                    button.classList.add('incorrect');
                }
            });

            if (timedOut) {
                 // Mostra a barra vermelha para indicar o timeout
                 timerBar.style.width = '0%';
                 timerBar.style.transition = 'none';
                 timerBar.classList.add('pulse-red');
            }

            // Avança para a próxima pergunta após 2 segundos
            setTimeout(() => {
                currentQuestionIndex++;
                loadQuestion();
            }, 2000);
        };

        const endQuiz = () => {
            clearInterval(timerInterval);
            showScreen('result-screen');
            
            const percentage = (score / TOTAL_QUESTIONS_PER_ROUND) * 100;
            finalScoreEl.textContent = `${score} / ${TOTAL_QUESTIONS_PER_ROUND}`;
            
            if (percentage > 50) {
                feedbackMessageEl.innerHTML = `<span class="text-green-600">🎉 Parabéns! Excelente conhecimento em VACD!</span>`;
            } else {
                feedbackMessageEl.innerHTML = `<span class="text-red-600">📚 Continue aprendendo! A Vigilância Ambiental é crucial.</span>`;
            }
        };

        // Inicia na tela de início
        window.onload = showStartScreen;
    </script>
</body>
</html>
