import React, { useState, useEffect, useRef } from 'react';

// 读取环境变量中的 API 密钥（本地用 .env.local，线上用 Vercel 环境变量）
const API_KEY = process.env.NEXT_PUBLIC_API_KEY as string;
const API_URL = 'https://api.siliconflow.cn/v1/chat/completions';

// 获取随机吵架开场话术的函数
async function fetchRandomPhrases(scene: string, tone: number): Promise<string[]> {
  // 构造prompt，要求AI生成3个适合当前场景和语气强度的经典吵架开场
  const prompt = `请根据以下要求，生成3个适合"${scene}"场景，语气强度为${tone}（1为最温和，10为最强硬）的中文吵架开场白，每个开场白用一行分隔，不要编号，不要解释。`;

  const res = await fetch(API_URL, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${API_KEY}`,
    },
    body: JSON.stringify({
      model: 'deepseek-ai/DeepSeek-R1-Distill-Qwen-7B',
      messages: [
        { role: 'system', content: '你是一个善于生成吵架话术的AI助手。' },
        { role: 'user', content: prompt },
      ],
      max_tokens: 256,
      temperature: 1.0,
    }),
  });
  if (!res.ok) throw new Error('API请求失败');
  const data = await res.json();
  // 解析AI回复，按行分割
  const text = data.choices?.[0]?.message?.content || '';
  return text.split('\n').map(s => s.trim()).filter(Boolean);
}

// 获取吵架回复的函数（支持多轮上下文）
async function fetchAiRepliesWithContext(context: {role: string, content: string}[], scene: string, tone: number): Promise<string[]> {
  // 构造系统提示
  const systemPrompt = `你是一个善于生成吵架回击的AI助手。当前场景：${scene}，语气强度：${tone}（1为最温和，10为最强硬）。`;
  const messages = [
    { role: 'system', content: systemPrompt },
    ...context,
    { role: 'user', content: '请你生成3条精彩的中文吵架回击，每条回复用一行分隔，不要编号，不要解释。' }
  ];
  const res = await fetch(API_URL, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${API_KEY}`,
    },
    body: JSON.stringify({
      model: 'deepseek-ai/DeepSeek-R1-Distill-Qwen-7B',
      messages,
      max_tokens: 256,
      temperature: 1.0,
    }),
  });
  if (!res.ok) throw new Error('API请求失败');
  const data = await res.json();
  const text = data.choices?.[0]?.message?.content || '';
  return text.split('\n').map(s => s.trim()).filter(Boolean);
}

// 表情包关键词映射
const emojiMap = {
  '生气': '😡',
  '开心': '😄',
  '无语': '🙄',
  '爆炸': '💥',
  // ...可扩展
};
function renderWithEmoji(text: string) {
  let result = text;
  Object.entries(emojiMap).forEach(([key, emoji]) => {
    result = result.replaceAll(key, emoji);
  });
  return result;
}

// 历史记录本地存储
function saveHistory(history: any[]) {
  localStorage.setItem('argue_history', JSON.stringify(history));
}
function loadHistory(): any[] {
  try {
    return JSON.parse(localStorage.getItem('argue_history') || '[]');
  } catch {
    return [];
  }
}

