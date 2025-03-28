<h2 align="left">Hi 👋! My name is ducalofc and I'm a developer, from html, css, js</h2>

###

<div align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=Itzduduzin1&hide_title=false&hide_rank=false&show_icons=true&include_all_commits=true&count_private=true&disable_animations=false&theme=dracula&locale=en&hide_border=false" height="150" alt="stats graph"  />
  <img src="https://github-readme-stats.vercel.app/api/top-langs?username=Itzduduzin1&locale=en&hide_title=false&layout=compact&card_width=320&langs_count=5&theme=dracula&hide_border=false" height="150" alt="languages graph"  />
</div>

###

<img align="right" height="150" src="https://itzduduzin1.github.io/ducalofc-portfolio/img/logo.png"  />

###

<div align="left">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" height="30" alt="javascript logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" height="30" alt="typescript logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" height="30" alt="react logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" height="30" alt="html5 logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" height="30" alt="css3 logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" height="30" alt="python logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/csharp/csharp-original.svg" height="30" alt="csharp logo"  />
</div>

###

<div align="left">
  <img src="https://img.shields.io/static/v1?message=Youtube&logo=youtube&label=&color=FF0000&logoColor=white&labelColor=&style=for-the-badge" height="35" alt="youtube logo"  />
  <img src="https://img.shields.io/static/v1?message=Instagram&logo=instagram&label=&color=E4405F&logoColor=white&labelColor=&style=for-the-badge" height="35" alt="instagram logo"  />
  <img src="https://img.shields.io/static/v1?message=Twitch&logo=twitch&label=&color=9146FF&logoColor=white&labelColor=&style=for-the-badge" height="35" alt="twitch logo"  />
  <img src="https://img.shields.io/static/v1?message=Discord&logo=discord&label=&color=7289DA&logoColor=white&labelColor=&style=for-the-badge" height="35" alt="discord logo"  />
  <img src="https://img.shields.io/static/v1?message=Gmail&logo=gmail&label=&color=D14836&logoColor=white&labelColor=&style=for-the-badge" height="35" alt="gmail logo"  />
  <img src="https://img.shields.io/static/v1?message=LinkedIn&logo=linkedin&label=&color=0077B5&logoColor=white&labelColor=&style=for-the-badge" height="35" alt="linkedin logo"  />
</div>

###

<br clear="both">

###

javascript:(function(){
    // Configuração da API
    const GEMINI_API_KEY = 'AIzaSyAXAWwF0j-ksvgVf4VrqSoj8Bm74f6z_CQ';
    const GEMINI_API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent';

    // Criar interface
    function createUI() {
        // Remover UI existente
        const existingUI = document.getElementById('gemini-helper-container');
        if (existingUI) existingUI.remove();

        // Container principal
        const container = document.createElement('div');
        container.id = 'gemini-helper-container';
        container.style.position = 'fixed';
        container.style.bottom = '20px';
        container.style.right = '20px';
        container.style.zIndex = '999999';
        container.style.display = 'flex';
        container.style.flexDirection = 'column';
        container.style.gap = '10px';

        // Botão de ação
        const actionBtn = document.createElement('button');
        actionBtn.id = 'gemini-helper-btn';
        actionBtn.textContent = '🔍 Encontrar Resposta';
        actionBtn.style.padding = '12px 20px';
        actionBtn.style.backgroundColor = '#4285f4';
        actionBtn.style.color = 'white';
        actionBtn.style.border = 'none';
        actionBtn.style.borderRadius = '24px';
        actionBtn.style.cursor = 'pointer';
        actionBtn.style.fontWeight = 'bold';
        actionBtn.style.boxShadow = '0 2px 10px rgba(0, 0, 0, 0.2)';

        // Painel de resposta
        const responsePanel = document.createElement('div');
        responsePanel.id = 'gemini-response-panel';
        responsePanel.style.display = 'none';
        responsePanel.style.backgroundColor = 'white';
        responsePanel.style.borderRadius = '12px';
        responsePanel.style.padding = '15px';
        responsePanel.style.boxShadow = '0 4px 15px rgba(0, 0, 0, 0.2)';
        responsePanel.style.maxWidth = '300px';

        // Montar UI
        container.appendChild(actionBtn);
        container.appendChild(responsePanel);
        document.body.appendChild(container);

        return { actionBtn, responsePanel };
    }

    // Extrair todo o conteúdo textual e imagens da página
    function extractPageContent() {
        // Clonar o body para não afetar o DOM original
        const bodyClone = document.cloneNode(true);

        // Remover elementos indesejados
        const unwantedTags = ['script', 'style', 'noscript', 'svg', 'iframe', 'head'];
        unwantedTags.forEach(tag => {
            bodyClone.querySelectorAll(tag).forEach(el => el.remove());
        });

        // Extrair imagens e texto
        const images = Array.from(bodyClone.querySelectorAll('img')).map(img => img.src);
        const textContent = bodyClone.body.textContent
            .replace(/\s+/g, ' ')
            .substring(0, 15000);  // Limitar o tamanho do texto extraído

        return { textContent, images };
    }

    // Analisar com Gemini (usando modelo flash)
    async function analyzeContent(text, images) {
        const prompt = `Analise este conteúdo de página e responda APENAS com a informação solicitada de forma direta:\n\n${text}\n\nResposta:`;

        try {
            const response = await fetch(`${GEMINI_API_URL}?key=${GEMINI_API_KEY}`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    contents: [{ parts: [{ text: prompt }] }],
                    generationConfig: {
                        maxOutputTokens: 100,
                        temperature: 0.2,
                    },
                }),
            });
