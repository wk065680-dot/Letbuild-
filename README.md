<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text to Website · Demo</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
        }
        body {
            background: #f1f5f9;
            margin: 0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1rem;
        }
        .card {
            max-width: 1200px;
            width: 100%;
            background: white;
            border-radius: 2rem;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.25);
            overflow: hidden;
        }
        .header {
            background: #0f172a;
            color: white;
            padding: 2rem 2.5rem;
        }
        .header h1 {
            margin: 0;
            font-weight: 600;
            font-size: 2rem;
            letter-spacing: -0.02em;
        }
        .header p {
            margin: 0.5rem 0 0;
            opacity: 0.8;
        }
        .content {
            padding: 2.5rem;
        }
        .input-area {
            margin-bottom: 2rem;
        }
        label {
            display: block;
            font-weight: 500;
            margin-bottom: 0.5rem;
            color: #1e293b;
        }
        textarea {
            width: 100%;
            padding: 1rem;
            border: 2px solid #e2e8f0;
            border-radius: 1rem;
            font-size: 1rem;
            resize: vertical;
            transition: border 0.2s;
        }
        textarea:focus {
            outline: none;
            border-color: #3b82f6;
        }
        button {
            background: #0f172a;
            color: white;
            border: none;
            padding: 0.75rem 2rem;
            font-size: 1rem;
            font-weight: 500;
            border-radius: 3rem;
            cursor: pointer;
            margin-top: 1rem;
            transition: background 0.2s;
        }
        button:hover {
            background: #1e293b;
        }
        .preview {
            margin-top: 2.5rem;
            border-top: 2px solid #e2e8f0;
            padding-top: 2rem;
        }
        .preview h2 {
            font-weight: 500;
            color: #0f172a;
            margin-bottom: 1rem;
        }
        iframe {
            width: 100%;
            height: 400px;
            border: 2px solid #e2e8f0;
            border-radius: 1rem;
            background: white;
        }
        .note {
            background: #f8fafc;
            padding: 1rem 1.5rem;
            border-radius: 1rem;
            margin-top: 1rem;
            font-size: 0.9rem;
            color: #475569;
        }
        .note code {
            background: #e2e8f0;
            padding: 0.2rem 0.4rem;
            border-radius: 0.3rem;
        }
    </style>
