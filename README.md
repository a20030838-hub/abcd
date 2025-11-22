<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>청소년 프로그램 개발과 평가 - Aurora Edition</title>
    
    <!-- 폰트: 명조 계열과 산세리프의 조화 -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&family=Noto+Serif+KR:wght@200;400;700;900&family=Outfit:wght@100;500;800&display=swap" rel="stylesheet">

    <style>
        /* 기본 설정 */
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: #050505; /* 아주 깊은 검정 */
            overflow: hidden;
            font-family: 'Outfit', 'Noto Serif KR', serif;
        }

        /* 3D 캔버스 (배경) */
        #canvas-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            filter: blur(1px) brightness(1.2); /* 몽환적인 느낌 추가 */
        }

        /* 콘텐츠 레이어 */
        .ui-wrapper {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 10;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            pointer-events: none; /* 마우스가 배경 3D에 반응하도록 */
        }

        /* 1. 상단 소제목 */
        .top-label {
            font-family: 'Cinzel', serif;
            font-size: 1.2rem;
            letter-spacing: 1.2rem;
            color: #a8c0ff;
            margin-bottom: 2vh;
            opacity: 0;
            text-transform: uppercase;
            text-shadow: 0 0 20px rgba(168, 192, 255, 0.6);
        }

        /* 2. 메인 타이틀 (홀로그래픽 효과) */
        .main-title {
            font-size: 6rem;
            font-weight: 900;
            line-height: 1.1;
            text-align: center;
            margin: 0;
            padding: 20px;
            
            /* 오로라 텍스트 클리핑 */
            background: linear-gradient(
                135deg, 
                #ffffff 0%, 
                #00f260 25%, 
                #0575E6 50%, 
                #b21f1f 75%, 
                #fdbb2d 100%
            );
            background-size: 300% 300%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            
            animation: auroraText 5s ease infinite;
            opacity: 0;
            filter: blur(20px);
            transform: translateY(50px);
        }

        /* 텍스트 색상 흐름 애니메이션 */
        @keyframes auroraText {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .sub-title {
            font-family: 'Outfit', sans-serif;
            font-size: 1.8rem;
            font-weight: 100;
            letter-spacing: 5px;
            color: rgba(255, 255, 255, 0.8);
            margin-top: 10px;
            opacity: 0;
        }

        /* 3. 조원 소개 섹션 (글래스 카드) */
        .team-wrapper {
            margin-top: 8vh;
            display: flex;
            gap: 40px;
            perspective: 1000px;
        }

        .member-card {
            pointer-events: auto; /* 카드에는 마우스 반응 */
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 20px 40px;
            border-radius: 15px;
            backdrop-filter: blur(10px); /* 유리 효과 */
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
            text-align: center;
            
            opacity: 0;
            transform: rotateX(90deg); /* 처음에 누워있음 */
            transition: transform 0.3s, background 0.3s;
        }

        .member-card:hover {
            transform: translateY(-10px) !important;
            background: rgba(255, 255, 255, 0.1);
            border-color: rgba(0, 242, 96, 0.5); /* 오로라 그린 */
            box-shadow: 0 0 30px rgba(0, 242, 96, 0.2);
        }

        .role {
            font-size: 0.7rem;
            color: #00f260;
            letter-spacing: 2px;
            text-transform: uppercase;
            margin-bottom: 5px;
        }

        .name {
            font-size: 1.5rem;
            font-weight: 700;
            color: #fff;
            font-family: 'Noto Serif KR', serif;
        }

        /* 하단 장식 요소 */
        .footer-deco {
            position: absolute;
            bottom: 50px;
            width: 1px;
            height: 0px; /* JS로 늘림 */
            background: linear-gradient(to top, transparent, #fff, transparent);
            opacity: 0.5;
        }

        /* 반응형 */
        @media (max-width: 768px) {
            .main-title { font-size: 3rem; }
            .team-wrapper { flex-wrap: wrap; justify-content: center; gap: 15px; }
            .member-card { padding: 15px 30px; width: 40%; }
        }
    </style>
</head>
<body>

    <!-- 3D 배경 -->
    <div id="canvas-container"></div>

    <!-- UI 레이어 -->
    <div class="ui-wrapper">
        <div class="top-label">2025 Mokwon University Group Project</div>
        
        <h1 class="main-title">청소년 프로그램<br>개발과 평가</h1>
        <div class="sub-title">Creative Solution & Evaluation</div>

        <div class="team-wrapper">
            <div class="member-card">
                <div class="role">Research</div>
                <div class="name">강진</div>
            </div>
            <div class="member-card">
                <div class="role">Planning</div>
                <div class="name">채은</div>
            </div>
            <div class="member-card">
                <div class="role">Design</div>
                <div class="name">서하</div>
            </div>
            <div class="member-card">
                <div class="role">Present</div>
                <div class="name">영훈</div>
            </div>
        </div>

        <div class="footer-deco"></div>
    </div>

    <!-- CDN 로드 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>

    <script>
        /** 
         * PART 1: THREE.JS - 오로라 웨이브 (Aurora Wave)
         * 
         * 수천 개의 정점이 있는 평면(Plane)을 만들고, 
         * Sine, Cosine 파동을 이용해 물결치게 만든 뒤
         * 3개의 움직이는 조명(Cyan, Magenta, Blue)을 비추어 오로라 효과를 냅니다.
         */
        const scene = new THREE.Scene();
        // 배경색과 안개(Fog)로 깊이감 추가
        scene.fog = new THREE.FogExp2(0x050505, 0.0015);

        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(0, -50, 200); // 카메라를 살짝 아래에서 위로 보게 설정
        camera.lookAt(0, 0, 0);

        const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(window.devicePixelRatio); // 고해상도 지원
        document.getElementById('canvas-container').appendChild(renderer.domElement);

        // -- 1. 지오메트리 (물결칠 표면) --
        // 세그먼트를 많이 주어(60x60) 부드러운 곡선 표현
        const planeGeometry = new THREE.PlaneGeometry(800, 400, 80, 80);
        
        // -- 2. 머티리얼 (재질) --
        // 빛에 반응하는 MeshLambertMaterial 사용, Low Poly 스타일을 위해 flatShading 사용
        const planeMaterial = new THREE.MeshPhongMaterial({
            color: 0x222222,
            emissive: 0x000000,
            specular: 0x111111,
            shininess: 50, // 광택
            side: THREE.DoubleSide,
            flatShading: true, // 다각형 느낌 (크리스탈 느낌)
            vertexColors: false
        });

        const plane = new THREE.Mesh(planeGeometry, planeMaterial);
        scene.add(plane);

        // -- 3. 조명 (오로라 색상) --
        // 전체 은은한 빛
        const ambientLight = new THREE.AmbientLight(0x222222); 
        scene.add(ambientLight);

        // 움직이는 조명 1 (청록색)
        const light1 = new THREE.PointLight(0x00ffff, 2, 400);
        scene.add(light1);

        // 움직이는 조명 2 (자주색/핑크)
        const light2 = new THREE.PointLight(0xff00ff, 2, 400);
        scene.add(light2);

        // 움직이는 조명 3 (파란색)
        const light3 = new THREE.PointLight(0x0000ff, 2, 400);
        scene.add(light3);


        // -- 4. 애니메이션 변수 --
        let count = 0;
        
        // 정점들의 원래 위치 저장
        const positionAttribute = planeGeometry.attributes.position;
        const originalPositions = [];
        for (let i = 0; i < positionAttribute.count; i++) {
            originalPositions.push(
                positionAttribute.getX(i),
                positionAttribute.getY(i),
                positionAttribute.getZ(i)
            );
        }

        function animateThree() {
            requestAnimationFrame(animateThree);
            count += 0.02;

            // 1. 파도 효과 (Wave Calculation)
            const positions = planeGeometry.attributes.position;
            
            for (let i = 0; i < positions.count; i++) {
                const x = originalPositions[i * 3];
                const y = originalPositions[i * 3 + 1];
                
                // x, y 좌표에 따라 서로 다른 사인파를 적용하여 복잡한 물결 생성
                // 여기가 오로라 움직임의 핵심 알고리즘
                const z = Math.sin(x * 0.01 + count) * 20 + Math.sin(y * 0.01 + count) * 20;
                
                positions.setZ(i, z);
            }
            positions.needsUpdate = true;

            // 2. 조명 움직임 (Lights Dance)
            // 조명들이 원을 그리며 회전하여 색상이 계속 변하는 느낌을 줌
            light1.position.set(Math.sin(count * 0.7) * 150, Math.cos(count * 0.5) * 150, 50);
            light2.position.set(Math.cos(count * 0.3) * 150, Math.sin(count * 0.5) * 150, 80);
            light3.position.set(Math.sin(count * 0.3) * 200, Math.sin(count * 0.2) * 200, 60);

            renderer.render(scene, camera);
        }
        animateThree();

        // 창 크기 조절
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });


        /**
         * PART 2: GSAP - 시네마틱 타임라인
         * 12초 동안 이어지는 텍스트와 요소의 등장 연출
         */
        const tl = gsap.timeline();

        // 1. 카메라 무빙 효과 (멀리서 다가옴)
        gsap.from(camera.position, {
            z: 500,
            duration: 4,
            ease: "power3.out"
        });

        // 2. 상단 라벨: 천천히 투명도 증가
        tl.to(".top-label", {
            opacity: 1,
            duration: 2,
            ease: "power2.out"
        }, 1.0); // 1초 딜레이

        // 3. 메인 타이틀: 블러 제거 + 아래에서 위로 + 스케일 축소
        tl.to(".main-title", {
            opacity: 1,
            filter: "blur(0px)",
            y: 0,
            scale: 0.9, // 약간 작아지면서 자리 잡음
            duration: 2.5,
            ease: "power4.out" // 강렬한 등장
        }, 2.0);

        // 4. 서브 타이틀: 글자 간격 좁혀지며 등장
        tl.to(".sub-title", {
            opacity: 1,
            letterSpacing: "2px", // 5px에서 2px로 모임
            duration: 1.5,
            ease: "power2.out"
        }, 3.5);

        // 5. 조원 카드: 3D 회전하며 순차 등장 (Stagger)
        tl.to(".member-card", {
            opacity: 1,
            rotateX: 0, // 누워있던 카드가 일어섬
            duration: 1.2,
            stagger: 0.3, // 0.3초 간격으로 하나씩
            ease: "back.out(1.7)" // 팅기는 효과
        }, 4.5);

        // 6. 하단 데코 라인: 쭉 늘어나기
        tl.to(".footer-deco", {
            height: "150px",
            duration: 2,
            ease: "power1.inOut"
        }, 6.0);

        // 7. 피날레: 타이틀이 미세하게 계속 움직임 (Yoyo Effect)
        gsap.to(".main-title", {
            y: -10,
            textShadow: "0 0 50px rgba(255,255,255,0.5)",
            duration: 3,
            repeat: -1,
            yoyo: true,
            ease: "sine.inOut",
            delay: 6
        });

    </script>
</body>
</html>
