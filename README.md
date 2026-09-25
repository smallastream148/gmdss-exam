# GMDSS · 整合讲评

> GMDSS(全球海上遇险与安全系统)英语听力与会话整合讲评系统

## 🎯 项目内容

| 模块 | 内容 | 题量 |
|---|---|---|
| 知识题库 | `GMDSS英语阅读_知识点题库.html` | 827 题 |
| 听力与会话 | `app.html` | 47 口述 + 84 问答 |
| 信号题 | (在 app.html 内) | 200 题 |
| 码语读法 | (在 app.html 内) | 38 个 |

## 📁 目录结构

```
.
├── index.html                整合入口页(tab 切换)
├── app.html                  听力与会话讲评
├── GMDSS英语阅读_知识点题库.html   知识题库
├── audio/                    所有音频文件(449 个 mp3)
└── data/                     题目数据 json(5 个)
```

## 🚀 部署

详见 [DEPLOY.md](./DEPLOY.md)

## ✨ 功能特性

- **海事规范 TTS**:GuyNeural 英文男声,语速 -15%
  - 呼号转 ICAO 码语(如 BSJXR → Bravo Sierra Juliett X-ray Romeo)
  - 数字逐位读(00→zero zero)
  - SECURITE 转法语发音(say-cure-ee-tay)
  - MV → motor vessel
  - 经纬度 → "X degrees Y minutes N/S/E/W"
- **音频控制**:暂停/继续/停止/重播、进度条
- **划词翻译**:选中文字即时翻译
- **字号缩放**:50%–400%
- **全局搜索**

## 📊 题库分类

- **信号题**(200 题):20 组一套 ×10 套
- **码语**(38 个):Alfa–Zulu 全码语发音练习
- **口述题**(47 题):Mayday、PAN-PAN、SECURITE、遇险、紧急、安全呼叫
- **问答题**(84 题):覆盖 GMDSS 设备、海区、AIS-SART、EPIRB、海岸电台等

## 🛠 本地预览

```bash
# 直接打开(任一静态服务器)
python -m http.server 8000
# 或
npx serve .
```

浏览器访问 `http://localhost:8000`

## 📜 许可

内部教学使用。