</head>
<body>
    <div class="card">
        <div class="header">
            <h1>✧ Text to Website (demo)</h1>
            <p>Describe the site you want — we’ll build a simple preview using keywords.</p>
        </div>
        <div class="content">
            <div class="input-area">
                <label for="desc">Describe your website</label>
                <textarea id="desc" rows="4" placeholder="e.g. a dark themed portfolio with a header, three project cards and a contact section">dark themed portfolio with a header, three project cards and a contact section</textarea>
                <button id="generateBtn">Generate Website</button>
            </div>
            <div class="preview">
                <h2>⚡ Live preview</h2>
                <iframe id="previewFrame" sandbox="allow-same-origin allow-scripts allow-popups allow-forms" title="Generated site"></iframe>
            </div>
            <div class="note">
                <strong>🔍 How it works:</strong> This demo uses keyword matching (dark, portfolio, cards, blog, etc.) to assemble a basic HTML page. For a real AI version, connect to the <a href="https://platform.openai.com/docs/api-reference/chat" target="_blank">OpenAI API</a> (or similar) and ask it to generate complete HTML/CSS.
            </div>
        </div>
    </div>
    <script>
        (function() {
            const textarea = document.getElementById('desc');
            const btn = document.getElementById('generateBtn');
            const iframe = document.getElementById('previewFrame');

            // Mock AI generator – replaces keywords with simple components
            function generateHTML(description) {
                const desc = description.toLowerCase();

                // Determine theme
                const isDark = desc.includes('dark') || desc.includes('black') || desc.includes('night');
                const theme = isDark ? {
                    bg: '#1e1e2f',
                    text: '#f0eefc',
                    cardBg: '#2d2d44',
                    accent: '#a78bfa',
                    headerBg: '#2a2a3b'
                } : {
                    bg: '#ffffff',
                    text: '#1f2937',
                    cardBg: '#f9fafb',
                    accent: '#3b82f6',
                    headerBg: '#f3f4f6'
                };

                // Detect page type / sections
                const isPortfolio = desc.includes('portfolio') || desc.includes('project') || desc.includes('work');
                const isBlog = desc.includes('blog') || desc.includes('post') || desc.includes('article');
                const isLanding = desc.includes('landing') || desc.includes('startup') || desc.includes('product');
                const hasContact = desc.includes('contact') || desc.includes('email') || desc.includes('get in touch');
                const hasAbout = desc.includes('about') || desc.includes('who we are');
                const hasHeader = desc.includes('header') || desc.includes('navbar') || desc.includes('navigation');
                const cardCount = desc.includes('three') ? 3 : (desc.includes('four') ? 4 : (desc.includes('two') ? 2 : 3));

                // Start building HTML
                let html = `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generated Site</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: system-ui, -apple-system, sans-serif; }
        body { background: ${theme.bg}; color: ${theme.text}; line-height: 1.6; padding: 2rem; }
        .container { max-width: 1200px; margin: 0 auto; }
        header { background: ${theme.headerBg}; padding: 2rem; border-radius: 1rem; margin-bottom: 2rem; }
        h1 { color: ${theme.accent}; margin-bottom: 1rem; }
        h2 { margin: 2rem 0 1rem; color: ${theme.accent}; }
        .card-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 1.5rem; margin: 1.5rem 0; }
        .card { background: ${theme.cardBg}; padding: 1.5rem; border-radius: 1rem; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .card h3 { margin-bottom: 0.5rem; }
        .btn { background: ${theme.accent}; color: white; border: none; padding: 0.6rem 1.5rem; border-radius: 2rem; cursor: pointer; display: inline-block; margin-top: 1rem; text-decoration: none; }
        .contact-form { background: ${theme.cardBg}; padding: 2rem; border-radius: 1rem; margin-top: 2rem; }
        input, textarea { width: 100%; padding: 0.8rem; margin-bottom: 1rem; border: 1px solid #ccc; border-radius: 0.5rem; background: ${theme.bg}; color: ${theme.text}; }
        footer { margin-top: 3rem; text-align: center; opacity: 0.7; }
    </style>
</head>
<body>
    <div class="container">
`;

                // Header
                if (hasHeader) {
                    html += `        <header>
            <h1>✨ My Awesome Site</h1>
            <nav style="display: flex; gap: 2rem; margin-top: 1rem;">
                <a href="#" style="color: ${theme.text};">Home</a>
                <a href="#" style="color: ${theme.text};">Work</a>
                <a href="#" style="color: ${theme.text};">About</a>
                <a href="#" style="color: ${theme.text};">Contact</a>
            </nav>
        </header>\n`;
                } else {
                    html += `        <h1>✨ My Awesome Site</h1>\n`;
                }

                // About section
                if (hasAbout) {
                    html += `        <section>
            <h2>About</h2>
            <p>We are a creative team building amazing digital experiences. This section was generated from your description.</p>
        </section>\n`;
                }

                // Portfolio / project section
                if (isPortfolio) {
                    html += `        <h2>Projects</h2>
        <div class="card-grid">\n`;
                    for (let i = 1; i <= cardCount; i++) {
                        html += `            <div class="card">
                <h3>Project ${i}</h3>
                <p>A short description of this amazing project. It solves real problems with elegant design.</p>
                <a href="#" class="btn">View case</a>
            </div>\n`;
                    }
                    html += `        </div>\n`;
                }

                // Blog section
                if (isBlog) {
                    html += `        <h2>Latest posts</h2>
        <div class="card-grid">\n`;
                    for (let i = 1; i <= 3; i++) {
                        html += `            <div class="card">
                <h3>Blog post ${i}</h3>
                <p>Excerpt from the article where we discuss industry trends and insights.</p>
                <a href="#" class="btn">Read more</a>
            </div>\n`;
                    }
                    html += `        </div>\n`;
                }

                // Landing / product section
                if (isLanding) {
                    html += `        <section style="background: ${theme.cardBg}; padding: 3rem; border-radius: 2rem; text-align: center; margin: 2rem 0;">
            <h2 style="font-size: 2rem;">Supercharge your workflow</h2>
            <p style="max-width: 600px; margin: 1rem auto;">Our product helps teams collaborate faster and ship better code.</p>
            <a href="#" class="btn" style="font-size: 1.2rem;">Get started</a>
        </section>\n`;
                }

                // Contact section
                if (hasContact) {
                    html += `        <h2>Contact us</h2>
        <div class="contact-form">
            <form>
                <input type="text" placeholder="Your name">
                <input type="email" placeholder="Email address">
                <textarea rows="3" placeholder="Your message"></textarea>
                <button class="btn" type="submit">Send message</button>
            </form>
        </div>\n`;
                }

                // Footer
                html += `        <footer>
            <p>© 2025 · Generated with ❤️ by the demo AI</p>
        </footer>
    </div>
</body>
</html>`;

                return html;
            }

            function updatePreview() {
                const description = textarea.value.trim() || "a simple page";
                const generatedHTML = generateHTML(description);
                const blob = new Blob([generatedHTML], { type: 'text/html' });
                const url = URL.createObjectURL(blob);
                iframe.src = url;
                // Clean up after load
                iframe.onload = () => URL.revokeObjectURL(url);
            }

            btn.addEventListener('click', updatePreview);

            // Load initial example
            updatePreview();
        })();
    </script>
</body>
</html>
