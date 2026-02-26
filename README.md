# Store-Operation-Test
小红书店铺运营实操（心理学类）
[index.html](https://github.com/user-attachments/files/25572540/index.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PsycheTalent - 天赋职业深度潜能评估</title>
    <!-- 引入 Tailwind CSS 进行高级 UI 样式绘制 -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- 引入 Chart.js 用于绘制雷达图 -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- 引入 html2canvas 用于一键生成图片 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700&display=swap');
        body {
            font-family: 'Noto Sans SC', sans-serif;
            background-color: #f8f9fc;
            -webkit-tap-highlight-color: transparent;
        }
        .fade-in { animation: fadeIn 0.4s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        .slide-in { animation: slideIn 0.3s ease-out forwards; }
        @keyframes slideIn { from { opacity: 0; transform: translateX(20px); } to { opacity: 1; transform: translateX(0); } }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="text-slate-800 antialiased h-screen w-screen overflow-hidden flex justify-center bg-gradient-to-br from-indigo-100 via-purple-50 to-blue-100 relative">

    <!-- 手机外壳容器 (要截图的区域) -->
    <div class="w-full max-w-md h-full bg-white relative overflow-hidden shadow-2xl flex flex-col" id="app-container">
        
        <!-- 页面 1: 首页欢迎页 -->
        <div id="page-start" class="flex-1 flex flex-col items-center justify-center p-8 fade-in h-full overflow-y-auto no-scrollbar">
            <div class="w-24 h-24 bg-gradient-to-br from-indigo-100 to-purple-100 rounded-[2rem] flex items-center justify-center mb-8 shadow-inner transform rotate-3">
                <svg class="w-12 h-12 text-indigo-600 transform -rotate-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"></path></svg>
            </div>
            <h1 class="text-3xl font-bold mb-3 text-center text-slate-800 tracking-tight">天赋职业潜能测评</h1>
            <p class="text-slate-500 text-center mb-10 text-sm leading-relaxed px-4">
                深度剖析你的底层行为逻辑与核心动机，找到真正能让你发光发热的职场天命。
            </p>
            
            <div class="w-full space-y-3 mb-8">
                <div class="flex items-center text-sm text-slate-600 bg-slate-50 border border-slate-100 p-4 rounded-2xl shadow-sm">
                    <span class="text-xl mr-3">🧭</span> 
                    <div>
                        <div class="font-bold text-slate-700">90道情境题</div>
                        <div class="text-xs text-slate-400 mt-0.5">全方位覆盖4大潜能维度</div>
                    </div>
                </div>
                <div class="flex items-center text-sm text-slate-600 bg-slate-50 border border-slate-100 p-4 rounded-2xl shadow-sm">
                    <span class="text-xl mr-3">⏱️</span> 
                    <div>
                        <div class="font-bold text-slate-700">预计耗时 15 分钟</div>
                        <div class="text-xs text-slate-400 mt-0.5">请遵循第一直觉作答</div>
                    </div>
                </div>
            </div>

            <button onclick="startQuiz()" class="w-full py-4 rounded-2xl bg-slate-900 text-white font-bold text-lg shadow-[0_10px_20px_rgba(0,0,0,0.1)] transform transition active:scale-95 hover:bg-slate-800">
                开启我的天赋解析
            </button>
        </div>

        <!-- 页面 2: 答题页 -->
        <div id="page-quiz" class="hidden flex-1 flex flex-col p-6 bg-slate-50 h-full">
            <div class="w-full mb-6 pt-2">
                <div class="flex justify-between items-end mb-2">
                    <span class="text-xs font-bold text-indigo-400 tracking-wider">PROGRESS</span>
                    <span id="progress-text" class="text-sm font-bold text-slate-700">1 <span class="text-slate-400 text-xs font-normal">/ 90</span></span>
                </div>
                <div class="w-full bg-slate-200 rounded-full h-2 overflow-hidden">
                    <div id="progress-bar" class="bg-gradient-to-r from-indigo-500 to-purple-500 h-2 rounded-full transition-all duration-300 ease-out" style="width: 1.11%;"></div>
                </div>
            </div>
            <div class="flex-1 flex flex-col relative overflow-hidden">
                <div id="question-container" class="flex-1 overflow-y-auto no-scrollbar pb-10 slide-in">
                    <h2 id="question-text" class="text-[1.15rem] font-bold text-slate-800 mb-8 leading-relaxed"></h2>
                    <div id="options-container" class="space-y-3"></div>
                </div>
            </div>
        </div>

        <!-- 页面 3: 加载分析页 -->
        <div id="page-loading" class="hidden flex-1 flex flex-col items-center justify-center p-8 bg-slate-50 h-full">
            <div class="relative w-28 h-28 mb-8">
                <div class="absolute inset-0 rounded-full border-4 border-slate-200"></div>
                <div class="absolute inset-0 rounded-full border-4 border-indigo-600 border-t-transparent animate-spin"></div>
                <div class="absolute inset-0 flex items-center justify-center">
                    <svg class="w-8 h-8 text-indigo-600 animate-pulse" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"></path></svg>
                </div>
            </div>
            <h3 class="text-xl font-bold text-slate-800 tracking-tight">正在生成深度洞察</h3>
            <p class="text-slate-500 text-sm mt-3 text-center" id="loading-text">正在交叉比对多维潜能矩阵...</p>
        </div>

        <!-- 页面 4: 结果页 (将作为截图目标) 去除了 pb-24 防止底部留白过长 -->
        <div id="page-result" class="hidden flex-1 flex flex-col bg-slate-50 h-full overflow-y-auto no-scrollbar fade-in relative">
            <!-- 顶部导航条 -->
            <div class="bg-white/80 backdrop-blur-md px-5 py-4 flex items-center justify-between sticky top-0 z-10 border-b border-slate-100" id="result-header">
                <button onclick="location.reload()" class="text-slate-400 hover:text-slate-600 transition">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path></svg>
                </button>
                <span class="font-bold text-slate-800 tracking-wide text-sm">PsycheTalent 测评报告</span>
                <span class="w-6"></span>
            </div>

            <div class="p-5 space-y-6 bg-slate-50" id="capture-area">
                <!-- 头部身份定义 -->
                <div class="text-center py-4">
                    <h2 class="text-sm font-bold text-indigo-500 tracking-widest uppercase mb-2">你的核心天赋模型</h2>
                    <h1 id="dynamic-title" class="text-3xl font-black text-slate-800 mb-4">分析中...</h1>
                    <div id="dynamic-tags" class="flex flex-wrap justify-center gap-2"></div>
                </div>

                <!-- 雷达图卡片 -->
                <div class="bg-white p-6 rounded-3xl shadow-[0_8px_30px_rgba(0,0,0,0.04)] border border-slate-100 relative overflow-hidden">
                    <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-indigo-500 to-purple-500"></div>
                    <h3 class="text-center text-slate-400 text-xs font-bold tracking-widest uppercase mb-6">能力雷达解析</h3>
                    <div class="relative w-full aspect-square">
                        <canvas id="radarChart"></canvas>
                    </div>
                </div>

                <!-- 深度自我分析 -->
                <div class="mt-8">
                    <div class="flex items-center mb-5">
                        <div class="w-8 h-8 rounded-full bg-indigo-100 flex items-center justify-center mr-3">
                            <svg class="w-4 h-4 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path></svg>
                        </div>
                        <h2 class="text-xl font-bold text-slate-800">职场深度剖析</h2>
                    </div>
                    
                    <div class="bg-emerald-50 p-6 rounded-3xl border border-emerald-100 shadow-sm relative overflow-hidden">
                        <div class="flex items-center mb-4">
                            <h3 class="font-bold text-emerald-800 text-lg">核心天赋矩阵</h3>
                        </div>
                        <ul id="dynamic-strengths" class="text-sm text-emerald-700 space-y-3 list-none"></ul>
                    </div>

                    <div class="bg-amber-50 p-6 rounded-3xl border border-amber-100 shadow-sm mt-4 relative overflow-hidden">
                        <div class="flex items-center mb-4">
                            <h3 class="font-bold text-amber-800 text-lg">可能遇到的陷阱</h3>
                        </div>
                        <ul id="dynamic-weaknesses" class="text-sm text-amber-700 space-y-3 list-none"></ul>
                    </div>

                    <div class="bg-indigo-50/50 p-6 rounded-3xl border border-indigo-100 shadow-sm mt-4">
                        <h3 class="font-bold text-indigo-800 text-sm mb-2">💡 专家发展建议</h3>
                        <p id="dynamic-advice" class="text-sm text-indigo-700/90 leading-relaxed"></p>
                    </div>
                </div>
                
                <!-- 底部留白给截图 -->
                <div class="h-6" id="bottom-padding"></div>
            </div>

            <!-- 底部按钮区 (取消了 fixed 悬浮定位，改为正常的文档流，跟随页面滚动到最后) -->
            <div id="save-btn-container" class="w-full px-5 pb-10 bg-slate-50 mt-2">
                <button onclick="saveReportAsImage()" id="save-btn" class="w-full py-4 rounded-2xl bg-slate-900 text-white font-bold text-sm shadow-[0_8px_20px_rgba(0,0,0,0.1)] transform transition active:scale-95 hover:bg-slate-800 flex items-center justify-center">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
                    一键保存结果海报
                </button>
            </div>
        </div>
    </div>

    <script>
        // 90道题库 (选项顺序固定：A逻辑系统 B共情社交 C创意表达 D执行落地)
        const rawQuestions = `你最愿意长期投入的事情更像是：A. 把复杂问题拆成清晰规则并优化 B. 让人群更有凝聚力与情绪更稳定 C. 把灵感变成作品或表达 D. 让一个系统真正落地并跑起来
当你觉得“这份工作值得”，通常因为：A. 成果可量化且进步明显 B. 对他人产生了真实帮助 C. 允许自我风格与审美存在 D. 有自主权与决策空间
你最难忍受的工作体验是：A. 逻辑混乱、标准不一致 B. 冷漠竞争、互相消耗 C. 反复重复、缺少新意 D. 事事审批、无法推进
你更希望别人因为什么认可你：A. 专业判断准确 B. 可靠体贴、会照顾人 C. 观点独特、表达有感染力 D. 执行强、能把事做成
你对“成功”的第一反应更接近：A. 能成为该领域的专家 B. 关系和谐并被信任 C. 作品被看见并影响他人 D. 资源整合后持续增长
你最喜欢的奖励方式是：A. 看到数据与指标提升 B. 收到真诚感谢或口碑 C. 有舞台展示成果 D. 获得更大项目与权限
你更愿意把时间花在：A. 精进一项技能到极致 B. 陪伴与支持他人成长 C. 体验与探索不同风格 D. 把机会变成现金流或成果
当你在选择职业方向时，最先考虑：A. 专业壁垒与成长路径 B. 价值观与团队氛围 C. 兴趣与表达空间 D. 市场需求与可扩展性
你对“稳定”的理解更像：A. 有清晰方法论 B. 有可靠的人际支持 C. 心里有自由与创作余地 D. 能掌控节奏与资源
你最可能为哪种事情熬夜：A. 把方案推演到无漏洞 B. 帮朋友解决关键难题 C. 做出一个满意的作品 D. 赶在窗口期把项目上线
你更相信：A. 正确的方法带来正确结果 B. 好关系能带来好合作 C. 独特视角创造差异化 D. 行动速度决定机会
你愿意承担更大压力，如果换来：A. 更高的技术含金量 B. 更强的社会意义 C. 更高的审美或表达完成度 D. 更大的业务影响力
你更愿意与哪类使命绑定：A. 提升效率与准确性 B. 促进健康、教育或公益 C. 文化、内容或体验创新 D. 商业增长与规模化
你做决定时，最在意：A. 证据与推理链 B. 人的感受与长期关系 C. 直觉与整体氛围 D. 结果与可操作性
如果必须放弃一项，你最不愿放弃：A. 专业深度 B. 人际信任 C. 创造自由 D. 推进权力
你学新东西最舒服的方式：A. 先看原理框架再做题 B. 通过讨论与互教加深 C. 先做一个小作品边学边改 D. 直接上手做项目快速迭代
面对陌生领域，你常见第一步：A. 搜集资料建立术语表 B. 找懂的人请教并建立连接 C. 先体验样例找感觉 D. 先找可用模板马上动手
你更擅长记住：A. 结构与因果关系 B. 人与事件的细节 C. 画面、比喻与故事 D. 流程、步骤与节点
你读一篇长文时更关注：A. 论点是否严密 B. 立场是否照顾到人 C. 表达是否有画面感 D. 结论是否能行动
你最容易进入“心流”的学习场景：A. 安静独处、深度推理 B. 小组讨论、互相启发 C. 有音乐与灵感氛围 D. 有截止时间与任务驱动
你更喜欢的老师/导师风格：A. 讲清逻辑并纠错严格 B. 关心状态并鼓励持续 C. 启发式提问、尊重个性 D. 目标导向、给清单与反馈
你遇到复杂概念时更倾向：A. 写公式/画逻辑图 B. 用例子讲给别人听 C. 用隐喻或故事理解 D. 做一个最小可行实验
你更喜欢的学习成果呈现：A. 论文/报告/推导 B. 分享会/带教笔记 C. 作品集/视频/海报 D. Demo/原型/上线结果
你对“细节”的态度：A. 细节决定正确性 B. 细节体现关怀 C. 细节决定风格与质感 D. 细节要服务进度
你做笔记更像：A. 层级大纲与定义 B. 人名、观点与关系图 C. 涂鸦、关键词与灵感碎片 D. 任务列表与时间线
你更容易被哪种内容吸引：A. 逻辑推理与模型 B. 人物与情感故事 C. 视觉、音乐与表达 D. 商业案例与实战复盘
你遇到不理解的部分通常：A. 继续查资料直到闭环 B. 直接问人求解释 C. 先放着等待顿悟 D. 先跳过做出成果再补
你更喜欢被如何评估：A. 是否严谨与可复现 B. 是否合作与影响他人 C. 是否原创与有风格 D. 是否达成目标与收益
你学习时最怕：A. 概念模糊导致走偏 B. 氛围紧张影响发挥 C. 无趣枯燥消耗热情 D. 节奏拖慢看不到成果
你更愿意把知识用来：A. 做系统化方法论 B. 做培训/咨询/支持 C. 做内容输出与表达 D. 做产品/业务落地
你最理想的工作节奏：A. 大块时间深度工作 B. 穿插协作与沟通 C. 灵活自由、随时捕捉灵感 D. 快速推进、短周期交付
你更喜欢的办公形态：A. 独立工位或远程专注 B. 团队同频、可随时交流 C. 空间有美感与创作氛围 D. 能随时开会拍板执行
你更愿意加入的团队：A. 专业强、标准高 B. 互相照应、关系好 C. 多元开放、包容表达 D. 目标清晰、行动力强
你面对规则与流程：A. 需要清晰、可优化 B. 只要不伤害人就好 C. 过多会扼杀创造 D. 有用就用，没用就改
你更喜欢哪类任务占比高：A. 分析、建模、优化 B. 沟通、协调、支持 C. 创意、写作、设计 D. 推进、谈判、运营
你理想的工作成果形态：A. 指标提升与系统稳定 B. 用户/客户满意度提升 C. 作品被传播与记住 D. 业务增长与规模扩张
你更舒适的汇报方式：A. 书面、结构化文档 B. 会议对话与共识 C. 演示与故事化表达 D. 结论+计划+资源需求
你更适应的管理方式：A. 给目标，允许我自推演 B. 及时沟通，关注情绪 C. 给空间，不要过度干预 D. 给权限，要求结果
你更愿意跟谁合作：A. 思维严密的专家 B. 善于共情的伙伴 C. 有创意的表达者 D. 强执行的推动者
你对“多任务并行”的态度：A. 尽量避免，影响深度 B. 可以，但要沟通清楚 C. 灵感驱动时反而更好 D. 必须并行，效率优先
你更喜欢的工作地点：A. 安静低干扰 B. 人气适中、有互动 C. 有灵感刺激的环境 D. 资源集中、方便协同
你更需要的工作边界：A. 明确职责与标准 B. 尊重与善意沟通 C. 不要限制表达方式 D. 不要拖慢决策链路
你对加班的接受度取决于：A. 是否在解决硬问题 B. 是否在帮助重要的人 C. 是否在完成一个作品 D. 是否在抓住关键窗口
你更喜欢的会议类型：A. 决策依据充分、少而精 B. 大家都能表达与被听见 C. 头脑风暴与灵感碰撞 D. 明确行动项与责任人
你最想避免的团队气质：A. 拍脑袋与不讲理 B. 冷战、内耗、互不信任 C. 只讲KPI忽视体验 D. 空谈不落地
当压力上来，你更常见表现：A. 变得挑剔、追求正确 B. 变得敏感、担心关系 C. 变得分心、想逃到兴趣里 D. 变得急躁、只想快点搞定
你遇到冲突时更倾向：A. 用事实与逻辑澄清 B. 先安抚情绪再谈事 C. 先冷静观察、找新角度 D. 直接谈条件与方案
你最害怕的失败类型：A. 推理错误导致重大损失 B. 伤害到别人或失去信任 C. 作品平庸或被误解 D. 错过机会或被竞争超越
你处理不确定性更像：A. 收集信息减少变量 B. 拉上相关人一起对齐 C. 允许模糊，先感受方向 D. 边走边调整，先动起来
你解决问题的常用起点：A. 找根因与关键约束 B. 找关键人物与共识点 C. 找突破口与替代方案 D. 找最短路径与资源
当计划被打乱，你会：A. 重新建模并改计划 B. 先通知相关人并协调 C. 顺势改成更有趣的方案 D. 立即做应急动作止损
你被批评时最在意：A. 是否指出真实问题 B. 语气是否尊重 C. 是否理解你的表达意图 D. 是否给出改进方向
你面对高压项目更需要：A. 明确的质量标准 B. 团队支持与情绪稳定 C. 灵活空间与创造出口 D. 快速决策与资源保障
你处理“烂摊子”更擅长：A. 梳理结构与重建规则 B. 修复关系与稳定人心 C. 重新包装与提升体验 D. 砍掉无效项快速交付
当你心态崩了，最能让你恢复的是：A. 独处复盘并找回掌控 B. 与信任的人聊一聊 C. 换个环境获取灵感 D. 做成一个小成果提气
你更容易在什么时候爆发潜能：A. 难题需要深度思考时 B. 有人需要你站出来时 C. 有创作灵感涌现时 D. 有明确竞争或期限时
你在压力下最可能犯的错：A. 过度分析拖延 B. 过度顾及他人忽视自己 C. 过度理想化缺乏落地 D. 过度冒进忽略风险
你更喜欢的风险管理方式：A. 建立预案与校验机制 B. 先确认对人影响最小 C. 留出探索空间不一次定死 D. 用里程碑快速验证
遇到意见不统一，你通常：A. 争取定义标准与边界 B. 促成大家互相理解 C. 提供一个第三种叙事 D. 让试点结果说话
当资源不足，你会优先：A. 重新排序优先级与范围 B. 寻求协作与互助 C. 用创意降低成本 D. 找赞助、拉资源或谈条件
你在群体中的自然角色更像：A. 规则制定者/质量把关 B. 氛围维护者/关怀者 C. 灵感提供者/表达者 D. 推动者/整合者
你更喜欢与人建立连接的方式：A. 因专业讨论而互相尊重 B. 因真诚关心而变亲近 C. 因共同审美或兴趣共鸣 D. 因共同目标而快速结盟
你更擅长说服别人靠：A. 逻辑与证据 B. 同理与信任 C. 故事与愿景 D. 利益与可行性
你更享受的社交场景：A. 小范围深度讨论 B. 一对一走心交流 C. 多人创意碰撞 D. 商务场合快速拓展
你对“领导”的理解更接近：A. 定方向与标准 B. 让人愿意跟随 C. 激发创造与想象 D. 拿结果与承担责任
你更容易被谁吸引合作：A. 思想清晰、能提升你的人 B. 温暖靠谱、会照顾团队的人 C. 有品味、有表达张力的人 D. 有野心、能做大事的人
你对陌生人更常用的开启方式：A. 从话题与观点切入 B. 从关心与礼貌切入 C. 从幽默与分享切入 D. 从需求与机会切入
当你要给反馈时，你更倾向：A. 直接指出问题与标准 B. 先肯定再提出建议 C. 用比喻或体验描述 D. 结论先行并给动作项
别人向你求助，你最常提供：A. 诊断问题的框架 B. 情绪支持与陪伴 C. 视角转换与灵感 D. 资源路径与推进策略
你更愿意维护的人际网络：A. 专业圈子与同行 B. 长期朋友与深关系 C. 同好社群与创作圈 D. 合作伙伴与资源方
你更容易在什么时候“被看见”：A. 解决了别人解决不了的难题 B. 让团队更团结更安心 C. 做出了让人惊艳的表达 D. 把项目从0到1推起来
你更习惯的协作方式：A. 分工明确、接口清晰 B. 随时沟通、相互照应 C. 允许分叉探索再汇合 D. 先对齐目标然后冲刺
你在团队里最想避免被贴的标签：A. 不懂业务只会理论 B. 情绪化或太软弱 C. 不靠谱或不接地气 D. 太功利或太强势
你更能接受哪种沟通风格：A. 简洁、精准、少废话 B. 温和、有耐心、有回应 C. 有趣、开放、带想象力 D. 高效、果断、直奔主题
你在长期关系里更看重：A. 共同成长与互相启发 B. 安全感与相互支持 C. 彼此欣赏与精神共鸣 D. 一起做事与共同目标
让你最有成就感的“产出”是：A. 一套可复用的方法/系统 B. 一个人因为你而改变 C. 一件能代表你的作品 D. 一个能持续增长的业务
你更适合的岗位评价体系：A. 质量、准确率、可靠性 B. 口碑、满意度、留存 C. 传播、审美、影响力 D. 收入、转化、增长率
你更喜欢的职业方向更像：A. 研究/工程/分析 B. 教育/咨询/人力与服务 C. 内容/设计/品牌与艺术 D. 产品/运营/销售与创业
如果让你选“解决对象”，你更想服务：A. 系统与技术问题 B. 个人与群体需求 C. 文化与体验表达 D. 市场与商业机会
你更能长期坚持的一类工作是：A. 不断优化与迭代一个体系 B. 长期陪伴与培养他人 C. 不断尝试新的表达方式 D. 不断开拓新场景与新增长
你更喜欢的任务输入是：A. 明确问题+边界条件 B. 用户故事+真实痛点 C. 模糊命题+创作空间 D. 目标指标+资源限制
你更喜欢的职业路径：A. 专家路线越钻越深 B. 以影响人和组织为中心 C. 以作品与个人品牌为中心 D. 以项目与商业体量为中心
你对“重复性工作”的态度：A. 可以，只要能优化成流程 B. 可以，只要对人有意义 C. 不行，会迅速消耗热情 D. 可以，但要能带来收益
你更适合的决策场景：A. 需要严谨评估风险 B. 需要兼顾多方感受 C. 需要提出新概念与方向 D. 需要快速拍板与试错
你更喜欢的客户/用户类型：A. 专业度高、讲逻辑 B. 重视关系、愿意沟通 C. 追求体验、愿意尝鲜 D. 目标明确、讲结果
你最像哪种“工作驱动器”：A. 逻辑一致性 B. 情感连接 C. 灵感与审美 D. 目标与收益
你更愿意把天赋用在：A. 搭建工具与基础设施 B. 做沟通桥梁与服务 C. 做内容叙事与体验 D. 做增长策略与商业谈判
你更适合的行业气质：A. 技术密集与高标准 B. 以人为本与强服务 C. 文化创意与体验驱动 D. 商业竞争与快节奏
如果只能选一种“影响世界”的方式：A. 提升效率与正确性 B. 增加理解与善意 C. 激发想象与审美 D. 推动资源流动与机会
你最希望未来别人提到你时说：A. “非常专业，解决问题很强” B. “让人安心，值得信任” C. “很有才华，有独特表达” D. “很能成事，带着大家做大”`;

        // 格式化题库
        const finalQuizQuestions = rawQuestions.trim().split('\n').map(line => {
            const [q, opts] = line.split('：');
            if(!opts) return null;
            const options = opts.split(/[A-D]\. /).filter(Boolean).map(s => s.trim());
            return { q, options };
        }).filter(Boolean);

        let currentIndex = 0;
        let userScores = { A: 0, B: 0, C: 0, D: 0 }; // 对应: 逻辑、共情、创意、执行

        // UI 控制
        function showPage(pageId) {
            const pages = ['page-start', 'page-quiz', 'page-loading', 'page-result'];
            pages.forEach(id => {
                const el = document.getElementById(id);
                if (id === pageId) {
                    el.classList.remove('hidden');
                    el.style.animation = 'none';
                    el.offsetHeight; 
                    el.style.animation = null; 
                    el.classList.add('fade-in');
                } else {
                    el.classList.add('hidden');
                    el.classList.remove('fade-in');
                }
            });
            document.getElementById('app-container').scrollTo({ top: 0, behavior: 'smooth' });
        }

        function startQuiz() {
            currentIndex = 0;
            userScores = { A: 0, B: 0, C: 0, D: 0 };
            showPage('page-quiz');
            renderQuestion();
        }

        function renderQuestion() {
            const qData = finalQuizQuestions[currentIndex];
            const qContainer = document.getElementById('question-container');
            
            qContainer.classList.remove('slide-in');
            void qContainer.offsetWidth; 
            qContainer.classList.add('slide-in');

            document.getElementById('question-text').innerText = `Q${currentIndex + 1}. ${qData.q}`;
            document.getElementById('progress-text').innerHTML = `${currentIndex + 1} <span class="text-slate-400 text-xs font-normal">/ 90</span>`;
            document.getElementById('progress-bar').style.width = `${((currentIndex + 1) / 90) * 100}%`;
            
            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = ''; 

            const optionKeys = ['A', 'B', 'C', 'D'];
            qData.options.forEach((opt, idx) => {
                const btn = document.createElement('button');
                const letter = optionKeys[idx]; 
                btn.className = "w-full text-left p-4 rounded-2xl bg-white border-2 border-slate-100 hover:border-indigo-400 hover:bg-indigo-50/50 transition-all text-slate-700 text-sm font-medium shadow-[0_2px_10px_rgba(0,0,0,0.02)] active:scale-[0.98] flex items-center group";
                btn.innerHTML = `
                    <span class="w-8 h-8 rounded-full bg-slate-100 text-slate-500 flex items-center justify-center mr-3 font-bold group-hover:bg-indigo-100 group-hover:text-indigo-600 transition-colors flex-shrink-0">${letter}</span>
                    <span class="leading-relaxed">${opt}</span>
                `;
                btn.onclick = () => handleAnswer(btn, letter);
                optionsContainer.appendChild(btn);
            });
        }

        function handleAnswer(btnElement, choice) {
            // 记录得分
            userScores[choice]++;

            // 选中效果
            btnElement.classList.remove('border-slate-100', 'bg-white');
            btnElement.classList.add('border-indigo-500', 'bg-indigo-50', 'ring-2', 'ring-indigo-200');
            
            const options = document.getElementById('options-container');
            options.style.pointerEvents = 'none';

            setTimeout(() => {
                if (currentIndex < 89) {
                    currentIndex++;
                    renderQuestion();
                    options.style.pointerEvents = 'auto';
                } else {
                    showPage('page-loading');
                    simulateLoading();
                }
            }, 150); 
        }

        function simulateLoading() {
            const texts = ["正在交叉比对核心驱动力...", "计算底层性格逻辑...", "正在生成你的专属职业画像..."];
            let i = 0;
            const textEl = document.getElementById('loading-text');
            const interval = setInterval(() => {
                i++;
                if(i < texts.length) {
                    textEl.innerText = texts[i];
                }
            }, 800);

            setTimeout(() => {
                clearInterval(interval);
                calculateAndRenderResult();
                showPage('page-result');
            }, 2500);
        }

        // 根据得分生成动态结果
        function calculateAndRenderResult() {
            const maxScore = Math.max(userScores.A, userScores.B, userScores.C, userScores.D);
            let personaType = '';
            
            // 定义四种主要人格维度的数据集
            const profiles = {
                'A': {
                    title: "理性架构师", tags: ["#深度思考", "#战略远见", "#理性克制", "#系统构建"],
                    strengths: ["极其敏锐的系统洞察力：能够迅速从混沌中抽丝剥茧。", "剥离情绪的客观决策：高压下保持冷酷与理性。", "长线战略规划：擅长布大局，做规则的制定者。"],
                    weaknesses: ["容易陷入“智商优越”的傲慢，对效率低下缺乏耐心。", "重战略而轻视“人情世故”，忽略利益相关者的情绪价值。"],
                    advice: "你在逻辑分析与系统构建上得分极高，是天生的破局者。建议刻意练习“向下兼容”的能力，寻找互补搭档处理人际润滑。适合：架构师、战略咨询、数据分析。"
                },
                'B': {
                    title: "共情治愈者", tags: ["#温暖包容", "#情绪价值", "#赋能他人", "#人际枢纽"],
                    strengths: ["超强的情绪感知力：能精准捕捉他人的需求与痛点。", "天生的粘合剂：能在撕裂的团队中重建信任与凝聚力。", "利他驱动：在帮助他人成长中爆发出惊人的能量。"],
                    weaknesses: ["容易吸收他人的负能量，导致自我情绪内耗。", "在需要冷酷决策时过于优柔寡断，难以做“坏人”。"],
                    advice: "你具有卓越的同理心与人际天赋。但要注意划定能量边界，学会课题分离。你的价值在于连接与赋能，适合：人力资源、心理咨询、用户运营、教育培训。"
                },
                'C': {
                    title: "灵感创作者", tags: ["#自由灵魂", "#独特审美", "#不拘一格", "#体验至上"],
                    strengths: ["跳跃的创新思维：总能提供打破常规的第三方视角。", "极高的审美张力：能赋予冷冰冰的产品以灵魂和温度。", "敏锐的趋势直觉：能感知到市场未被满足的隐性需求。"],
                    weaknesses: ["极度厌恶重复与规则，容易对常规落地工作失去耐心。", "灵感驱动导致状态起伏大，产出不够稳定。"],
                    advice: "你是稀缺的内容生产者和美学引擎。建议寻找能帮你把概念落地的“执行者”搭档，让你的才华变成可交付的产品。适合：设计、策划、内容创作、艺术指导。"
                },
                'D': {
                    title: "铁腕实干家", tags: ["#结果导向", "#高效推进", "#铁血执行", "#商业直觉"],
                    strengths: ["极强的目标感：认准目标后能排除万难，拿到结果。", "优秀的资源整合力：能在极度匮乏的条件下调动资源。", "抗压与兜底：在混乱和危机中能迅速站出来稳定大局。"],
                    weaknesses: ["有时过于强势和只看KPI，容易给团队带来极强压迫感。", "缺乏耐心倾听不同意见，可能错过潜在的风险预警。"],
                    advice: "你拥有极强的商业嗅觉和落地能力。为了走得更远，需要学会“柔性管理”，激发团队的自驱力而非单靠压力驱动。适合：项目总监、创业者、销售负责人、运营操盘手。"
                }
            };

            if (maxScore === userScores.A) personaType = 'A';
            else if (maxScore === userScores.B) personaType = 'B';
            else if (maxScore === userScores.C) personaType = 'C';
            else personaType = 'D';

            const profile = profiles[personaType];

            // 渲染文字内容
            document.getElementById('dynamic-title').innerText = profile.title;
            document.getElementById('dynamic-tags').innerHTML = profile.tags.map(t => `<span class="px-3 py-1 bg-white border border-slate-200 text-slate-600 rounded-full text-xs font-bold shadow-sm">${t}</span>`).join('');
            document.getElementById('dynamic-strengths').innerHTML = profile.strengths.map(s => `<li class="flex items-start"><span class="mr-2 text-emerald-500">●</span> ${s}</li>`).join('');
            document.getElementById('dynamic-weaknesses').innerHTML = profile.weaknesses.map(s => `<li class="flex items-start"><span class="mr-2 text-amber-500">●</span> ${s}</li>`).join('');
            document.getElementById('dynamic-advice').innerText = profile.advice;

            // 渲染雷达图 (按 A, B, C, D 比例转换)
            const total = 90;
            const aPercent = (userScores.A / total) * 100 * 2.5; // 放大系数让图更好看
            const bPercent = (userScores.B / total) * 100 * 2.5;
            const cPercent = (userScores.C / total) * 100 * 2.5;
            const dPercent = (userScores.D / total) * 100 * 2.5;

            initRadarChart([
                Math.min(aPercent + 20, 100), // 逻辑洞察
                Math.min(dPercent + 15, 100), // 实操落地
                Math.min(cPercent + 10, 100), // 艺术创新
                Math.min(bPercent + 20, 100), // 共情社交
                Math.min(dPercent + 20, 100), // 商业领导
                Math.min(aPercent + 10, 100)  // 秩序统筹
            ]);
        }

        // 雷达图绘制
        function initRadarChart(dataArr) {
            const ctx = document.getElementById('radarChart').getContext('2d');
            if(window.myRadarChart) window.myRadarChart.destroy();

            window.myRadarChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['逻辑与架构', '执行与落地', '创意与审美', '共情与协作', '商业与领导', '秩序与推演'],
                    datasets: [{
                        data: dataArr, 
                        backgroundColor: 'rgba(99, 102, 241, 0.25)', 
                        borderColor: 'rgba(99, 102, 241, 1)',     
                        borderWidth: 2, pointRadius: 2,
                        pointBackgroundColor: 'rgba(255, 255, 255, 1)',
                        pointBorderColor: 'rgba(99, 102, 241, 1)',
                    }]
                },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    scales: {
                        r: {
                            angleLines: { color: 'rgba(0, 0, 0, 0.05)' },
                            grid: { color: 'rgba(0, 0, 0, 0.05)' }, 
                            pointLabels: { color: '#475569', font: { family: "'Noto Sans SC', sans-serif", size: 10, weight: '700' } },
                            ticks: { display: false, min: 0, max: 100 }
                        }
                    },
                    plugins: { legend: { display: false }, tooltip: { enabled: false } },
                    animation: { duration: 0 } // 取消动画以防截图时未渲染完
                }
            });
        }

        // ================= 一键保存直接下载逻辑 =================
        function saveReportAsImage() {
            const btn = document.getElementById('save-btn');
            const originalText = btn.innerHTML;
            btn.innerHTML = `<svg class="animate-spin w-5 h-5 mr-2" viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg> 正在生成完整海报...`;
            btn.disabled = true;

            const captureTarget = document.getElementById('page-result');

            // 1. 隐藏多余UI（顶部栏、底部按钮）
            document.getElementById('save-btn-container').style.display = 'none';
            document.getElementById('result-header').style.display = 'none';
            document.getElementById('bottom-padding').style.display = 'none';

            // 2. 保证图片能够完整截取：滚动回顶部，临时释放父级高度和滚动限制
            captureTarget.scrollTop = 0;
            const originalOverflow = captureTarget.style.overflow;
            const originalHeight = captureTarget.style.height;
            captureTarget.style.overflow = 'visible';
            captureTarget.style.height = 'auto';

            setTimeout(() => {
                html2canvas(captureTarget, {
                    scale: window.devicePixelRatio * 2, // 提高到2倍高清画质
                    useCORS: true,
                    backgroundColor: '#f8f9fc',
                    windowWidth: captureTarget.scrollWidth,
                    windowHeight: captureTarget.scrollHeight
                }).then(canvas => {
                    // 3. 恢复UI状态
                    captureTarget.style.overflow = originalOverflow;
                    captureTarget.style.height = originalHeight;
                    document.getElementById('save-btn-container').style.display = 'block';
                    document.getElementById('result-header').style.display = 'flex';
                    document.getElementById('bottom-padding').style.display = 'block';
                    
                    btn.innerHTML = originalText;
                    btn.disabled = false;

                    // 4. 创建 a 标签，直接触发系统级文件下载
                    const imgData = canvas.toDataURL('image/png');
                    const downloadLink = document.createElement('a');
                    downloadLink.href = imgData;
                    downloadLink.download = 'PsycheTalent_专属天赋测评报告.png';
                    document.body.appendChild(downloadLink);
                    downloadLink.click();
                    document.body.removeChild(downloadLink);

                    // 如果是在部分严格限制拦截下载的 App 内置浏览器（如微信/小红书），可能需要提示用户
                    setTimeout(() => {
                        alert('✅ 测评长图已为您自动下载保存！\n\n(提示: 如果相册内未出现，说明当前应用限制了自动下载功能。请点击右上角在浏览器/Safari中打开本测试即可正常保存)');
                    }, 600);

                }).catch(err => {
                    alert('生成海报失败，请稍后再试。');
                    captureTarget.style.overflow = originalOverflow;
                    captureTarget.style.height = originalHeight;
                    document.getElementById('save-btn-container').style.display = 'block';
                    document.getElementById('result-header').style.display = 'flex';
                    document.getElementById('bottom-padding').style.display = 'block';
                    btn.innerHTML = originalText;
                    btn.disabled = false;
                });
            }, 300); // 留出渲染时间
        }
    </script>
</body>
</html>
