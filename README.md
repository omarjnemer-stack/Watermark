## Example Code
index.html
```html
<!DOCTYPE html>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF Watermark Studio Pro</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <style>
        :root {
            --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            --secondary-gradient: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            --success-gradient: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            --error-gradient: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
            --glass-bg: rgba(255, 255, 255, 0.25);
            --glass-border: rgba(255, 255, 255, 0.18);
            --text-primary: #2d3748;
            --text-secondary: #4a5568;
            --shadow-soft: 0 8px 32px rgba(31, 38, 135, 0.37);
            --shadow-hard: 0 15px 35px rgba(31, 38, 135, 0.5);
        }

```
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
        background: linear-gradient(135deg, #1e3c72 0%, #2a5298 25%, #667eea 50%, #764ba2 75%, #f093fb 100%);
        background-size: 400% 400%;
        animation: gradientShift 15s ease infinite;
        min-height: 100vh;
        padding: 20px;
        color: var(--text-primary);
    }

    @keyframes gradientShift {
        0% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }

    .container {
        max-width: 1400px;
        margin: 0 auto;
        background: var(--glass-bg);
        backdrop-filter: blur(20px);
        border-radius: 30px;
        padding: 50px;
        box-shadow: var(--shadow-soft);
        border: 1px solid var(--glass-border);
        position: relative;
        overflow: hidden;
    }

    .container::before {
        content: '';
        position: absolute;
        top: -50%;
        left: -50%;
        width: 200%;
        height: 200%;
        background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
        animation: shimmer 3s ease-in-out infinite;
        pointer-events: none;
    }

    @keyframes shimmer {
        0%, 100% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
        50% { transform: translateX(100%) translateY(100%) rotate(45deg); }
    }

    .header {
        text-align: center;
        margin-bottom: 50px;
        position: relative;
        z-index: 2;
    }

    h1 {
        font-size: 4em;
        font-weight: 800;
        margin-bottom: 20px;
        background: var(--primary-gradient);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        letter-spacing: -2px;
        text-shadow: 0 0 30px rgba(102, 126, 234, 0.3);
    }

    .subtitle {
        font-size: 1.4em;
        color: var(--text-secondary);
        font-weight: 300;
        margin-bottom: 30px;
        opacity: 0.9;
    }

    .stats-bar {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 20px;
        margin-bottom: 30px;
    }

    .stat-item {
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        border-radius: 15px;
        padding: 20px;
        text-align: center;
        border: 1px solid var(--glass-border);
        box-shadow: var(--shadow-soft);
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .stat-item:hover {
        transform: translateY(-5px);
        box-shadow: var(--shadow-hard);
    }

    .stat-number {
        font-size: 2em;
        font-weight: 700;
        background: var(--primary-gradient);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    .stat-label {
        font-size: 0.9em;
        color: var(--text-secondary);
        margin-top: 5px;
    }

    .main-grid {
        display: grid;
        grid-template-columns: 2fr 1fr;
        gap: 40px;
        margin-bottom: 40px;
    }

    .workspace {
        display: flex;
        flex-direction: column;
        gap: 30px;
    }

    .sidebar {
        background: var(--glass-bg);
        backdrop-filter: blur(15px);
        border-radius: 25px;
        padding: 35px;
        border: 1px solid var(--glass-border);
        box-shadow: var(--shadow-soft);
        height: fit-content;
        position: sticky;
        top: 20px;
    }

    .upload-zone {
        border: 3px dashed rgba(255, 255, 255, 0.3);
        border-radius: 20px;
        padding: 50px 30px;
        text-align: center;
        transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        position: relative;
        overflow: hidden;
        min-height: 200px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .upload-zone::before {
        content: '';
        position: absolute;
        top: -50%;
        left: -50%;
        width: 200%;
        height: 200%;
        background: conic-gradient(from 0deg, transparent, rgba(102, 126, 234, 0.1), transparent);
        animation: rotate 4s linear infinite;
        opacity: 0;
        transition: opacity 0.3s ease;
    }

    @keyframes rotate {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }

    .upload-zone:hover::before {
        opacity: 1;
    }

    .upload-zone:hover {
        border-color: rgba(102, 126, 234, 0.6);
        background: rgba(102, 126, 234, 0.05);
        transform: translateY(-3px);
        box-shadow: 0 20px 40px rgba(102, 126, 234, 0.2);
    }

    .upload-zone.dragover {
        border-color: #10b981;
        background: rgba(16, 185, 129, 0.1);
        transform: scale(1.02);
    }

    .upload-zone.processing {
        background: linear-gradient(45deg, rgba(245, 158, 11, 0.1) 25%, transparent 25%, transparent 50%, rgba(245, 158, 11, 0.1) 50%, rgba(245, 158, 11, 0.1) 75%, transparent 75%);
        background-size: 20px 20px;
        animation: processing 1s linear infinite;
    }

    @keyframes processing {
        0% { background-position: 0 0; }
        100% { background-position: 20px 20px; }
    }

    .upload-icon {
        font-size: 4em;
        margin-bottom: 20px;
        opacity: 0.7;
        transition: all 0.3s ease;
    }

    .upload-zone:hover .upload-icon {
        transform: scale(1.1);
        opacity: 1;
    }

    .upload-title {
        font-size: 1.5em;
        font-weight: 600;
        margin-bottom: 10px;
        color: var(--text-primary);
    }

    .upload-subtitle {
        color: var(--text-secondary);
        margin-bottom: 25px;
        font-size: 1.1em;
    }

    .upload-btn {
        display: inline-flex;
        align-items: center;
        gap: 12px;
        padding: 18px 35px;
        background: var(--primary-gradient);
        color: white;
        border: none;
        border-radius: 15px;
        cursor: pointer;
        font-size: 16px;
        font-weight: 600;
        transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        text-decoration: none;
        box-shadow: 0 8px 25px rgba(102, 126, 234, 0.3);
        position: relative;
        overflow: hidden;
    }

    .upload-btn::before {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
        transition: left 0.5s ease;
    }

    .upload-btn:hover::before {
        left: 100%;
    }

    .upload-btn:hover {
        transform: translateY(-3px) scale(1.05);
        box-shadow: 0 15px 35px rgba(102, 126, 234, 0.4);
    }

    .file-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
        gap: 15px;
        margin-top: 20px;
        max-height: 200px;
        overflow-y: auto;
    }

    .file-card {
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        border-radius: 12px;
        padding: 15px;
        border: 1px solid var(--glass-border);
        transition: all 0.3s ease;
        position: relative;
    }

    .file-card:hover {
        transform: translateY(-2px);
        box-shadow: var(--shadow-soft);
    }

    .file-name {
        font-weight: 600;
        font-size: 0.9em;
        margin-bottom: 5px;
        word-break: break-word;
    }

    .file-size {
        color: var(--text-secondary);
        font-size: 0.8em;
    }

    .remove-file {
        position: absolute;
        top: -5px;
        right: -5px;
        background: #ef4444;
        color: white;
        border: none;
        border-radius: 50%;
        width: 20px;
        height: 20px;
        cursor: pointer;
        font-size: 12px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.3s ease;
    }

    .remove-file:hover {
        transform: scale(1.1);
        background: #dc2626;
    }

    .preview-container {
        margin-top: 25px;
        text-align: center;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .preview-container.show {
        opacity: 1;
        transform: translateY(0);
    }

    .preview-image {
        max-width: 300px;
        max-height: 250px;
        border-radius: 15px;
        box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
        margin-bottom: 20px;
        border: 4px solid rgba(255, 255, 255, 0.3);
        transition: all 0.3s ease;
    }

    .preview-image:hover {
        transform: scale(1.05);
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
    }

    .control-panel {
        background: var(--glass-bg);
        backdrop-filter: blur(15px);
        border-radius: 20px;
        padding: 30px;
        border: 1px solid var(--glass-border);
        box-shadow: var(--shadow-soft);
        margin-bottom: 20px;
    }

    .control-title {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 25px;
        font-size: 1.2em;
        font-weight: 700;
        color: var(--text-primary);
    }

    .control-group {
        margin-bottom: 25px;
    }

    .control-label {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
        font-weight: 600;
        color: var(--text-primary);
    }

    .slider-container {
        position: relative;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 15px;
        padding: 20px;
        border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .slider-wrapper {
        display: flex;
        align-items: center;
        gap: 15px;
    }

    input[type="range"] {
        flex: 1;
        height: 8px;
        border-radius: 10px;
        background: rgba(255, 255, 255, 0.2);
        outline: none;
        -webkit-appearance: none;
        transition: all 0.3s ease;
    }

    input[type="range"]::-webkit-slider-thumb {
        appearance: none;
        width: 28px;
        height: 28px;
        border-radius: 50%;
        background: var(--primary-gradient);
        cursor: pointer;
        box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        border: 3px solid white;
        transition: all 0.3s ease;
    }

    input[type="range"]::-webkit-slider-thumb:hover {
        transform: scale(1.2);
        box-shadow: 0 8px 25px rgba(102, 126, 234, 0.6);
    }

    .slider-value {
        min-width: 80px;
        text-align: center;
        font-weight: 700;
        background: var(--primary-gradient);
        color: white;
        padding: 12px 18px;
        border-radius: 12px;
        font-size: 14px;
        box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
    }

    .position-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
        margin-top: 15px;
    }

    .position-btn {
        padding: 15px;
        border: 2px solid rgba(255, 255, 255, 0.2);
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        border-radius: 12px;
        cursor: pointer;
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 11px;
        font-weight: 600;
        text-align: center;
        color: var(--text-primary);
        position: relative;
        overflow: hidden;
    }

    .position-btn::before {
        content: '';
        position: absolute;
        top: 50%;
        left: 50%;
        width: 0;
        height: 0;
        background: var(--primary-gradient);
        transition: all 0.4s ease;
        border-radius: 50%;
        transform: translate(-50%, -50%);
        z-index: -1;
    }

    .position-btn:hover::before {
        width: 100%;
        height: 100%;
        border-radius: 0;
    }

    .position-btn:hover {
        color: white;
        transform: translateY(-2px);
        box-shadow: var(--shadow-soft);
    }

    .position-btn.active {
        background: var(--primary-gradient);
        color: white;
        border-color: rgba(102, 126, 234, 0.5);
        box-shadow: var(--shadow-soft);
    }

    .text-watermark-panel {
        background: rgba(248, 113, 113, 0.1);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(248, 113, 113, 0.2);
        border-radius: 15px;
        padding: 20px;
        margin-top: 20px;
    }

    .watermark-type-tabs {
        display: flex;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 12px;
        padding: 4px;
        margin-bottom: 20px;
    }

    .tab-btn {
        flex: 1;
        padding: 12px;
        border: none;
        background: transparent;
        border-radius: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
        font-weight: 600;
        color: var(--text-primary);
    }

    .tab-btn.active {
        background: var(--primary-gradient);
        color: white;
        box-shadow: var(--shadow-soft);
    }

    .text-input {
        width: 100%;
        padding: 15px;
        border: 2px solid rgba(255, 255, 255, 0.2);
        border-radius: 12px;
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        color: var(--text-primary);
        font-size: 16px;
        margin-bottom: 15px;
        transition: all 0.3s ease;
    }

    .text-input:focus {
        outline: none;
        border-color: rgba(102, 126, 234, 0.5);
        box-shadow: 0 0 20px rgba(102, 126, 234, 0.2);
    }

    .font-controls {
        display: grid;
        grid-template-columns: 2fr 1fr;
        gap: 15px;
    }

    .font-select {
        padding: 12px;
        border: 2px solid rgba(255, 255, 255, 0.2);
        border-radius: 10px;
        background: var(--glass-bg);
        color: var(--text-primary);
        cursor: pointer;
    }

    .color-input {
        width: 100%;
        height: 45px;
        border: 2px solid rgba(255, 255, 255, 0.2);
        border-radius: 10px;
        cursor: pointer;
        background: var(--glass-bg);
    }

    .advanced-options {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 15px;
        margin: 20px 0;
    }

    .option-card {
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        border-radius: 12px;
        padding: 20px;
        border: 1px solid var(--glass-border);
        transition: all 0.3s ease;
        cursor: pointer;
    }

    .option-card:hover {
        transform: translateY(-2px);
        box-shadow: var(--shadow-soft);
    }

    .option-card.active {
        background: rgba(102, 126, 234, 0.1);
        border-color: rgba(102, 126, 234, 0.3);
    }

    .checkbox-wrapper {
        display: flex;
        align-items: center;
        gap: 12px;
    }

    .custom-checkbox {
        width: 20px;
        height: 20px;
        border: 2px solid rgba(102, 126, 234, 0.3);
        border-radius: 4px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: all 0.3s ease;
    }

    .custom-checkbox.checked {
        background: var(--primary-gradient);
        border-color: rgba(102, 126, 234, 0.6);
    }

    .process-section {
        text-align: center;
        background: var(--glass-bg);
        backdrop-filter: blur(15px);
        border-radius: 25px;
        padding: 40px;
        border: 1px solid var(--glass-border);
        box-shadow: var(--shadow-soft);
        position: relative;
        overflow: hidden;
    }

    .process-section::before {
        content: '';
        position: absolute;
        top: -100%;
        left: -100%;
        width: 300%;
        height: 300%;
        background: conic-gradient(from 0deg, transparent 340deg, rgba(102, 126, 234, 0.1) 360deg);
        animation: rotate 8s linear infinite;
        opacity: 0.5;
    }

    .process-btn {
        display: inline-flex;
        align-items: center;
        gap: 15px;
        padding: 22px 45px;
        background: var(--success-gradient);
        color: white;
        border: none;
        border-radius: 20px;
        font-size: 18px;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        box-shadow: 0 10px 30px rgba(79, 172, 254, 0.3);
        text-transform: uppercase;
        letter-spacing: 1px;
        position: relative;
        overflow: hidden;
        z-index: 1;
    }

    .process-btn::before {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
        transition: left 0.6s ease;
        z-index: -1;
    }

    .process-btn:hover::before {
        left: 100%;
    }

    .process-btn:hover:not(:disabled) {
        transform: translateY(-5px) scale(1.05);
        box-shadow: 0 20px 40px rgba(79, 172, 254, 0.4);
    }

    .process-btn:disabled {
        opacity: 0.6;
        cursor: not-allowed;
        transform: none;
        box-shadow: none;
    }

    .status-panel {
        margin-top: 30px;
        border-radius: 15px;
        transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .status-panel.success {
        background: linear-gradient(135deg, rgba(79, 172, 254, 0.1), rgba(0, 242, 254, 0.1));
        border: 2px solid rgba(79, 172, 254, 0.3);
        color: #0369a1;
    }

    .status-panel.error {
        background: linear-gradient(135deg, rgba(250, 112, 154, 0.1), rgba(254, 225, 64, 0.1));
        border: 2px solid rgba(250, 112, 154, 0.3);
        color: #dc2626;
    }

    .status-panel.processing {
        background: linear-gradient(135deg, rgba(245, 158, 11, 0.1), rgba(251, 191, 36, 0.1));
        border: 2px solid rgba(245, 158, 11, 0.3);
        color: #d97706;
    }

    .status-content {
        padding: 25px;
        text-align: center;
        font-weight: 600;
    }

    .progress-container {
        margin: 20px 0;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 10px;
        overflow: hidden;
        height: 12px;
        position: relative;
    }

    .progress-bar {
        height: 100%;
        background: var(--success-gradient);
        transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        position: relative;
        overflow: hidden;
    }

    .progress-bar::after {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
        animation: progressShine 2s ease-in-out infinite;
    }

    @keyframes progressShine {
        0% { left: -100%; }
        100% { left: 100%; }
    }

    .loading-spinner {
        display: inline-block;
        width: 28px;
        height: 28px;
        border: 4px solid rgba(245, 158, 11, 0.3);
        border-radius: 50%;
        border-top-color: #d97706;
        animation: spin 1s ease-in-out infinite;
        margin-right: 15px;
    }

    @keyframes spin {
        to { transform: rotate(360deg); }
    }

    .download-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 15px;
        margin-top: 25px;
    }

    .download-card {
        background: var(--glass-bg);
        backdrop-filter: blur(10px);
        border-radius: 15px;
        padding: 20px;
        border: 1px solid var(--glass-border);
        text-align: center;
        transition: all 0.3s ease;
    }

    .download-card:hover {
        transform: translateY(-3px);
        box-shadow: var(--shadow-soft);
    }

    .download-link {
        display: inline-flex;
        align-items: center;
        gap: 10px;
        padding: 12px 20px;
        background: var(--success-gradient);
        color: white;
        text-decoration: none;
        border-radius: 10px;
        font-weight: 600;
        transition: all 0.3s ease;
        font-size: 14px;
    }

    .download-link:hover {
        transform: scale(1.05);
        box-shadow: 0 8px 25px rgba(79, 172, 254, 0.3);
    }

    .batch-download-btn {
        grid-column: 1 / -1;
        background: var(--secondary-gradient);
        padding: 18px 30px;
        font-size: 16px;
    }

    @media (max-width: 1024px) {
        .main-grid {
            grid-template-columns: 1fr;
            gap: 30px;
        }

        .sidebar {
            position: static;
            order: -1;
        }

        h1 {
            font-size: 3em;
        }

        .container {
            padding: 30px 20px;
        }
    }

    @media (max-width: 768px) {
        .stats-bar {
            grid-template-columns: repeat(2, 1fr);
        }

        .advanced-options {
            grid-template-columns: 1fr;
        }

        .font-controls {
            grid-template-columns: 1fr;
        }

        h1 {
            font-size: 2.5em;
        }
    }

    .file-input {
        display: none;
    }

    .hidden {
        display: none;
    }
</style>
```

</head>
<body>
    <div class="container">
        <div class="header">
            <h1>PDF Watermark Studio Pro</h1>
            <p class="subtitle">Advanced PDF watermarking with AI-powered positioning and professional batch processing</p>

```
        <div class="stats-bar">
            <div class="stat-item">
                <div class="stat-number" id="fileCount">0</div>
                <div class="stat-label">Files Loaded</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="totalSize">0MB</div>
                <div class="stat-label">Total Size</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="processedCount">0</div>
                <div class="stat-label">Processed</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="successRate">0%</div>
                <div class="stat-label">Success Rate</div>
            </div>
        </div>
    </div>

    <div class="main-grid">
        <div class="workspace">
            <!-- PDF Upload Zone -->
            <div class="upload-zone" id="pdfZone">
                <div class="upload-icon">📄</div>
                <div class="upload-title">Upload PDF Documents</div>
                <div class="upload-subtitle">Drag & drop multiple PDF files or click to browse</div>
                <input type="file" id="pdfFile" class="file-input" accept=".pdf" multiple>
                <label for="pdfFile" class="upload-btn">
                    <span>Choose PDF Files</span>
                </label>
                <div id="pdfFileGrid" class="file-grid"></div>
            </div>

            <!-- Watermark Type Selector -->
            <div class="control-panel">
                <div class="control-title">
                    <span>🎨</span>
                    <span>Watermark Type</span>
                </div>
                <div class="watermark-type-tabs">
                    <button class="tab-btn active" data-type="image">Image Watermark</button>
                    <button class="tab-btn" data-type="text">Text Watermark</button>
                </div>

                <!-- Image Watermark Panel -->
                <div id="imageWatermarkPanel">
                    <div class="upload-zone" id="imageZone" style="min-height: 150px; padding: 30px 20px;">
                        <div class="upload-icon">🖼️</div>
                        <div class="upload-title" style="font-size: 1.2em;">Upload Watermark Image</div>
                        <div class="upload-subtitle">PNG, JPG, JPEG, SVG supported</div>
                        <input type="file" id="imageFile" class="file-input" accept="image/*">
                        <label for="imageFile" class="upload-btn">
                            <span>Choose Image</span>
                        </label>
                        <div id="imagePreview" class="preview-container">
                            <img id="previewImg" class="preview-image" alt="Watermark Preview">
                            <div style="color: var(--text-secondary); font-weight: 600; margin-top: 10px;">Preview</div>
                        </div>
                    </div>
                </div>

                <!-- Text Watermark Panel -->
                <div id="textWatermarkPanel" class="text-watermark-panel hidden">
                    <input type="text" id="watermarkText" class="text-input" placeholder="Enter watermark text..." value="CONFIDENTIAL">
                    <div class="font-controls">
                        <select id="fontFamily" class="font-select">
                            <option value="Helvetica">Helvetica</option>
                            <option value="Times-Roman">Times New Roman</option>
                            <option value="Courier">Courier</option>
                            <option value="Arial">Arial</option>
                        </select>
                        <input type="color" id="textColor" class="color-input" value="#FF0000">
                    </div>
                    <div class="control-group">
                        <div class="control-label">
                            <span>Font Size</span>
                            <span id="fontSizeValue" class="slider-value">48px</span>
                        </div>
                        <div class="slider-container">
                            <div class="slider-wrapper">
                                <input type="range" id="fontSize" min="12" max="120" step="4" value="48">
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Processing Section -->
            <div class="process-section">
                <button id="processBtn" class="process-btn" disabled>
                    <span>⚡</span>
                    <span>Process All Documents</span>
                </button>
                <div id="statusPanel" class="status-panel" style="display: none;">
                    <div class="status-content" id="statusContent"></div>
                    <div class="progress-container" id="progressContainer" style="display: none;">
                        <div class="progress-bar" id="progressBar" style="width: 0%;"></div>
                    </div>
                    <div id="downloadGrid" class="download-grid"></div>
                </div>
            </div>
        </div>

        <div class="sidebar">
            <!-- Appearance Controls -->
            <div class="control-panel">
                <div class="control-title">
                    <span>✨</span>
                    <span>Appearance</span>
                </div>
                
                <div class="control-group">
                    <div class="control-label">
                        <span>Transparency</span>
                        <span id="transparencyValue" class="slider-value">30%</span>
                    </div>
                    <div class="slider-container">
                        <div class="slider-wrapper">
                            <input type="range" id="transparency" min="0.05" max="1" step="0.05" value="0.3">
                        </div>
                    </div>
                </div>

                <div class="control-group">
                    <div class="control-label">
                        <span>Size Scale</span>
                        <span id="scaleValue" class="slider-value">50%</span>
                    </div>
                    <div class="slider-container">
                        <div class="slider-wrapper">
                            <input type="range" id="scale" min="0.1" max="4" step="0.1" value="0.5">
                        </div>
                    </div>
                </div>

                <div class="control-group">
                    <div class="control-label">
                        <span>Rotation</span>
                        <span id="rotationValue" class="slider-value">0°</span>
                    </div>
                    <div class="slider-container">
                        <div class="slider-wrapper">
                            <input type="range" id="rotation" min="-180" max="180" step="15" value="0">
                        </div>
                    </div>
                </div>
            </div>

            <!-- Position Controls -->
            <div class="control-panel">
                <div class="control-title">
                    <span>📍</span>
                    <span>Position</span>
                </div>
                <div class="position-grid">
                    <button class="position-btn" data-position="top-left">Top Left</button>
                    <button class="position-btn" data-position="top-center">Top Center</button>
                    <button class="position-btn" data-position="top-right">Top Right</button>
                    <button class="position-btn" data-position="center-left">Center Left</button>
                    <button class="position-btn active" data-position="center">Center</button>
                    <button class="position-btn" data-position="center-right">Center Right</button>
                    <button class="position-btn" data-position="bottom-left">Bottom Left</button>
                    <button class="position-btn" data-position="bottom-center">Bottom Center</button>
                    <button class="position-btn" data-position="bottom-right">Bottom Right</button>
                </div>
            </div>

            <!-- Advanced Options -->
            <div class="control-panel">
                <div class="control-title">
                    <span>⚙️</span>
                    <span>Advanced Options</span>
                </div>
                <div class="advanced-options">
                    <div class="option-card active" id="allPagesOption">
                        <div class="checkbox-wrapper">
                            <div class="custom-checkbox checked" id="allPagesCheck">✓</div>
                            <div>
                                <div style="font-weight: 600;">All Pages</div>
                                <div style="font-size: 0.9em; color: var(--text-secondary);">Apply to every page</div>
                            </div>
                        </div>
                    </div>
                    <div class="option-card active" id="highQualityOption">
                        <div class="checkbox-wrapper">
                            <div class="custom-checkbox checked" id="highQualityCheck">✓</div>
                            <div>
                                <div style="font-weight: 600;">High Quality</div>
                                <div style="font-size: 0.9em; color: var(--text-secondary);">Preserve PDF quality</div>
                            </div>
                        </div>
                    </div>
                    <div class="option-card" id="behindTextOption">
                        <div class="checkbox-wrapper">
                            <div class="custom-checkbox" id="behindTextCheck"></div>
                            <div>
                                <div style="font-weight: 600;">Behind Text</div>
                                <div style="font-size: 0.9em; color: var(--text-secondary);">Place watermark behind content</div>
                            </div>
                        </div>
                    </div>
                    <div class="option-card" id="batchNamingOption">
                        <div class="checkbox-wrapper">
                            <div class="custom-checkbox" id="batchNamingCheck"></div>
                            <div>
                                <div style="font-weight: 600;">Smart Naming</div>
                                <div style="font-size: 0.9em; color: var(--text-secondary);">Auto-generate file names</div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Page Range Selector -->
                <div class="control-group" id="pageRangeGroup" style="display: none;">
                    <div class="control-label">
                        <span>Page Range</span>
                    </div>
                    <input type="text" id="pageRange" class="text-input" placeholder="e.g., 1-5, 8, 10-12" style="margin-bottom: 0;">
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    class PDFWatermarkStudio {
        constructor() {
            this.pdfFiles = [];
            this.imageFile = null;
            this.currentPosition = 'center';
            this.watermarkType = 'image';
            this.processedFiles = [];
            this.stats = {
                fileCount: 0,
                totalSize: 0,
                processedCount: 0,
                successRate: 0
            };

            this.initializeElements();
            this.bindEvents();
            this.setupDragAndDrop();
            this.initializeSliders();
        }

        initializeElements() {
            // File inputs
            this.pdfInput = document.getElementById('pdfFile');
            this.imageInput = document.getElementById('imageFile');
            
            // Zones
            this.pdfZone = document.getElementById('pdfZone');
            this.imageZone = document.getElementById('imageZone');
            
            // Controls
            this.transparencySlider = document.getElementById('transparency');
            this.scaleSlider = document.getElementById('scale');
            this.rotationSlider = document.getElementById('rotation');
            this.fontSizeSlider = document.getElementById('fontSize');
            
            // Values
            this.transparencyValue = document.getElementById('transparencyValue');
            this.scaleValue = document.getElementById('scaleValue');
            this.rotationValue = document.getElementById('rotationValue');
            this.fontSizeValue = document.getElementById('fontSizeValue');
            
            // Buttons and panels
            this.processBtn = document.getElementById('processBtn');
            this.statusPanel = document.getElementById('statusPanel');
            this.statusContent = document.getElementById('statusContent');
            this.progressContainer = document.getElementById('progressContainer');
            this.progressBar = document.getElementById('progressBar');
            
            // Position buttons
            this.positionButtons = document.querySelectorAll('.position-btn');
            
            // Tab buttons
            this.tabButtons = document.querySelectorAll('.tab-btn');
            
            // Watermark panels
            this.imageWatermarkPanel = document.getElementById('imageWatermarkPanel');
            this.textWatermarkPanel = document.getElementById('textWatermarkPanel');
            
            // Text watermark controls
            this.watermarkText = document.getElementById('watermarkText');
            this.fontFamily = document.getElementById('fontFamily');
            this.textColor = document.getElementById('textColor');
            
            // Option cards
            this.optionCards = document.querySelectorAll('.option-card');
            
            // Stats elements
            this.fileCountEl = document.getElementById('fileCount');
            this.totalSizeEl = document.getElementById('totalSize');
            this.processedCountEl = document.getElementById('processedCount');
            this.successRateEl = document.getElementById('successRate');
        }

        bindEvents() {
            // File inputs
            this.pdfInput.addEventListener('change', (e) => this.handlePDFFiles(Array.from(e.target.files)));
            this.imageInput.addEventListener('change', (e) => this.handleImageFile(e.target.files[0]));
            
            // Sliders
            this.transparencySlider.addEventListener('input', () => this.updateSliderValue('transparency'));
            this.scaleSlider.addEventListener('input', () => this.updateSliderValue('scale'));
            this.rotationSlider.addEventListener('input', () => this.updateSliderValue('rotation'));
            this.fontSizeSlider.addEventListener('input', () => this.updateSliderValue('fontSize'));
            
            // Position buttons
            this.positionButtons.forEach(btn => {
                btn.addEventListener('click', () => this.selectPosition(btn));
            });
            
            // Tab buttons
            this.tabButtons.forEach(btn => {
                btn.addEventListener('click', () => this.switchWatermarkType(btn.dataset.type));
            });
            
            // Option cards
            this.optionCards.forEach(card => {
                card.addEventListener('click', () => this.toggleOption(card));
            });
            
            // Process button
            this.processBtn.addEventListener('click', () => this.processAllDocuments());
        }

        setupDragAndDrop() {
            this.setupZoneDragAndDrop(this.pdfZone, (files) => {
                const pdfFiles = files.filter(f => f.type === 'application/pdf');
                if (pdfFiles.length > 0) this.handlePDFFiles(pdfFiles);
            });

            this.setupZoneDragAndDrop(this.imageZone, (files) => {
                const imageFile = files.find(f => f.type.startsWith('image/'));
                if (imageFile) this.handleImageFile(imageFile);
            });
        }

        setupZoneDragAndDrop(zone, handler) {
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                zone.addEventListener(eventName, this.preventDefaults);
            });

            ['dragenter', 'dragover'].forEach(eventName => {
                zone.addEventListener(eventName, () => zone.classList.add('dragover'));
            });

            ['dragleave', 'drop'].forEach(eventName => {
                zone.addEventListener(eventName, () => zone.classList.remove('dragover'));
            });

            zone.addEventListener('drop', (e) => {
                const files = Array.from(e.dataTransfer.files);
                handler(files);
            });
        }

        preventDefaults(e) {
            e.preventDefault();
            e.stopPropagation();
        }

        handlePDFFiles(files) {
            const validPDFs = files.filter(f => f.type === 'application/pdf');
            if (validPDFs.length === 0) return;

            // Add new files or replace existing ones
            validPDFs.forEach(newFile => {
                const existingIndex = this.pdfFiles.findIndex(f => f.name === newFile.name);
                if (existingIndex >= 0) {
                    this.pdfFiles[existingIndex] = newFile;
                } else {
                    this.pdfFiles.push(newFile);
                }
            });

            this.updatePDFFileGrid();
            this.updateStats();
            this.checkReadyToProcess();
        }

        updatePDFFileGrid() {
            const grid = document.getElementById('pdfFileGrid');
            grid.innerHTML = '';

            this.pdfFiles.forEach((file, index) => {
                const fileCard = document.createElement('div');
                fileCard.className = 'file-card';
                fileCard.innerHTML = `
                    <button class="remove-file" onclick="watermarkStudio.removeFile(${index})">×</button>
                    <div class="file-name">${file.name}</div>
                    <div class="file-size">${this.formatFileSize(file.size)}</div>
                `;
                grid.appendChild(fileCard);
            });
        }

        removeFile(index) {
            this.pdfFiles.splice(index, 1);
            this.updatePDFFileGrid();
            this.updateStats();
            this.checkReadyToProcess();
        }

        handleImageFile(file) {
            if (!file || !file.type.startsWith('image/')) return;

            this.imageFile = file;
            
            const reader = new FileReader();
            reader.onload = (e) => {
                document.getElementById('previewImg').src = e.target.result;
                document.getElementById('imagePreview').classList.add('show');
            };
            reader.readAsDataURL(file);
            
            this.checkReadyToProcess();
        }

        switchWatermarkType(type) {
            this.watermarkType = type;
            
            // Update tabs
            this.tabButtons.forEach(btn => {
                btn.classList.toggle('active', btn.dataset.type === type);
            });
            
            // Show/hide panels
            this.imageWatermarkPanel.classList.toggle('hidden', type !== 'image');
            this.textWatermarkPanel.classList.toggle('hidden', type !== 'text');
            
            this.checkReadyToProcess();
        }

        selectPosition(btn) {
            this.positionButtons.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            this.currentPosition = btn.dataset.position;
        }

        toggleOption(card) {
            card.classList.toggle('active');
            const checkbox = card.querySelector('.custom-checkbox');
            const isActive = card.classList.contains('active');
            
            checkbox.classList.toggle('checked', isActive);
            checkbox.textContent = isActive ? '✓' : '';
            
            // Special handling for "All Pages" option
            if (card.id === 'allPagesOption') {
                const pageRangeGroup = document.getElementById('pageRangeGroup');
                pageRangeGroup.style.display = isActive ? 'none' : 'block';
            }
        }

        updateSliderValue(type) {
            const slider = this[type + 'Slider'];
            const valueEl = this[type + 'Value'];
            const value = parseFloat(slider.value);
            
            switch (type) {
                case 'transparency':
                    valueEl.textContent = Math.round(value * 100) + '%';
                    break;
                case 'scale':
                    valueEl.textContent = Math.round(value * 100) + '%';
                    break;
                case 'rotation':
                    valueEl.textContent = value + '°';
                    break;
                case 'fontSize':
                    valueEl.textContent = value + 'px';
                    break;
            }
            
            this.updateSliderBackground(slider);
        }

        updateSliderBackground(slider) {
            const min = parseFloat(slider.min);
            const max = parseFloat(slider.max);
            const value = parseFloat(slider.value);
            const percentage = ((value - min) / (max - min)) * 100;
            
            slider.style.background = `linear-gradient(to right, 
                rgba(102, 126, 234, 0.8) 0%, 
                rgba(102, 126, 234, 0.8) ${percentage}%, 
                rgba(255, 255, 255, 0.2) ${percentage}%, 
                rgba(255, 255, 255, 0.2) 100%)`;
        }

        initializeSliders() {
            ['transparency', 'scale', 'rotation', 'fontSize'].forEach(type => {
                this.updateSliderValue(type);
            });
        }

        updateStats() {
            this.stats.fileCount = this.pdfFiles.length;
            this.stats.totalSize = this.pdfFiles.reduce((sum, file) => sum + file.size, 0);
            
            this.fileCountEl.textContent = this.stats.fileCount;
            this.totalSizeEl.textContent = this.formatFileSize(this.stats.totalSize);
            
            if (this.stats.fileCount > 0) {
                this.stats.successRate = Math.round((this.stats.processedCount / this.stats.fileCount) * 100);
                this.successRateEl.textContent = this.stats.successRate + '%';
            }
        }

        formatFileSize(bytes) {
            if (bytes === 0) return '0MB';
            const k = 1024;
            const sizes = ['B', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(1)) + sizes[i];
        }

        checkReadyToProcess() {
            const hasFiles = this.pdfFiles.length > 0;
            const hasWatermark = this.watermarkType === 'image' ? 
                this.imageFile !== null : 
                this.watermarkText.value.trim() !== '';
            
            this.processBtn.disabled = !(hasFiles && hasWatermark);
        }

        getPositionCoordinates(pageWidth, pageHeight, imgWidth, imgHeight, position) {
            const margin = 50;
            const positions = {
                'top-left': { x: margin, y: pageHeight - imgHeight - margin },
                'top-center': { x: (pageWidth - imgWidth) / 2, y: pageHeight - imgHeight - margin },
                'top-right': { x: pageWidth - imgWidth - margin, y: pageHeight - imgHeight - margin },
                'center-left': { x: margin, y: (pageHeight - imgHeight) / 2 },
                'center': { x: (pageWidth - imgWidth) / 2, y: (pageHeight - imgHeight) / 2 },
                'center-right': { x: pageWidth - imgWidth - margin, y: (pageHeight - imgHeight) / 2 },
                'bottom-left': { x: margin, y: margin },
                'bottom-center': { x: (pageWidth - imgWidth) / 2, y: margin },
                'bottom-right': { x: pageWidth - imgWidth - margin, y: margin }
            };
            return positions[position] || positions['center'];
        }

        showStatus(message, type) {
            this.statusContent.innerHTML = message;
            this.statusPanel.className = `status-panel ${type}`;
            this.statusPanel.style.display = 'block';
        }

        updateProgress(current, total) {
            const percentage = Math.round((current / total) * 100);
            this.progressBar.style.width = percentage + '%';
            return percentage;
        }

        async processAllDocuments() {
            if (this.pdfFiles.length === 0) return;

            this.showStatus('<div class="loading-spinner"></div>Initializing processing pipeline...', 'processing');
            this.processBtn.disabled = true;
            this.progressContainer.style.display = 'block';
            this.processedFiles = [];

            try {
                const settings = this.getProcessingSettings();
                let successCount = 0;

                for (let i = 0; i < this.pdfFiles.length; i++) {
                    const file = this.pdfFiles[i];
                    const progress = this.updateProgress(i, this.pdfFiles.length);
                    
                    this.showStatus(
                        `<div class="loading-spinner"></div>Processing "${file.name}" (${i + 1}/${this.pdfFiles.length})`, 
                        'processing'
                    );

                    try {
                        const result = await this.processSinglePDF(file, settings);
                        this.processedFiles.push(result);
                        if (!result.error) successCount++;
                    } catch (error) {
                        this.processedFiles.push({
                            name: file.name,
                            error: error.message
                        });
                    }
                }

                this.updateProgress(this.pdfFiles.length, this.pdfFiles.length);
                this.stats.processedCount = successCount;
                this.updateStats();
                this.showProcessingResults(successCount);

            } catch (error) {
                this.showStatus(`❌ Processing failed: ${error.message}`, 'error');
            } finally {
                this.processBtn.disabled = false;
                this.checkReadyToProcess();
            }
        }

        getProcessingSettings() {
            return {
                transparency: parseFloat(this.transparencySlider.value),
                scale: parseFloat(this.scaleSlider.value),
                rotation: parseFloat(this.rotationSlider.value),
                position: this.currentPosition,
                watermarkType: this.watermarkType,
                allPages: document.getElementById('allPagesOption').classList.contains('active'),
                highQuality: document.getElementById('highQualityOption').classList.contains('active'),
                behindText: document.getElementById('behindTextOption').classList.contains('active'),
                smartNaming: document.getElementById('batchNamingOption').classList.contains('active'),
                pageRange: document.getElementById('pageRange').value.trim(),
                // Text watermark settings
                text: this.watermarkText.value,
                fontFamily: this.fontFamily.value,
                fontSize: parseFloat(this.fontSizeSlider.value),
                textColor: this.textColor.value
            };
        }

        async processSinglePDF(file, settings) {
            try {
                const pdfArrayBuffer = await file.arrayBuffer();
                const pdfDoc = await PDFLib.PDFDocument.load(pdfArrayBuffer);
                const pages = pdfDoc.getPages();
                
                let watermarkElement;
                
                if (settings.watermarkType === 'image') {
                    // Handle image watermark
                    const imageArrayBuffer = await this.imageFile.arrayBuffer();
                    if (this.imageFile.type === 'image/png') {
                        watermarkElement = await pdfDoc.embedPng(imageArrayBuffer);
                    } else {
                        watermarkElement = await pdfDoc.embedJpg(imageArrayBuffer);
                    }
                } else {
                    // Handle text watermark
                    watermarkElement = {
                        type: 'text',
                        text: settings.text,
                        font: await pdfDoc.embedFont(PDFLib.StandardFonts[settings.fontFamily] || PDFLib.StandardFonts.Helvetica),
                        size: settings.fontSize,
                        color: this.hexToRgb(settings.textColor)
                    };
                }
                
                // Determine which pages to process
                let pagesToProcess = settings.allPages ? pages : [pages[0]];
                
                if (!settings.allPages && settings.pageRange) {
                    pagesToProcess = this.parsePageRange(settings.pageRange, pages.length)
                        .map(pageNum => pages[pageNum - 1])
                        .filter(page => page);
                }
                
                // Apply watermark to selected pages
                pagesToProcess.forEach(page => {
                    this.applyWatermarkToPage(page, watermarkElement, settings);
                });
                
                const pdfBytes = await pdfDoc.save({
                    useObjectStreams: settings.highQuality,
                    addDefaultPage: false
                });
                
                const fileName = settings.smartNaming ? 
                    this.generateSmartFileName(file.name, settings) :
                    file.name.replace('.pdf', '_watermarked.pdf');
                
                const blob = new Blob([pdfBytes], { type: 'application/pdf' });
                const url = URL.createObjectURL(blob);
                
                return {
                    name: fileName,
                    originalName: file.name,
                    url: url,
                    size: pdfBytes.length,
                    pages: pagesToProcess.length
                };
                
            } catch (error) {
                throw new Error(`Failed to process ${file.name}: ${error.message}`);
            }
        }

        applyWatermarkToPage(page, watermarkElement, settings) {
            const { width: pageWidth, height: pageHeight } = page.getSize();
            
            if (settings.watermarkType === 'image') {
                const imgDims = watermarkElement.scale(settings.scale);
                const { x, y } = this.getPositionCoordinates(
                    pageWidth, pageHeight, 
                    imgDims.width, imgDims.height, 
                    settings.position
                );
                
                if (settings.behindText) {
                    page.drawImage(watermarkElement, {
                        x, y,
                        width: imgDims.width,
                        height: imgDims.height,
                        opacity: settings.transparency,
                        rotate: PDFLib.degrees(settings.rotation),
                    });
                } else {
                    page.drawImage(watermarkElement, {
                        x, y,
                        width: imgDims.width,
                        height: imgDims.height,
                        opacity: settings.transparency,
                        rotate: PDFLib.degrees(settings.rotation),
                    });
                }
            } else {
                // Text watermark
                const textWidth = watermarkElement.font.widthOfTextAtSize(
                    watermarkElement.text, 
                    watermarkElement.size
                );
                const textHeight = watermarkElement.size;
                
                const { x, y } = this.getPositionCoordinates(
                    pageWidth, pageHeight, 
                    textWidth, textHeight, 
                    settings.position
                );
                
                page.drawText(watermarkElement.text, {
                    x, y,
                    size: watermarkElement.size,
                    font: watermarkElement.font,
                    color: PDFLib.rgb(
                        watermarkElement.color.r / 255,
                        watermarkElement.color.g / 255,
                        watermarkElement.color.b / 255
                    ),
                    opacity: settings.transparency,
                    rotate: PDFLib.degrees(settings.rotation),
                });
            }
        }

        hexToRgb(hex) {
            const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
            return result ? {
                r: parseInt(result[1], 16),
                g: parseInt(result[2], 16),
                b: parseInt(result[3], 16)
            } : { r: 255, g: 0, b: 0 }; // Default to red
        }

        parsePageRange(rangeStr, totalPages) {
            const pages = new Set();
            const ranges = rangeStr.split(',');
            
            ranges.forEach(range => {
                range = range.trim();
                if (range.includes('-')) {
                    const [start, end] = range.split('-').map(n => parseInt(n.trim()));
                    for (let i = Math.max(1, start); i <= Math.min(totalPages, end); i++) {
                        pages.add(i);
                    }
                } else {
                    const pageNum = parseInt(range);
                    if (pageNum >= 1 && pageNum <= totalPages) {
                        pages.add(pageNum);
                    }
                }
            });
            
            return Array.from(pages).sort((a, b) => a - b);
        }

        generateSmartFileName(originalName, settings) {
            const baseName = originalName.replace('.pdf', '');
            const timestamp = new Date().toISOString().slice(0, 16).replace(/[:-]/g, '');
            const watermarkType = settings.watermarkType === 'image' ? 'img' : 'txt';
            const position = settings.position.replace('-', '');
            
            return `${baseName}_${watermarkType}_${position}_${timestamp}.pdf`;
        }

        async showProcessingResults(successCount) {
            const totalCount = this.pdfFiles.length;
            const errorCount = totalCount - successCount;
            
            let resultHTML = `
                <div style="display: flex; align-items: center; justify-content: center; gap: 15px; margin-bottom: 20px;">
                    <div style="text-align: center;">
                        <div style="font-size: 2em; font-weight: 700; color: #10b981;">✅</div>
                        <div>${successCount} Success</div>
                    </div>
                    <div style="width: 2px; height: 40px; background: rgba(255,255,255,0.2);"></div>
                    <div style="text-align: center;">
                        <div style="font-size: 2em; font-weight: 700; color: #ef4444;">❌</div>
                        <div>${errorCount} Errors</div>
                    </div>
                </div>
            `;

            if (successCount > 0) {
                resultHTML += '<div style="font-weight: 600; margin-bottom: 15px;">📥 Download Processed Files:</div>';
            }

            this.showStatus(resultHTML, successCount > 0 ? 'success' : 'error');
            
            if (successCount > 0) {
                await this.renderDownloadGrid();
            }
        }

        async renderDownloadGrid() {
            const grid = document.getElementById('downloadGrid');
            grid.innerHTML = '';

            const successFiles = this.processedFiles.filter(f => !f.error);
            
            // Individual download cards
            successFiles.forEach(file => {
                const card = document.createElement('div');
                card.className = 'download-card';
                card.innerHTML = `
                    <div style="margin-bottom: 10px; font-weight: 600; font-size: 0.9em;">${file.name}</div>
                    <div style="margin-bottom: 15px; color: var(--text-secondary); font-size: 0.8em;">
                        ${this.formatFileSize(file.size)} • ${file.pages} page${file.pages !== 1 ? 's' : ''}
                    </div>
                    <a href="${file.url}" download="${file.name}" class="download-link">
                        <span>💾</span>
                        <span>Download</span>
                    </a>
                `;
                grid.appendChild(card);
            });

            // Batch download button for multiple files
            if (successFiles.length > 1) {
                const batchCard = document.createElement('div');
                batchCard.className = 'download-card';
                batchCard.innerHTML = `
                    <div style="margin-bottom: 10px; font-weight: 700;">📦 Batch Download</div>
                    <div style="margin-bottom: 15px; color: var(--text-secondary); font-size: 0.8em;">
                        All ${successFiles.length} files as ZIP
                    </div>
                    <button onclick="watermarkStudio.downloadAsZip()" class="download-link batch-download-btn" style="border: none; cursor: pointer;">
                        <span>📦</span>
                        <span>Download ZIP</span>
                    </button>
                `;
                grid.appendChild(batchCard);
            }
        }

        async downloadAsZip() {
            try {
                this.showStatus('<div class="loading-spinner"></div>Creating ZIP archive...', 'processing');
                
                const zip = new JSZip();
                const successFiles = this.processedFiles.filter(f => !f.error);
                
                // Add each file to ZIP
                for (const file of successFiles) {
                    const response = await fetch(file.url);
                    const blob = await response.blob();
                    zip.file(file.name, blob);
                }
                
                // Generate ZIP
                const zipBlob = await zip.generateAsync({ type: 'blob' });
                
                // Create download link
                const url = URL.createObjectURL(zipBlob);
                const timestamp = new Date().toISOString().slice(0, 16).replace(/[:-]/g, '');
                const fileName = `watermarked_pdfs_${timestamp}.zip`;
                
                const link = document.createElement('a');
                link.href = url;
                link.download = fileName;
                link.click();
                
                URL.revokeObjectURL(url);
                
                this.showStatus('✅ ZIP download started successfully!', 'success');
                setTimeout(() => this.showProcessingResults(successFiles.length), 2000);
                
            } catch (error) {
                this.showStatus(`❌ Failed to create ZIP: ${error.message}`, 'error');
            }
        }

        // Cleanup URLs when page unloads
        cleanup() {
            this.processedFiles.forEach(file => {
                if (file.url) URL.revokeObjectURL(file.url);
            });
            this.processedFiles = [];
        }
    }

    // Initialize the application
    let watermarkStudio;
    
    document.addEventListener('DOMContentLoaded', function() {
        watermarkStudio = new PDFWatermarkStudio();
        
        // Add text input listeners for real-time validation
        document.getElementById('watermarkText').addEventListener('input', () => {
            watermarkStudio.checkReadyToProcess();
        });
        
        // Cleanup on page unload
        window.addEventListener('beforeunload', () => {
            if (watermarkStudio) watermarkStudio.cleanup();
        });
        
        // Add keyboard shortcuts
        document.addEventListener('keydown', (e) => {
            if (e.ctrlKey || e.metaKey) {
                switch (e.key) {
                    case 'Enter':
                        e.preventDefault();
                        if (!watermarkStudio.processBtn.disabled) {
                            watermarkStudio.processAllDocuments();
                        }
                        break;
                    case 'Backspace':
                        if (e.shiftKey) {
                            e.preventDefault();
                            watermarkStudio.pdfFiles = [];
                            watermarkStudio.updatePDFFileGrid();
                            watermarkStudio.updateStats();
                            watermarkStudio.checkReadyToProcess();
                        }
                        break;
                }
            }
        });
        
        // Add context menu for advanced options
        document.addEventListener('contextmenu', (e) => {
            if (e.target.closest('.file-card')) {
                e.preventDefault();
                // Could add context menu for individual file operations
            }
        });
        
        // Performance monitoring
        if (window.performance && window.performance.mark) {
            window.performance.mark('pdf-watermark-studio-loaded');
        }
    });

    // Global helper functions for external access
    window.removeFile = (index) => watermarkStudio.removeFile(index);
    window.downloadAsZip = () => watermarkStudio.downloadAsZip();

    // Add some useful global shortcuts and utilities
    window.PDFWatermarkUtils = {
        supportedImageTypes: ['image/png', 'image/jpeg', 'image/jpg', 'image/gif', 'image/svg+xml'],
        maxFileSize: 100 * 1024 * 1024, // 100MB
        maxBatchSize: 50,
        
        validateFile: (file) => {
            if (file.type === 'application/pdf' && file.size <= window.PDFWatermarkUtils.maxFileSize) {
                return { valid: true };
            }
            if (file.type.startsWith('image/') && window.PDFWatermarkUtils.supportedImageTypes.includes(file.type)) {
                return { valid: true };
            }
            return { 
                valid: false, 
                error: file.type === 'application/pdf' ? 'File too large' : 'Unsupported file type' 
            };
        },
        
        formatBytes: (bytes) => {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }
    };
</script>
```

</body>
</html>