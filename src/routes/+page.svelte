<script>
	import { onDestroy } from 'svelte';

	let currentMode = $state('encyclopedia'); // 'encyclopedia' or 'battle'
	let activeRarityTab = $state('basic'); // 'basic', 'rare', 'uber'

	// Cats Database (6 Basic, 10 Rare, 3 Uber Rare)
	const cats = [
		// --- BASIC CATS ---
		{
			id: 'cat',
			rarity: 'basic',
			isArea: false,
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
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="35,42 45,20 58,40" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="62,40 75,20 85,42" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="60" cy="70" r="35" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="48" cy="65" r="3" fill="#1e293b" /><circle cx="72" cy="65" r="3" fill="#1e293b" /><path d="M 54 73 Q 57 77 60 73 Q 63 77 66 73" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="42" cy="72" r="3" fill="#f43f5e" opacity="0.4" /><circle cx="78" cy="72" r="3" fill="#f43f5e" opacity="0.4" /><circle cx="48" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="35,42 45,20 58,40" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="62,40 75,20 85,42" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 25 70 C 5 70 10 50 22 55" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 95 70 C 115 70 110 50 98 55" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="60" cy="70" r="35" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="48" cy="65" r="3" fill="#1e293b" /><circle cx="72" cy="65" r="3" fill="#1e293b" /><path d="M 43 57 L 51 61" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><path d="M 77 57 L 69 61" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><path d="M 54 73 Q 57 77 60 73 Q 63 77 66 73" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="48" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 52 35 Q 60 2 68 35 Z" fill="#ef4444" stroke="#b91c1c" stroke-width="3" /><circle cx="60" cy="12" r="5" fill="#f59e0b" /><polygon points="35,42 45,23 58,40" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="62,40 75,23 85,42" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="60" cy="70" r="35" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="48" cy="65" r="3" fill="#1e293b" /><circle cx="72" cy="65" r="3" fill="#1e293b" /><path d="M 54 74 Q 60 78 66 74" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="48" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="8" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`
			]
		},
		{
			id: 'tank',
			rarity: 'basic',
			isArea: false,
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
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="43,28 48,12 55,27" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="65,27 72,12 77,28" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><rect x="40" y="25" width="40" height="78" rx="14" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="45" r="3" fill="#1e293b" /><circle cx="67" cy="45" r="3" fill="#1e293b" /><path d="M 57 52 Q 60 55 63 52" fill="none" stroke="#1e293b" stroke-width="3" stroke-linecap="round" /><circle cx="48" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="72" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="35" y="20" width="50" height="83" rx="4" fill="white" stroke="#1e293b" stroke-width="4" /><line x1="35" y1="45" x2="85" y2="45" stroke="#e2e8f0" stroke-width="2" /><line x1="35" y1="70" x2="85" y2="70" stroke="#e2e8f0" stroke-width="2" /><line x1="60" y1="20" x2="60" y2="45" stroke="#e2e8f0" stroke-width="2" /><line x1="48" y1="45" x2="48" y2="70" stroke="#e2e8f0" stroke-width="2" /><line x1="72" y1="45" x2="72" y2="70" stroke="#e2e8f0" stroke-width="2" /><circle cx="53" cy="32" r="3" fill="#1e293b" /><circle cx="67" cy="32" r="3" fill="#1e293b" /><path d="M 58 38 Q 60 40 62 38" fill="none" stroke="#1e293b" stroke-width="3" /><circle cx="45" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="35" y="20" width="50" height="83" rx="4" fill="white" stroke="#1e293b" stroke-width="4" /><path d="M 35 50 L 85 50 L 85 103 L 35 103 Z" fill="#2563eb" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><rect x="42" y="60" width="36" height="15" fill="#facc15" rx="2" /><text x="60" y="71" font-size="9" font-weight="bold" fill="#1e293b" text-anchor="middle">MONO</text><circle cx="53" cy="32" r="3" fill="#1e293b" /><circle cx="67" cy="32" r="3" fill="#1e293b" /><path d="M 57 38 Q 60 41 63 38" fill="none" stroke="#1e293b" stroke-width="3" /><circle cx="45" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="103" r="6" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`
			]
		},
		{
			id: 'gross',
			rarity: 'basic',
			isArea: true,
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
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="45,35 50,23 57,34" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><polygon points="63,34 70,23 75,35" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="42" r="2.5" fill="#1e293b" /><circle cx="67" cy="42" r="2.5" fill="#1e293b" /><path d="M 56 49 Q 60 52 64 49" fill="none" stroke="#1e293b" stroke-width="3.5" /><path d="M 50 62 L 45 105 L 52 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 70 62 L 75 105 L 68 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="45,35 50,23 57,34" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><polygon points="63,34 70,23 75,35" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="42" r="2.5" fill="#1e293b" /><circle cx="67" cy="42" r="2.5" fill="#1e293b" /><path d="M 56 49 Q 60 52 64 49" fill="none" stroke="#1e293b" stroke-width="3.5" /><path d="M 50 62 Q 42 85 45 105 L 52 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 70 62 Q 78 85 75 105 L 68 105" fill="none" stroke="#1e293b" stroke-width="4.5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 45 105 L 41 109 L 46 109 Z" fill="#ec4899" /><path d="M 75 105 L 79 109 L 74 109 Z" fill="#ec4899" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="45,35 50,23 57,34" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><polygon points="63,34 70,23 75,35" fill="white" stroke="#1e293b" stroke-width="3" stroke-linejoin="round" /><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="53" cy="42" r="2.5" fill="#1e293b" /><circle cx="67" cy="42" r="2.5" fill="#1e293b" /><path d="M 56 49 Q 60 52 64 49" fill="none" stroke="#1e293b" stroke-width="3.5" /><path d="M 50 62 Q 38 75 42 88 T 45 105 L 52 105" fill="none" stroke="#1e293b" stroke-width="5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 70 62 Q 82 75 78 88 T 75 105 L 68 105" fill="none" stroke="#1e293b" stroke-width="5" stroke-linecap="round" stroke-linejoin="round" /><path d="M 45 74 Q 40 80 43 85" fill="none" stroke="#1e293b" stroke-width="2" /><path d="M 75 74 Q 80 80 77 85" fill="none" stroke="#1e293b" stroke-width="2" /></svg>`
			]
		},
		{
			id: 'cow',
			rarity: 'basic',
			isArea: false,
			names: ['ウシネコ', 'ネコキリン', 'ネコライオン'],
			englishNames: ['Cow Cat', 'Giraffe Cat', 'Lion Cat'],
			descriptions: [
				'牛の頭部を持つ高速移動アタッカー。圧倒的なスピードで前線に駆け上がり噛みつくにゃ！',
				'首が極限まで伸びたキリンネコ。攻撃頻度が大幅に上がり、敵を高速で突き続けるにゃ！',
				'百獣の王のたてがみを手に入れた超高速ライオンネコ。さらに機動力が増し、一気に敵を圧倒するにゃ！'
			],
			stats: [
				{ cost: '750円', hp: '500', atk: '35', range: '110', speed: '16' },
				{ cost: '750円', hp: '750', atk: '55', range: '110', speed: '20' },
				{ cost: '750円', hp: '1200', atk: '90', range: '110', speed: '24' }
			],
			color: '#d97706',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="30,30 25,12 36,25" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><polygon points="70,25 81,12 76,30" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="53" cy="50" r="28" fill="white" stroke="#1e293b" stroke-width="4" /><path d="M 28 35 Q 22 28 25 35" stroke="#1e293b" stroke-width="3" fill="none"/><circle cx="45" cy="42" r="3" fill="#1e293b"/><circle cx="61" cy="42" r="3" fill="#1e293b"/><path d="M 49 48 Q 53 50 57 48" fill="none" stroke="#1e293b" stroke-width="3"/><circle cx="38" cy="52" r="4.5" fill="#1e293b"/><circle cx="62" cy="62" r="5" fill="#1e293b"/><circle cx="35" cy="100" r="5" fill="white" stroke="#1e293b" stroke-width="3"/><circle cx="71" cy="100" r="5" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 48 95 L 48 30 L 64 30 L 64 95" fill="white" stroke="#1e293b" stroke-width="4" /><circle cx="56" cy="30" r="16" fill="white" stroke="#1e293b" stroke-width="4"/><circle cx="50" cy="27" r="2" fill="#1e293b"/><circle cx="62" cy="27" r="2" fill="#1e293b"/><path d="M 53 33 Q 56 35 59 33" fill="none" stroke="#1e293b" stroke-width="2"/><circle cx="52" cy="52" r="3.5" fill="#1e293b"/><circle cx="60" cy="72" r="4" fill="#1e293b"/><circle cx="42" cy="100" r="5" fill="white" stroke="#1e293b" stroke-width="3"/><circle cx="70" cy="100" r="5" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="56" cy="60" r="30" fill="#f59e0b" stroke="#d97706" stroke-width="3.5"/><circle cx="56" cy="60" r="20" fill="white" stroke="#1e293b" stroke-width="4"/><circle cx="50" cy="57" r="2.5" fill="#1e293b"/><circle cx="62" cy="57" r="2.5" fill="#1e293b"/><path d="M 53 63 Q 56 65 59 63" fill="none" stroke="#1e293b" stroke-width="2.5"/><path d="M 30 75 Q 20 85 30 95" fill="none" stroke="#f59e0b" stroke-width="5" stroke-linecap="round"/><circle cx="42" cy="100" r="5" fill="white" stroke="#1e293b" stroke-width="3"/><circle cx="70" cy="100" r="5" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`
			]
		},
		{
			id: 'bird',
			rarity: 'basic',
			isArea: true,
			names: ['ネコノトリ', 'ネコＵＦＯ', '天空のネコ'],
			englishNames: ['Bird Cat', 'UFO Cat', 'The Flying Cat'],
			descriptions: [
				'空中から広範囲に大打撃を与える鳥型キャラクター。攻撃力は高いが、単体での体力は低めだにゃ。',
				'謎の空飛ぶ円盤に進化した姿。範囲攻撃で複数の敵をまとめて一撃のもとに葬り去るにゃ！',
				'空中要塞となって爆撃攻撃を行う天空ネコ。凄まじい破壊力で範囲内の全ての敵を焦土と化すにゃ！'
			],
			stats: [
				{ cost: '975円', hp: '800', atk: '380', range: '170', speed: '7' },
				{ cost: '975円', hp: '1200', atk: '540', range: '180', speed: '7' },
				{ cost: '975円', hp: '1900', atk: '920', range: '200', speed: '8' }
			],
			color: '#06b6d4',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="56" cy="60" r="26" fill="white" stroke="#1e293b" stroke-width="4"/><polygon points="62,56 82,60 62,64" fill="#f59e0b" stroke="#d97706" stroke-width="2.5" /><circle cx="48" cy="52" r="2.5" fill="#1e293b"/><path d="M 33 60 Q 22 50 33 46 Z" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><ellipse cx="56" cy="75" rx="42" ry="16" fill="#475569" stroke="#1e293b" stroke-width="4"/><ellipse cx="56" cy="69" rx="20" ry="20" fill="#38bdf8" opacity="0.6" stroke="#0284c7" stroke-width="2.5"/><circle cx="56" cy="64" r="12" fill="white" stroke="#1e293b" stroke-width="3"/><circle cx="51" cy="60" r="1.5" fill="#1e293b"/><circle cx="61" cy="60" r="1.5" fill="#1e293b"/><path d="M 53 65 Q 56 67 59 65" fill="none" stroke="#1e293b" stroke-width="2"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="28" y="42" width="56" height="34" rx="8" fill="#334155" stroke="#1e293b" stroke-width="4"/><path d="M 28 58 L 10 48 L 28 48 Z" fill="#475569" stroke="#1e293b" stroke-width="3"/><path d="M 84 58 L 102 48 L 84 48 Z" fill="#475569" stroke="#1e293b" stroke-width="3"/><circle cx="56" cy="58" r="10" fill="white" stroke="#1e293b" stroke-width="3"/><line x1="56" y1="42" x2="56" y2="28" stroke="#be123c" stroke-width="3.5" /></svg>`
			]
		},
		{
			id: 'titan',
			rarity: 'basic',
			isArea: false,
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
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="35" y="45" width="50" height="52" rx="10" fill="white" stroke="#1e293b" stroke-width="4.5" /><path d="M 48 58 L 54 62" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 72 58 L 66 62" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 53 72 Q 60 76 67 72" fill="none" stroke="#1e293b" stroke-width="4" /><path d="M 35 55 C 15 55 10 90 32 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 85 55 C 105 55 110 90 88 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="45" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 45 46 Q 30 20 40 35 Q 50 15 55 46" fill="#1e293b" /><path d="M 75 46 Q 90 20 80 35 Q 70 15 65 46" fill="#1e293b" /><rect x="35" y="45" width="50" height="52" rx="10" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" /><path d="M 46 58 L 54 60" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 74 58 L 66 60" stroke="#1e293b" stroke-width="4" stroke-linecap="round" /><path d="M 50 72 L 70 72" stroke="#1e293b" stroke-width="3.5" stroke-linecap="round" /><polygon points="32,50 25,43 36,45" fill="#1e293b" /><polygon points="88,50 95,43 84,45" fill="#1e293b" /><path d="M 35 55 C 12 55 8 92 32 88" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" /><path d="M 85 55 C 108 55 112 92 88 88" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" /><circle cx="45" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 38 48 C 30 25 90 25 82 48 Z" fill="#1e293b" /><rect x="35" y="45" width="50" height="52" rx="10" fill="white" stroke="#1e293b" stroke-width="4.5" /><path d="M 45 45 Q 60 60 75 45" fill="none" stroke="#ef4444" stroke-width="3" /><text x="60" y="75" font-size="10" font-weight="bold" fill="#ef4444" text-anchor="middle">NYAN</text><rect x="42" y="52" width="16" height="10" rx="2" fill="#1e293b" /><rect x="62" y="52" width="16" height="10" rx="2" fill="#1e293b" /><line x1="58" y1="56" x2="62" y2="56" stroke="#1e293b" stroke-width="3" /><path d="M 35 55 C 15 55 10 90 32 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><path d="M 85 55 C 105 55 110 90 88 88" fill="white" stroke="#1e293b" stroke-width="4" stroke-linejoin="round" /><circle cx="45" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /><circle cx="75" cy="102" r="7" fill="white" stroke="#1e293b" stroke-width="3" /></svg>`
			]
		},

		// --- RARE CATS ---
		{
			id: 'hop',
			rarity: 'rare',
			isArea: false,
			survive: true,
			names: ['ネコホッピング', 'ネコマサイ', 'ネコキョンシー'],
			englishNames: ['Cat Hop', 'Masai Cat', 'Jiangshi Cat'],
			descriptions: [
				'ホッピングでぴょんぴょん跳ねるネコ。たまに致命傷をうけても1回だけ生き残るにゃ。',
				'マサイ族の跳躍力を手に入れたネコ。生き残る能力で戦線の崩壊を粘り強く防ぐにゃ。',
				'キョンシーの姿となり呪符を貼った姿。何度倒されそうになっても絶対に1回は踏みとどまるにゃ！'
			],
			stats: [
				{ cost: '240円', hp: '400', atk: '30', range: '140', speed: '6' },
				{ cost: '240', hp: '600', atk: '45', range: '140', speed: '6' },
				{ cost: '240円', hp: '900', atk: '70', range: '140', speed: '7' }
			],
			color: '#f43f5e',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="45" r="22" fill="white" stroke="#1e293b" stroke-width="4"/><path d="M 50 67 L 70 67 M 60 67 L 60 92 M 50 92 L 70 92" stroke="#1e293b" stroke-width="4"/><path d="M 55 75 Q 60 80 65 75" stroke="#1e293b" stroke-width="3" fill="none"/><circle cx="53" cy="42" r="2" fill="#1e293b"/><circle cx="67" cy="42" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 40 25 L 60 5 L 80 25 Z" fill="#b91c1c" stroke="#1e293b" stroke-width="3"/><circle cx="60" cy="50" r="22" fill="white" stroke="#1e293b" stroke-width="4"/><path d="M 50 72 L 70 72 M 60 72 L 60 95" stroke="#1e293b" stroke-width="4"/><circle cx="53" cy="47" r="2" fill="#1e293b"/><circle cx="67" cy="47" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="54" y="5" width="12" height="25" fill="#eab308" stroke="#ca8a04" stroke-width="2"/><text x="60" y="22" font-size="9" fill="red" font-weight="bold" text-anchor="middle">勑</text><circle cx="60" cy="52" r="22" fill="#e0f2fe" stroke="#1e293b" stroke-width="4"/><path d="M 50 74 L 70 74 M 60 74 L 60 96" stroke="#1e293b" stroke-width="4"/><circle cx="53" cy="49" r="2" fill="#1e293b"/><circle cx="67" cy="49" r="2" fill="#1e293b"/></svg>`
			]
		},
		{
			id: 'thief',
			rarity: 'rare',
			isArea: false,
			doubleMoney: true,
			names: ['ねこ泥棒', 'ねこ怪盗', 'ネゴエモン'],
			englishNames: ['Thief Cat', 'Phantom Cat', 'Gemon Cat'],
			descriptions: [
				'盗みの美学を持つネコ。敵を倒したときにお金が多くもらえる（2倍）特殊能力を持つにゃ。',
				'華麗に財宝を奪う怪盗ネコ。スタイリッシュな仮面をつけ、相変わらず敵撃破時にお財布を潤すにゃ。',
				'伝説の義賊の生まれ変わり。敵の懐に入り込み、倍額のお小遣いをがっぽり奪い取るにゃ！'
			],
			stats: [
				{ cost: '330円', hp: '500', atk: '60', range: '140', speed: '10' },
				{ cost: '330円', hp: '750', atk: '90', range: '140', speed: '10' },
				{ cost: '330円', hp: '1100', atk: '150', range: '140', speed: '12' }
			],
			color: '#6366f1',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 40 45 L 80 45 L 75 75 L 45 75 Z" fill="#22c55e" stroke="#16a34a" stroke-width="3"/><circle cx="60" cy="52" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="55" cy="49" r="2" fill="#1e293b"/><circle cx="65" cy="49" r="2" fill="#1e293b"/><circle cx="45" cy="100" r="4" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 42 42 L 78 42 M 42 42 L 52 50 L 68 50 L 78 42" fill="#1e293b"/><circle cx="53" cy="45" r="2" fill="white"/><circle cx="67" cy="45" r="2" fill="white"/><circle cx="45" cy="100" r="4" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 30 15 Q 60 5 90 15 L 75 45 L 45 45 Z" fill="#b91c1c" stroke="#1e293b" stroke-width="3"/><circle cx="60" cy="55" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="53" cy="52" r="2" fill="#1e293b"/><circle cx="67" cy="52" r="2" fill="#1e293b"/><circle cx="45" cy="102" r="4" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`
			]
		},
		{
			id: 'salon',
			rarity: 'rare',
			isArea: true,
			names: ['ネコエステ', 'ネコジェンヌ', 'ネコパーフェクト'],
			englishNames: ['Salon Cat', 'Paris Cat', 'Cyborg Cat'],
			descriptions: [
				'美しさを追求するセレブなエステネコ。中距離から非常に素早い範囲攻撃を繰り出す超名作レアキャラだにゃ。',
				'パリ帰りのトップモデル。さらに美しく磨きがかかった連続のビンタ攻撃で敵の群れを一掃するにゃ。',
				'全身をサイボーグ化し、パーフェクトな美を手に入れたネコ。凄まじい範囲火力を叩き出すにゃ！'
			],
			stats: [
				{ cost: '350円', hp: '450', atk: '160', range: '350', speed: '7' },
				{ cost: '350円', hp: '700', atk: '250', range: '350', speed: '7' },
				{ cost: '350円', hp: '1000', atk: '420', range: '350', speed: '8' }
			],
			color: '#ec4899',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="40" r="16" fill="white" stroke="#1e293b" stroke-width="4"/><path d="M 45 42 Q 35 25 50 35" fill="none" stroke="#1e293b" stroke-width="3"/><path d="M 50 56 Q 42 85 46 105 L 53 105" fill="none" stroke="#1e293b" stroke-width="4"/><path d="M 70 56 Q 78 85 74 105 L 67 105" fill="none" stroke="#1e293b" stroke-width="4"/><circle cx="53" cy="38" r="2" fill="#1e293b"/><circle cx="67" cy="38" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 35 30 Q 60 5 85 30 Z" fill="#ec4899" /><circle cx="60" cy="45" r="16" fill="white" stroke="#1e293b" stroke-width="4"/><path d="M 50 61 Q 42 85 46 105 L 53 105" fill="none" stroke="#1e293b" stroke-width="4"/><path d="M 70 61 Q 78 85 74 105 L 67 105" fill="none" stroke="#1e293b" stroke-width="4"/><circle cx="53" cy="43" r="2" fill="#1e293b"/><circle cx="67" cy="43" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="40" r="16" fill="#cbd5e1" stroke="#475569" stroke-width="4"/><path d="M 45 42 Q 35 25 50 35" fill="none" stroke="#1e293b" stroke-width="3"/><path d="M 50 56 Q 40 85 44 105 L 51 105" fill="none" stroke="#475569" stroke-width="4"/><path d="M 70 56 Q 80 85 76 105 L 69 105" fill="none" stroke="#475569" stroke-width="4"/><circle cx="53" cy="38" r="2" fill="#be123c"/><circle cx="67" cy="38" r="2" fill="#be123c"/><line x1="60" y1="40" x2="60" y2="56" stroke="#475569" stroke-width="3"/></svg>`
			]
		},
		{
			id: 'jura',
			rarity: 'rare',
			isArea: false,
			critChance: 0.2,
			names: ['ねこジュラ', 'ねこジュラザウルス', 'ねこジュラシッター'],
			englishNames: ['Jurassic Cat', 'Jurassic Rex', 'Jurassic Sitter'],
			descriptions: [
				'恐竜にあこがれて骨のヘルメットをかぶったネコ。低確率で会心の一撃（クリティカル）を放つにゃ。',
				'さらに恐竜になりきった姿。攻撃速度が早く、クリティカル発生時の3倍ダメージでメタルな敵を砕くにゃ！',
				'恐竜の子供をお世話するベビーシッター。愛情のこもった一撃で、会心の一撃の確率が上がったにゃ！'
			],
			stats: [
				{ cost: '525円', hp: '800', atk: '80', range: '140', speed: '8' },
				{ cost: '525円', hp: '1200', atk: '120', range: '140', speed: '8' },
				{ cost: '525円', hp: '1800', atk: '200', range: '140', speed: '9' }
			],
			color: '#15803d',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 40 40 Q 60 10 80 40 Z" fill="#22c55e" stroke="#16a34a" stroke-width="3"/><circle cx="60" cy="65" r="20" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 78 75 C 90 75 90 90 82 92" fill="none" stroke="#1e293b" stroke-width="3"/><circle cx="53" cy="60" r="2.5" fill="#1e293b"/><circle cx="67" cy="60" r="2.5" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 30 35 L 90 35 L 60 5 Z" fill="#16a34a" stroke="#1e293b" stroke-width="3"/><circle cx="60" cy="65" r="20" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 80 75 Q 100 65 95 90" fill="none" stroke="#16a34a" stroke-width="4.5"/><circle cx="53" cy="60" r="2.5" fill="#1e293b"/><circle cx="67" cy="60" r="2.5" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="55" r="20" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="53" cy="50" r="2.5" fill="#1e293b"/><circle cx="67" cy="50" r="2.5" fill="#1e293b"/><rect x="42" y="80" width="36" height="22" rx="4" fill="#a7f3d0" stroke="#059669" stroke-width="2"/></svg>`
			]
		},
		{
			id: 'wheel',
			rarity: 'rare',
			isArea: true,
			names: ['ネコ車輪', 'ネコ太陽', 'ネコクール'],
			englishNames: ['Wheel Cat', 'Solar Cat', 'Cool Cat'],
			descriptions: [
				'一輪車にのってバランスをとるネコ。複数の敵をまとめて焼き払う範囲攻撃アタッカーにゃ。',
				'太陽神の力を宿した一輪車ネコ。熱風をともなう広範囲の攻撃力で敵軍をじわじわ焦がすにゃ。',
				'クールな未来都市の一輪車に乗るネコ。最先端のエネルギー波により範囲攻撃の威力が増したにゃ！'
			],
			stats: [
				{ cost: '600円', hp: '600', atk: '190', range: '180', speed: '6' },
				{ cost: '600円', hp: '900', atk: '300', range: '180', speed: '6' },
				{ cost: '600円', hp: '1400', atk: '500', range: '180', speed: '7' }
			],
			color: '#f97316',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="90" r="16" fill="none" stroke="#1e293b" stroke-width="4"/><circle cx="60" cy="45" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><line x1="60" y1="61" x2="60" y2="74" stroke="#1e293b" stroke-width="4"/><circle cx="54" cy="42" r="2" fill="#1e293b"/><circle cx="66" cy="42" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="90" r="16" fill="none" stroke="#1e293b" stroke-width="4"/><circle cx="60" cy="45" r="16" fill="#f97316" stroke="#1e293b" stroke-width="3.5"/><circle cx="60" cy="45" r="12" fill="#eab308"/><circle cx="55" cy="42" r="1.5" fill="#1e293b"/><circle cx="65" cy="42" r="1.5" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="90" r="16" fill="none" stroke="#06b6d4" stroke-width="5"/><circle cx="60" cy="45" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><rect x="48" y="38" width="24" height="6" fill="#06b6d4"/><circle cx="45" cy="100" r="4" fill="white" stroke="#1e293b" stroke-width="3"/></svg>`
			]
		},
		{
			id: 'witch',
			rarity: 'rare',
			isArea: false,
			slowChance: 0.5,
			names: ['ネコ魔女', 'ネコ大魔女', 'ネコ法師'],
			englishNames: ['Witch Cat', 'Great Witch Cat', 'Sorcerer Cat'],
			descriptions: [
				'魔法学校に通う見習いネコ。たまに魔法により敵の移動速度を半減させる（遅くする）にゃ。',
				'魔法の研究を極めた大魔女ネコ。呪文を唱えて敵をより高確率でスロー状態にするにゃ。',
				'仙人の教えを得て悟りを開いた法師ネコ。徳の高い法力で敵のスピードを確実に奪うにゃ！'
			],
			stats: [
				{ cost: '315円', hp: '550', atk: '40', range: '220', speed: '6' },
				{ cost: '315円', hp: '800', atk: '60', range: '220', speed: '6' },
				{ cost: '315円', hp: '1200', atk: '95', range: '220', speed: '6' }
			],
			color: '#a855f7',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="35,30 60,5 85,30" fill="#7c3aed" stroke="#1e293b" stroke-width="3"/><circle cx="60" cy="55" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><line x1="30" y1="80" x2="90" y2="80" stroke="#d97706" stroke-width="3.5"/><circle cx="53" cy="50" r="2" fill="#1e293b"/><circle cx="67" cy="50" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="30,30 60,0 90,30" fill="#5b21b6" stroke="#1e293b" stroke-width="3"/><circle cx="60" cy="55" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 25 80 L 95 80 M 35 75 L 35 85" stroke="#475569" stroke-width="4"/><circle cx="53" cy="50" r="2" fill="#1e293b"/><circle cx="67" cy="50" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="45" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 40 85 Q 60 70 80 85" fill="none" stroke="#d97706" stroke-width="3.5"/><circle cx="53" cy="40" r="2" fill="#1e293b"/><circle cx="67" cy="40" r="2" fill="#1e293b"/><rect x="56" y="5" width="8" height="28" fill="#facc15" stroke="#ca8a04"/></svg>`
			]
		},
		{
			id: 'archer',
			rarity: 'rare',
			isArea: false,
			names: ['ネコアーチャー', 'ネコキューピット', 'ネコいて座'],
			englishNames: ['Archer Cat', 'Cupid Cat', 'Sagittarius Cat'],
			descriptions: [
				'遠くの獲物を狙うネコ。弓矢を用いて超長距離からチクチク一方的にダメージを稼ぐにゃ。',
				'愛の矢を放つキューピット。遠くの敵に向けて容赦のないダメージを突き刺すにゃ。',
				'星座の力を手に入れた宇宙戦士。光速の矢を放ち、射程距離から安全に敵を貫くにゃ！'
			],
			stats: [
				{ cost: '390円', hp: '400', atk: '120', range: '390', speed: '7' },
				{ cost: '390円', hp: '600', atk: '180', range: '400', speed: '7' },
				{ cost: '390円', hp: '900', atk: '300', range: '420', speed: '7' }
			],
			color: '#84cc16',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="55" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 80 40 A 15 15 0 0 1 80 70" fill="none" stroke="#d97706" stroke-width="3.5"/><circle cx="53" cy="50" r="2" fill="#1e293b"/><circle cx="67" cy="50" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="55" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 82 40 Q 95 55 82 70" fill="none" stroke="#ef4444" stroke-width="3.5"/><path d="M 75 55 L 90 55" stroke="#f43f5e" stroke-width="2"/><circle cx="53" cy="50" r="2" fill="#1e293b"/><circle cx="67" cy="50" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="56" cy="55" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><path d="M 75 35 L 95 35 L 95 55" fill="none" stroke="#eab308" stroke-width="4"/><circle cx="49" cy="50" r="2" fill="#1e293b"/><circle cx="63" cy="50" r="2" fill="#1e293b"/></svg>`
			]
		},
		{
			id: 'rover',
			rarity: 'rare',
			isArea: false,
			names: ['ネコ探査機', 'ネコサーチライト', 'ネコサテライト'],
			englishNames: ['Rover Cat', 'Searchlight Cat', 'Satellite Cat'],
			descriptions: [
				'未知なる星を探索する宇宙用ロボ。高い体力を持ち、敵からの激しい攻撃を受ける防壁として動くにゃ。',
				'強力な光源を装備したサーチライトネコ。夜間戦闘でも敵の弱点を見出し、がっしり防衛するにゃ。',
				'人工衛星となり軌道上から作戦を支援するネコ。耐久力が最高レベルに達し、超タフな壁になるにゃ！'
			],
			stats: [
				{ cost: '270円', hp: '1100', atk: '50', range: '140', speed: '9' },
				{ cost: '270円', hp: '1700', atk: '75', range: '140', speed: '9' },
				{ cost: '270円', hp: '2600', atk: '120', range: '140', speed: '10' }
			],
			color: '#06b6d4',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="42" y="32" width="36" height="42" rx="6" fill="#e2e8f0" stroke="#1e293b" stroke-width="4"/><circle cx="60" cy="52" r="10" fill="white" stroke="#1e293b" stroke-width="2.5"/><circle cx="56" cy="50" r="1.5" fill="#1e293b"/><circle cx="64" cy="50" r="1.5" fill="#1e293b"/><rect x="35" y="74" width="50" height="8" rx="2" fill="#475569"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="42" y="32" width="36" height="42" rx="6" fill="#cbd5e1" stroke="#1e293b" stroke-width="4"/><circle cx="60" cy="48" r="8" fill="#eab308" stroke="#1e293b" stroke-width="2"/><line x1="60" y1="20" x2="60" y2="32" stroke="#be123c" stroke-width="3.5"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="50" r="22" fill="white" stroke="#06b6d4" stroke-width="4.5"/><path d="M 30 50 L 15 50 M 90 50 L 105 50" stroke="#06b6d4" stroke-width="4"/><circle cx="53" cy="46" r="2" fill="#1e293b"/><circle cx="67" cy="46" r="2" fill="#1e293b"/></svg>`
			]
		},
		{
			id: 'stilts',
			rarity: 'rare',
			isArea: false,
			names: ['たけうまネコ', 'ネコ仕事人', 'ネコおろしがね'],
			englishNames: ['Stilts Cat', 'Worker Cat', 'Grater Cat'],
			descriptions: [
				'竹馬に乗って高所から攻撃するおちゃめなネコ。高い位置から頭上目掛けてキックを繰り出すにゃ。',
				'高いところの窓拭き作業などを行う高所仕事人ネコ。仕事で鍛えたバランス感覚で敵を蹴飛ばすにゃ。',
				'巨大なおろしがねを武器にしたネコ。大根と共におろし攻撃を仕掛け、敵の戦線をぐいぐい押し戻すにゃ！'
			],
			stats: [
				{ cost: '360円', hp: '700', atk: '70', range: '180', speed: '6' },
				{ cost: '360円', hp: '1050', atk: '105', range: '180', speed: '6' },
				{ cost: '360円', hp: '1600', atk: '170', range: '180', speed: '6' }
			],
			color: '#84cc16',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><line x1="48" y1="20" x2="48" y2="105" stroke="#d97706" stroke-width="4"/><line x1="72" y1="20" x2="72" y2="105" stroke="#d97706" stroke-width="4"/><circle cx="60" cy="40" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="54" cy="37" r="2" fill="#1e293b"/><circle cx="66" cy="37" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><line x1="48" y1="20" x2="48" y2="105" stroke="#475569" stroke-width="4"/><line x1="72" y1="20" x2="72" y2="105" stroke="#475569" stroke-width="4"/><circle cx="60" cy="40" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><rect x="52" y="20" width="16" height="10" fill="#facc15"/><circle cx="54" cy="37" r="2" fill="#1e293b"/><circle cx="66" cy="37" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="40" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><rect x="42" y="60" width="36" height="42" fill="#94a3b8" stroke="#475569" stroke-width="2.5"/><circle cx="54" cy="37" r="2" fill="#1e293b"/><circle cx="66" cy="37" r="2" fill="#1e293b"/></svg>`
			]
		},
		{
			id: 'gunman',
			rarity: 'rare',
			isArea: false,
			slowChance: 0.5,
			names: ['ねこガンマン', 'ねこウエスタン', 'ねこロデオ'],
			englishNames: ['Gunslinger Cat', 'Western Cat', 'Rodeo Cat'],
			descriptions: [
				'西部の荒野を旅する早撃ちガンマン。銃弾により遠くから敵の動きをたまに遅くするにゃ。',
				'二丁拳銃を華麗に操る保安官。銃撃ラッシュでより高確率で敵の足を縛り動きを止めるにゃ。',
				'暴れ牛のロボットにまたがるロデオネコ。激しい暴れ馬突進攻撃で、敵の進軍スピードを奪い去るにゃ！'
			],
			stats: [
				{ cost: '375円', hp: '600', atk: '80', range: '240', speed: '7' },
				{ cost: '375円', hp: '900', atk: '120', range: '240', speed: '7' },
				{ cost: '375円', hp: '1350', atk: '190', range: '240', speed: '7' }
			],
			color: '#78350f',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 35 45 L 85 45 L 75 25 L 45 25 Z" fill="#78350f" stroke="#451a03" stroke-width="3"/><circle cx="60" cy="60" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="53" cy="55" r="2" fill="#1e293b"/><circle cx="67" cy="55" r="2" fill="#1e293b"/><rect x="80" y="60" width="12" height="6" fill="#475569"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 35 42 L 85 42 L 75 22 L 45 22 Z" fill="#78350f" stroke="#451a03" stroke-width="3"/><circle cx="60" cy="58" r="18" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="53" cy="54" r="2" fill="#1e293b"/><circle cx="67" cy="54" r="2" fill="#1e293b"/><rect x="80" y="55" width="12" height="6" fill="#eab308"/><rect x="28" y="55" width="12" height="6" fill="#eab308"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="40" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><circle cx="54" cy="37" r="2" fill="#1e293b"/><circle cx="66" cy="37" r="2" fill="#1e293b"/><rect x="35" y="65" width="50" height="35" rx="8" fill="#451a03" stroke="#1e293b" stroke-width="3"/></svg>`
			]
		},

		// --- UBER RARE CATS ---
		{
			id: 'jizo',
			rarity: 'uber',
			isArea: true,
			names: ['かさじぞう', '地蔵要塞カムイ', '地蔵要塞魔王カムイ'],
			englishNames: ['Kasa Jizo', 'Jizo Fortress', 'Jizo Demonic Fort'],
			descriptions: [
				'お地蔵様に化けた小さなネコ。超高速で生産可能で、圧倒的な範囲爆破ダメージを与える最強の超激レアにゃ。',
				'巨大な古代空中要塞「カムイ」へと超進化。凄まじい超極大エネルギー砲で敵の大群を灰にするにゃ！',
				'魔王の力を手に入れ漆黒に染まった巨大要塞。画面全体を揺るがす破壊光線で敵城ごと消し飛ばすにゃ！'
			],
			stats: [
				{ cost: '750円', hp: '800', atk: '350', range: '320', speed: '9' }, // cheap form!
				{ cost: '3750円', hp: '6000', atk: '2500', range: '450', speed: '5' }, // giant fort!
				{ cost: '3750円', hp: '9500', atk: '4200', range: '450', speed: '5' }
			],
			color: '#ca8a04',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><polygon points="30,52 60,25 90,52" fill="#d97706" stroke="#1e293b" stroke-width="4"/><circle cx="60" cy="68" r="16" fill="white" stroke="#1e293b" stroke-width="4"/><rect x="52" y="84" width="16" height="20" fill="#94a3b8" stroke="#1e293b" stroke-width="3"/><circle cx="54" cy="65" r="2" fill="#1e293b"/><circle cx="66" cy="65" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="25" y="35" width="70" height="65" rx="8" fill="#1e293b" stroke="#0f172a" stroke-width="4"/><rect x="35" y="15" width="50" height="20" fill="#475569" stroke="#1e293b" stroke-width="3"/><line x1="30" y1="55" x2="10" y2="55" stroke="#ef4444" stroke-width="6" stroke-linecap="round"/><line x1="90" y1="55" x2="110" y2="55" stroke="#ef4444" stroke-width="6" stroke-linecap="round"/><circle cx="60" cy="65" r="8" fill="#eab308"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><rect x="20" y="25" width="80" height="75" rx="10" fill="#0f172a" stroke="#ef4444" stroke-width="4"/><rect x="35" y="5" width="50" height="20" fill="#1e293b" stroke="#ef4444" stroke-width="3"/><line x1="25" y1="45" x2="5" y2="45" stroke="#eab308" stroke-width="8" stroke-linecap="round"/><line x1="95" y1="45" x2="115" y2="45" stroke="#eab308" stroke-width="8" stroke-linecap="round"/><circle cx="60" cy="55" r="10" fill="#ef4444"/></svg>`
			]
		},
		{
			id: 'valkyrie',
			rarity: 'uber',
			isArea: true,
			names: ['ネコヴァルキリー', 'ネコヴァルキリー・真', 'ネコヴァルキリー・聖'],
			englishNames: ['Valkyrie Cat', 'Valkyrie True', 'Holy Valkyrie'],
			descriptions: [
				'神の使いである戦乙女。高貴な槍を手に持ち、素晴らしい移動速度で戦場を翔け範囲攻撃を放つにゃ。',
				'真の力を解放したヴァルキリー。スピードと威力が大きく強化され、敵前線を光の槍で粉砕するにゃ。',
				'聖なる加護を受けた神聖ヴァルキリー。たまに敵を一瞬完全に静止させる（動きを止める）にゃ！'
			],
			stats: [
				{ cost: '2000円', hp: '3500', atk: '600', range: '320', speed: '18' },
				{ cost: '2000円', hp: '5200', atk: '950', range: '320', speed: '20' },
				{ cost: '2000円', hp: '8000', atk: '1500', range: '320', speed: '22' }
			],
			color: '#06b6d4',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 25 35 Q 10 20 20 60 Z M 95 35 Q 110 20 100 60 Z" fill="white" stroke="#cbd5e1" stroke-width="3"/><circle cx="60" cy="45" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><line x1="80" y1="20" x2="80" y2="90" stroke="#eab308" stroke-width="3"/><circle cx="54" cy="42" r="2" fill="#1e293b"/><circle cx="66" cy="42" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 20 30 Q 0 10 15 65 Z M 100 30 Q 120 10 105 65 Z" fill="#38bdf8" stroke="#1e293b" stroke-width="3"/><circle cx="60" cy="45" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><line x1="82" y1="10" x2="82" y2="92" stroke="#eab308" stroke-width="4"/><circle cx="54" cy="42" r="2" fill="#1e293b"/><circle cx="66" cy="42" r="2" fill="#1e293b"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 20 25 Q -5 5 10 70 Z M 100 25 Q 125 5 110 70 Z" fill="#eab308" stroke="#ca8a04" stroke-width="3"/><circle cx="60" cy="42" r="16" fill="white" stroke="#1e293b" stroke-width="3.5"/><line x1="82" y1="5" x2="82" y2="95" stroke="#ef4444" stroke-width="4"/><circle cx="54" cy="39" r="2" fill="#1e293b"/><circle cx="66" cy="39" r="2" fill="#1e293b"/></svg>`
			]
		},
		{
			id: 'bahamut',
			rarity: 'uber',
			isArea: true,
			names: ['ココムート', '狂乱のネコムート', '覚醒のネコムート'],
			englishNames: ['Bahamut Cat', 'Crazed Bahamut', 'Awakened Bahamut'],
			descriptions: [
				'超古代兵器とされる伝説のドラゴン。動きは非常に遅いが、超射程から全てを消滅させる玉を放つにゃ。',
				'狂乱に陥った破滅のネコムート。エネルギー波を放ち、広域範囲攻撃でボス以外の雑魚を全滅させるにゃ。',
				'ついに本来のスピードと力を覚醒させた姿。超高速（スピード32）で突撃し、超破壊乱舞を繰り出すにゃ！'
			],
			stats: [
				{ cost: '4500円', hp: '5000', atk: '3200', range: '450', speed: '5' },
				{ cost: '4500円', hp: '6800', atk: '4800', range: '450', speed: '5' },
				{ cost: '4500円', hp: '8500', atk: '6500', range: '140', speed: '32' } // Speed 32, range shortens to melee!
			],
			color: '#7c3aed',
			svgPaths: [
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 25 70 Q 5 20 35 45 Z M 95 70 Q 115 20 85 45 Z" fill="#475569" stroke="#1e293b" stroke-width="3.5"/><circle cx="60" cy="52" r="20" fill="#334155" stroke="#1e293b" stroke-width="4.5"/><polygon points="45,32 60,10 75,32" fill="#1e293b"/><circle cx="53" cy="48" r="3" fill="#ef4444"/><circle cx="67" cy="48" r="3" fill="#ef4444"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 25 70 Q 2 10 35 42 Z M 95 70 Q 118 10 85 42 Z" fill="#9d174d" stroke="#1e293b" stroke-width="3.5"/><circle cx="60" cy="52" r="20" fill="#1e293b" stroke="#ef4444" stroke-width="4.5"/><polygon points="42,32 60,5 78,32" fill="#ef4444"/><circle cx="53" cy="48" r="3" fill="#eab308"/><circle cx="67" cy="48" r="3" fill="#eab308"/></svg>`,
				`<svg viewBox="0 0 120 120" class="w-full h-full"><path d="M 20 65 Q -10 5 35 35 Z M 100 65 Q 130 5 85 35 Z" fill="#7c3aed" stroke="#6d28d9" stroke-width="4"/><circle cx="60" cy="50" r="20" fill="#1e1b4b" stroke="#7c3aed" stroke-width="4.5"/><polygon points="42,30 60,2 78,30" fill="#a855f7"/><circle cx="52" cy="46" r="3" fill="#22d3ee"/><circle cx="68" cy="46" r="3" fill="#22d3ee"/></svg>`
			]
		}
	];

	// Stages setup (Japan Chapter)
	const stages = [
		{
			id: 'shizuoka',
			name: '静岡県 (Shizuoka)',
			difficulty: '★☆☆☆☆',
			description: 'のどかな富士山のふもと。お試し戦闘にぴったりの基本コースだにゃ。',
			enemyCastleHp: 800,
			reward: 100,
			enemyList: ['doge'],
			spawnFrequency: 120,
			boss: null
		},
		{
			id: 'kyoto',
			name: '京都府 (Kyoto)',
			difficulty: '★★☆☆☆',
			description: '歴史ある古都の戦い。にょろ（蛇）が大量発生するトリッキーなコースだにゃ。',
			enemyCastleHp: 1500,
			reward: 250,
			enemyList: ['doge', 'snache'],
			spawnFrequency: 90,
			boss: null
		},
		{
			id: 'osaka',
			name: '大阪府 (Osaka)',
			difficulty: '★★★☆☆',
			description: '天下の台所。体力の高いゴリさんが中盤から突進してくる高難易度コースだにゃ！',
			enemyCastleHp: 3000,
			reward: 550,
			enemyList: ['doge', 'snache', 'gory'],
			spawnFrequency: 75,
			boss: null
		},
		{
			id: 'iriomote',
			name: '西表島 (Iriomote Island)',
			difficulty: '★★★★★',
			description: '日本編・第1章の最終決戦地！城の体力が50%を切ると、超巨大ボス「カオル君」が出現するにゃ！',
			enemyCastleHp: 10000,
			reward: 1500,
			enemyList: ['doge', 'snache', 'gory'],
			spawnFrequency: 65,
			boss: {
				type: 'the_face',
				name: 'カオル君 (The Face)',
				hp: 9000,
				maxHp: 9000,
				atk: 300,
				range: 9.5,
				speed: 0.2,
				svg: `<svg viewBox="0 0 120 120" class="w-full h-full"><circle cx="60" cy="60" r="50" fill="#f8fafc" stroke="#1e293b" stroke-width="5"/><circle cx="40" cy="50" r="6" fill="#ef4444" stroke="#b91c1c" stroke-width="2"/><circle cx="80" cy="50" r="6" fill="#ef4444" stroke="#b91c1c" stroke-width="2"/><path d="M 34 40 Q 40 36 46 41" stroke="#1e293b" stroke-width="3" fill="none"/><path d="M 86 40 Q 80 36 74 41" stroke="#1e293b" stroke-width="3" fill="none"/><path d="M 40 80 Q 60 102 80 80 Z" fill="#1e293b" stroke="#1e293b" stroke-width="3"/></svg>`,
				color: '#ef4444'
			}
		}
	];

	// Battle deck composition (max 10 cats, identified by id)
	let battleDeck = $state(['cat', 'tank', 'gross', 'cow', 'bird', 'titan']);
	let deckMessage = $state('');

	// Shared evolution levels for all 19 cats
	let catForms = $state([
		0, 0, 0, 0, 0, 0, // basic (6)
		0, 0, 0, 0, 0, 0, 0, 0, 0, 0, // rare (10)
		0, 0, 0 // uber (3)
	]);

	// Encyclopedia current selection
	let selectedIndex = $state(0);
	let activeCat = $derived(cats[selectedIndex]);
	let isEvolving = $state(false);

	// Battle Stage selection
	let selectedStageIndex = $state(0);
	let activeStage = $derived(stages[selectedStageIndex]);

	// Simulator game state
	let gameStatus = $state('idle'); // 'idle', 'playing', 'victory', 'defeat'
	let money = $state(0);
	let walletLevel = $state(1);
	let playerCastleHp = $state(1000);
	let enemyCastleHp = $state(1000);
	let enemyCastleMaxHp = $state(1000);

	let allies = $state([]);
	let enemies = $state([]);

	// Cooldown map that keeps track of spawn recharge
	let cooldowns = $state({});
	cats.forEach(c => {
		cooldowns[c.id] = 0;
	});

	let walletMax = $derived(walletLevels[walletLevel - 1].max);
	let walletSpeed = $derived(walletLevels[walletLevel - 1].speed);

	let entityIdCounter = 0;
	let gameInterval;
	let bossSpawned = $state(false);
	let showBossWarning = $state(false);

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

	// Toggle a character in/out of the Battle Deck
	function toggleDeck(catId) {
		if (battleDeck.includes(catId)) {
			if (battleDeck.length === 1) {
				showDeckMessage('⚠️ デッキには最低1体必要にゃ！');
				return;
			}
			battleDeck = battleDeck.filter(id => id !== catId);
			showDeckMessage('👋 デッキから外したにゃ');
		} else {
			if (battleDeck.length >= 10) {
				showDeckMessage('⚠️ デッキ枠（最大10体）がいっぱいにゃ！');
				return;
			}
			battleDeck = [...battleDeck, catId];
			showDeckMessage('🎒 デッキに追加したにゃ！');
		}
	}

	function showDeckMessage(msg) {
		deckMessage = msg;
		setTimeout(() => {
			if (deckMessage === msg) deckMessage = '';
		}, 1500);
	}

	// Game Engine Spawning and Logic
	function startGame() {
		gameStatus = 'playing';
		money = 0;
		walletLevel = 1;
		playerCastleHp = 1000;
		enemyCastleHp = activeStage.enemyCastleHp;
		enemyCastleMaxHp = activeStage.enemyCastleHp;
		allies = [];
		enemies = [];
		bossSpawned = false;
		showBossWarning = false;
		
		// Reset cooldowns
		cats.forEach(c => {
			cooldowns[c.id] = 0;
		});

		// Spawn initial basic enemy
		spawnEnemy(activeStage.enemyList[0]);

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
			let nextCooldowns = { ...cooldowns };
			for (let key in nextCooldowns) {
				nextCooldowns[key] = Math.max(0, nextCooldowns[key] - 0.04);
			}
			cooldowns = nextCooldowns;

			// 3. Auto enemy spawner
			if (ticks % activeStage.spawnFrequency === 0) {
				const eligibleEnemies = activeStage.enemyList;
				const randEnemy = eligibleEnemies[Math.floor(Math.random() * eligibleEnemies.length)];
				spawnEnemy(randEnemy);
			}

			// 4. Boss trigger logic (Iriomote Island - Castle HP below 50%)
			if (activeStage.boss && enemyCastleHp <= (enemyCastleMaxHp / 2) && !bossSpawned) {
				bossSpawned = true;
				showBossWarning = true;
				setTimeout(() => {
					showBossWarning = false;
				}, 2500);
				spawnBoss(activeStage.boss);
			}

			// 5. Update combat and movement physics
			updateEntities();
		}, 40);
	}

	function spawnAlly(catId) {
		const catIdx = cats.findIndex(c => c.id === catId);
		const cat = cats[catIdx];
		const form = catForms[catIdx];
		const cost = parseInt(cat.stats[form].cost);

		if (money < cost || cooldowns[cat.id] > 0) return;

		money -= cost;
		cooldowns[cat.id] = getCooldownTime(cat.id);

		// Special stats adjustment for Kasa Jizo/Bahamut depending on Evolved/True forms
		let finalRange = getRangePercent(cat.id, form);
		let finalSpeed = getSpeedPercent(cat.id, form);

		const newAlly = {
			id: `ally_${entityIdCounter++}`,
			type: cat.id,
			form: form,
			name: cat.names[form],
			x: 12,
			hp: parseInt(cat.stats[form].hp),
			maxHp: parseInt(cat.stats[form].hp),
			atk: parseInt(cat.stats[form].atk),
			range: finalRange,
			speed: finalSpeed,
			svg: cat.svgPaths[form],
			color: cat.color,
			isArea: cat.isArea,
			survive: cat.survive || false,
			hasSurvived: false,
			doubleMoney: cat.doubleMoney || false,
			critChance: cat.critChance || 0,
			slowChance: cat.slowChance || 0,
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
				x: 88,
				hp: 240,
				maxHp: 240,
				atk: 22,
				range: 4.5,
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
				hp: 1500,
				maxHp: 1500,
				atk: 90,
				range: 4,
				speed: 0.72,
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

	function spawnBoss(bossData) {
		const newBoss = {
			id: `boss_${entityIdCounter++}`,
			type: bossData.type,
			name: bossData.name,
			x: 88,
			hp: bossData.hp,
			maxHp: bossData.maxHp,
			atk: bossData.atk,
			range: bossData.range,
			speed: bossData.speed,
			value: 600,
			svg: bossData.svg,
			color: bossData.color,
			isAttacking: false,
			attackTimer: 0,
			hitEffect: false,
			isBoss: true
		};
		enemies = [...enemies, newBoss];
	}

	function getCooldownTime(catId) {
		if (catId === 'cat') return 2;
		if (catId === 'tank') return 4;
		if (catId === 'gross') return 8;
		if (catId === 'cow') return 6;
		if (catId === 'bird') return 9;
		if (catId === 'titan') return 20;

		// Rare cooldowns
		if (catId === 'hop' || catId === 'thief' || catId === 'salon') return 7;
		if (catId === 'jura' || catId === 'wheel' || catId === 'witch') return 9;
		if (catId === 'archer' || catId === 'rover' || catId === 'stilts' || catId === 'gunman') return 8;

		// Uber Rare cooldowns
		if (catId === 'jizo') return 6.5; // Kasa jizo is super spammy in normal!
		if (catId === 'valkyrie') return 35;
		if (catId === 'bahamut') return 65;
		return 8;
	}

	function getRangePercent(catId, form) {
		if (catId === 'cat') return 4.5;
		if (catId === 'tank') return 3;
		if (catId === 'gross') return 18;
		if (catId === 'cow') return 4;
		if (catId === 'bird') return 9;
		if (catId === 'titan') return 5.5;

		// Rare ranges
		if (catId === 'hop') return 4.5;
		if (catId === 'thief') return 4.5;
		if (catId === 'salon') return 16;
		if (catId === 'jura') return 4.5;
		if (catId === 'wheel') return 8;
		if (catId === 'witch') return 12;
		if (catId === 'archer') return 22; // Archer Cat range
		if (catId === 'rover') return 4.5;
		if (catId === 'stilts') return 8;
		if (catId === 'gunman') return 14;

		// Uber ranges
		if (catId === 'jizo') return form === 0 ? 15 : 28;
		if (catId === 'valkyrie') return 16;
		if (catId === 'bahamut') return form === 2 ? 6 : 25; // Bahamut melee in awakened!
		return 5;
	}

	function getSpeedPercent(catId, form) {
		let base = 0.45;
		if (catId === 'cat') base = 0.55;
		if (catId === 'tank') base = 0.4;
		if (catId === 'gross') base = 0.5;
		if (catId === 'cow') base = 1.3; // Cow is fast
		if (catId === 'bird') base = 0.45;
		if (catId === 'titan') base = 0.32;

		// Rare speeds
		if (catId === 'hop') base = 0.48;
		if (catId === 'thief') base = 0.65;
		if (catId === 'salon') base = 0.42;
		if (catId === 'jura') base = 0.48;
		if (catId === 'wheel') base = 0.4;
		if (catId === 'witch') base = 0.42;
		if (catId === 'archer') base = 0.44;
		if (catId === 'rover') base = 0.55;
		if (catId === 'stilts') base = 0.38;
		if (catId === 'gunman') base = 0.42;

		// Uber speeds
		if (catId === 'jizo') base = form === 0 ? 0.6 : 0.38;
		if (catId === 'valkyrie') base = 1.1;
		if (catId === 'bahamut') base = form === 2 ? 2.2 : 0.3; // Awakened Bahamut is speed 32!

		if (form === 2 && catId !== 'bahamut' && catId !== 'cow') base *= 1.25; 
		return base;
	}

	function updateEntities() {
		// A. Update Allies
		let nextAllies = allies.map(ally => {
			let updated = { ...ally };
			let targets = enemies.filter(e => e.x > ally.x);
			targets.sort((a, b) => a.x - b.x);
			let target = targets[0];

			let isCloseToEnemy = target && (target.x - ally.x) <= ally.range;
			let isCloseToCastle = (88 - ally.x) <= ally.range;

			if (isCloseToEnemy) {
				updated.isAttacking = true;
				updated.attackTimer = (updated.attackTimer || 0) + 40;
				if (updated.attackTimer >= 800) {
					updated.attackTimer = 0;

					// Apply combat damage
					if (ally.isArea) {
						// Area damage to all enemies in range
						enemies.forEach(e => {
							if (e.x > ally.x && (e.x - ally.x) <= ally.range) {
								applyDamageToEnemy(e, ally);
							}
						});
					} else {
						// Single target damage
						const targetEnemy = enemies.find(e => e.id === target.id);
						if (targetEnemy) {
							applyDamageToEnemy(targetEnemy, ally);
						}
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
			let targets = allies.filter(a => a.x < enemy.x);
			targets.sort((a, b) => b.x - a.x);
			let target = targets[0];

			let isCloseToAlly = target && (enemy.x - target.x) <= enemy.range;
			let isCloseToCastle = (enemy.x - 12) <= enemy.range;

			if (isCloseToAlly) {
				updated.isAttacking = true;
				updated.attackTimer = (updated.attackTimer || 0) + 40;
				if (updated.attackTimer >= 800) {
					updated.attackTimer = 0;
					const targetAlly = allies.find(a => a.id === target.id);
					if (targetAlly) {
						targetAlly.hp -= enemy.atk;
						targetAlly.hitEffect = true;
						
						// Survive fatal hit once (Hop Skill)
						if (targetAlly.hp <= 0 && targetAlly.survive && !targetAlly.hasSurvived) {
							targetAlly.hp = 1;
							targetAlly.hasSurvived = true;
						}

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

		// C. Sync and Filter dead elements
		allies = nextAllies.filter(a => a.hp > 0);
		enemies = nextEnemies.filter(e => {
			if (e.hp <= 0) {
				return false;
			}
			return true;
		});
	}

	function applyDamageToEnemy(enemyObj, attacker) {
		let finalDamage = attacker.atk;
		
		// 1. Critical strike skill check (Jurassic Cat)
		if (attacker.critChance && Math.random() < attacker.critChance) {
			finalDamage *= 3;
		}

		enemyObj.hp -= finalDamage;
		enemyObj.hitEffect = true;

		// 2. Slow down target skill check (Witch / Gunslinger)
		if (attacker.slowChance && Math.random() < attacker.slowChance && !enemyObj.isBoss) {
			const originalSpeed = enemyObj.speed;
			enemyObj.speed *= 0.35; // major speed reduction
			setTimeout(() => {
				const found = enemies.find(e => e.id === enemyObj.id);
				if (found) found.speed = originalSpeed;
			}, 2000);
		}

		// 3. Double Money drop on death (Thief Cat)
		if (enemyObj.hp <= 0) {
			let bonus = enemyObj.value;
			if (attacker.doubleMoney) bonus *= 2.0;
			money = Math.min(money + bonus, walletMax);
		}

		setTimeout(() => {
			const found = enemies.find(e => e.id === enemyObj.id);
			if (found) found.hitEffect = false;
		}, 200);
	}

	onDestroy(() => {
		if (gameInterval) clearInterval(gameInterval);
	});
</script>

<svelte:head>
	<title>にゃんこ大戦争 - にゃんこ図鑑＆戦闘シミュレーター</title>
</svelte:head>

<main class="min-h-screen flex flex-col justify-between p-4 md:p-8 relative overflow-hidden">
	<!-- Background grid graphics -->
	<div class="absolute inset-0 bg-[linear-gradient(rgba(255,255,255,0.02)_1px,transparent_1px),linear-gradient(90deg,rgba(255,255,255,0.02)_1px,transparent_1px)] bg-[size:32px_32px] pointer-events-none"></div>
	<div class="absolute -top-40 -right-40 w-96 h-96 bg-blue-500 rounded-full blur-3xl opacity-20 pointer-events-none"></div>
	<div class="absolute -bottom-40 -left-40 w-96 h-96 bg-purple-500 rounded-full blur-3xl opacity-20 pointer-events-none"></div>

	<!-- Mode Selection Tab Bar -->
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

	<!-- Deck edit Toast alert -->
	{#if deckMessage}
		<div class="fixed top-20 left-1/2 -translate-x-1/2 z-50 bg-slate-900 border border-white/10 px-4 py-2 rounded-xl text-xs font-bold text-white shadow-xl animate-bounce">
			{deckMessage}
		</div>
	{/if}

	{#if currentMode === 'encyclopedia'}
		<!-- 図鑑モード (ENCYCLOPEDIA) -->
		<header class="relative text-center py-4">
			<h1 class="text-3xl md:text-5xl font-black tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-amber-300 to-yellow-500 drop-shadow-lg">
				にゃんこ大戦争
			</h1>
			<p class="text-xs md:text-sm font-semibold text-blue-300 mt-2 uppercase tracking-widest">
				── キャラクター大図鑑 ──
			</p>
		</header>

		<!-- Main Catalog Grid (Sidebar + Showcase) -->
		<div class="relative w-full max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-12 gap-8 my-auto z-10">
			
			<!-- Left Column: Filter and Roster selection -->
			<div class="lg:col-span-5 flex flex-col gap-4">
				<div class="bg-slate-950/40 backdrop-blur-xl border border-white/10 rounded-2xl p-4 shadow-xl flex flex-col">
					<!-- Rarity filtering tabs -->
					<div class="grid grid-cols-3 gap-1.5 bg-black/40 p-1 rounded-xl mb-4 text-center">
						<button
							onclick={() => { activeRarityTab = 'basic'; }}
							class="py-1.5 text-xs font-bold rounded-lg transition cursor-pointer
								{activeRarityTab === 'basic' ? 'bg-blue-600 text-white shadow' : 'text-slate-400 hover:text-white'}"
						>
							基本
						</button>
						<button
							onclick={() => { activeRarityTab = 'rare'; }}
							class="py-1.5 text-xs font-bold rounded-lg transition cursor-pointer
								{activeRarityTab === 'rare' ? 'bg-emerald-600 text-white shadow' : 'text-slate-400 hover:text-white'}"
						>
							レア
						</button>
						<button
							onclick={() => { activeRarityTab = 'uber'; }}
							class="py-1.5 text-xs font-bold rounded-lg transition cursor-pointer
								{activeRarityTab === 'uber' ? 'bg-amber-600 text-white shadow' : 'text-slate-400 hover:text-white'}"
						>
							超激レア
						</button>
					</div>

					<!-- Scrollable Character Grid -->
					<div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-2 gap-2 max-h-[380px] overflow-y-auto pr-1">
						{#each cats as cat, i}
							{#if cat.rarity === activeRarityTab}
								<button
									onclick={() => changeCat(i)}
									class="flex items-center gap-2 p-2.5 rounded-xl border transition text-left group cursor-pointer
										{selectedIndex === i
											? 'bg-blue-600/20 border-blue-500 text-white'
											: 'bg-white/5 border-white/5 hover:bg-white/10 text-slate-300'}"
								>
									<div class="w-8 h-8 rounded-lg bg-slate-900 border border-white/15 flex items-center justify-center p-0.5 select-none pointer-events-none">
										{@html cat.svgPaths[catForms[i]]}
									</div>
									<div class="min-w-0 flex-1">
										<p class="text-[10px] text-slate-400 font-mono truncate">No.0{i+1}</p>
										<p class="text-xs font-bold truncate">{cat.names[catForms[i]]}</p>
									</div>
								</button>
							{/if}
						{/each}
					</div>
				</div>

				<!-- Deck Toggle/Registration Box -->
				<div class="bg-slate-950/40 border border-white/10 rounded-2xl p-4 shadow-xl space-y-3">
					<h3 class="text-xs font-bold text-slate-300">⚔️ このキャラクターを編成に登録</h3>
					<div class="flex items-center justify-between gap-4">
						<div class="min-w-0">
							<p class="text-xs text-slate-400 truncate">現在の編成状況: {battleDeck.length}/10体</p>
						</div>
						<button
							onclick={() => toggleDeck(activeCat.id)}
							class="px-4 py-2 rounded-xl text-xs font-bold transition cursor-pointer flex items-center gap-1.5
								{battleDeck.includes(activeCat.id)
									? 'bg-rose-600 hover:bg-rose-500 text-white'
									: 'bg-emerald-600 hover:bg-emerald-500 text-white'}"
						>
							{#if battleDeck.includes(activeCat.id)}
								➖ 編成から外す
							{:else}
								➕ 編成に入れる
							{/if}
						</button>
					</div>
				</div>
			</div>

			<!-- Right Column: Character Showcase card & stats -->
			<div class="lg:col-span-7 flex flex-col gap-6">
				<!-- Big Showcase Box -->
				<div class="relative bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-6 md:p-8 shadow-2xl overflow-hidden flex flex-col sm:flex-row gap-6 items-center">
					<div class="absolute top-0 left-0 right-0 h-[1.5px] bg-gradient-to-r from-transparent via-blue-400/40 to-transparent"></div>

					<!-- Visual Cat Area -->
					<div class="w-48 h-48 flex-shrink-0 bg-slate-950/60 rounded-2xl border border-white/10 flex items-center justify-center relative p-4 group">
						<div 
							class="absolute w-32 h-32 rounded-full blur-2xl opacity-40 transition-colors duration-500"
							style="background-color: {activeCat.color};"
						></div>

						<div class="w-32 h-32 z-10 transition-transform duration-300 group-hover:scale-105 {isEvolving ? 'evolving-effect' : ''}">
							{@html activeCat.svgPaths[catForms[selectedIndex]]}
						</div>

						<span class="absolute bottom-2.5 right-2.5 text-[9px] font-black uppercase tracking-wider px-2 py-0.5 rounded bg-slate-900 border border-white/15 text-slate-300">
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
					<div class="flex-1 space-y-4 text-center sm:text-left w-full">
						<div>
							<span class="inline-block text-[9px] font-bold tracking-widest uppercase px-2 py-0.5 rounded mb-2
								{activeCat.rarity === 'basic' ? 'bg-blue-500/20 text-blue-300 border border-blue-500/30' : ''}
								{activeCat.rarity === 'rare' ? 'bg-emerald-500/20 text-emerald-300 border border-emerald-500/30' : ''}
								{activeCat.rarity === 'uber' ? 'bg-amber-500/20 text-amber-300 border border-amber-500/30 animate-pulse' : ''}"
							>
								{activeCat.rarity === 'basic' ? '基本' : ''}
								{activeCat.rarity === 'rare' ? 'レア' : ''}
								{activeCat.rarity === 'uber' ? '超激レア' : ''}
							</span>

							<div class="flex flex-col sm:flex-row sm:items-baseline gap-2 justify-center sm:justify-start">
								<h2 class="text-xl md:text-2xl font-extrabold text-white tracking-tight">
									{activeCat.names[catForms[selectedIndex]]}
								</h2>
								<span class="text-[10px] text-slate-400 font-mono">
									({activeCat.englishNames[catForms[selectedIndex]]})
								</span>
							</div>
						</div>

						<!-- Description -->
						<p class="text-slate-300 text-xs leading-relaxed h-16 overflow-y-auto">
							{activeCat.descriptions[catForms[selectedIndex]]}
						</p>

						<!-- Evolve Trigger -->
						<button
							onclick={evolveCat}
							disabled={isEvolving}
							class="w-full sm:w-auto relative group overflow-hidden px-6 py-2.5 rounded-xl text-white font-bold text-xs transition duration-300 disabled:opacity-70 flex items-center justify-center gap-1.5 hover:scale-[1.02] active:scale-[0.98] cursor-pointer"
							style="background: linear-gradient(135deg, {activeCat.color} 0%, #0f172a 100%);"
						>
							{#if isEvolving}
								進化中...
							{:else}
								⚔️ このキャラクターを進化させる！
							{/if}
						</button>
					</div>
				</div>

				<!-- Stats Box -->
				<div class="bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-6 shadow-xl">
					<div class="flex justify-between items-center mb-4">
						<h3 class="text-xs font-bold text-slate-300 flex items-center gap-2">
							📊 基本性能 <span class="text-[10px] font-normal text-slate-500">(Lv.30値)</span>
						</h3>
						<span class="text-[10px] font-bold text-slate-400">
							攻撃形態: {activeCat.isArea ? '🔊 範囲攻撃' : '🎯 単体攻撃'}
						</span>
					</div>
					<div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
						<div class="bg-slate-950/40 p-3 rounded-xl border border-white/5 text-center">
							<p class="text-[9px] text-slate-400 font-bold uppercase tracking-wider mb-0.5">生産コスト</p>
							<p class="text-base font-black text-amber-300">{activeCat.stats[catForms[selectedIndex]].cost}</p>
						</div>
						<div class="bg-slate-950/40 p-3 rounded-xl border border-white/5 text-center">
							<p class="text-[9px] text-slate-400 font-bold uppercase tracking-wider mb-0.5">体力 (HP)</p>
							<p class="text-base font-black text-emerald-400">{activeCat.stats[catForms[selectedIndex]].hp}</p>
						</div>
						<div class="bg-slate-950/40 p-3 rounded-xl border border-white/5 text-center">
							<p class="text-[9px] text-slate-400 font-bold uppercase tracking-wider mb-0.5">攻撃力</p>
							<p class="text-base font-black text-rose-400">{activeCat.stats[catForms[selectedIndex]].atk}</p>
						</div>
						<div class="bg-slate-950/40 p-3 rounded-xl border border-white/5 text-center">
							<p class="text-[9px] text-slate-400 font-bold uppercase tracking-wider mb-0.5">射程 / 速度</p>
							<p class="text-xs font-black text-cyan-300 mt-1">
								{activeCat.stats[catForms[selectedIndex]].range} <span class="text-slate-500">/</span> {activeCat.stats[catForms[selectedIndex]].speed}
							</p>
						</div>
					</div>
				</div>
			</div>
		</div>

	{:else}
		<!-- 戦闘シミュレーター (BATTLE SIMULATOR) -->
		<header class="relative text-center py-2">
			<h1 class="text-3xl md:text-5xl font-black tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-red-500 via-rose-400 to-amber-500 drop-shadow-lg">
				戦闘シミュレーター
			</h1>
			<p class="text-xs font-semibold text-rose-300 mt-1 uppercase tracking-widest">
				── 日本編 第1章 ──
			</p>
		</header>

		<div class="relative w-full max-w-6xl mx-auto z-10 flex flex-col gap-6 my-auto">
			{#if gameStatus === 'idle'}
				<!-- Pre-battle layout: Stage selection and active deck configuration -->
				<div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-stretch">
					
					<!-- Left: Stage selection (5 cols) -->
					<div class="lg:col-span-5 bg-slate-950/40 backdrop-blur-xl border border-white/10 rounded-3xl p-5 shadow-2xl flex flex-col">
						<h2 class="text-xs font-bold text-slate-400 mb-4 px-1 uppercase tracking-wider">
							🗺️ コース選択
						</h2>
						<div class="space-y-3 flex-1 overflow-y-auto max-h-[360px] pr-1">
							{#each stages as stage, idx}
								<button
									onclick={() => { selectedStageIndex = idx; }}
									class="w-full text-left p-3.5 rounded-2xl border transition duration-200 cursor-pointer flex justify-between items-center group
										{selectedStageIndex === idx
											? 'bg-rose-950/35 border-rose-500 text-white shadow-lg'
											: 'bg-white/5 border-white/5 hover:bg-white/10 text-slate-300'}"
								>
									<div>
										<p class="text-[10px] font-mono text-rose-400">STAGE 0{idx+1}</p>
										<h3 class="text-xs font-bold mt-0.5">{stage.name}</h3>
									</div>
									<div class="text-right">
										<span class="text-xs text-amber-400 font-mono block">{stage.difficulty}</span>
										<span class="text-[9px] text-slate-500 block mt-0.5">城体力: {stage.enemyCastleHp}</span>
									</div>
								</button>
							{/each}
						</div>
					</div>

					<!-- Right: Deck Member List & Spawner start (7 cols) -->
					<div class="lg:col-span-7 bg-slate-950/40 backdrop-blur-xl border border-white/10 rounded-3xl p-6 shadow-2xl flex flex-col justify-between space-y-6">
						<div class="space-y-4">
							<div>
								<span class="text-xs font-bold text-rose-400 uppercase tracking-widest">選択中ステージ</span>
								<h2 class="text-xl md:text-2xl font-black text-white mt-1">{activeStage.name}</h2>
								<p class="text-slate-300 text-xs mt-2 leading-relaxed h-12 overflow-y-auto">
									{activeStage.description}
								</p>
							</div>

							<!-- Enemies detailed list -->
							<div class="bg-white/5 p-3 rounded-xl border border-white/5 space-y-1.5">
								<span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider block">出現敵キャラクター</span>
								<div class="flex flex-wrap gap-2">
									{#each activeStage.enemyList as enemyType}
										<span class="px-2.5 py-1 text-[10px] rounded bg-slate-900 border border-white/10 text-slate-300 font-bold">
											{#if enemyType === 'doge'}わんこ{:else if enemyType === 'snache'}にょろ{:else}ゴリさん{/if}
										</span>
									{/each}
									{#if activeStage.boss}
										<span class="px-2.5 py-1 text-[10px] rounded bg-red-950/80 border border-red-500/30 text-red-300 font-black animate-pulse">
											👑 BOSS: {activeStage.boss.name}
										</span>
									{/if}
								</div>
							</div>

							<!-- Active Spawning Deck -->
							<div class="space-y-2">
								<div class="flex justify-between items-center">
									<span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">戦闘デッキメンバー ({battleDeck.length}/10)</span>
									<button onclick={() => { currentMode = 'encyclopedia'; }} class="text-[9px] text-blue-400 hover:underline cursor-pointer">
										図鑑で編成を変更する ➔
									</button>
								</div>
								<div class="grid grid-cols-5 gap-2 bg-black/30 p-2.5 rounded-xl border border-white/5 min-h-[90px]">
									{#each battleDeck as dCatId}
										{@const dIdx = cats.findIndex(c => c.id === dCatId)}
										{@const dCat = cats[dIdx]}
										<div class="flex flex-col items-center justify-center bg-white/5 p-1 rounded-lg border border-white/5 relative">
											<div class="w-8 h-8 rounded bg-slate-900 flex items-center justify-center p-0.5 select-none pointer-events-none">
												{@html dCat.svgPaths[catForms[dIdx]]}
											</div>
											<span class="text-[8px] font-bold text-slate-300 truncate w-full text-center mt-1">{dCat.names[catForms[dIdx]]}</span>
										</div>
									{/each}
									<!-- Render empty slot icons -->
									{#each Array(Math.max(0, 10 - battleDeck.length)) as _}
										<div class="flex items-center justify-center border border-dashed border-white/10 rounded-lg text-slate-600 text-xs">
											➕
										</div>
									{/each}
								</div>
							</div>
						</div>

						<button
							onclick={startGame}
							class="w-full relative group overflow-hidden px-8 py-4 rounded-2xl bg-gradient-to-r from-rose-500 to-red-600 text-white font-black text-lg transition duration-300 shadow-xl shadow-rose-950/40 hover:scale-101 cursor-pointer"
						>
							戦闘開始するにゃ！
						</button>
					</div>
				</div>

			{:else}
				<!-- ACTIVE GAME SCREEN -->
				<div class="bg-slate-950/40 backdrop-blur-xl border border-white/10 rounded-3xl p-4 shadow-2xl relative">
					
					<!-- Boss Warning overlay visual -->
					{#if showBossWarning}
						<div class="absolute inset-0 bg-red-600/10 border-4 border-red-600 animate-pulse pointer-events-none z-40 flex items-center justify-center rounded-3xl">
							<div class="bg-red-950/95 text-white font-black text-xl md:text-3xl px-6 py-4 rounded-2xl border-2 border-red-500 shadow-2xl uppercase tracking-wider text-center animate-bounce">
								⚠️ BOSS APPEARED! ⚠️
								<span class="block text-xs font-bold text-red-200 mt-1.5">強敵「カオル君」が出現したにゃ！</span>
							</div>
						</div>
					{/if}

					<!-- Castle HP status row -->
					<div class="flex justify-between items-center mb-4 px-2">
						<!-- Ally Castle HP -->
						<div class="w-2/5 max-w-[200px]">
							<div class="flex justify-between text-[10px] font-bold text-slate-300 mb-1">
								<span class="text-blue-400">味方城</span>
								<span>{playerCastleHp} / 1000</span>
							</div>
							<div class="w-full h-2 bg-slate-900 rounded-full overflow-hidden border border-white/5">
								<div class="h-full bg-blue-500 transition-all duration-150" style="width: {(playerCastleHp / 1000) * 100}%"></div>
							</div>
						</div>

						<span class="text-xs font-mono font-bold text-rose-500 bg-rose-500/10 px-2.5 py-1 rounded border border-rose-500/20">
							{activeStage.name}
						</span>

						<!-- Enemy Castle HP -->
						<div class="w-2/5 max-w-[200px]">
							<div class="flex justify-between text-[10px] font-bold text-slate-300 mb-1">
								<span class="text-red-400">敵城</span>
								<span>{enemyCastleHp} / {enemyCastleMaxHp}</span>
							</div>
							<div class="w-full h-2 bg-slate-900 rounded-full overflow-hidden border border-white/5">
								<div class="h-full bg-red-500 transition-all duration-150" style="width: {(enemyCastleHp / enemyCastleMaxHp) * 100}%"></div>
							</div>
						</div>
					</div>

					<!-- Battlefield strip track -->
					<div class="w-full h-64 bg-gradient-to-b from-slate-950 via-slate-900 to-slate-800 rounded-2xl relative overflow-hidden border border-white/5 shadow-inner">
						
						<!-- Night Stars decoration -->
						<div class="absolute top-4 left-10 w-1 h-1 bg-white rounded-full opacity-60 animate-pulse"></div>
						<div class="absolute top-10 left-1/3 w-1.5 h-1.5 bg-white rounded-full opacity-40"></div>
						<div class="absolute top-6 right-1/4 w-1 h-1 bg-white rounded-full opacity-80 animate-pulse"></div>

						<!-- Grass ground -->
						<div class="absolute bottom-0 left-0 right-0 h-10 bg-gradient-to-t from-emerald-950 to-emerald-900 border-t border-emerald-800">
							<div class="w-full h-full bg-[linear-gradient(90deg,transparent_50%,rgba(0,0,0,0.15)_50%)] bg-[size:16px_100%] opacity-30"></div>
						</div>

						<!-- Ally Castle -->
						<div class="absolute left-[5%] bottom-10 w-20 h-20 transform -translate-x-1/2 translate-y-1.5 select-none pointer-events-none">
							<svg viewBox="0 0 120 120" class="w-full h-full">
								<polygon points="20,40 30,10 45,35" fill="#e2e8f0" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" />
								<polygon points="65,35 80,10 90,40" fill="#e2e8f0" stroke="#1e293b" stroke-width="4.5" stroke-linejoin="round" />
								<rect x="15" y="30" width="80" height="80" rx="15" fill="#f8fafc" stroke="#1e293b" stroke-width="4.5" />
								<circle cx="40" cy="55" r="4" fill="#1e293b" />
								<circle cx="70" cy="55" r="4" fill="#1e293b" />
								<path d="M 50 66 Q 55 70 60 66" fill="none" stroke="#1e293b" stroke-width="3.5" stroke-linecap="round" />
								<rect x="42" y="80" width="26" height="30" rx="4" fill="#334155" stroke="#1e293b" stroke-width="3" />
							</svg>
						</div>

						<!-- Enemy Castle -->
						<div class="absolute right-[5%] bottom-10 w-20 h-20 transform translate-x-1/2 translate-y-1.5 select-none pointer-events-none">
							<svg viewBox="0 0 120 120" class="w-full h-full">
								<rect x="25" y="25" width="70" height="85" rx="8" fill="#1e1b4b" stroke="#1e293b" stroke-width="4.5" />
								<polygon points="20,30 5,5 30,25" fill="#be123c" stroke="#1e293b" stroke-width="4" />
								<polygon points="100,25 115,5 95,30" fill="#be123c" stroke="#1e293b" stroke-width="4" />
								<polygon points="40,45 50,52 35,55" fill="#f43f5e" />
								<polygon points="80,45 70,52 85,55" fill="#f43f5e" />
								<rect x="47" y="75" width="26" height="35" rx="2" fill="#0f172a" stroke="#1e293b" stroke-width="3" />
							</svg>
						</div>

						<!-- Allies Renders -->
						{#each allies as ally (ally.id)}
							<div
								class="absolute z-20 flex items-end justify-center transition-[left] duration-75 select-none pointer-events-none
									{ally.hitEffect ? 'hit-effect' : (ally.isAttacking ? 'attack-right' : 'walk-bob')}"
								style="left: {ally.x}%; bottom: 38px; transform-origin: bottom center;
									width: {ally.type === 'bahamut' && ally.form !== 2 ? '70px' : '44px'}; 
									height: {ally.type === 'bahamut' && ally.form !== 2 ? '70px' : '44px'};"
							>
								<!-- Small HP bar -->
								<div class="absolute -top-3 w-8 h-1 bg-slate-900 border border-white/10 rounded overflow-hidden">
									<div class="h-full bg-emerald-500" style="width: {(ally.hp / ally.maxHp) * 100}%"></div>
								</div>
								<div class="w-full h-full p-0.5">
									{@html ally.svg}
								</div>
							</div>
						{/each}

						<!-- Enemies Renders -->
						{#each enemies as enemy (enemy.id)}
							<div
								class="absolute z-20 flex items-end justify-center transition-[left] duration-75 select-none pointer-events-none
									{enemy.hitEffect ? 'hit-effect' : (enemy.isAttacking ? 'attack-left' : 'walk-bob')}"
								style="left: {enemy.x}%; bottom: 38px; transform-origin: bottom center;
									width: {enemy.isBoss ? '74px' : '44px'}; height: {enemy.isBoss ? '74px' : '44px'};"
							>
								<div class="absolute -top-3 w-8 h-1 bg-slate-900 border border-white/10 rounded overflow-hidden">
									<div class="h-full bg-rose-500" style="width: {(enemy.hp / enemy.maxHp) * 100}%"></div>
								</div>
								<div class="w-full h-full p-0.5">
									{@html enemy.svg}
								</div>
							</div>
						{/each}
					</div>

					<!-- Wallet & Spawner Cards Layout -->
					<div class="grid grid-cols-1 lg:grid-cols-12 gap-4 mt-6">
						<!-- Wallet controller (4 cols) -->
						<div class="lg:col-span-4 bg-slate-900/60 rounded-2xl p-4 border border-white/5 flex flex-col justify-between gap-3">
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
									🐱 働きネコ Lv.MAX
								{:else}
									💸 働きネコ Lv.{walletLevel} ➔ Lv.{walletLevel+1}
									<span class="font-mono bg-yellow-500/20 px-1.5 py-0.5 rounded ml-1">
										-{walletLevels[walletLevel - 1].upgradeCost}円
									</span>
								{/if}
							</button>
						</div>

						<!-- Spawn card deck tray (8 cols) -->
						<div class="lg:col-span-8 bg-slate-900/40 rounded-2xl p-4 border border-white/5">
							<span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider block mb-3">出撃にゃんこ生産 (登録順)</span>
							<div class="grid grid-cols-5 gap-2">
								{#each battleDeck as activeId}
									{@const catIdx = cats.findIndex(c => c.id === activeId)}
									{@const cat = cats[catIdx]}
									{@const cost = parseInt(cat.stats[catForms[catIdx]].cost)}
									{@const cdVal = cooldowns[cat.id]}
									{@const cooling = cdVal > 0}
									{@const canAfford = money >= cost}

									<button
										onclick={() => spawnAlly(cat.id)}
										disabled={cooling || !canAfford}
										class="relative aspect-[4/5] rounded-lg border transition duration-200 flex flex-col items-center justify-between p-2 overflow-hidden cursor-pointer
											{cooling || !canAfford
												? 'bg-slate-950/60 border-white/5 opacity-55'
												: 'bg-white/5 border-white/10 hover:bg-white/10 shadow shadow-black/30'}"
									>
										{#if cooling}
											<div 
												class="absolute bottom-0 left-0 right-0 bg-slate-900/80 flex items-center justify-center text-[9px] font-black text-slate-300 z-10"
												style="height: {(cdVal / getCooldownTime(cat.id)) * 100}%; top: 0;"
											>
												{cdVal.toFixed(1)}s
											</div>
										{/if}

										<div class="w-8 h-8 bg-slate-900/50 rounded p-0.5">
											{@html cat.svgPaths[catForms[catIdx]]}
										</div>

										<div class="text-center w-full mt-1">
											<p class="text-[8px] font-bold text-slate-200 truncate">{cat.names[catForms[catIdx]]}</p>
											<p class="text-[9px] font-black text-yellow-400 font-mono">{cost}円</p>
										</div>
									</button>
								{/each}
							</div>
						</div>
					</div>

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

		<!-- Victory & defeat modal overlays -->
		{#if gameStatus === 'victory'}
			<div class="fixed inset-0 bg-slate-950/80 backdrop-blur-md flex items-center justify-center z-50 animate-fade-in">
				<div class="bg-slate-900 border border-yellow-500/20 rounded-3xl p-8 max-w-sm text-center shadow-2xl space-y-5 border-t-[4px] border-t-yellow-400 animate-scale-up">
					<h2 class="text-3xl font-black text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-amber-300 to-yellow-500 tracking-wider">
						🎉 完全勝利にゃ！
					</h2>
					<p class="text-slate-300 text-sm leading-relaxed">
						{activeStage.name}の攻略に成功したにゃ！<br>
						<span class="text-yellow-400 font-bold">クリア報酬: +{activeStage.reward}XP</span> 獲得！
					</p>
					<div class="flex gap-3 pt-2">
						<button
							onclick={startGame}
							class="flex-1 py-3 rounded-xl bg-gradient-to-r from-yellow-500 to-amber-600 text-slate-950 font-black text-sm transition cursor-pointer"
						>
							再挑戦
						</button>
						<button
							onclick={() => { gameStatus = 'idle'; }}
							class="flex-1 py-3 rounded-xl bg-white/5 border border-white/15 hover:bg-white/10 text-white font-bold text-sm transition cursor-pointer"
						>
							ステージ選択
						</button>
					</div>
				</div>
			</div>
		{/if}

		{#if gameStatus === 'defeat'}
			<div class="fixed inset-0 bg-slate-950/80 backdrop-blur-md flex items-center justify-center z-50 animate-fade-in">
				<div class="bg-slate-900 border border-rose-500/20 rounded-3xl p-8 max-w-sm text-center shadow-2xl space-y-5 border-t-[4px] border-t-rose-500 animate-scale-up">
					<h2 class="text-3xl font-black text-transparent bg-clip-text bg-gradient-to-r from-rose-500 via-red-400 to-rose-600 tracking-wider">
						💀 敗北したにゃ...
					</h2>
					<p class="text-slate-300 text-sm leading-relaxed">
						味方の城が破壊されてしまったにゃ... キャラクターを「にゃんこ図鑑」で更に強く進化させてリベンジするにゃ！
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