// 主页面组件
export default function Home() {
  // 输入框内容
  const [input, setInput] = useState('');
  // 场景选择
  const [scene, setScene] = useState('情侣');
  // 语气强度（1-10）
  const [tone, setTone] = useState(1);
  // 随机话术（由AI生成，初始为空）
  const [randomPhrases, setRandomPhrases] = useState<string[]>([]);
  // AI回复（初始为空）
  const [aiReplies, setAiReplies] = useState<string[]>([]);
  // 是否显示历史记录弹窗
  const [showHistory, setShowHistory] = useState(false);
  // 随机话术加载状态
  const [loadingRandom, setLoadingRandom] = useState(false);
  // 吵架回复加载状态
  const [loadingReply, setLoadingReply] = useState(false);
  // 打字机动画：每条回复当前显示到第几个字
  const [repliesDisplay, setRepliesDisplay] = useState<number[]>([]);
  // 动画定时器引用（用number[]代替NodeJS.Timeout[]，兼容类型）
  const timersRef = useRef<number[]>([]);
  // 多轮对话上下文
  const [context, setContext] = useState<{role: string, content: string}[]>([]);
  // 是否处于多轮对话中
  const [inMultiTurn, setInMultiTurn] = useState(false);
  // 历史记录
  const [history, setHistory] = useState<any[]>(loadHistory());

  // 处理AI回复打字机动画
  useEffect(() => {
    // 如果没有AI回复，或回复为空，直接返回
    if (!aiReplies.length) {
      setRepliesDisplay([]);
      return;
    }
    // 初始化每条回复显示0个字
    setRepliesDisplay(Array(aiReplies.length).fill(0));
    // 清除上一次的定时器
    timersRef.current.forEach(timer => clearInterval(timer));
    timersRef.current = [];
    // 为每条回复设置打字机动画
    aiReplies.forEach((reply, idx) => {
      let i = 0;
      const timer = window.setInterval(() => {
        setRepliesDisplay(prev => {
          const next = [...prev];
          // 每次+1，直到显示完整
          if (next[idx] < reply.length) {
            next[idx] = next[idx] + 1;
          }
          return next;
        });
        i++;
        if (i >= reply.length) {
          clearInterval(timer);
        }
      }, 30 + Math.random() * 40); // 每个字间隔30-70ms，略带随机感
      timersRef.current.push(timer);
    });
    // 卸载时清理定时器
    return () => {
      timersRef.current.forEach(timer => clearInterval(timer));
      timersRef.current = [];
    };
  }, [aiReplies]);

  // 点击"随机话术"按钮事件
  const handleRandomPhrases = async () => {
    setLoadingRandom(true);
    setRandomPhrases([]);
    try {
      const phrases = await fetchRandomPhrases(scene, tone);
      setRandomPhrases(phrases.slice(0, 3)); // 只取前3条
    } catch (e) {
      setRandomPhrases(['获取失败，请稍后重试']);
    } finally {
      setLoadingRandom(false);
    }
  };

  // 点击"开始吵架"按钮事件（单轮或多轮第一轮）
  const handleAiReply = async () => {
    if (!input.trim()) return;
    setLoadingReply(true);
    setAiReplies([]);
    let newContext = [...context];
    if (!inMultiTurn) newContext = [];
    newContext.push({ role: 'user', content: input });
    setContext(newContext);
    try {
      const replies = await fetchAiRepliesWithContext(newContext, scene, tone);
      setAiReplies(replies.slice(0, 3));
      setInMultiTurn(true);
    } catch (e) {
      setAiReplies(['获取失败，请稍后重试']);
    } finally {
      setLoadingReply(false);
    }
  };

  // 用户点击AI回复，进入下一轮
  const handleSelectReply = (reply: string) => {
    // 把AI回复加入上下文
    const newContext = [...context, { role: 'assistant', content: reply }];
    setContext(newContext);
    setInput(''); // 清空输入框，等待用户输入对方新一句
    setAiReplies([]); // 清空AI回复，准备下一轮
    setRepliesDisplay([]);
  };

  // 结束对话并保存历史
  const handleEndDialog = () => {
    if (context.length > 0) {
      const newHistory = [
        { timestamp: Date.now(), context: [...context] },
        ...history
      ].slice(0, 20); // 最多保存20条
      setHistory(newHistory);
      saveHistory(newHistory);
    }
    setContext([]);
    setInMultiTurn(false);
    setAiReplies([]);
    setRepliesDisplay([]);
    setInput('');
  };

  // 清空历史
  const handleClearHistory = () => {
    setHistory([]);
    saveHistory([]);
  };

  return (
    <div style={{ fontFamily: 'WeChatFont, sans-serif', background: '#f7f7f7', minHeight: '100vh' }}>
      {/* 顶部导航栏 */}
      <header style={{ background: '#09bb07', color: '#fff', padding: '16px 0', borderRadius: '0 0 16px 16px', boxShadow: '0 2px 8px #e0e0e0' }}>
        <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'space-between', maxWidth: 480, margin: '0 auto', padding: '0 16px' }}>
          {/* Logo 可替换为图片 */}
          <span style={{ fontWeight: 'bold', fontSize: 22 }}>吵架包赢</span>
          {/* 历史记录按钮 */}
          <button onClick={() => setShowHistory(true)} style={{ background: 'rgba(255,255,255,0.2)', color: '#fff', border: 'none', borderRadius: 8, padding: '6px 14px', cursor: 'pointer' }}>历史记录</button>
        </div>
      </header>

      {/* 主体内容区域 */}
      <main style={{ maxWidth: 480, margin: '0 auto', padding: '24px 8px 80px 8px' }}>
        {/* 多轮对话历史展示 */}
        {inMultiTurn && context.length > 0 && (
          <section style={{ background: '#fff', borderRadius: 12, boxShadow: '0 2px 8px #e0e0e0', padding: 12, marginBottom: 16, fontSize: 15 }}>
            <div style={{ marginBottom: 8, color: '#09bb07', fontWeight: 'bold' }}>当前对话</div>
            {context.map((msg, idx) => (
              <div key={idx} style={{ marginBottom: 4, textAlign: msg.role === 'user' ? 'right' : 'left' }}>
                <span style={{ background: msg.role === 'user' ? '#e6f9ea' : '#f0f0f0', borderRadius: 8, padding: '4px 10px', display: 'inline-block' }}>
                  {renderWithEmoji(msg.content)}
                </span>
              </div>
            ))}
            <button onClick={handleEndDialog} style={{ marginTop: 8, background: '#f0f0f0', color: '#888', border: 'none', borderRadius: 8, padding: '4px 12px', cursor: 'pointer', fontSize: 14 }}>结束对话并保存</button>
          </section>
        )}
        {/* 输入区 */}
        <section style={{ background: '#fff', borderRadius: 16, boxShadow: '0 2px 8px #e0e0e0', padding: 16, marginBottom: 24 }}>
          {/* 输入框 */}
          <label style={{ fontWeight: 'bold', fontSize: 16 }}>对方的话：</label>
          <textarea
            value={input}
            onChange={e => setInput(e.target.value)}
            placeholder="请输入对方说的话..."
            style={{ width: '100%', minHeight: 60, borderRadius: 8, border: '1px solid #e0e0e0', padding: 8, marginTop: 8, marginBottom: 16, fontSize: 15 }}
          />

          {/* 场景选择 */}
          <div style={{ marginBottom: 16 }}>
            <span style={{ fontWeight: 'bold', marginRight: 8 }}>场景：</span>
            {['情侣', '长辈', '朋友', '陌生人'].map(s => (
              <button
                key={s}
                onClick={() => setScene(s)}
                style={{
                  background: scene === s ? '#09bb07' : '#f0f0f0',
                  color: scene === s ? '#fff' : '#333',
                  border: 'none',
                  borderRadius: 8,
                  padding: '6px 14px',
                  marginRight: 8,
                  cursor: 'pointer',
                  fontWeight: scene === s ? 'bold' : 'normal',
                }}
              >
                {s}
              </button>
            ))}
          </div>

          {/* 语气强度滑动条 */}
          <div style={{ display: 'flex', alignItems: 'center', marginBottom: 16 }}>
            {/* 最左端小花图标 */}
            <span role="img" aria-label="温和" style={{ marginRight: 8 }}>🌸</span>
            <input
              type="range"
              min={1}
              max={10}
              value={tone}
              onChange={e => setTone(Number(e.target.value))}
              style={{ flex: 1 }}
            />
            {/* 最右端核弹爆炸图标 */}
            <span role="img" aria-label="强硬" style={{ marginLeft: 8 }}>💥</span>
            <span style={{ marginLeft: 8, fontWeight: 'bold', color: '#09bb07' }}>{tone}</span>
          </div>

          {/* 随机话术按钮 */}
          <button
            style={{ background: '#e6f9ea', color: '#09bb07', border: 'none', borderRadius: 8, padding: '8px 16px', fontWeight: 'bold', cursor: 'pointer', marginBottom: 12 }}
            onClick={handleRandomPhrases}
            disabled={loadingRandom}
          >
            {loadingRandom ? '生成中...' : '不知道怎么开口？点我随机生成3个经典吵架开场'}
          </button>
          {/* 随机话术展示区 */}
          <div style={{ marginTop: 8, marginBottom: 8 }}>
            {/* 展示AI生成的随机话术，点击可填入输入框 */}
            {randomPhrases.map((phrase, idx) => (
              <span
                key={idx}
                onClick={() => setInput(phrase)}
                style={{ display: 'inline-block', background: '#f0f0f0', borderRadius: 8, padding: '4px 10px', marginRight: 8, marginBottom: 4, cursor: 'pointer', fontSize: 14 }}
              >
                {phrase}
              </span>
            ))}
          </div>
        </section>

        {/* 生成按钮 */}
        <div style={{ textAlign: 'center', marginBottom: 24 }}>
          <button
            style={{ background: '#09bb07', color: '#fff', border: 'none', borderRadius: 12, padding: '12px 40px', fontWeight: 'bold', fontSize: 18, boxShadow: '0 2px 8px #e0e0e0', cursor: 'pointer' }}
            onClick={handleAiReply}
            disabled={loadingReply || !input.trim()}
          >
            {loadingReply ? '生成中...' : inMultiTurn ? '继续吵架' : '开始吵架'}
          </button>
        </div>

        {/* AI回复展示区 */}
        <section style={{ minHeight: 120, marginBottom: 24 }}>
          {/* TODO: 展示AI生成的3条回复，带打字机动画和表情包 */}
          {aiReplies.map((reply, idx) => (
            <div
              key={idx}
              style={{ background: '#fff', borderRadius: 12, boxShadow: '0 2px 8px #e0e0e0', padding: 14, marginBottom: 12, fontSize: 16, cursor: 'pointer', position: 'relative', minHeight: 32 }}
              onClick={() => repliesDisplay[idx] === reply.length && inMultiTurn && handleSelectReply(reply)}
              title={inMultiTurn ? '点击选择这条回复进入下一轮' : ''}
            >
              {typeof repliesDisplay[idx] === 'number' && repliesDisplay[idx] < reply.length
                ? renderWithEmoji(reply.slice(0, repliesDisplay[idx])) + '▋'
                : renderWithEmoji(reply)}
            </div>
          ))}
        </section>
      </main>

      {/* 历史记录弹窗 */}
      {showHistory && (
        <div style={{ position: 'fixed', top: 0, left: 0, width: '100vw', height: '100vh', background: 'rgba(0,0,0,0.25)', zIndex: 1000, display: 'flex', alignItems: 'center', justifyContent: 'center' }}>
          <div style={{ background: '#fff', borderRadius: 16, maxWidth: 400, width: '90vw', maxHeight: '80vh', overflowY: 'auto', padding: 24, boxShadow: '0 4px 24px #888' }}>
            <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: 16 }}>
              <span style={{ fontWeight: 'bold', fontSize: 18 }}>历史记录</span>
              <button onClick={() => setShowHistory(false)} style={{ background: 'none', border: 'none', fontSize: 20, cursor: 'pointer' }}>✖️</button>
            </div>
            {history.length === 0 ? (
              <div style={{ color: '#888', textAlign: 'center' }}>暂无历史记录</div>
            ) : (
              <div>
                <button onClick={handleClearHistory} style={{ marginBottom: 8, background: '#f0f0f0', color: '#888', border: 'none', borderRadius: 8, padding: '4px 12px', cursor: 'pointer', fontSize: 14 }}>清空历史</button>
                {history.map((item, idx) => (
                  <div key={idx} style={{ marginBottom: 16, borderBottom: '1px solid #eee', paddingBottom: 8 }}>
                    <div style={{ color: '#09bb07', fontSize: 13, marginBottom: 4 }}>{new Date(item.timestamp).toLocaleString()}</div>
                    {item.context.map((msg: any, i: number) => (
                      <div key={i} style={{ marginBottom: 2, textAlign: msg.role === 'user' ? 'right' : 'left' }}>
                        <span style={{ background: msg.role === 'user' ? '#e6f9ea' : '#f0f0f0', borderRadius: 8, padding: '2px 8px', display: 'inline-block', fontSize: 13 }}>
                          {renderWithEmoji(msg.content)}
                        </span>
                      </div>
                    ))}
                  </div>
                ))}
              </div>
            )}
          </div>
        </div>
      )}
    </div>
  );
} 
