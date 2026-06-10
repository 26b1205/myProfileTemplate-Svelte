<script>
	import { onDestroy } from 'svelte';

	let currentMode = $state('encyclopedia'); // 'encyclopedia' or 'battle'

	// Cats data structure
	const cats = [
		{
			id: 'cat',
			names: ['ネコ', 'ネコビルダー', 'ネコモヒカン'],
			englishNames: ['Cat', 'Macho Cat', 'Mohawk Cat'],
			descriptions: [
				'安価で生産できる基本キャラ。すべての戦術の基礎となる、頼りになる盾役だにゃ。',
				'体を鍛えてマッチョになったネコ。鍛え抜かれた筋肉で攻撃力とHPがちょっと上がったにゃ。',
				'頭に真っ赤なモヒカンをのせたネコ。戦闘能力が飛躍的にアップし、足も速くなったにゃ！'
			],
			stats: [
				{ cost: '75円', hp: '240', atk: '20', range: '140', speed: '8' },
				{ cost: '75円', hp: '320', atk: '28', range: '140', speed: '8' },
				{ cost: '75円', hp: '480', atk: '45', range: '140', speed: '10' }
			],
			color: '#3b82f6',
			svgPaths: [
				// Normal Form SVG
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="35,42 45,20 58,40" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="62,40 75,20 85,42" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="60" cy="70" r="35" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="48" cy="65" r="3" fill="#1e293b" /><circle cx="72" cy="65" r="3" fill="#1e293b" /><path d="M 54 73 Q 57 77 60 73 Q 63 77 66 73" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="42" cy="72" r="3" fill="#f43f5e" opacity="0.4" /><circle cx="78" cy="72" r="3" fill="#f43f5e" opacity="0.4" /><circle cx="48" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				// Evolved Form SVG (Macho Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="35,42 45,20 58,40" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="62,40 75,20 85,42" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 25 70 C 5 70 10 50 22 55" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 95 70 C 115 70 110 50 98 55" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="60" cy="70" r="35" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="48" cy="65" r="3" fill="#1e293b" /><circle cx="72" cy="65" r="3" fill="#1e293b" /><path d="M 43 57 L 51 61" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><path d="M 77 57 L 69 61" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><path d="M 54 73 Q 57 77 60 73 Q 63 77 66 73" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="48" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				// True Form SVG (Mohawk Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 52 35 Q 60 2 68 35 Z" fill="#ef4444" stroke="#b91c1c" stroke-width="3" /><circle cx="60" cy="12" r="5" fill="#f59e0b" /><polygon points="35,42 45,23 58,40" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="62,40 75,23 85,42" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="60" cy="70" r="35" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="48" cy="65" r="3" fill="#1e293b" /><circle cx="72" cy="65" r="3" fill="#1e293b" /><path d="M 54 74 Q 60 78 66 74" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="48" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`
			]
		},
		{
			id: 'tank',
			names: ['タンクネコ', 'ネコカベ', 'ネコゴム'],
			englishNames: ['Tank Cat', 'Wall Cat', 'Eraser Cat'],
			descriptions: [
				'高い体力を誇る盾専用キャラクター。攻撃力はほとんどないが、味方を守る壁として超優秀にゃ。',
				'壁に進化して防御力がさらに上がったタンクネコ。強靭なブロックで敵の進軍を阻むにゃ！',
				'消しゴムのような超頑丈な盾に進化した姿。もはや崩すことのできない絶対防壁だにゃ！'
			],
			stats: [
				{ cost: '150円', hp: '1200', atk: '2', range: '110', speed: '8' },
				{ cost: '150円', hp: '1800', atk: '4', range: '110', speed: '8' },
				{ cost: '150円', hp: '2900', atk: '8', range: '110', speed: '10' }
			],
			color: '#10b981',
			svgPaths: [
				// Normal Form SVG
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="43,28 48,12 55,27" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="65,27 72,12 77,28" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><rect x="40" y="25" width="40" height="78" rx="14" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="45" r="3" fill="#1e293b" /><circle cx="67" cy="45" r="3" fill="#1e293b" /><path d="M 57 52 Q 60 55 63 52" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="48" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				// Evolved Form SVG (Wall Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="35" y="20" width="50" height="83" rx="4" fill="white" stroke="#1e293b" stroke-width="4" /><line x1="35" y1="45" x2="85" y2="45" stroke="#e2e8f0" stroke-width="2" /><line x1="35" y1="70" x2="85" y2="70" stroke="#e2e8f0" stroke-width="2" /><line x1="60" y1="20" x2="60" y2="45" stroke="#e2e8f0" stroke-width="2" /><line x1="48" y1="45" x2="48" y2="70" stroke="#e2e8f0" stroke-width="2" /><line x1="72" y1="45" x2="72" y2="70" stroke="#e2e8f0" stroke-width="2" /><circle cx="53" cy="32" r="3" fill="#1e293b" /><circle cx="67" cy="32" r="3" fill="#1e293b" /><path d="M 58 38 Q 60 40 62 38" fill="none" stroke="#1e293b" stroke-width="3" /><circle cx="45" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				// True Form SVG (Eraser Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="35" y="20" width="50" height="83" rx="4" fill="white" stroke="#1e293b" stroke-width="4" /><path d="M 35 50 L 85 50 L 85 103 L 35 103 Z" fill="#2563eb" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><rect x="42" y="60" width="36" height="15" fill="#facc15" rx="2" /><text x="60" y="71" font-size="9" font-weight="bold" fill="#1e293b" text-anchor="middle">MONO</text><circle cx="53" cy="32" r="3" fill="#1e293b" /><circle cx="67" cy="32" r="3" fill="#1e293b" /><path d="M 57 38 Q 60 41 63 38" fill="none" stroke="#1e293b" stroke-width="3" /><circle cx="45" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`
			]
		},
		{
			id: 'gross',
			names: ['キモネコ', '美脚ネコ', 'ムキあしネコ'],
			englishNames: ['Gross Cat', 'Sexy Legs Cat', 'Macho Legs Cat'],
			descriptions: [
				'美脚が自慢の遠距離攻撃型キャラクター。その長い脚から繰り出されるキックは強力にゃ！',
				'さらに美しく、セクシーに伸びた脚を持つネコ。スラリとした美脚で敵を一網打尽にするにゃ！',
				'すさまじい筋肉を誇る美脚ネコ。鍛え上げられたムキあしから放つキックは強力無比だにゃ！'
			],
			stats: [
				{ cost: '400円', hp: '600', atk: '100', range: '350', speed: '8' },
				{ cost: '400円', hp: '900', atk: '160', range: '350', speed: '8' },
				{ cost: '400円', hp: '1450', atk: '280', range: '350', speed: '10' }
			],
			color: '#e11d48',
			svgPaths: [
				// Normal Form SVG
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="45,35 50,23 57,34" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><polygon points="63,34 70,23 75,35" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="42" r="2.5" fill="#1e293b" /><circle cx="67" cy="42" r="2.5" fill="#1e293b" /><path d="M 56 49 Q 60 52 64 49" fill="none" stroke="#1e293b" stroke-width="3.5" /><path d="M 50 62 L 45 105 L 52 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 70 62 L 75 105 L 68 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /></svg>`,
				// Evolved Form SVG (Sexy Legs Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="45,35 50,23 57,34" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><polygon points="63,34 70,23 75,35" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="42" r="2.5" fill="#1e293b" /><circle cx="67" cy="42" r="2.5" fill="#1e293b" /><path d="M 56 49 Q 60 52 64 49" fill="none" stroke="#1e293b" stroke-width="3.5" /><path d="M 50 62 Q 42 85 45 105 L 52 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 70 62 Q 78 85 75 105 L 68 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 45 105 L 41 109 L 46 109 Z" fill="#ec4899" /><path d="M 75 105 L 79 109 L 74 109 Z" fill="#ec4899" /></svg>`,
				// True Form SVG (Macho Legs)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="45,35 50,23 57,34" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><polygon points="63,34 70,23 75,35" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="42" r="2.5" fill="#1e293b" /><circle cx="67" cy="42" r="2.5" fill="#1e293b" /><path d="M 56 49 Q 60 52 64 49" fill="none" stroke="#1e293b" stroke-width="3.5" /><path d="M 50 62 Q 38 75 42 88 T 45 105 L 52 105" fill="none" stroke="#1e293b" stroke-width="5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 70 62 Q 82 75 78 88 T 75 105 L 68 105" fill="none" stroke="#1e293b" stroke-width="5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 45 74 Q 40 80 43 85" fill="none" stroke="#1e293b" stroke-width="2" /><path d="M 75 74 Q 80 80 77 85" fill="none" stroke="#1e293b" stroke-width="2" /></svg>`
			]
		},
		{
			id: 'titan',
			names: ['巨神ネコ', 'ネコダラボッチ', 'ネコジャラミ'],
			englishNames: ['Titan Cat', 'Mythical Titan Cat', 'Jamiera Cat'],
			descriptions: [
				'超破壊力と強靭な体力を備えた最高峰のネコキャラ。たまに敵をノックバックさせる大打撃を放つにゃ！',
				'神話級のパワーを手に入れた大巨人ネコ。怒りに満ちた一撃は範囲内の敵をまとめて薙ぎ払うにゃ！',
				'怪しいTシャツを着た最強のネコ。たまに敵をふっとばして動きを遅くする極大衝撃波を放つにゃ！'
			],
			stats: [
				{ cost: '1300円', hp: '4000', atk: '650', range: '150', speed: '8' },
				{ cost: '1300円', hp: '6200', atk: '1100', range: '150', speed: '8' },
				{ cost: '1300円', hp: '9800', atk: '1950', range: '150', speed: '8' }
			],
			color: '#7c3aed',
			svgPaths: [
				// Normal Form SVG
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="35" y="45" width="50" height="52" rx="10" fill="white" stroke="#1e293b" stroke-width="4.5" /><path d="M 48 58 L 54 62" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 72 58 L 66 62" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 53 72 Q 60 76 67 72" fill="none" stroke="#1e293b" stroke-width="4" /><path d="M 35 55 C 15 55 10 90 32 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 85 55 C 105 55 110 90 88 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="45" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				// Evolved Form SVG (Mythical Titan Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 45 46 Q 30 20 40 35 Q 50 15 55 46" fill="#1e293b" /><path d="M 75 46 Q 90 20 80 35 Q 70 15 65 46" fill="#1e293b" /><rect x="35" y="45" width="50" height="52" rx="10" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" /><path d="M 46 58 L 54 60" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 74 58 L 66 60" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 50 72 L 70 72" stroke="#1e293b" stroke-width="3.5" stroke-linecap="round" /><polygon points="32,50 25,43 36,45" fill="#1e293b" /><polygon points="88,50 95,43 84,45" fill="#1e293b" /><path d="M 35 55 C 12 55 8 92 32 88" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" /><path d="M 85 55 C 108 55 112 92 88 88" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" /><circle cx="45" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				// True Form SVG (Jamiera Cat)
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 38 48 C 30 25 90 25 82 48 Z" fill="#1e293b" /><rect x="35" y="45" width="50" height="52" rx="10" fill="white" stroke="#1e293b" stroke-width="4.5" /><path d="M 45 45 Q 60 60 75 45" fill="none" stroke="#ef4444" stroke-width="3" /><text x="60" y="75" font-size="10" font-weight="bold" fill="#ef4444" text-anchor="middle">NYAN</text><rect x="42" y="52" width="16" height="10" rx="2" fill="#1e293b" /><rect x="62" y="52" width="16" height="10" rx="2" fill="#1e293b" /><line x1="58" y1="56" x2="62" y2="56" stroke="#1e293b" stroke-width="3" /><path d="M 35 55 C 15 55 10 90 32 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 85 55 C 105 55 110 90 88 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="45" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`
			]
		}
	];

	// Shared form level state for the 4 cats (Index 0 = Cat, 1 = Tank, 2 = Gross, 3 = Titan)
	let catForms = $state([0, 0, 0, 0]);

	// Encyclopedia state variables
	let selectedIndex = $state(0);
	let activeCat = $derived(cats[selectedIndex]);
	let isEvolving = $state(false);

	function changeCat(index) {
		selectedIndex = index;
	}

	function evolveCat() {
		if (isEvolving) return;
		isEvolving = true;
		setTimeout(() => {
			catForms[selectedIndex] = (catForms[selectedIndex] + 1) % 3;
			isEvolving = false;
		}, 600);
	}

	// Battle Simulator state variables
	let gameStatus = $state('idle'); // 'idle', 'playing', 'victory', 'defeat'
	let money = $state(0);
	let walletLevel = $state(1);
	let playerCastleHp = $state(1000);
	let enemyCastleHp = $state(1000);

	let allies = $state([]);
	let enemies = $state([]);

	let cooldowns = $state({ cat: 0, tank: 0, gross: 0, titan: 0 });
	let cooldownMax = { cat: 2, tank: 4, gross: 8, titan: 20 };

	const walletLevels = [
		{ max: 300, speed: 1.5, upgradeCost: 100 },
		{ max: 500, speed: 2.5, upgradeCost: 200 },
		{ max: 800, speed: 4.0, upgradeCost: 350 },
		{ max: 1200, speed: 6.0, upgradeCost: 550 },
		{ max: 1800, speed: 9.0, upgradeCost: 850 },
		{ max: 2500, speed: 13.0, upgradeCost: 1250 },
		{ max: 3500, speed: 18.0, upgradeCost: 1800 },
		{ max: 5000, speed: 25.0, upgradeCost: null } // level 8 max
	];

	let walletMax = $derived(walletLevels[walletLevel - 1].max);
	let walletSpeed = $derived(walletLevels[walletLevel - 1].speed);

	let entityIdCounter = 0;
	let gameInterval;

	function startGame() {
		gameStatus = 'playing';
		money = 0;
		walletLevel = 1;
		playerCastleHp = 1000;
		enemyCastleHp = 1000;
		allies = [];
		enemies = [];
		cooldowns = { cat: 0, tank: 0, gross: 0, titan: 0 };

		// Spawn initial basic enemies to start the challenge
		spawnEnemy('doge');
		
		if (gameInterval) clearInterval(gameInterval);

		let ticks = 0;
		gameInterval = setInterval(() => {
			if (gameStatus !== 'playing') {
				clearInterval(gameInterval);
				return;
			}

			ticks++;

			// 1. Generate money
			money = Math.min(money + walletSpeed, walletMax);

			// 2. Reduce cooldowns
			cooldowns = {
				cat: Math.max(0, cooldowns.cat - 0.04),
				tank: Math.max(0, cooldowns.tank - 0.04),
				gross: Math.max(0, cooldowns.gross - 0.04),
				titan: Math.max(0, cooldowns.titan - 0.04)
			};

			// 3. Auto enemy spawner
			if (ticks % 80 === 0) { // every 3.2 seconds
				const rand = Math.random();
				if (rand < 0.45) {
					spawnEnemy('doge');
				} else if (rand < 0.8) {
					spawnEnemy('snache');
				} else {
					spawnEnemy('gory');
				}
			}

			// 4. Update combat and movement physics
			updateEntities();
		}, 40);
	}

	function spawnAlly(catIndex) {
		const cat = cats[catIndex];
		const form = catForms[catIndex];
		const cost = parseInt(cat.stats[form].cost);

		if (money < cost || cooldowns[cat.id] > 0) return;

		money -= cost;
		cooldowns[cat.id] = cooldownMax[cat.id];

		const newAlly = {
			id: `ally_${entityIdCounter++}`,
			type: cat.id,
			form: form,
			name: cat.names[form],
			x: 12, // starts at player castle exit
			hp: parseInt(cat.stats[form].hp),
			maxHp: parseInt(cat.stats[form].hp),
			atk: parseInt(cat.stats[form].atk),
			range: getRangePercent(cat.id),
			speed: getSpeedPercent(cat.id, form),
			svg: cat.svgPaths[form],
			color: cat.color,
			isAttacking: false,
			attackTimer: 0,
			hitEffect: false
		};

		allies = [...allies, newAlly];
	}

	function spawnEnemy(type) {
		let newEnemy;
		if (type === 'doge') {
			newEnemy = {
				id: `enemy_${entityIdCounter++}`,
				type: 'doge',
				name: 'わんこ',
				x: 88, // starts at enemy castle
				hp: 240,
				maxHp: 240,
				atk: 20,
				range: 4,
				speed: 0.5,
				value: 45,
				svg: `<svg viewBox="0 0 100 100" class="w-full h-full"><ellipse cx="50" cy="60" rx="30" ry="22" fill="white" stroke="#1e293b" stroke-width="4"/><path d="M 28 45 C 20 40 25 25 35 38" fill="white" stroke="#1e293b" stroke-width="4"/><path d="M 72 45 C 80 40 75 25 65 38" fill="white" stroke="#1e293b" stroke-width="4"/><circle cx="42" cy="55" r="3" fill="#1e293b"/><circle cx="58" cy="55" r="3" fill="#1e293b"/><ellipse cx="50" cy="62" rx="4" ry="3" fill="#1e293b"/><path d="M 46 72 Q 50 76 54 72" fill="none" stroke="#1e293b" stroke-width="3"/></svg>`,
				color: '#f8fafc',
				isAttacking: false,
				attackTimer: 0,
				hitEffect: false
			};
		} else if (type === 'snache') {
			newEnemy = {
				id: `enemy_${entityIdCounter++}`,
				type: 'snache',
				name: 'にょろ',
				x: 88,
				hp: 130,
				maxHp: 130,
				atk: 32,
				range: 3.5,
				speed: 0.8,
				value: 35,
				svg: `<svg viewBox="0 0 100 100" class="w-full h-full"><path d="M 30 90 Q 50 80 40 60 Q 30 40 50 25 Q 70 10 75 30 Q 60 50 70 70 Q 80 90 60 90" fill="none" stroke="#f472b6" stroke-width="12" stroke-linecap="round" stroke-linejoin="round"/><circle cx="58" cy="22" r="2.5" fill="#1e293b"/><circle cx="68" cy="24" r="2.5" fill="#1e293b"/><path d="M 64 30 L 72 32" stroke="#ef4444" stroke-width="2"/></svg>`,
				color: '#f472b6',
				isAttacking: false,
				attackTimer: 0,
				hitEffect: false
			};
		} else if (type === 'gory') {
			newEnemy = {
				id: `enemy_${entityIdCounter++}`,
				type: 'gory',
				name: 'ゴリさん',
				x: 88,
				hp: 1400,
				maxHp: 1400,
				atk: 85,
				range: 4,
				speed: 0.7,
				value: 220,
				svg: `<svg viewBox="0 0 100 100" class="w-full h-full"><rect x="30" y="32" width="40" height="50" rx="8" fill="#64748b" stroke="#1e293b" stroke-width="4"/><circle cx="35" cy="45" r="14" fill="#64748b" stroke="#1e293b" stroke-width="4"/><circle cx="65" cy="45" r="14" fill="#64748b" stroke="#1e293b" stroke-width="4"/><circle cx="43" cy="43" r="2.5" fill="white"/><circle cx="57" cy="43" r="2.5" fill="white"/><path d="M 46 58 L 54 58" stroke="#1e293b" stroke-width="3" stroke-linecap="round"/><ellipse cx="50" cy="52" rx="4" ry="2" fill="#1e293b"/></svg>`,
				color: '#64748b',
				isAttacking: false,
				attackTimer: 0,
				hitEffect: false
			};
		}
		enemies = [...enemies, newEnemy];
	}

	function upgradeWallet() {
		const current = walletLevels[walletLevel - 1];
		if (!current.upgradeCost || money < current.upgradeCost) return;
		money -= current.upgradeCost;
		walletLevel += 1;
	}

	function getRangePercent(catId) {
		if (catId === 'cat') return 4.5;
		if (catId === 'tank') return 3;
		if (catId === 'gross') return 18;
		if (catId === 'titan') return 5.5;
		return 5;
	}

	function getSpeedPercent(catId, form) {
		let base = 0.45;
		if (catId === 'cat') base = 0.55;
		if (catId === 'tank') base = 0.4;
		if (catId === 'gross') base = 0.5;
		if (catId === 'titan') base = 0.32;

		if (form === 2) base *= 1.25; // Speed boost in third form
		return base;
	}

	function updateEntities() {
		// A. Update Allies
		let nextAllies = allies.map(ally => {
			let updated = { ...ally };
			// Find closest enemy in front
			let targets = enemies.filter(e => e.x > ally.x);
			targets.sort((a, b) => a.x - b.x);
			let target = targets[0];

			let isCloseToEnemy = target && (target.x - ally.x) <= ally.range;
			let isCloseToCastle = (88 - ally.x) <= ally.range; // enemy castle is at 88%

			if (isCloseToEnemy) {
				updated.isAttacking = true;
				updated.attackTimer = (updated.attackTimer || 0) + 40;
				if (updated.attackTimer >= 800) {
					updated.attackTimer = 0;
					// Deal damage to target (id-safe find)
					const targetEnemy = enemies.find(e => e.id === target.id);
					if (targetEnemy) {
						targetEnemy.hp -= ally.atk;
						targetEnemy.hitEffect = true;
						setTimeout(() => {
							const found = enemies.find(e => e.id === target.id);
							if (found) found.hitEffect = false;
						}, 200);
					}
				}
			} else if (isCloseToCastle) {
				updated.isAttacking = true;
				updated.attackTimer = (updated.attackTimer || 0) + 40;
				if (updated.attackTimer >= 800) {
					updated.attackTimer = 0;
					enemyCastleHp = Math.max(0, enemyCastleHp - ally.atk);
					if (enemyCastleHp <= 0) {
						gameStatus = 'victory';
					}
				}
			} else {
				updated.isAttacking = false;
				updated.attackTimer = 0;
				updated.x += ally.speed;
			}
			return updated;
		});

		// B. Update Enemies
		let nextEnemies = enemies.map(enemy => {
			let updated = { ...enemy };
			// Find closest ally in front
			let targets = allies.filter(a => a.x < enemy.x);
			targets.sort((a, b) => b.x - a.x);
			let target = targets[0];

			let isCloseToAlly = target && (enemy.x - target.x) <= enemy.range;
			let isCloseToCastle = (enemy.x - 12) <= enemy.range; // ally castle is at 12%

			if (isCloseToAlly) {
				updated.isAttacking = true;
				updated.attackTimer = (updated.attackTimer || 0) + 40;
				if (updated.attackTimer >= 800) {
					updated.attackTimer = 0;
					// Deal damage to target (id-safe find)
					const targetAlly = allies.find(a => a.id === target.id);
					if (targetAlly) {
						targetAlly.hp -= enemy.atk;
						targetAlly.hitEffect = true;
						setTimeout(() => {
							const found = allies.find(a => a.id === target.id);
							if (found) found.hitEffect = false;
						}, 200);
					}
				}
			} else if (isCloseToCastle) {
				updated.isAttacking = true;
				updated.attackTimer = (updated.attackTimer || 0) + 40;
				if (updated.attackTimer >= 800) {
					updated.attackTimer = 0;
					playerCastleHp = Math.max(0, playerCastleHp - enemy.atk);
					if (playerCastleHp <= 0) {
						gameStatus = 'defeat';
					}
				}
			} else {
				updated.isAttacking = false;
				updated.attackTimer = 0;
				updated.x -= enemy.speed;
			}
			return updated;
		});

		// C. Apply filtering for dead entities, and reward money for dead enemies
		allies = nextAllies.filter(a => a.hp > 0);
		enemies = nextEnemies.filter(e => {
			if (e.hp <= 0) {
				money = Math.min(money + e.value, walletMax);
				return false;
			}
			return true;
		});
	}

	onDestroy(() => {
		if (gameInterval) clearInterval(gameInterval);
	});
