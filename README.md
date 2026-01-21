<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>小红脸 (Rotbäckchen) 金牌客服话术助手</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body { font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif; background-color: #f3f4f6; }
        .script-card { transition: all 0.2s; cursor: pointer; }
        .script-card:hover { transform: translateY(-2px); box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06); border-color: #ef4444; }
        .toast { visibility: hidden; min-width: 250px; background-color: #333; color: #fff; text-align: center; border-radius: 4px; padding: 16px; position: fixed; z-index: 1; left: 50%; bottom: 30px; transform: translateX(-50%); font-size: 14px; opacity: 0; transition: opacity 0.3s; }
        .toast.show { visibility: visible; opacity: 1; }
        .category-btn.active { background-color: #ef4444; color: white; }
    </style>
</head>
<body class="h-screen flex flex-col overflow-hidden">

    <!-- 顶部导航栏 -->
    <header class="bg-white shadow-sm z-10 flex-none">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <div class="flex items-center">
                <div class="w-10 h-10 bg-red-500 rounded-full flex items-center justify-center text-white font-bold text-xl mr-3">R</div>
                <div>
                    <h1 class="text-xl font-bold text-gray-900">小红脸话术助手 V1.0</h1>
                    <p class="text-xs text-gray-500">基于 V4.0 运营方案 | 点击卡片复制</p>
                </div>
            </div>
            <div class="relative w-96">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                    <i class="fas fa-search text-gray-400"></i>
                </span>
                <input type="text" id="searchInput" class="block w-full pl-10 pr-3 py-2 border border-gray-300 rounded-md leading-5 bg-gray-50 placeholder-gray-500 focus:outline-none focus:placeholder-gray-400 focus:ring-1 focus:ring-red-500 focus:border-red-500 sm:text-sm" placeholder="搜索关键词：倒刺、缺铁、Salus...">
            </div>
        </div>
    </header>

    <div class="flex flex-1 overflow-hidden">
        <!-- 左侧分类侧边栏 -->
        <aside class="w-64 bg-white border-r border-gray-200 overflow-y-auto hidden md:block flex-none">
            <nav class="p-4 space-y-1">
                <button onclick="filterCategory('all')" class="category-btn active w-full text-left px-4 py-3 rounded-md text-sm font-medium text-gray-700 hover:bg-red-50 hover:text-red-700 transition-colors flex items-center">
                    <i class="fas fa-th-large w-6 text-center mr-2"></i> 全部话术
                </button>
                <button onclick="filterCategory('sop')" class="category-btn w-full text-left px-4 py-3 rounded-md text-sm font-medium text-gray-700 hover:bg-red-50 hover:text-red-700 transition-colors flex items-center">
                    <i class="fas fa-handshake w-6 text-center mr-2"></i> 接待流程 (SOP)
                </button>
                <button onclick="filterCategory('diagnosis')" class="category-btn w-full text-left px-4 py-3 rounded-md text-sm font-medium text-gray-700 hover:bg-red-50 hover:text-red-700 transition-colors flex items-center">
                    <i class="fas fa-user-md w-6 text-center mr-2"></i> 症状诊断 & 推荐
                </button>
                <button onclick="filterCategory('combat')" class="category-btn w-full text-left px-4 py-3 rounded-md text-sm font-medium text-gray-700 hover:bg-red-50 hover:text-red-700 transition-colors flex items-center">
                    <i class="fas fa-fist-raised w-6 text-center mr-2"></i> 竞品攻防 & 逼单
                </button>
                <button onclick="filterCategory('crisis')" class="category-btn w-full text-left px-4 py-3 rounded-md text-sm font-medium text-gray-700 hover:bg-red-50 hover:text-red-700 transition-colors flex items-center">
                    <i class="fas fa-shield-alt w-6 text-center mr-2"></i> 售后危机处理
                </button>
                <button onclick="filterCategory('private')" class="category-btn w-full text-left px-4 py-3 rounded-md text-sm font-medium text-gray-700 hover:bg-red-50 hover:text-red-700 transition-colors flex items-center">
                    <i class="fas fa-users w-6 text-center mr-2"></i> 私域导流
                </button>
            </nav>
        </aside>

        <!-- 右侧内容区域 -->
        <main class="flex-1 overflow-y-auto p-6 bg-gray-50" id="scriptsContainer">
            <!-- 话术卡片将通过JS动态生成 -->
        </main>
    </div>

    <!-- 复制成功提示 -->
    <div id="toast" class="toast">
        <i class="fas fa-check-circle text-green-400 mr-2"></i> 已复制到剪贴板！
    </div>

    <script>
        // 话术数据源
        const scripts = [
            // 1. 接待流程
            {
                id: 1,
                category: 'sop',
                title: '标准开场 (人设建立)',
                tags: ['开场', '首句'],
                content: '您好！这里是始于1805年的德国小红脸 (Haus Rabenhorst) 旗舰店。我是您的专属营养顾问。请问是给多大的宝宝咨询？最近宝宝有什么具体的成长烦恼吗（比如不爱吃饭、睡觉出汗、注意力不集中）？'
            },
            {
                id: 2,
                category: 'sop',
                title: '品牌信任背书',
                tags: ['信任', '历史', '德国'],
                content: '亲，可以放心哦。我们是德国药房常驻品牌，拥有200多年历史，连续11年获得DLG（德国农业协会）长期品质金奖。在德国，几乎每个宝宝都是喝着小红脸长大的，成分非常安全。'
            },
            
            // 2. 症状诊断
            {
                id: 3,
                category: 'diagnosis',
                title: '缺锌诊断 (挑食/倒刺)',
                tags: ['挑食', '倒刺', '不爱吃饭', '烂嘴角', '锌'],
                content: '这通常是缺锌的表现。建议带【小红脸有机锌维他】。它含有有机液态锌和维C，能改善味蕾敏感度，让宝宝胃口大开。锌被称为“生长火花塞”，缺锌会影响身高的，建议尽早干预。'
            },
            {
                id: 4,
                category: 'diagnosis',
                title: '缺钙诊断 (夜哭/盗汗)',
                tags: ['夜哭', '出汗', '枕秃', '出牙晚', '钙'],
                content: '这是典型的缺钙信号。推荐【小红脸钙维他】。我们用的是柠檬酸钙+维生素D3的黄金组合，吸收率是普通碳酸钙的3-6倍，而且不伤肠胃。晚餐后喝，帮助宝宝安睡一整晚。'
            },
            {
                id: 5,
                category: 'diagnosis',
                title: '缺铁诊断 (多动/脸色白)',
                tags: ['专注力', '多动', '脸色白', '记忆力', '铁'],
                content: '这可能是隐性缺铁。缺铁会影响大脑供氧，导致记忆力下降、注意力不集中。建议尝试【经典铁元 (Eisen)】。它是液态二价铁，吸收极快，而且是鲜榨草莓味的，宝宝爱喝才能坚持补。'
            },
            {
                id: 6,
                category: 'diagnosis',
                title: '免疫力/流感防护',
                tags: ['感冒', '咳嗽', '流感', '免疫力'],
                content: '最近流感高发，建议常备两款：\n1. 【Halswohl (可舒克)】：含有百里香，针对咳嗽、嗓子干痒，润肺效果很好。\n2. 【Immunkraft (小黑果)】：接骨木莓提取，在欧美是“天然抵抗力”，每天喝一点，给宝宝穿上隐形防护服。'
            },

            // 3. 竞品攻防
            {
                id: 7,
                category: 'combat',
                title: '攻 Salus (红铁) - 口感/便携',
                tags: ['竞品', '红铁', '绿铁', '味道', '便携'],
                content: '亲，区别很大哦：\n1. 口感完胜：竞品铁锈味重，宝宝很难喂；我们是 Muttersaft（第一道原汁），像喝鲜榨草莓汁，宝宝主动要喝。\n2. 更便携：竞品开封必须冷藏，出门没法带；我们有125ml Mini瓶，一次一瓶，包包里随时补铁，不用担心氧化。'
            },
            {
                id: 8,
                category: 'combat',
                title: '攻 Eric Favre (艾瑞可)',
                tags: ['竞品', '艾瑞可', '性价比'],
                content: '亲，入口的东西品牌底蕴最重要。小红脸母公司始于1805年，是制药级标准，拿过370+国际大奖。我们的营养素是天然有机螯合的，不是简单的化学勾兑，对宝宝肝肾负担更小。'
            },
            {
                id: 9,
                category: 'combat',
                title: '逼单必杀 - Mini瓶试错',
                tags: ['犹豫', '试喝', '贵'],
                content: '亲，特别理解您的担心。要不您先拍一瓶【125ml Mini体验装】？现在活动价才39.9元，一瓶是一周的量。如果宝宝爱喝您再来买大瓶，如果不爱喝也就一杯奶茶钱。\n⚠️ 现在下单，我这边还能额外备注送您一张【大瓶装20元回购券】，仅限今天有效哦！'
            },
            {
                id: 10,
                category: 'combat',
                title: '逼单必杀 - CP组合凑单',
                tags: ['凑单', '关联销售', '组合'],
                content: '亲，看到您买了钙。提醒一下，如果宝宝平时也不爱吃肉，建议【铁+钙】一起补——铁管专注力，钙管长个子。现在加购一瓶铁，正好凑满300-50，相当于第二瓶半价！非常划算，需要我帮您备注优先发货吗？'
            },

            // 4. 售后危机
            {
                id: 11,
                category: 'crisis',
                title: '沉淀物/浑浊解释',
                tags: ['沉淀', '浑浊', '杂质'],
                content: '亲，您的眼力真好！这正是我们 Muttersaft (第一道原汁) 的特征。我们坚持不澄清、不过滤，保留了水果原本的果绒和活性微量元素。这些沉淀全是营养精华（活性铁/钙），请务必摇匀后饮用。如果清澈透明反而是勾兑的哦。'
            },
            {
                id: 12,
                category: 'crisis',
                title: '精准用量指导 (3岁示例)',
                tags: ['怎么喝', '用量', '喝多少'],
                content: '亲，德式喂养讲究精准，根据您宝宝3岁的年龄：\n- 钙维他：晚餐后服用 40-60ml。\n- 铁维他：午餐后服用 10-15ml。\n- 止咳饮：每天2次，每次 5ml，建议温热饮用。\n注意：开封后的大瓶请务必放冰箱冷藏，并在14天内喝完。'
            },

            // 5. 私域
            {
                id: 13,
                category: 'private',
                title: '发货/收货后导流',
                tags: ['私域', '微信', '入群'],
                content: '亲，感谢选择小红脸！为了指导您更精准地喂养，我们特聘的【德国注册营养师】已经在会员群等您啦。\n👉 入会福利：免费领宝宝【生长发育评估报告】+【0元试新品】资格。\n点击链接加入，备注“宝宝年龄”即可。'
            }
        ];

        // 渲染函数
        function renderScripts(data) {
            const container = document.getElementById('scriptsContainer');
            container.innerHTML = '';

            if (data.length === 0) {
                container.innerHTML = '<div class="text-center text-gray-500 mt-10"><i class="fas fa-search mb-2 text-2xl"></i><p>没有找到相关话术</p></div>';
                return;
            }

            data.forEach(script => {
                const card = document.createElement('div');
                card.className = 'script-card bg-white p-5 rounded-lg border border-gray-200 mb-4 relative group';
                card.onclick = () => copyText(script.content);

                let badgeColor = 'bg-gray-100 text-gray-800';
                if(script.category === 'diagnosis') badgeColor = 'bg-blue-100 text-blue-800';
                if(script.category === 'combat') badgeColor = 'bg-red-100 text-red-800';
                if(script.category === 'crisis') badgeColor = 'bg-yellow-100 text-yellow-800';

                // 将换行符转换为HTML换行
                const formattedContent = script.content.replace(/\n/g, '<br>');

                card.innerHTML = `
                    <div class="flex justify-between items-start mb-2">
                        <div class="flex items-center">
                            <h3 class="font-bold text-gray-900 text-lg mr-3">${script.title}</h3>
                            <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium ${badgeColor}">
                                ${getCategoryName(script.category)}
                            </span>
                        </div>
                        <button class="text-gray-400 hover:text-red-500" title="复制">
                            <i class="far fa-copy text-xl"></i>
                        </button>
                    </div>
                    <div class="text-gray-600 text-sm leading-relaxed cursor-text select-all font-mono bg-gray-50 p-3 rounded border border-gray-100">
                        ${formattedContent}
                    </div>
                    <div class="mt-3 flex flex-wrap gap-2">
                        ${script.tags.map(tag => `<span class="text-xs text-gray-400 bg-gray-100 px-2 py-1 rounded">#${tag}</span>`).join('')}
                    </div>
                    <div class="absolute inset-0 bg-red-500 opacity-0 group-hover:opacity-5 rounded-lg pointer-events-none transition-opacity"></div>
                `;
                container.appendChild(card);
            });
        }

        // 获取分类名称
        function getCategoryName(cat) {
            const map = {
                'sop': '接待流程',
                'diagnosis': '诊断推荐',
                'combat': '竞品攻防',
                'crisis': '售后危机',
                'private': '私域导流'
            };
            return map[cat] || '通用';
        }

        // 复制功能
        function copyText(text) {
            navigator.clipboard.writeText(text).then(() => {
                const toast = document.getElementById('toast');
                toast.className = "toast show";
                setTimeout(() => { toast.className = toast.className.replace("show", ""); }, 2000);
            });
        }

        // 分类筛选
        function filterCategory(category) {
            // 更新按钮状态
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active', 'bg-red-50', 'text-red-700'));
            const activeBtn = event.currentTarget;
            activeBtn.classList.add('active');

            if (category === 'all') {
                renderScripts(scripts);
            } else {
                const filtered = scripts.filter(s => s.category === category);
                renderScripts(filtered);
            }
        }

        // 搜索功能
        document.getElementById('searchInput').addEventListener('input', (e) => {
            const keyword = e.target.value.toLowerCase();
            const filtered = scripts.filter(s => 
                s.title.toLowerCase().includes(keyword) || 
                s.content.toLowerCase().includes(keyword) ||
                s.tags.some(t => t.toLowerCase().includes(keyword))
            );
            renderScripts(filtered);
        });

        // 初始化
        renderScripts(scripts);

    </script>
</body>
</html># rotbackchen-cs
