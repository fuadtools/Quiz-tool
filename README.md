<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Quiz Video Generator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            padding: 15px;
            background-color: #f5f5f5;
            color: #333;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 100%;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        h1, h2 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        h1 {
            font-size: 1.8rem;
        }
        
        h2 {
            font-size: 1.3rem;
            margin-top: 5px;
        }
        
        .section {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        
        textarea {
            width: 100%;
            min-height: 150px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-family: monospace;
            resize: vertical;
            font-size: 14px;
        }
        
        .preview-container {
            position: relative;
            margin: 15px auto;
            overflow: hidden;
            background-size: cover;
            background-position: center;
            width: 100%;
            aspect-ratio: 16/9;
        }
        
        .video-frame {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        
        .question-box, .answer-box {
            margin: 10px;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            font-size: 1.2rem;
            max-width: 90%;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            position: absolute;
            transition: all 0.5s ease;
        }
        
        .question-box {
            background-color: #3498db;
            color: white;
        }
        
        .answer-box {
            background-color: #2ecc71;
            color: white;
        }
        
        .countdown {
            font-size: 2rem;
            font-weight: bold;
            color: #e74c3c;
            margin: 10px;
            position: absolute;
            top: 10px;
            right: 10px;
        }
        
        .controls {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }
        
        .control-group {
            display: flex;
            flex-direction: column;
        }
        
        label {
            margin-bottom: 5px;
            font-weight: bold;
            font-size: 0.9rem;
        }
        
        select, input, button {
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            margin-bottom: 8px;
            font-size: 0.9rem;
            width: 100%;
        }
        
        button {
            background-color: #3498db;
            color: white;
            border: none;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.3s;
            padding: 10px;
        }
        
        button:hover {
            background-color: #2980b9;
        }
        
        .download-btn {
            background-color: #2ecc71;
        }
        
        .download-btn:hover {
            background-color: #27ae60;
        }
        
        .upload-btn {
            background-color: #9b59b6;
        }
        
        .upload-btn:hover {
            background-color: #8e44ad;
        }
        
        .cancel-btn {
            background-color: #e74c3c;
        }
        
        .cancel-btn:hover {
            background-color: #c0392b;
        }
        
        .color-picker {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .color-picker input {
            width: 40px;
            height: 25px;
            padding: 0;
        }
        
        .progress-container {
            width: 100%;
            background-color: #f1f1f1;
            border-radius: 4px;
            margin-top: 10px;
            display: none;
        }
        
        .progress-bar {
            width: 0%;
            height: 20px;
            background-color: #3498db;
            border-radius: 4px;
            text-align: center;
            line-height: 20px;
            color: white;
            font-size: 0.8rem;
        }
        
        #status {
            margin-top: 10px;
            font-weight: bold;
            font-size: 0.9rem;
        }
        
        .preview-controls {
            display: flex;
            gap: 10px;
            margin-top: 10px;
            justify-content: center;
        }
        
        .file-upload {
            position: relative;
            overflow: hidden;
        }
        
        .file-upload input {
            position: absolute;
            top: 0;
            right: 0;
            min-width: 100%;
            min-height: 100%;
            opacity: 0;
            cursor: pointer;
        }

        /* Transition Effects */
        .fade {
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .fade.visible {
            opacity: 1;
        }
        
        .slide-up {
            transform: translateY(100px);
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .slide-up.visible {
            transform: translateY(0);
            opacity: 1;
        }
        
        .slide-down {
            transform: translateY(-100px);
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .slide-down.visible {
            transform: translateY(0);
            opacity: 1;
        }
        
        .slide-left {
            transform: translateX(100px);
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .slide-left.visible {
            transform: translateX(0);
            opacity: 1;
        }
        
        .slide-right {
            transform: translateX(-100px);
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .slide-right.visible {
            transform: translateX(0);
            opacity: 1;
        }
        
        .zoom {
            transform: scale(0.5);
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .zoom.visible {
            transform: scale(1);
            opacity: 1;
        }
        
        .flip {
            transform: rotateX(90deg);
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .flip.visible {
            transform: rotateX(0);
            opacity: 1;
        }
        
        .bounce {
            transform: translateY(-100px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.68, -0.6, 0.32, 1.6);
        }
        
        .bounce.visible {
            transform: translateY(0);
            opacity: 1;
        }

        /* Mobile Responsiveness */
        @media (max-width: 768px) {
            .preview-container {
                aspect-ratio: 9/16;
            }
            
            .question-box, .answer-box {
                font-size: 1rem;
                padding: 10px;
            }
            
            .controls {
                grid-template-columns: 1fr;
            }
            
            button, select, input {
                padding: 12px;
                font-size: 1rem;
            }
            
            .preview-controls {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Advanced Quiz Video Generator</h1>
        
        <div class="section">
            <h2>Enter Questions and Answers</h2>
            <p>Format: One question and answer per line, separated by "|"</p>
            <textarea id="qa-input" placeholder="What is the capital of France?|Paris
How many continents are there?|7
What is 2+2?|4"></textarea>
        </div>
        
        <div class="section">
            <h2>Video Settings</h2>
            <div class="controls">
                <div class="control-group">
                    <label for="aspect-ratio">Aspect Ratio</label>
                    <select id="aspect-ratio">
                        <option value="16/9">Horizontal (16:9)</option>
                        <option value="9/16">Vertical (9:16)</option>
                        <option value="1/1">Square (1:1)</option>
                        <option value="4/5">Portrait (4:5)</option>
                        <option value="5/4">Landscape (5:4)</option>
                    </select>
                    
                    <label for="layout">Layout</label>
                    <select id="layout">
                        <option value="stacked">Stacked (Vertical)</option>
                        <option value="side-by-side">Side by Side (Horizontal)</option>
                        <option value="question-only">Question Only</option>
                        <option value="answer-only">Answer Only</option>
                    </select>
                    
                    <label for="question-time">Question Time (seconds)</label>
                    <input type="number" id="question-time" min="1" max="60" value="5">
                    
                    <label for="answer-time">Answer Time (seconds)</label>
                    <input type="number" id="answer-time" min="1" max="60" value="5">
                    
                    <label for="transition-time">Transition Time (seconds)</label>
                    <input type="number" id="transition-time" min="0.5" max="10" value="2" step="0.5">
                    
                    <label for="transition-effect">Transition Effect</label>
                    <select id="transition-effect">
                        <option value="fade">Fade</option>
                        <option value="slide-up">Slide Up</option>
                        <option value="slide-down">Slide Down</option>
                        <option value="slide-left">Slide Left</option>
                        <option value="slide-right">Slide Right</option>
                        <option value="zoom">Zoom</option>
                        <option value="flip">Flip</option>
                        <option value="bounce">Bounce</option>
                    </select>
                </div>
                
                <div class="control-group">
                    <label for="bg-type">Background</label>
                    <select id="bg-type">
                        <option value="color">Color</option>
                        <option value="image">Image</option>
                        <option value="gradient">Gradient</option>
                        <option value="video">Video</option>
                    </select>
                    
                    <div id="bg-color-control">
                        <label for="bg-color">Background Color</label>
                        <div class="color-picker">
                            <input type="color" id="bg-color" value="#ffffff">
                        </div>
                    </div>
                    
                    <div id="bg-image-control" style="display:none">
                        <label>Background Image</label>
                        <div class="file-upload">
                            <button class="upload-btn">Upload Image</button>
                            <input type="file" id="bg-image" accept="image/*">
                        </div>
                        <label for="bg-image-blur">Image Blur</label>
                        <input type="range" id="bg-image-blur" min="0" max="20" value="0">
                    </div>
                    
                    <div id="bg-gradient-control" style="display:none">
                        <label for="gradient-type">Gradient Type</label>
                        <select id="gradient-type">
                            <option value="linear">Linear</option>
                            <option value="radial">Radial</option>
                        </select>
                        <label for="gradient-direction">Gradient Direction</label>
                        <select id="gradient-direction">
                            <option value="to right">Left to Right</option>
                            <option value="to left">Right to Left</option>
                            <option value="to bottom">Top to Bottom</option>
                            <option value="to top">Bottom to Top</option>
                            <option value="to bottom right">Diagonal</option>
                        </select>
                        <label for="gradient-color1">Color 1</label>
                        <div class="color-picker">
                            <input type="color" id="gradient-color1" value="#3498db">
                        </div>
                        <label for="gradient-color2">Color 2</label>
                        <div class="color-picker">
                            <input type="color" id="gradient-color2" value="#2ecc71">
                        </div>
                    </div>
                    
                    <div id="bg-video-control" style="display:none">
                        <label>Background Video</label>
                        <div class="file-upload">
                            <button class="upload-btn">Upload Video</button>
                            <input type="file" id="bg-video" accept="video/*">
                        </div>
                        <label for="bg-video-opacity">Video Opacity</label>
                        <input type="range" id="bg-video-opacity" min="0" max="100" value="100">
                    </div>
                </div>
                
                <div class="control-group">
                    <label for="question-color">Question Box Color</label>
                    <div class="color-picker">
                        <input type="color" id="question-color" value="#3498db">
                    </div>
                    
                    <label for="answer-color">Answer Box Color</label>
                    <div class="color-picker">
                        <input type="color" id="answer-color" value="#2ecc71">
                    </div>
                    
                    <label for="text-color">Text Color</label>
                    <div class="color-picker">
                        <input type="color" id="text-color" value="#ffffff">
                    </div>
                    
                    <label for="font-size">Font Size</label>
                    <input type="range" id="font-size" min="12" max="72" value="24">
                    
                    <label for="font-family">Font Family</label>
                    <select id="font-family">
                        <option value="Arial, sans-serif">Arial</option>
                        <option value="Helvetica, sans-serif">Helvetica</option>
                        <option value="Verdana, sans-serif">Verdana</option>
                        <option value="Georgia, serif">Georgia</option>
                        <option value="Times New Roman, serif">Times New Roman</option>
                        <option value="Courier New, monospace">Courier New</option>
                        <option value="Impact, sans-serif">Impact</option>
                    </select>
                    
                    <label for="text-shadow">Text Shadow</label>
                    <input type="checkbox" id="text-shadow" checked>
                </div>
                
                <div class="control-group">
                    <label for="countdown-enabled">Show Countdown</label>
                    <input type="checkbox" id="countdown-enabled" checked>
                    
                    <label for="countdown-position">Countdown Position</label>
                    <select id="countdown-position">
                        <option value="top-right">Top Right</option>
                        <option value="top-left">Top Left</option>
                        <option value="bottom-right">Bottom Right</option>
                        <option value="bottom-left">Bottom Left</option>
                        <option value="center">Center</option>
                    </select>
                    
                    <label for="countdown-color">Countdown Color</label>
                    <div class="color-picker">
                        <input type="color" id="countdown-color" value="#e74c3c">
                    </div>
                    
                    <label for="countdown-size">Countdown Size</label>
                    <input type="range" id="countdown-size" min="10" max="100" value="32">
                    
                    <label for="sound-effects">Enable Sound Effects</label>
                    <input type="checkbox" id="sound-effects" checked>
                </div>
            </div>
            
            <div style="display: flex; gap: 10px; flex-wrap: wrap;">
                <button id="preview-btn">Preview First QA</button>
                <button id="generate-btn">Generate Video</button>
                <button id="download-btn" class="download-btn" disabled>Download Video</button>
                <button id="cancel-btn" class="cancel-btn" style="display:none">Cancel</button>
            </div>
            
            <div class="progress-container" id="progress-container">
                <div class="progress-bar" id="progress-bar">0%</div>
            </div>
            <div id="status">Ready</div>
        </div>
        
        <div class="section">
            <h2>Preview</h2>
            <div class="preview-container" id="preview-container">
                <div class="video-frame" id="video-frame">
                    <div class="question-box" id="question-box"></div>
                    <div class="answer-box" id="answer-box"></div>
                    <div class="countdown" id="countdown"></div>
                </div>
            </div>
            <div class="preview-controls">
                <button id="start-preview-btn">Start Full Preview</button>
                <button id="stop-preview-btn" disabled>Stop Preview</button>
            </div>
        </div>
    </div>

    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <script>
        // DOM Elements
        const elements = {
            qaInput: document.getElementById('qa-input'),
            aspectRatio: document.getElementById('aspect-ratio'),
            layout: document.getElementById('layout'),
            questionTime: document.getElementById('question-time'),
            answerTime: document.getElementById('answer-time'),
            transitionTime: document.getElementById('transition-time'),
            transitionEffect: document.getElementById('transition-effect'),
            bgType: document.getElementById('bg-type'),
            bgColor: document.getElementById('bg-color'),
            bgImage: document.getElementById('bg-image'),
            bgImageBlur: document.getElementById('bg-image-blur'),
            gradientType: document.getElementById('gradient-type'),
            gradientDirection: document.getElementById('gradient-direction'),
            gradientColor1: document.getElementById('gradient-color1'),
            gradientColor2: document.getElementById('gradient-color2'),
            bgVideo: document.getElementById('bg-video'),
            bgVideoOpacity: document.getElementById('bg-video-opacity'),
            questionColor: document.getElementById('question-color'),
            answerColor: document.getElementById('answer-color'),
            textColor: document.getElementById('text-color'),
            fontSize: document.getElementById('font-size'),
            fontFamily: document.getElementById('font-family'),
            textShadow: document.getElementById('text-shadow'),
            countdownEnabled: document.getElementById('countdown-enabled'),
            countdownPosition: document.getElementById('countdown-position'),
            countdownColor: document.getElementById('countdown-color'),
            countdownSize: document.getElementById('countdown-size'),
            soundEffects: document.getElementById('sound-effects'),
            previewBtn: document.getElementById('preview-btn'),
            generateBtn: document.getElementById('generate-btn'),
            downloadBtn: document.getElementById('download-btn'),
            cancelBtn: document.getElementById('cancel-btn'),
            startPreviewBtn: document.getElementById('start-preview-btn'),
            stopPreviewBtn: document.getElementById('stop-preview-btn'),
            progressContainer: document.getElementById('progress-container'),
            progressBar: document.getElementById('progress-bar'),
            status: document.getElementById('status'),
            previewContainer: document.getElementById('preview-container'),
            videoFrame: document.getElementById('video-frame'),
            questionBox: document.getElementById('question-box'),
            answerBox: document.getElementById('answer-box'),
            countdown: document.getElementById('countdown'),
            bgColorControl: document.getElementById('bg-color-control'),
            bgImageControl: document.getElementById('bg-image-control'),
            bgGradientControl: document.getElementById('bg-gradient-control'),
            bgVideoControl: document.getElementById('bg-video-control')
        };

        // State
        let state = {
            qaPairs: [],
            isGenerating: false,
            isPreviewing: false,
            currentIndex: 0,
            mediaRecorder: null,
            recordedChunks: [],
            videoBlob: null,
            backgroundImage: null,
            backgroundVideo: null,
            previewInterval: null,
            countdownInterval: null,
            videoElement: null
        };

        // Enhanced Sound Effects
        const sounds = {
            questionStart: new Audio('https://assets.mixkit.co/sfx/preview/mixkit-game-show-suspense-waiting-668.mp3'),
            answerReveal: new Audio('https://assets.mixkit.co/sfx/preview/mixkit-winning-chimes-2015.mp3'),
            countdownTick: new Audio('https://assets.mixkit.co/sfx/preview/mixkit-clock-countdown-bleeps-916.mp3'),
            videoComplete: new Audio('https://assets.mixkit.co/sfx/preview/mixkit-achievement-bell-600.mp3'),
            transition: new Audio('https://assets.mixkit.co/sfx/preview/mixkit-arcade-game-jump-coin-216.mp3')
        };

        // Initialize
        function init() {
            setupEventListeners();
            updateBackgroundControls();
            updatePreview();
        }

        // Event Listeners
        function setupEventListeners() {
            elements.bgType.addEventListener('change', updateBackgroundControls);
            elements.previewBtn.addEventListener('click', previewFirstQA);
            elements.generateBtn.addEventListener('click', generateVideo);
            elements.downloadBtn.addEventListener('click', downloadVideo);
            elements.cancelBtn.addEventListener('click', cancelGeneration);
            elements.startPreviewBtn.addEventListener('click', startFullPreview);
            elements.stopPreviewBtn.addEventListener('click', stopFullPreview);
            elements.bgImage.addEventListener('change', handleImageUpload);
            elements.bgVideo.addEventListener('change', handleVideoUpload);
            
            // Update preview when these change
            const previewUpdateElements = [
                elements.aspectRatio, elements.layout, 
                elements.questionColor, elements.answerColor,
                elements.textColor, elements.fontSize, elements.fontFamily,
                elements.textShadow, elements.bgColor, elements.gradientType,
                elements.gradientDirection, elements.gradientColor1,
                elements.gradientColor2, elements.bgImageBlur,
                elements.countdownEnabled, elements.countdownPosition,
                elements.countdownColor, elements.countdownSize,
                elements.transitionEffect
            ];
            
            previewUpdateElements.forEach(el => {
                el.addEventListener('input', updatePreview);
                el.addEventListener('change', updatePreview);
            });
        }

        // Update background controls visibility
        function updateBackgroundControls() {
            const type = elements.bgType.value;
            elements.bgColorControl.style.display = type === 'color' ? 'flex' : 'none';
            elements.bgImageControl.style.display = type === 'image' ? 'block' : 'none';
            elements.bgGradientControl.style.display = type === 'gradient' ? 'block' : 'none';
            elements.bgVideoControl.style.display = type === 'video' ? 'block' : 'none';
            updatePreview();
        }

        // Handle image upload
        function handleImageUpload(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (event) => {
                    state.backgroundImage = event.target.result;
                    updatePreview();
                };
                reader.readAsDataURL(file);
            }
        }

        // Handle video upload
        function handleVideoUpload(e) {
            const file = e.target.files[0];
            if (file) {
                if (state.videoElement) {
                    state.videoElement.remove();
                }
                
                const videoUrl = URL.createObjectURL(file);
                state.videoElement = document.createElement('video');
                state.videoElement.src = videoUrl;
                state.videoElement.loop = true;
                state.videoElement.muted = true;
                state.videoElement.autoplay = true;
                state.videoElement.style.position = 'absolute';
                state.videoElement.style.width = '100%';
                state.videoElement.style.height = '100%';
                state.videoElement.style.objectFit = 'cover';
                state.videoElement.style.zIndex = '-1';
                
                elements.previewContainer.insertBefore(state.videoElement, elements.videoFrame);
                state.videoElement.play().catch(e => console.error("Video play error:", e));
            }
        }

        // Update preview display
        function updatePreview() {
            // Aspect ratio
            elements.previewContainer.style.aspectRatio = elements.aspectRatio.value;
            
            // Layout
            if (elements.layout.value === 'stacked') {
                elements.videoFrame.style.flexDirection = 'column';
                elements.questionBox.style.position = 'relative';
                elements.answerBox.style.position = 'relative';
            } else if (elements.layout.value === 'side-by-side') {
                elements.videoFrame.style.flexDirection = 'row';
                elements.questionBox.style.position = 'relative';
                elements.answerBox.style.position = 'relative';
            } else if (elements.layout.value === 'question-only') {
                elements.videoFrame.style.flexDirection = 'column';
                elements.questionBox.style.position = 'absolute';
                elements.answerBox.style.position = 'absolute';
            } else if (elements.layout.value === 'answer-only') {
                elements.videoFrame.style.flexDirection = 'column';
                elements.questionBox.style.position = 'absolute';
                elements.answerBox.style.position = 'absolute';
            }
            
            // Colors and text styling
            elements.questionBox.style.backgroundColor = elements.questionColor.value;
            elements.answerBox.style.backgroundColor = elements.answerColor.value;
            elements.questionBox.style.color = elements.textColor.value;
            elements.answerBox.style.color = elements.textColor.value;
            elements.questionBox.style.fontSize = `${elements.fontSize.value}px`;
            elements.answerBox.style.fontSize = `${elements.fontSize.value}px`;
            elements.questionBox.style.fontFamily = elements.fontFamily.value;
            elements.answerBox.style.fontFamily = elements.fontFamily.value;
            
            if (elements.textShadow.checked) {
                elements.questionBox.style.textShadow = '2px 2px 4px rgba(0,0,0,0.5)';
                elements.answerBox.style.textShadow = '2px 2px 4px rgba(0,0,0,0.5)';
            } else {
                elements.questionBox.style.textShadow = 'none';
                elements.answerBox.style.textShadow = 'none';
            }
            
            // Background
            switch(elements.bgType.value) {
                case 'color':
                    elements.previewContainer.style.background = elements.bgColor.value;
                    break;
                case 'image':
                    if (state.backgroundImage) {
                        elements.previewContainer.style.background = `url('${state.backgroundImage}')`;
                        elements.previewContainer.style.backgroundSize = 'cover';
                        elements.previewContainer.style.filter = `blur(${elements.bgImageBlur.value}px)`;
                    }
                    break;
                case 'gradient':
                    if (elements.gradientType.value === 'linear') {
                        elements.previewContainer.style.background = 
                            `linear-gradient(${elements.gradientDirection.value}, ${elements.gradientColor1.value}, ${elements.gradientColor2.value})`;
                    } else {
                        elements.previewContainer.style.background = 
                            `radial-gradient(${elements.gradientColor1.value}, ${elements.gradientColor2.value})`;
                    }
                    break;
                case 'video':
                    if (state.videoElement) {
                        state.videoElement.style.opacity = elements.bgVideoOpacity.value / 100;
                    }
                    break;
            }
            
            // Countdown styling
            elements.countdown.style.color = elements.countdownColor.value;
            elements.countdown.style.fontSize = `${elements.countdownSize.value}px`;
            
            switch(elements.countdownPosition.value) {
                case 'top-right':
                    elements.countdown.style.top = '10px';
                    elements.countdown.style.right = '10px';
                    elements.countdown.style.left = 'auto';
                    elements.countdown.style.bottom = 'auto';
                    break;
                case 'top-left':
                    elements.countdown.style.top = '10px';
                    elements.countdown.style.left = '10px';
                    elements.countdown.style.right = 'auto';
                    elements.countdown.style.bottom = 'auto';
                    break;
                case 'bottom-right':
                    elements.countdown.style.bottom = '10px';
                    elements.countdown.style.right = '10px';
                    elements.countdown.style.top = 'auto';
                    elements.countdown.style.left = 'auto';
                    break;
                case 'bottom-left':
                    elements.countdown.style.bottom = '10px';
                    elements.countdown.style.left = '10px';
                    elements.countdown.style.top = 'auto';
                    elements.countdown.style.right = 'auto';
                    break;
                case 'center':
                    elements.countdown.style.top = '50%';
                    elements.countdown.style.left = '50%';
                    elements.countdown.style.transform = 'translate(-50%, -50%)';
                    break;
            }
            
            // Reset transition classes
            elements.questionBox.className = 'question-box';
            elements.answerBox.className = 'answer-box';
        }

        // Parse QA input
        function parseQAInput() {
            const lines = elements.qaInput.value.split('\n');
            state.qaPairs = lines
                .filter(line => line.trim() !== '')
                .map(line => {
                    const [question, answer] = line.split('|').map(part => part.trim());
                    return { question, answer };
                })
                .filter(qa => qa.question && qa.answer);
            
            return state.qaPairs.length > 0;
        }

        // Show question with transition
        function showQuestion(index) {
            if (index < 0 || index >= state.qaPairs.length) return false;
            
            // Reset boxes
            elements.questionBox.classList.remove('visible');
            elements.answerBox.classList.remove('visible');
            
            // Set transition class
            const effect = elements.transitionEffect.value;
            elements.questionBox.classList.add(effect);
            
            // Show question based on layout
            if (elements.layout.value !== 'answer-only') {
                elements.questionBox.textContent = state.qaPairs[index].question;
                
                // Trigger reflow to restart animation
                void elements.questionBox.offsetWidth;
                
                elements.questionBox.classList.add('visible');
            }
            
            // Play sound if enabled
            if (elements.soundEffects.checked) {
                sounds.questionStart.currentTime = 0;
                sounds.questionStart.play().catch(e => console.error("Sound error:", e));
            }
            
            return true;
        }

        // Show answer with transition
        function showAnswer(index) {
            if (index < 0 || index >= state.qaPairs.length) return false;
            
            // Set transition class
            const effect = elements.transitionEffect.value;
            elements.answerBox.classList.add(effect);
            
            // Show answer based on layout
            if (elements.layout.value !== 'question-only') {
                elements.answerBox.textContent = state.qaPairs[index].answer;
                
                // Trigger reflow to restart animation
                void elements.answerBox.offsetWidth;
                
                elements.answerBox.classList.add('visible');
            }
            
            // Play sound if enabled
            if (elements.soundEffects.checked) {
                sounds.answerReveal.currentTime = 0;
                sounds.answerReveal.play().catch(e => console.error("Sound error:", e));
                sounds.transition.currentTime = 0;
                sounds.transition.play().catch(e => console.error("Sound error:", e));
            }
            
            return true;
        }

        // Hide both question and answer
        function hideQA() {
            elements.questionBox.classList.remove('visible');
            elements.answerBox.classList.remove('visible');
        }

        // Show countdown
        function showCountdown(seconds, callback) {
            if (!elements.countdownEnabled.checked) {
                if (callback) setTimeout(callback, seconds * 1000);
                return;
            }
            
            let remaining = seconds;
            elements.countdown.textContent = remaining;
            elements.countdown.style.display = 'block';
            
            state.countdownInterval = setInterval(() => {
                remaining--;
                elements.countdown.textContent = remaining;
                
                if (elements.soundEffects.checked) {
                    sounds.countdownTick.currentTime = 0;
                    sounds.countdownTick.play().catch(e => console.error("Sound error:", e));
                }
                
                if (remaining <= 0) {
                    clearInterval(state.countdownInterval);
                    elements.countdown.style.display = 'none';
                    if (callback) callback();
                }
            }, 1000);
        }

        // Preview first QA pair
        function previewFirstQA() {
            if (!parseQAInput()) {
                alert('Please enter valid questions and answers.');
                return;
            }
            
            updatePreview();
            showQuestion(0);
            
            // After question time, show countdown
            setTimeout(() => {
                showCountdown(elements.transitionTime.value, () => {
                    // After transition, show answer
                    showAnswer(0);
                    
                    // After answer time, hide both
                    setTimeout(() => {
                        hideQA();
                        
                        // Play completion sound
                        if (elements.soundEffects.checked) {
                            sounds.videoComplete.currentTime = 0;
                            sounds.videoComplete.play().catch(e => console.error("Sound error:", e));
                        }
                    }, elements.answerTime.value * 1000);
                });
            }, elements.questionTime.value * 1000);
        }

        // Start full preview
        function startFullPreview() {
            if (!parseQAInput()) {
                alert('Please enter valid questions and answers.');
                return;
            }
            
            state.isPreviewing = true;
            state.currentIndex = 0;
            elements.startPreviewBtn.disabled = true;
            elements.stopPreviewBtn.disabled = false;
            updatePreview();
            
            runPreviewSequence();
        }

        // Run preview sequence
        function runPreviewSequence() {
            if (!state.isPreviewing || state.currentIndex >= state.qaPairs.length) {
                stopFullPreview();
                
                // Play completion sound
                if (elements.soundEffects.checked) {
                    sounds.videoComplete.currentTime = 0;
                    sounds.videoComplete.play().catch(e => console.error("Sound error:", e));
                }
                return;
            }
            
            // Show question
            showQuestion(state.currentIndex);
            
            // After question time, show countdown
            setTimeout(() => {
                showCountdown(elements.transitionTime.value, () => {
                    // After transition, show answer
                    showAnswer(state.currentIndex);
                    
                    // After answer time, hide both and move to next question
                    setTimeout(() => {
                        hideQA();
                        state.currentIndex++;
                        runPreviewSequence();
                    }, elements.answerTime.value * 1000);
                });
            }, elements.questionTime.value * 1000);
        }

        // Stop full preview
        function stopFullPreview() {
            state.isPreviewing = false;
            clearInterval(state.countdownInterval);
            elements.countdown.style.display = 'none';
            elements.startPreviewBtn.disabled = false;
            elements.stopPreviewBtn.disabled = true;
            hideQA();
        }

        // Generate video
        async function generateVideo() {
            if (state.isGenerating || !parseQAInput()) return;
            
            state.isGenerating = true;
            state.recordedChunks = [];
            elements.generateBtn.disabled = true;
            elements.cancelBtn.style.display = 'block';
            elements.progressContainer.style.display = 'block';
            elements.progressBar.style.width = '0%';
            elements.status.textContent = 'Preparing...';
            
            try {
                // Set up canvas
                const [width, height] = elements.aspectRatio.value.split('/').map(Number);
                const canvas = document.createElement('canvas');
                canvas.width = width * 100; // Scale for better quality
                canvas.height = height * 100;
                const ctx = canvas.getContext('2d');
                
                // Create temporary container
                const tempContainer = document.createElement('div');
                tempContainer.style.width = `${width * 100}px`;
                tempContainer.style.height = `${height * 100}px`;
                tempContainer.style.position = 'absolute';
                tempContainer.style.left = '-9999px';
                tempContainer.style.overflow = 'hidden';
                
                // Apply background
                switch(elements.bgType.value) {
                    case 'color':
                        tempContainer.style.background = elements.bgColor.value;
                        break;
                    case 'image':
                        if (state.backgroundImage) {
                            tempContainer.style.background = `url('${state.backgroundImage}')`;
                            tempContainer.style.backgroundSize = 'cover';
                            tempContainer.style.filter = `blur(${elements.bgImageBlur.value}px)`;
                        }
                        break;
                    case 'gradient':
                        if (elements.gradientType.value === 'linear') {
                            tempContainer.style.background = 
                                `linear-gradient(${elements.gradientDirection.value}, ${elements.gradientColor1.value}, ${elements.gradientColor2.value})`;
                        } else {
                            tempContainer.style.background = 
                                `radial-gradient(${elements.gradientColor1.value}, ${elements.gradientColor2.value})`;
                        }
                        break;
                    case 'video':
                        // For video background, we'll just use a solid color in the generated video
                        tempContainer.style.background = elements.bgColor.value;
                        break;
                }
                
                // Create question and answer boxes
                const tempQuestionBox = document.createElement('div');
                tempQuestionBox.style.padding = '20px';
                tempQuestionBox.style.borderRadius = '8px';
                tempQuestionBox.style.textAlign = 'center';
                tempQuestionBox.style.fontSize = `${elements.fontSize.value * 3}px`;
                tempQuestionBox.style.maxWidth = '90%';
                tempQuestionBox.style.backgroundColor = elements.questionColor.value;
                tempQuestionBox.style.color = elements.textColor.value;
                tempQuestionBox.style.boxShadow = '0 2px 5px rgba(0, 0, 0, 0.2)';
                if (elements.textShadow.checked) {
                    tempQuestionBox.style.textShadow = '2px 2px 4px rgba(0,0,0,0.5)';
                }
                tempQuestionBox.style.fontFamily = elements.fontFamily.value;
                tempQuestionBox.style.opacity = '0';
                tempQuestionBox.style.transition = 'all 0.5s ease';
                
                const tempAnswerBox = document.createElement('div');
                tempAnswerBox.style.padding = '20px';
                tempAnswerBox.style.borderRadius = '8px';
                tempAnswerBox.style.textAlign = 'center';
                tempAnswerBox.style.fontSize = `${elements.fontSize.value * 3}px`;
                tempAnswerBox.style.maxWidth = '90%';
                tempAnswerBox.style.backgroundColor = elements.answerColor.value;
                tempAnswerBox.style.color = elements.textColor.value;
                tempAnswerBox.style.boxShadow = '0 2px 5px rgba(0, 0, 0, 0.2)';
                tempAnswerBox.style.opacity = '0';
                tempAnswerBox.style.transition = 'all 0.5s ease';
                if (elements.textShadow.checked) {
                    tempAnswerBox.style.textShadow = '2px 2px 4px rgba(0,0,0,0.5)';
                }
                tempAnswerBox.style.fontFamily = elements.fontFamily.value;
                
                // Create countdown element
                const tempCountdown = document.createElement('div');
                tempCountdown.style.fontSize = `${elements.countdownSize.value * 3}px`;
                tempCountdown.style.fontWeight = 'bold';
                tempCountdown.style.color = elements.countdownColor.value;
                tempCountdown.style.position = 'absolute';
                
                switch(elements.countdownPosition.value) {
                    case 'top-right':
                        tempCountdown.style.top = '10px';
                        tempCountdown.style.right = '10px';
                        break;
                    case 'top-left':
                        tempCountdown.style.top = '10px';
                        tempCountdown.style.left = '10px';
                        break;
                    case 'bottom-right':
                        tempCountdown.style.bottom = '10px';
                        tempCountdown.style.right = '10px';
                        break;
                    case 'bottom-left':
                        tempCountdown.style.bottom = '10px';
                        tempCountdown.style.left = '10px';
                        break;
                    case 'center':
                        tempCountdown.style.top = '50%';
                        tempCountdown.style.left = '50%';
                        tempCountdown.style.transform = 'translate(-50%, -50%)';
                        break;
                }
                
                // Set layout
                if (elements.layout.value === 'stacked') {
                    tempContainer.style.display = 'flex';
                    tempContainer.style.flexDirection = 'column';
                    tempQuestionBox.style.position = 'relative';
                    tempAnswerBox.style.position = 'relative';
                } else if (elements.layout.value === 'side-by-side') {
                    tempContainer.style.display = 'flex';
                    tempContainer.style.flexDirection = 'row';
                    tempQuestionBox.style.position = 'relative';
                    tempAnswerBox.style.position = 'relative';
                } else if (elements.layout.value === 'question-only') {
                    tempContainer.style.display = 'flex';
                    tempContainer.style.flexDirection = 'column';
                    tempQuestionBox.style.position = 'absolute';
                    tempAnswerBox.style.position = 'absolute';
                } else if (elements.layout.value === 'answer-only') {
                    tempContainer.style.display = 'flex';
                    tempContainer.style.flexDirection = 'column';
                    tempQuestionBox.style.position = 'absolute';
                    tempAnswerBox.style.position = 'absolute';
                }
                
                tempContainer.appendChild(tempQuestionBox);
                tempContainer.appendChild(tempAnswerBox);
                if (elements.countdownEnabled.checked) {
                    tempContainer.appendChild(tempCountdown);
                }
                document.body.appendChild(tempContainer);
                
                // Set up media recorder
                const stream = canvas.captureStream(30);
                state.mediaRecorder = new MediaRecorder(stream, {
                    mimeType: 'video/webm;codecs=vp9',
                    videoBitsPerSecond: 2500000
                });
                
                state.mediaRecorder.ondataavailable = (e) => {
                    if (e.data.size > 0) {
                        state.recordedChunks.push(e.data);
                    }
                };
                
                state.mediaRecorder.onstop = () => {
                    if (!state.isGenerating) return;
                    
                    state.videoBlob = new Blob(state.recordedChunks, { type: 'video/webm' });
                    elements.downloadBtn.disabled = false;
                    elements.status.textContent = 'Ready to download';
                    document.body.removeChild(tempContainer);
                    state.isGenerating = false;
                    elements.generateBtn.disabled = false;
                    elements.cancelBtn.style.display = 'none';
                    
                    // Play completion sound
                    if (elements.soundEffects.checked) {
                        sounds.videoComplete.currentTime = 0;
                        sounds.videoComplete.play().catch(e => console.error("Sound error:", e));
                    }
                };
                
                state.mediaRecorder.start(100);
                
                // Process each QA pair
                for (let i = 0; i < state.qaPairs.length; i++) {
                    if (!state.isGenerating) break;
                    
                    elements.status.textContent = `Processing ${i+1}/${state.qaPairs.length}`;
                    elements.progressBar.style.width = `${(i / state.qaPairs.length) * 100}%`;
                    
                    // Show question
                    if (elements.layout.value !== 'answer-only') {
                        tempQuestionBox.textContent = state.qaPairs[i].question;
                        tempQuestionBox.style.opacity = '1';
                    }
                    tempAnswerBox.textContent = '';
                    tempAnswerBox.style.opacity = '0';
                    await renderFrames(tempContainer, canvas, ctx, elements.questionTime.value);
                    
                    // Show countdown
                    if (elements.countdownEnabled.checked) {
                        let remaining = elements.transitionTime.value;
                        tempCountdown.textContent = remaining;
                        
                        const countdownFrames = elements.transitionTime.value * 30;
                        for (let j = 0; j < countdownFrames; j++) {
                            if (!state.isGenerating) break;
                            
                            if (j % 30 === 0) {
                                remaining--;
                                tempCountdown.textContent = remaining;
                            }
                            
                            await renderFrame(tempContainer, canvas, ctx);
                            await new Promise(r => setTimeout(r, 1000/30));
                        }
                    } else {
                        await renderFrames(tempContainer, canvas, ctx, elements.transitionTime.value);
                    }
                    
                    // Show answer
                    if (elements.layout.value !== 'question-only') {
                        tempAnswerBox.textContent = state.qaPairs[i].answer;
                        tempAnswerBox.style.opacity = '1';
                    }
                    
                    // Keep question visible
                    if (elements.layout.value !== 'answer-only') {
                        tempQuestionBox.style.opacity = '1';
                    }
                    
                    await renderFrames(tempContainer, canvas, ctx, elements.answerTime.value);
                    
                    // Hide both
                    tempQuestionBox.style.opacity = '0';
                    tempAnswerBox.style.opacity = '0';
                    await renderFrames(tempContainer, canvas, ctx, 0.5); // Short fade out
                }
                
                if (state.isGenerating) {
                    state.mediaRecorder.stop();
                }
                
            } catch (error) {
                console.error("Generation error:", error);
                elements.status.textContent = 'Error: ' + error.message;
                state.isGenerating = false;
                elements.generateBtn.disabled = false;
                elements.cancelBtn.style.display = 'none';
                
                // Clean up
                const tempContainer = document.querySelector('div[style*="left: -9999px"]');
                if (tempContainer) document.body.removeChild(tempContainer);
                if (state.mediaRecorder && state.mediaRecorder.state !== 'inactive') {
                    state.mediaRecorder.stop();
                }
            }
        }

        // Render frames for duration
        async function renderFrames(container, canvas, ctx, seconds) {
            const frames = seconds * 30; // 30fps
            for (let i = 0; i < frames; i++) {
                if (!state.isGenerating) return;
                await renderFrame(container, canvas, ctx);
                await new Promise(r => setTimeout(r, 1000/30));
            }
        }

        // Render single frame
        function renderFrame(element, canvas, ctx) {
            return new Promise((resolve) => {
                html2canvas(element, {
                    canvas: canvas,
                    logging: false,
                    useCORS: true,
                    scale: 1,
                    onclone: (doc) => {
                        const clone = doc.getElementById(element.id) || element;
                        clone.style.width = element.style.width;
                        clone.style.height = element.style.height;
                    },
                    onrendered: () => {
                        ctx.drawImage(canvas, 0, 0);
                        resolve();
                    }
                });
            });
        }

        // Cancel generation
        function cancelGeneration() {
            state.isGenerating = false;
            if (state.mediaRecorder && state.mediaRecorder.state !== 'inactive') {
                state.mediaRecorder.stop();
            }
            elements.status.textContent = 'Cancelled';
            elements.generateBtn.disabled = false;
            elements.cancelBtn.style.display = 'none';
        }

        // Download video
        function downloadVideo() {
            if (!state.videoBlob) return;
            
            const url = URL.createObjectURL(state.videoBlob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'quiz-video.webm';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        // Initialize
        init();
    </script>
</body>
</html>