</script>

<svelte:head>
	<title>にゃんこ大戦争 - にゃんこ図鑑＆戦闘シミュレーター</title>
	<meta name="description" content="にゃんこ大戦争のおなじみのキャラクターたちを進化させたり、実際に敵と戦わせる戦闘シミュレーションを楽しめるアプリです。" />
</svelte:head>

<main class="min-h-screen flex flex-col justify-between p-4 md:p-8 relative overflow-hidden">
	<!-- Background grid and circles for retro game feel -->
	<div class="absolute inset-0 bg-[linear-gradient(rgba(255,255,255,0.02)_1px,transparent_1px),linear-gradient(90deg,rgba(255,255,255,0.02)_1px,transparent_1px)] bg-[size:32px_32px] pointer-events-none"></div>
	<div class="absolute -top-40 -right-40 w-96 h-96 bg-blue-500 rounded-full blur-3xl opacity-20 pointer-events-none"></div>
	<div class="absolute -bottom-40 -left-40 w-96 h-96 bg-purple-500 rounded-full blur-3xl opacity-20 pointer-events-none"></div>

	<!-- Mode Selector Tabs -->
	<div class="flex justify-center gap-4 mb-6 relative z-30">
		<button
			onclick={() => { currentMode = 'encyclopedia'; if (gameInterval) clearInterval(gameInterval); gameStatus = 'idle'; }}
			class="px-6 py-2.5 rounded-xl font-bold text-sm transition border flex items-center gap-2 cursor-pointer
				{currentMode === 'encyclopedia'
					? 'bg-blue-600 border-blue-500 text-white shadow-lg shadow-blue-500/20'
					: 'bg-white/5 border-white/10 text-slate-300 hover:bg-white/10 hover:text-white'}"
		>
			📕 にゃんこ図鑑
		</button>
		<button
			onclick={() => { currentMode = 'battle'; }}
			class="px-6 py-2.5 rounded-xl font-bold text-sm transition border flex items-center gap-2 cursor-pointer
				{currentMode === 'battle'
					? 'bg-rose-600 border-rose-500 text-white shadow-lg shadow-rose-500/20'
					: 'bg-white/5 border-white/10 text-slate-300 hover:bg-white/10 hover:text-white'}"
		>
			⚔️ 戦闘シミュレーター
		</button>
	</div>

	{#if currentMode === 'encyclopedia'}
		<!-- SECTION: ENCYCLOPEDIA -->
		<header class="relative text-center py-4">
			<h1 class="text-3xl md:text-5xl font-black tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-amber-300 to-yellow-500 drop-shadow-lg">
				にゃんこ大戦争
			</h1>
			<p class="text-xs md:text-sm font-semibold text-blue-300 mt-2 uppercase tracking-widest">
				── にゃんこキャラクター図鑑 ──
			</p>
		</header>

		<div class="relative w-full max-w-5xl mx-auto grid grid-cols-1 lg:grid-cols-12 gap-8 my-auto z-10">
			<!-- Left: Sidebar Cat List -->
			<div class="lg:col-span-4 flex flex-col gap-3">
				<div class="bg-slate-950/40 backdrop-blur-xl border border-white/10 rounded-2xl p-4 shadow-xl">
					<h2 class="text-xs font-bold text-slate-400 mb-3 px-1 uppercase tracking-wider">キャラクター選択</h2>
					<div class="grid grid-cols-2 lg:grid-cols-1 gap-2.5">
						{#each cats as cat, i}
							<button
								onclick={() => changeCat(i)}
								class="w-full flex items-center gap-3 p-3 rounded-xl border transition-all duration-300 text-left group cursor-pointer
									{selectedIndex === i
										? 'bg-blue-600/25 border-blue-500 text-white shadow-lg shadow-blue-500/10'
										: 'bg-white/5 border-white/5 hover:bg-white/10 hover:border-white/10 text-slate-300 hover:text-white'}"
							>
								<div class="w-10 h-10 rounded-lg bg-slate-900 border border-white/15 flex items-center justify-center overflow-hidden p-1 transition-transform group-hover:scale-105">
									{@html cat.svgPaths[catForms[i]]}
								</div>
								<div class="flex-1 min-w-0">
									<p class="text-xs text-slate-400 truncate font-mono">No.0{i+1}</p>
									<p class="text-sm font-bold truncate">{cat.names[catForms[i]]}</p>
								</div>
							</button>
						{/each}
					</div>
				</div>

				<!-- Helper Tip Box -->
				<div class="bg-white/5 border border-white/5 rounded-2xl p-4 text-xs text-slate-400 leading-relaxed">
					<p class="font-bold text-blue-300 mb-1">💡 にゃんこ育成システム</p>
					ここで進化させた形態は、**戦闘シミュレーター**モードでも適用されるにゃ！お気に入りの形態で出撃させるにゃ！
				</div>
			</div>

			<!-- Right: Showcase & Stats -->
			<div class="lg:col-span-8 flex flex-col gap-6">
				<!-- Main Showcase Box -->
				<div class="relative bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-6 md:p-8 shadow-2xl overflow-hidden flex flex-col md:flex-row gap-8 items-center">
					<div class="absolute top-0 left-0 right-0 h-[1.5px] bg-gradient-to-r from-transparent via-blue-400/40 to-transparent"></div>

					<!-- Character Art -->
					<div class="w-56 h-56 flex-shrink-0 bg-slate-950/60 rounded-2xl border border-white/10 flex items-center justify-center relative p-6 group">
						<div 
							class="absolute w-36 h-36 rounded-full blur-2xl opacity-40 transition-colors duration-500"
							style="background-color: {activeCat.color};"
						></div>

						<div class="w-40 h-40 z-10 transition-transform duration-300 group-hover:scale-105 {isEvolving ? 'evolving-effect' : ''}">
							{@html activeCat.svgPaths[catForms[selectedIndex]]}
						</div>

						<span class="absolute bottom-3 right-3 text-[10px] font-black uppercase tracking-wider px-2 py-1 rounded bg-slate-900 border border-white/15 text-slate-300">
							{#if catForms[selectedIndex] === 0}
								第1形態 (基本)
							{:else if catForms[selectedIndex] === 1}
								第2形態 (進化)
							{:else}
								第3形態 (超進化)
							{/if}
						</span>
					</div>

					<!-- Info & Action -->
					<div class="flex-1 space-y-5 text-center md:text-left w-full">
						<div>
							<div class="flex flex-col md:flex-row md:items-baseline gap-2">
								<h2 class="text-2xl md:text-3xl font-extrabold text-white tracking-tight">
									{activeCat.names[catForms[selectedIndex]]}
								</h2>
								<span class="text-xs text-slate-400 font-mono">
									({activeCat.englishNames[catForms[selectedIndex]]})
								</span>
							</div>
							<p class="text-xs text-slate-400 font-mono mt-1">No.0{selectedIndex+1} / Form.{catForms[selectedIndex]+1}</p>
						</div>

						<!-- Description -->
						<p class="text-slate-300 text-sm leading-relaxed h-16 md:h-12 overflow-y-auto">
							{activeCat.descriptions[catForms[selectedIndex]]}
						</p>

						<!-- Evolve Button -->
						<button
							onclick={evolveCat}
							disabled={isEvolving}
							class="w-full md:w-auto relative group overflow-hidden px-8 py-3.5 rounded-xl text-white font-bold text-sm transition-all duration-300 disabled:opacity-70 flex items-center justify-center gap-2 hover:scale-[1.02] active:scale-[0.98] cursor-pointer"
							style="background: linear-gradient(135deg, {activeCat.color} 0%, #1e1b4b 100%);"
						>
							<div class="absolute -inset-1 rounded-xl bg-white/20 blur opacity-0 group-hover:opacity-100 transition duration-500"></div>

							{#if isEvolving}
								<svg class="animate-spin -ml-1 mr-2 h-4 w-4 text-white" fill="none" viewBox="0 0 24 24">
									<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
									<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
								</svg>
								進化中...
							{:else}
								⚔️ 次の形態へ進化させる！
							{/if}
						</button>
					</div>
				</div>

				<!-- Stats Grid -->
				<div class="bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-6 shadow-xl">
					<h3 class="text-sm font-bold text-slate-300 mb-4 flex items-center gap-2">
						📊 ステータス <span class="text-xs font-normal text-slate-400">(Lv.30基準値)</span>
					</h3>
					<div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
						<div class="bg-slate-950/40 p-4 rounded-xl border border-white/5 text-center">
							<p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider mb-1">生産コスト</p>
							<p class="text-lg font-black text-amber-300">{activeCat.stats[catForms[selectedIndex]].cost}</p>
						</div>
						<div class="bg-slate-950/40 p-4 rounded-xl border border-white/5 text-center">
							<p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider mb-1">体力 (HP)</p>
							<p class="text-lg font-black text-emerald-400">{activeCat.stats[catForms[selectedIndex]].hp}</p>
						</div>
						<div class="bg-slate-950/40 p-4 rounded-xl border border-white/5 text-center">
							<p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider mb-1">攻撃力</p>
							<p class="text-lg font-black text-rose-400">{activeCat.stats[catForms[selectedIndex]].atk}</p>
						</div>
						<div class="bg-slate-950/40 p-4 rounded-xl border border-white/5 text-center">
							<p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider mb-1">射程 / 速度</p>
							<p class="text-sm font-black text-cyan-300">
								{activeCat.stats[catForms[selectedIndex]].range} <span class="text-xs text-slate-400">/</span> {activeCat.stats[catForms[selectedIndex]].speed}
							</p>
						</div>
					</div>
				</div>
			</div>
		</div>

	{:else}
		<!-- SECTION: BATTLE SIMULATOR -->
		<header class="relative text-center py-2">
			<h1 class="text-3xl md:text-5xl font-black tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-red-500 via-rose-400 to-amber-500 drop-shadow-lg">
				戦闘シミュレーター
			</h1>
			<p class="text-xs font-semibold text-rose-300 mt-1 uppercase tracking-widest">
				── 自軍の城を守り抜き、敵の城を攻め落とすにゃ！ ──
			</p>
		</header>

		<div class="relative w-full max-w-5xl mx-auto z-10 flex flex-col gap-6 my-auto">
			{#if gameStatus === 'idle'}
				<!-- Start Menu Screen -->
				<div class="bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-10 text-center shadow-2xl space-y-6 max-w-lg mx-auto">
					<h2 class="text-2xl font-bold text-white">戦闘開始の準備</h2>
					<p class="text-slate-300 text-sm leading-relaxed">
						敵の城を破壊すると**完全勝利**、味方の城が破壊されると**敗北**となります。
						お財布（働きネコ）をレベルアップさせながら、にゃんこたちを生産して進軍させましょう！
					</p>

					<div class="bg-slate-900/60 p-4 rounded-xl border border-white/5 text-left text-xs space-y-2">
						<p class="font-bold text-rose-300">⚔️ 出撃登録メンバー（現在の進化形態）</p>
						<div class="grid grid-cols-2 gap-2 mt-2">
							{#each cats as cat, i}
								<div class="flex items-center gap-2 bg-white/5 p-2 rounded-lg border border-white/5">
									<div class="w-6 h-6 rounded bg-slate-950 flex items-center justify-center p-0.5">
										{@html cat.svgPaths[catForms[i]]}
									</div>
									<span class="text-[11px] font-bold text-slate-300 truncate">{cat.names[catForms[i]]}</span>
								</div>
							{/each}
						</div>
					</div>

					<button
						onclick={startGame}
						class="w-full relative group overflow-hidden px-8 py-4 rounded-xl bg-gradient-to-r from-rose-500 to-red-600 text-white font-black text-lg transition duration-300 shadow-xl shadow-rose-950/40 hover:scale-102 cursor-pointer"
					>
						出撃するにゃ！
					</button>
				</div>

			{:else}
				<!-- Battlefield Area -->
				<div class="bg-slate-950/40 backdrop-blur-xl border border-white/10 rounded-3xl p-4 md:p-6 shadow-2xl relative">
					<!-- Castle Health Status Row -->
					<div class="flex justify-between items-center mb-4 px-2">
						<!-- Ally Castle HP -->
						<div class="w-2/5 max-w-[200px]">
							<div class="flex justify-between text-xs font-bold text-slate-300 mb-1">
								<span class="text-blue-400">味方城</span>
								<span>{playerCastleHp} / 1000</span>
							</div>
							<div class="w-full h-2.5 bg-slate-900 rounded-full overflow-hidden border border-white/5">
								<div class="h-full bg-blue-500 transition-all duration-150" style="width: {(playerCastleHp / 1000) * 100}%"></div>
							</div>
						</div>

						<span class="text-xs font-bold text-slate-500">VS</span>

						<!-- Enemy Castle HP -->
						<div class="w-2/5 max-w-[200px]">
							<div class="flex justify-between text-xs font-bold text-slate-300 mb-1">
								<span class="text-red-400">敵城</span>
								<span>{enemyCastleHp} / 1000</span>
							</div>
							<div class="w-full h-2.5 bg-slate-900 rounded-full overflow-hidden border border-white/5">
								<div class="h-full bg-red-500 transition-all duration-150" style="width: {(enemyCastleHp / 1000) * 100}%"></div>
							</div>
						</div>
					</div>

					<!-- The Actual Battlefield Strip -->
					<div class="w-full h-64 bg-gradient-to-b from-slate-950 via-slate-900 to-slate-800 rounded-2xl relative overflow-hidden border border-white/5 shadow-inner">
						<!-- Sky Stars -->
						<div class="absolute top-4 left-10 w-1 h-1 bg-white rounded-full opacity-60 animate-pulse"></div>
						<div class="absolute top-10 left-1/3 w-1.5 h-1.5 bg-white rounded-full opacity-40"></div>
						<div class="absolute top-6 right-1/4 w-1 h-1 bg-white rounded-full opacity-80 animate-pulse"></div>
						<div class="absolute top-16 right-10 w-1 h-1 bg-white rounded-full opacity-35"></div>

						<!-- Grass Ground Line -->
						<div class="absolute bottom-0 left-0 right-0 h-10 bg-gradient-to-t from-emerald-950 to-emerald-900 border-t border-emerald-800 flex items-center justify-center">
							<div class="w-full h-full bg-[linear-gradient(90deg,transparent_50%,rgba(0,0,0,0.15)_50%)] bg-[size:16px_100%] opacity-30"></div>
						</div>

						<!-- Ally Castle Visual -->
						<div class="absolute left-[5%] bottom-10 w-24 h-24 transform -translate-x-1/2 translate-y-2 select-none pointer-events-none">
							<svg viewBox="0 0 120 120" class="w-full h-full">
								<!-- Ears -->
								<polygon points="20,40 30,10 45,35" fill="#e2e8f0" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" />
								<polygon points="65,35 80,10 90,40" fill="#e2e8f0" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" />
								<!-- Castle Face -->
								<rect x="15" y="30" width="80" height="80" rx="15" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" />
								<circle cx="40" cy="55" r="4" fill="#1e293b" />
								<circle cx="70" cy="55" r="4" fill="#1e293b" />
								<path d="M 50 66 Q 55 70 60 66" fill="none" stroke="#1e293b" stroke-width="3.5" stroke-linecap="round" />
								<!-- Gate Door -->
								<rect x="42" y="80" width="26" height="30" rx="4" fill="#334155" stroke="#1e293b" stroke-width="3" />
							</svg>
						</div>

						<!-- Enemy Castle Visual -->
						<div class="absolute right-[5%] bottom-10 w-24 h-24 transform translate-x-1/2 translate-y-2 select-none pointer-events-none">
							<svg viewBox="0 0 120 120" class="w-full h-full">
								<rect x="25" y="25" width="70" height="85" rx="8" fill="#1e1b4b" stroke="#1e293b" stroke-width="4.5" />
								<!-- Spooky Devil horns -->
								<polygon points="20,30 5,5 30,25" fill="#be123c" stroke="#1e293b" stroke-width="4" />
								<polygon points="100,25 115,5 95,30" fill="#be123c" stroke="#1e293b" stroke-width="4" />
								<!-- Spooky red eyes -->
								<polygon points="40,45 50,52 35,55" fill="#f43f5e" />
								<polygon points="80,45 70,52 85,55" fill="#f43f5e" />
								<!-- Gate Door -->
								<rect x="47" y="75" width="26" height="35" rx="2" fill="#0f172a" stroke="#1e293b" stroke-width="3" />
							</svg>
						</div>

						<!-- Spawned Allies list rendering -->
						{#each allies as ally (ally.id)}
							<div
								class="absolute w-12 h-12 z-20 flex items-end justify-center transition-[left] duration-75 select-none pointer-events-none
									{ally.hitEffect ? 'hit-effect' : (ally.isAttacking ? 'attack-right' : 'walk-bob')}"
								style="left: {ally.x}%; bottom: 38px; transform-origin: bottom center;"
							>
								<!-- Health Bar Above Entity -->
								<div class="absolute -top-3 w-8 h-1 bg-slate-900 border border-white/10 rounded overflow-hidden">
									<div class="h-full bg-emerald-500" style="width: {(ally.hp / ally.maxHp) * 100}%"></div>
								</div>
								<!-- SVG Graphic -->
								<div class="w-10 h-10">
									{@html ally.svg}
								</div>
							</div>
						{/each}

						<!-- Spawned Enemies list rendering -->
						{#each enemies as enemy (enemy.id)}
							<div
								class="absolute w-12 h-12 z-20 flex items-end justify-center transition-[left] duration-75 select-none pointer-events-none
									{enemy.hitEffect ? 'hit-effect' : (enemy.isAttacking ? 'attack-left' : 'walk-bob')}"
								style="left: {enemy.x}%; bottom: 38px; transform-origin: bottom center;"
							>
								<!-- Health Bar Above Entity -->
								<div class="absolute -top-3 w-8 h-1 bg-slate-900 border border-white/10 rounded overflow-hidden">
									<div class="h-full bg-rose-500" style="width: {(enemy.hp / enemy.maxHp) * 100}%"></div>
								</div>
								<!-- SVG Graphic -->
								<div class="w-10 h-10">
									{@html enemy.svg}
								</div>
							</div>
						{/each}
					</div>

					<!-- Controls & Wallet Tray (Bottom of box) -->
					<div class="grid grid-cols-1 md:grid-cols-12 gap-4 mt-6">
						<!-- Wallet upgrade & money display (4 cols) -->
						<div class="md:col-span-4 bg-slate-900/60 rounded-2xl p-4 border border-white/5 flex flex-col justify-between gap-3">
							<div class="space-y-1">
								<span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider block">所持金</span>
								<div class="flex items-baseline justify-between">
									<span class="text-2xl font-black text-yellow-300 font-mono">{Math.floor(money)}円</span>
									<span class="text-xs text-slate-500 font-mono">/ {walletMax}円</span>
								</div>
								<div class="w-full h-2 bg-slate-950 rounded-full overflow-hidden border border-white/5 mt-1">
									<div class="h-full bg-yellow-400 transition-all duration-75" style="width: {(money / walletMax) * 100}%"></div>
								</div>
							</div>

							<button
								onclick={upgradeWallet}
								disabled={walletLevel >= 8 || money < (walletLevels[walletLevel - 1].upgradeCost || 0)}
								class="w-full py-2.5 rounded-xl border border-yellow-500/20 bg-yellow-500/10 hover:bg-yellow-500/20 disabled:bg-slate-900/40 disabled:border-white/5 disabled:opacity-40 text-yellow-400 disabled:text-slate-500 font-bold text-xs transition duration-200 cursor-pointer flex justify-center items-center gap-1"
							>
								{#if walletLevel >= 8}
									🐱 働きネコ Lv.MAX (最高)
								{:else}
									💸 働きネコ Lv.{walletLevel} ➔ Lv.{walletLevel+1}
									<span class="font-mono bg-yellow-500/20 px-1.5 py-0.5 rounded ml-1">
										-{walletLevels[walletLevel - 1].upgradeCost}円
									</span>
								{/if}
							</button>
						</div>

						<!-- Spawn Cards Tray (8 cols) -->
						<div class="md:col-span-8 bg-slate-900/40 rounded-2xl p-4 border border-white/5">
							<span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider block mb-3">にゃんこ軍団生産</span>
							<div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
								{#each cats as cat, i}
									{@const cost = parseInt(cat.stats[catForms[i]].cost)}
									{@const cdVal = cooldowns[cat.id]}
									{@const cooling = cdVal > 0}
									{@const canAfford = money >= cost}

									<button
										onclick={() => spawnAlly(i)}
										disabled={cooling || !canAfford}
										class="relative aspect-[4/5] rounded-xl border transition-all duration-300 flex flex-col items-center justify-between p-2.5 overflow-hidden cursor-pointer
											{cooling || !canAfford
												? 'bg-slate-950/60 border-white/5 opacity-55'
												: 'bg-white/5 border-white/10 hover:bg-white/10 hover:border-white/20 hover:scale-102 active:scale-98 shadow-md shadow-black/30'}"
									>
										<!-- Cooldown overlay slice -->
										{#if cooling}
											<div 
												class="absolute bottom-0 left-0 right-0 bg-slate-900/80 transition-all duration-75 flex items-center justify-center text-[10px] font-black text-slate-300 z-10"
												style="height: {(cdVal / cooldownMax[cat.id]) * 100}%; top: 0;"
											>
												{cdVal.toFixed(1)}s
											</div>
										{/if}

										<!-- Mini SVG representation -->
										<div class="w-11 h-11 bg-slate-900/50 rounded-lg p-1 border border-white/5">
											{@html cat.svgPaths[catForms[i]]}
										</div>

										<div class="text-center w-full mt-1.5 z-10">
											<p class="text-[10px] font-bold text-slate-200 truncate">{cat.names[catForms[i]]}</p>
											<p class="text-xs font-black text-yellow-400 font-mono mt-0.5">{cost}円</p>
										</div>
									</button>
								{/each}
							</div>
						</div>
					</div>

					<!-- Quit/Surrender button -->
					<div class="flex justify-end mt-4">
						<button
							onclick={() => { if (gameInterval) clearInterval(gameInterval); gameStatus = 'idle'; }}
							class="text-[10px] font-bold text-slate-500 hover:text-rose-400 transition cursor-pointer"
						>
							🏳️ 戦闘を降伏してメニューに戻る
						</button>
					</div>
				</div>
			{/if}
		</div>

		<!-- Victory & Defeat overlays -->
		{#if gameStatus === 'victory'}
			<div class="fixed inset-0 bg-slate-950/80 backdrop-blur-md flex items-center justify-center z-50 animate-fade-in">
				<div class="bg-slate-900 border border-yellow-500/20 rounded-3xl p-8 max-w-sm text-center shadow-2xl space-y-5 border-t-[4px] border-t-yellow-400">
					<h2 class="text-3xl font-black text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-amber-300 to-yellow-500 tracking-wider">
						🎉 完全勝利！
					</h2>
					<p class="text-slate-300 text-sm leading-relaxed">
						敵の城を完全に破壊したにゃ！にゃんこ軍団の勝利だにゃ！
					</p>
					<div class="flex gap-3 pt-2">
						<button
							onclick={startGame}
							class="flex-1 py-3 rounded-xl bg-gradient-to-r from-yellow-500 to-amber-600 hover:from-yellow-400 hover:to-amber-500 text-slate-950 font-black text-sm transition cursor-pointer"
						>
							もう一度出撃
						</button>
						<button
							onclick={() => { gameStatus = 'idle'; }}
							class="flex-1 py-3 rounded-xl bg-white/5 border border-white/15 hover:bg-white/10 text-white font-bold text-sm transition cursor-pointer"
						>
							閉じる
						</button>
					</div>
				</div>
			</div>
		{/if}

		{#if gameStatus === 'defeat'}
			<div class="fixed inset-0 bg-slate-950/80 backdrop-blur-md flex items-center justify-center z-50 animate-fade-in">
				<div class="bg-slate-900 border border-rose-500/20 rounded-3xl p-8 max-w-sm text-center shadow-2xl space-y-5 border-t-[4px] border-t-rose-500">
					<h2 class="text-3xl font-black text-transparent bg-clip-text bg-gradient-to-r from-rose-500 via-red-400 to-rose-600 tracking-wider">
						💀 敗北...
					</h2>
					<p class="text-slate-300 text-sm leading-relaxed">
						味方の城が破壊されてしまったにゃ... キャラクターを進化させて、再挑戦するにゃ！
					</p>
					<div class="flex gap-3 pt-2">
						<button
							onclick={startGame}
							class="flex-1 py-3 rounded-xl bg-gradient-to-r from-rose-500 to-red-600 text-white font-black text-sm transition cursor-pointer"
						>
							リトライ
						</button>
						<button
							onclick={() => { gameStatus = 'idle'; }}
							class="flex-1 py-3 rounded-xl bg-white/5 border border-white/15 hover:bg-white/10 text-white font-bold text-sm transition cursor-pointer"
						>
							閉じる
						</button>
					</div>
				</div>
			</div>
		{/if}
	{/if}

	<!-- Footer -->
	<footer class="text-center py-6 text-xs text-slate-500 font-mono mt-8">
		&copy; 2026 Battle Cats Fan Guide & Simulator - Powered by Svelte 5 & Tailwind CSS
	</footer>
</main>

<style>
	@keyframes evolve-glow {
		0% {
			transform: scale(1) rotate(0deg);
			filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.4));
		}
		50% {
			transform: scale(0.5) rotate(180deg);
			filter: drop-shadow(0 0 35px rgba(255, 255, 255, 0.9)) brightness(1.7);
		}
		100% {
			transform: scale(1) rotate(360deg);
			filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.4));
		}
	}

	@keyframes walk-bob {
		0%, 100% {
			transform: translate(-50%, -100%) scale(1) rotate(0deg);
		}
		50% {
			transform: translate(-50%, -100%) scale(1.02) translateY(-3px) rotate(2deg);
		}
	}

	@keyframes attack-lunge-right {
		0% {
			transform: translate(-50%, -100%) translateX(0) scale(1);
		}
		35% {
			transform: translate(-50%, -100%) translateX(12px) scale(1.08);
		}
		100% {
			transform: translate(-50%, -100%) translateX(0) scale(1);
		}
	}

	@keyframes attack-lunge-left {
		0% {
			transform: translate(-50%, -100%) translateX(0) scale(1);
		}
		35% {
			transform: translate(-50%, -100%) translateX(-12px) scale(1.08);
		}
		100% {
			transform: translate(-50%, -100%) translateX(0) scale(1);
		}
	}

	@keyframes hit-flash {
		0%, 100% {
			filter: brightness(1) drop-shadow(0 0 0px transparent);
		}
		50% {
			filter: brightness(1.2) sepia(1) saturate(8) hue-rotate(-50deg) drop-shadow(0 0 8px #ef4444);
		}
	}

	.evolving-effect {
		animation: evolve-glow 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	.walk-bob {
		animation: walk-bob 0.6s infinite ease-in-out;
	}

	.attack-right {
		animation: attack-lunge-right 0.4s ease-out;
	}

	.attack-left {
		animation: attack-lunge-left 0.4s ease-out;
	}

	.hit-effect {
		animation: hit-flash 0.2s ease-out;
	}
</style>
