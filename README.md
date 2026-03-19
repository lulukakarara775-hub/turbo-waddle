<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>my.project.kait</title>
    <style>
        body { margin: 0; overflow: hidden; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        
        .instruction {
            position: absolute;
            bottom: 80px;
            left: 20px;
            color: rgba(255,255,255,0.6);
            font-size: 12px;
            background: rgba(0,0,0,0.4);
            padding: 5px 10px;
            border-radius: 10px;
            z-index: 150;
        }
        
        /* --- СВОРАЧИВАЕМАЯ ИНФОРМАЦИОННАЯ ПАНЕЛЬ --- */
        .collapsible-panel {
            position: absolute;
            bottom: 30px;
            right: 30px;
            width: 320px;
            background: rgba(10, 5, 25, 0.85);
            backdrop-filter: blur(10px);
            color: #e0e0ff;
            border-radius: 20px;
            border-left: 4px solid #ffaa33;
            border-right: 4px solid #44aaff;
            box-shadow: 0 0 40px rgba(0,100,255,0.3);
            z-index: 200;
            font-size: 14px;
            line-height: 1.5;
            overflow: hidden;
            transition: box-shadow 0.3s;
            animation: panelGlow 4s infinite alternate;
        }
        @keyframes panelGlow {
            0% { box-shadow: 0 0 30px rgba(68, 170, 255, 0.3); }
            100% { box-shadow: 0 0 60px rgba(255, 100, 100, 0.5); }
        }
        .panel-header {
            background: rgba(255, 170, 50, 0.2);
            padding: 15px 20px;
            cursor: pointer;
            font-weight: bold;
            font-size: 18px;
            color: #ffaa66;
            letter-spacing: 1px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #335588;
            user-select: none;
        }
        .panel-header:hover {
            background: rgba(255, 170, 50, 0.3);
        }
        .toggle-icon {
            font-size: 22px;
            transition: transform 0.3s;
        }
        .panel-content {
            padding: 20px;
            max-height: 400px;
            overflow-y: auto;
            transition: max-height 0.4s ease-out, padding 0.3s;
            scrollbar-width: thin;
            scrollbar-color: #44aaff #1a1a2e;
        }
        .panel-content.collapsed {
            max-height: 0;
            padding: 0 20px;
            overflow: hidden;
        }
        .panel-content p {
            margin: 12px 0;
        }
        .panel-content strong {
            color: #88ddff;
        }
        .highlight {
            color: #ffaa88;
        }
        
        /* --- СЛАЙДЕР УРОВНЯ БУРИ --- */
        .storm-control {
            position: absolute;
            top: 20px;
            right: 30px;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(5px);
            padding: 15px 25px;
            border-radius: 40px;
            color: white;
            border: 1px solid #ffaa44;
            box-shadow: 0 0 20px rgba(255,170,0,0.3);
            z-index: 250;
            text-align: center;
            min-width: 250px;
        }
        .storm-control label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #ffaa66;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-size: 14px;
        }
        .storm-control input {
            width: 100%;
            cursor: pointer;
            accent-color: #ff5544;
            height: 8px;
            border-radius: 10px;
        }
        .storm-value {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
            font-size: 12px;
            color: #aaaaff;
        }
        .storm-value span:first-child { color: #88ff88; }
        .storm-value span:last-child { color: #ff8888; }
        
        /* --- ЧЕЛОВЕК И ИНДИКАТОР СОСТОЯНИЯ --- */
        .human-container {
            position: absolute;
            bottom: 30px;
            left: 30px;
            z-index: 300;
            display: flex;
            align-items: flex-end;
            gap: 15px;
            pointer-events: none;
        }
        .human-model {
            width: 100px;
            height: 140px;
            background: rgba(30, 40, 60, 0.6);
            backdrop-filter: blur(4px);
            border-radius: 50px;
            border: 2px solid #88aaff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            box-shadow: 0 0 30px rgba(100,150,255,0.5);
            animation: float 3s infinite alternate;
            pointer-events: auto;
        }
        @keyframes float {
            0% { transform: translateY(0); }
            100% { transform: translateY(-8px); }
        }
        .human-head {
            width: 30px;
            height: 30px;
            background: #ffdd99;
            border-radius: 50%;
            margin-bottom: 5px;
            border: 2px solid #aa88ff;
        }
        .human-body {
            width: 45px;
            height: 55px;
            background: #4466aa;
            border-radius: 30px 30px 15px 15px;
            position: relative;
            overflow: hidden;
        }
        .human-body::before {
            content: "";
            position: absolute;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #6688cc, #224488);
        }
        .heartbeat {
            position: absolute;
            top: 20px;
            left: 10px;
            font-size: 25px;
            color: #ff5555;
            animation: beat 1s infinite;
            filter: drop-shadow(0 0 5px red);
        }
        @keyframes beat {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }
        .status-indicator {
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(5px);
            border-radius: 30px;
            padding: 12px 20px;
            border-left: 5px solid #44ff44;
            color: white;
            min-width: 200px;
            box-shadow: 0 0 30px rgba(0,0,0,0.7);
            border: 1px solid rgba(255,255,255,0.2);
        }
        .status-title {
            font-weight: bold;
            margin-bottom: 5px;
            font-size: 16px;
        }
        .status-description {
            font-size: 14px;
            opacity: 0.9;
        }
    </style>
</head>
<body>

    <!-- КОНТРОЛЬ УРОВНЯ БУРИ (БЕЗ СМАЙЛИКОВ) -->
    <div class="storm-control">
        <label for="stormLevel">УРОВЕНЬ МАГНИТНОЙ БУРИ</label>
        <input type="range" id="stormLevel" min="0" max="1" step="0.01" value="0.7">
        <div class="storm-value">
            <span>Спокойно</span>
            <span>Умеренно</span>
            <span>Сильная буря</span>
        </div>
    </div>

    <!-- ЧЕЛОВЕК И СТАТУС (БЕЗ СМАЙЛИКОВ) -->
    <div class="human-container">
        <div class="human-model">
            <div class="human-head"></div>
            <div class="human-body">
                <div class="heartbeat">❤️</div>
            </div>
        </div>
        <div class="status-indicator" id="humanStatus">
            <div class="status-title" id="statusTitle">Самочувствие: норма</div>
            <div class="status-description" id="statusDesc">Магнитное поле стабильно. Вы чувствуете себя хорошо.</div>
        </div>
    </div>

    <!-- СВОРАЧИВАЕМАЯ ПАНЕЛЬ С ИНФОРМАЦИЕЙ (БЕЗ ЗАГОЛОВКА ВВЕРХУ) -->
    <div class="collapsible-panel" id="infoPanel">
        <div class="panel-header" id="panelHeader">
            <span>ЧТО НУЖНО ЗНАТЬ О МАГНИТНЫХ БУРЯХ</span>
            <span class="toggle-icon" id="toggleIcon">▼</span>
        </div>
        <div class="panel-content" id="panelContent">
            <p><strong>Что это?</strong> Магнитные бури — это возмущения геомагнитного поля Земли, вызванные потоками <span class="highlight">солнечного ветра</span> (заряженных частиц от Солнца).</p>
            <p><strong>Откуда берутся?</strong> Вспышки на Солнце выбрасывают плазму, которая через 1-3 дня достигает Земли и воздействует на магнитосферу.</p>
            <p><strong>Влияние на здоровье:</strong> Метеочувствительные люди могут испытывать головные боли, слабость, скачки давления. Это связано с влиянием на капиллярный кровоток и нервную систему. В дни бурь также возможны сбои в работе спутников и радиосвязи.</p>
            <p><strong>Интересно:</strong> Во время сильных бурь полярные сияния можно увидеть даже в средних широтах.</p>
            <p style="font-size: 13px; text-align: right; opacity: 0.8;">Используйте слайдер, чтобы менять силу бури и наблюдать за изменением состояния человека.</p>
        </div>
    </div>

    <div class="instruction">Мышь: вращение | Колесико: масштаб | Панель: сворачивается по клику</div>

    <!-- Подключаем Three.js и модули -->
    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.126.0/build/three.module.js",
                "three/addons/": "https://unpkg.com/three@0.126.0/examples/jsm/"
            }
        }
    </script>

    <script type="module">
        import * as THREE from 'three';
        import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
        import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
        import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
        import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';
        
        // --- Инициализация сцены, камеры, рендера ---
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x050510);
        
        const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(0, 5, 25);
        
        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.toneMapping = THREE.ReinhardToneMapping;
        renderer.toneMappingExposure = 1.2;
        document.body.appendChild(renderer.domElement);
        
        // --- Пост-обработка (свечение) ---
        const renderScene = new RenderPass(scene, camera);
        const bloomPass = new UnrealBloomPass(new THREE.Vector2(window.innerWidth, window.innerHeight), 1.5, 0.4, 0.85);
        bloomPass.threshold = 0.1;
        bloomPass.strength = 1.2;
        bloomPass.radius = 0.5;
        
        const composer = new EffectComposer(renderer);
        composer.addPass(renderScene);
        composer.addPass(bloomPass);
        
        // --- Управление ---
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.autoRotate = true;
        controls.autoRotateSpeed = 0.5;
        controls.enableZoom = true;
        controls.maxDistance = 50;
        controls.minDistance = 10;
        controls.target.set(0, 0, 0);
        
        // --- Освещение ---
        const ambientLight = new THREE.AmbientLight(0x404060);
        scene.add(ambientLight);
        
        const sunLight = new THREE.PointLight(0xffaa66, 2, 0, 0);
        sunLight.position.set(0, 0, 0);
        scene.add(sunLight);
        
        const fillLight = new THREE.DirectionalLight(0x5577aa, 0.5);
        fillLight.position.set(-5, 5, 10);
        scene.add(fillLight);
        
        // --- Звезды ---
        const starsGeometry = new THREE.BufferGeometry();
        const starsCount = 3000;
        const starsPositions = new Float32Array(starsCount * 3);
        for (let i = 0; i < starsCount * 3; i += 3) {
            const r = 80 + Math.random() * 40;
            const theta = Math.random() * Math.PI * 2;
            const phi = Math.acos(2 * Math.random() - 1);
            starsPositions[i] = Math.sin(phi) * Math.cos(theta) * r;
            starsPositions[i+1] = Math.sin(phi) * Math.sin(theta) * r;
            starsPositions[i+2] = Math.cos(phi) * r;
        }
        starsGeometry.setAttribute('position', new THREE.BufferAttribute(starsPositions, 3));
        const starsMaterial = new THREE.PointsMaterial({ color: 0xffffff, size: 0.2, transparent: true, opacity: 0.8 });
        const stars = new THREE.Points(starsGeometry, starsMaterial);
        scene.add(stars);
        
        // --- СОЛНЦЕ ---
        const sunGeometry = new THREE.SphereGeometry(3, 64, 64);
        const sunMaterial = new THREE.MeshStandardMaterial({
            color: 0xffaa33,
            emissive: 0xff5500,
            roughness: 0.3,
            emissiveIntensity: 2.5
        });
        const sun = new THREE.Mesh(sunGeometry, sunMaterial);
        scene.add(sun);
        
        const coronaGeometry = new THREE.SphereGeometry(3.3, 64, 64);
        const coronaMaterial = new THREE.MeshBasicMaterial({
            color: 0xff9933,
            transparent: true,
            opacity: 0.15,
            side: THREE.BackSide
        });
        const corona = new THREE.Mesh(coronaGeometry, coronaMaterial);
        scene.add(corona);
        
        // Частицы вокруг Солнца
        const sunParticlesGeo = new THREE.BufferGeometry();
        const sunPartCount = 400;
        const sunPartPos = new Float32Array(sunPartCount * 3);
        for (let i = 0; i < sunPartCount * 3; i += 3) {
            const r = 3.5 + Math.random() * 1.5;
            const theta = Math.random() * Math.PI * 2;
            const phi = Math.acos(2 * Math.random() - 1);
            sunPartPos[i] = Math.sin(phi) * Math.cos(theta) * r;
            sunPartPos[i+1] = Math.sin(phi) * Math.sin(theta) * r;
            sunPartPos[i+2] = Math.cos(phi) * r;
        }
        sunParticlesGeo.setAttribute('position', new THREE.BufferAttribute(sunPartPos, 3));
        const sunParticlesMat = new THREE.PointsMaterial({ color: 0xffaa44, size: 0.08, transparent: true, blending: THREE.AdditiveBlending });
        const sunParticles = new THREE.Points(sunParticlesGeo, sunParticlesMat);
        scene.add(sunParticles);
        
        // --- ЗЕМЛЯ ---
        const earthGroup = new THREE.Group();
        earthGroup.position.set(8, 0.5, 0);
        
        const earthGeometry = new THREE.SphereGeometry(1.5, 64, 64);
        const textureLoader = new THREE.TextureLoader();
        const earthMap = textureLoader.load('https://threejs.org/examples/textures/planets/earth_atmos_2048.jpg');
        const earthSpecularMap = textureLoader.load('https://threejs.org/examples/textures/planets/earth_specular_2048.jpg');
        const earthNormalMap = textureLoader.load('https://threejs.org/examples/textures/planets/earth_normal_2048.jpg');
        const cloudMap = textureLoader.load('https://threejs.org/examples/textures/planets/earth_clouds_1024.png');
        
        const earthMaterial = new THREE.MeshPhongMaterial({
            map: earthMap,
            specularMap: earthSpecularMap,
            specular: new THREE.Color('grey'),
            shininess: 10,
            normalMap: earthNormalMap,
            normalScale: new THREE.Vector2(0.8, 0.8)
        });
        const earth = new THREE.Mesh(earthGeometry, earthMaterial);
        earthGroup.add(earth);
        
        const cloudGeometry = new THREE.SphereGeometry(1.51, 64, 64);
        const cloudMaterial = new THREE.MeshPhongMaterial({
            map: cloudMap,
            transparent: true,
            opacity: 0.6,
            blending: THREE.AdditiveBlending,
            side: THREE.DoubleSide
        });
        const clouds = new THREE.Mesh(cloudGeometry, cloudMaterial);
        earthGroup.add(clouds);
        scene.add(earthGroup);
        
        // --- МАГНИТНОЕ ПОЛЕ (линии и частицы) ---
        const magneticFieldGroup = new THREE.Group();
        earthGroup.add(magneticFieldGroup);
        
        // Линии поля (силовые линии)
        const lineCount = 16;
        const linePointsCount = 120;
        
        // Храним линии, чтобы менять их цвет
        const fieldLines = [];
        
        for (let j = 0; j < lineCount; j++) {
            const points = [];
            const radius = 2.8;
            const angleOffset = (j / lineCount) * Math.PI * 2;
            
            for (let i = 0; i <= linePointsCount; i++) {
                const t = (i / linePointsCount) * Math.PI * 2;
                const x = radius * Math.sin(t) * Math.cos(angleOffset) * 1.2;
                const y = radius * Math.cos(t) * 1.8;
                const z = radius * Math.sin(t) * Math.sin(angleOffset) * 1.2;
                points.push(new THREE.Vector3(x, y, z));
            }
            
            const geometry = new THREE.BufferGeometry().setFromPoints(points);
            const material = new THREE.LineBasicMaterial({ color: 0x44aaff });
            const line = new THREE.Line(geometry, material);
            magneticFieldGroup.add(line);
            fieldLines.push(line);
        }
        
        // Частицы (основной слой)
        const particleCount = 800;
        const particleGeo = new THREE.BufferGeometry();
        const particlePositions = new Float32Array(particleCount * 3);
        const particleColors = new Float32Array(particleCount * 3);
        const particleData = [];
        
        for (let i = 0; i < particleCount; i++) {
            const angleT = Math.random() * Math.PI * 2;
            const offsetAngle = Math.random() * Math.PI * 2;
            const distFactor = 1.8 + Math.random() * 2.0;
            
            const x = distFactor * Math.sin(angleT) * Math.cos(offsetAngle) * 1.2;
            const y = distFactor * Math.cos(angleT) * 2.0;
            const z = distFactor * Math.sin(angleT) * Math.sin(offsetAngle) * 1.2;
            
            particlePositions[i*3] = x;
            particlePositions[i*3+1] = y;
            particlePositions[i*3+2] = z;
            
            particleData.push({
                baseX: x, baseY: y, baseZ: z,
                speed: 0.2 + Math.random() * 1.0,
                offset: Math.random() * Math.PI * 2
            });
        }
        particleGeo.setAttribute('position', new THREE.BufferAttribute(particlePositions, 3));
        particleGeo.setAttribute('color', new THREE.BufferAttribute(particleColors, 3));
        
        const particleMat = new THREE.PointsMaterial({
            size: 0.15,
            vertexColors: true,
            transparent: true,
            blending: THREE.AdditiveBlending,
            depthWrite: false
        });
        const particles = new THREE.Points(particleGeo, particleMat);
        magneticFieldGroup.add(particles);
        
        // Второй слой мелких частиц
        const smallParticleGeo = new THREE.BufferGeometry();
        const smallCount = 500;
        const smallPos = new Float32Array(smallCount * 3);
        for (let i = 0; i < smallCount; i++) {
            const r = 2.0 + Math.random() * 2.5;
            const theta = Math.random() * Math.PI * 2;
            const phi = Math.acos(2 * Math.random() - 1);
            smallPos[i*3] = Math.sin(phi) * Math.cos(theta) * r * 1.2;
            smallPos[i*3+1] = Math.sin(phi) * Math.sin(theta) * r * 1.8;
            smallPos[i*3+2] = Math.cos(phi) * r * 1.2;
        }
        smallParticleGeo.setAttribute('position', new THREE.BufferAttribute(smallPos, 3));
        const smallParticleMat = new THREE.PointsMaterial({ color: 0x88aaff, size: 0.08, blending: THREE.AdditiveBlending, transparent: true });
        const smallParticles = new THREE.Points(smallParticleGeo, smallParticleMat);
        magneticFieldGroup.add(smallParticles);
        
        // Солнечный ветер
        const solarWindGroup = new THREE.Group();
        scene.add(solarWindGroup);
        const windParticleCount = 250;
        const windGeo = new THREE.BufferGeometry();
        const windPos = new Float32Array(windParticleCount * 3);
        for (let i = 0; i < windParticleCount; i++) {
            windPos[i*3] = (Math.random() * 5) + 2;
            windPos[i*3+1] = (Math.random() - 0.5) * 5;
            windPos[i*3+2] = (Math.random() - 0.5) * 5;
        }
        windGeo.setAttribute('position', new THREE.BufferAttribute(windPos, 3));
        const windMat = new THREE.PointsMaterial({ color: 0xffaa55, size: 0.1, blending: THREE.AdditiveBlending, transparent: true });
        const windParticles = new THREE.Points(windGeo, windMat);
        solarWindGroup.add(windParticles);
        
        // --- ИНТЕРАКТИВНОСТЬ: ПОЛЗУНОК И СВОРАЧИВАНИЕ ---
        // Элементы управления
        const stormSlider = document.getElementById('stormLevel');
        const panelHeader = document.getElementById('panelHeader');
        const panelContent = document.getElementById('panelContent');
        const toggleIcon = document.getElementById('toggleIcon');
        const statusEl = document.getElementById('humanStatus');
        const statusTitle = document.getElementById('statusTitle');
        const statusDesc = document.getElementById('statusDesc');
        
        // Сворачивание панели
        let isPanelCollapsed = false;
        panelHeader.addEventListener('click', () => {
            isPanelCollapsed = !isPanelCollapsed;
            panelContent.classList.toggle('collapsed', isPanelCollapsed);
            toggleIcon.style.transform = isPanelCollapsed ? 'rotate(-90deg)' : 'rotate(0deg)';
        });
        
        // Функция обновления состояния человека и цвета бури
        function updateStormEffect(level) {
            // level от 0 до 1
            // Меняем цвет линий от голубого (0.6) до красного/фиолетового (0.0)
            const hue = 0.6 - level * 0.5; // от 0.6 (голубой) до 0.1 (красноватый)
            
            fieldLines.forEach(line => {
                if (line.material) {
                    line.material.color.setHSL(hue, 1.0, 0.5 + level * 0.3);
                }
            });
            
            // Меняем цвет частиц
            if (particles.geometry.attributes.color) {
                const colors = particles.geometry.attributes.color.array;
                for (let i = 0; i < particleCount; i++) {
                    const color = new THREE.Color().setHSL(hue + Math.random()*0.1, 1.0, 0.5 + level*0.3);
                    colors[i*3] = color.r;
                    colors[i*3+1] = color.g;
                    colors[i*3+2] = color.b;
                }
                particles.geometry.attributes.color.needsUpdate = true;
            }
            
            // Масштабируем группу поля (пульсация)
            magneticFieldGroup.scale.set(0.9 + level * 0.3, 0.9 + level * 0.3, 0.9 + level * 0.3);
            
            // Обновляем статус человека (без эмодзи)
            if (level < 0.3) {
                statusTitle.innerText = 'Самочувствие: отличное';
                statusDesc.innerText = 'Геомагнитная обстановка спокойная. Никаких симптомов нет.';
                statusEl.style.borderLeftColor = '#44ff44';
            } else if (level < 0.6) {
                statusTitle.innerText = 'Самочувствие: нормальное';
                statusDesc.innerText = 'Небольшие колебания. Метеочувствительные люди могут ощутить легкую сонливость.';
                statusEl.style.borderLeftColor = '#ffff44';
            } else if (level < 0.8) {
                statusTitle.innerText = 'Самочувствие: ухудшение';
                statusDesc.innerText = 'Умеренная буря. Возможны головные боли, перепады настроения.';
                statusEl.style.borderLeftColor = '#ff8844';
            } else {
                statusTitle.innerText = 'Самочувствие: сильное влияние';
                statusDesc.innerText = 'Сильная магнитная буря. Головная боль, слабость, скачки давления. Рекомендуется отдых.';
                statusEl.style.borderLeftColor = '#ff4444';
            }
        }
        
        // Слушаем слайдер
        stormSlider.addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            updateStormEffect(val);
        });
        
        // Начальное значение
        updateStormEffect(0.7);
        
        // --- Анимация ---
        let clock = new THREE.Clock();
        
        function animate() {
            const delta = clock.getDelta();
            const elapsedTime = performance.now() / 1000;
            
            // Вращение Солнца
            sun.rotation.y += 0.001;
            corona.rotation.y += 0.0005;
            sunParticles.rotation.y += 0.0008;
            
            // Вращение Земли
            earth.rotation.y += 0.005;
            clouds.rotation.y += 0.006;
            
            // Орбита Земли
            earthGroup.position.x = 8 * Math.cos(elapsedTime * 0.2);
            earthGroup.position.z = 8 * Math.sin(elapsedTime * 0.2);
            earthGroup.position.y = 0.5 * Math.sin(elapsedTime * 0.5);
            
            // Вращение магнитного поля
            magneticFieldGroup.rotation.y += 0.001;
            magneticFieldGroup.rotation.x += 0.0003;
            
            // Солнечный ветер
            if (windParticles.geometry.attributes.position) {
                const pos = windParticles.geometry.attributes.position.array;
                const level = parseFloat(stormSlider.value);
                for (let i = 0; i < windParticleCount; i++) {
                    pos[i*3] += 0.02 + level * 0.05;
                    if (pos[i*3] > 12) {
                        pos[i*3] = 2 + Math.random() * 2;
                        pos[i*3+1] = (Math.random() - 0.5) * 5;
                        pos[i*3+2] = (Math.random() - 0.5) * 5;
                    }
                }
                windParticles.geometry.attributes.position.needsUpdate = true;
            }
            
            // Звезды
            stars.rotation.y += 0.0001;
            
            controls.update();
            composer.render();
            
            requestAnimationFrame(animate);
        }
        
        animate();
        
        window.addEventListener('resize', onWindowResize, false);
        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
            composer.setSize(window.innerWidth, window.innerHeight);
        }
    </script>
</body>
</html>
