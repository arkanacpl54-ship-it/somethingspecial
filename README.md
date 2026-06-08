<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>GYUU AI</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Lora:ital,wght@0,400;0,500;1,400&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #07080f;
  --surface: #0f1120;
  --surface2: #161829;
  --border: #1f2238;
  --accent: #4fffb0;
  --accent2: #00c9ff;
  --accent3: #ff6ef7;
  --text: #e8eaf6;
  --muted: #5a5f80;
  --red: #ff4f6a;
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;overflow:hidden}
body{background:var(--bg);color:var(--text);font-family:'Lora',Georgia,serif;display:flex;flex-direction:column;height:100dvh;transition:background .3s}
body::before{content:'';position:fixed;inset:0;background:radial-gradient(ellipse 70% 50% at 15% 5%,rgba(79,255,176,.07) 0%,transparent 55%),radial-gradient(ellipse 60% 50% at 85% 90%,rgba(0,201,255,.07) 0%,transparent 55%),radial-gradient(ellipse 40% 30% at 70% 20%,rgba(255,110,247,.04) 0%,transparent 50%);pointer-events:none;z-index:0}
.header{position:relative;z-index:10;display:flex;align-items:center;justify-content:space-between;padding:10px 14px;border-bottom:1px solid var(--border);background:rgba(7,8,15,.97);backdrop-filter:blur(20px)}
.ai-info{display:flex;align-items:center;gap:10px}
.avatar-wrap{position:relative;width:40px;height:40px}
.avatar{width:40px;height:40px;border-radius:13px;background:var(--surface2);border:1.5px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:19px}
.online-dot{position:absolute;bottom:1px;right:1px;width:9px;height:9px;background:var(--accent);border-radius:50%;border:2px solid var(--bg);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.5;transform:scale(.8)}}
.ai-name{font-family:'Syne',sans-serif;font-weight:800;font-size:14px;color:var(--text)}
.ai-meta{font-size:9px;color:var(--accent);font-family:'Syne',sans-serif;font-weight:600;letter-spacing:1px;text-transform:uppercase}
.hbtns{display:flex;gap:5px}
.hbtn{width:34px;height:34px;border-radius:10px;border:1px solid var(--border);background:var(--surface);color:var(--muted);font-size:14px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .2s;flex-shrink:0;position:relative}
.hbtn:hover,.hbtn.on{border-color:var(--accent);color:var(--accent)}
.hbtn-main{padding:0 10px;width:auto;font-family:'Syne',sans-serif;font-size:11px;font-weight:700;color:var(--accent);border-color:rgba(79,255,176,.3);background:rgba(79,255,176,.05);white-space:nowrap}
.hbtn-main:hover{background:rgba(79,255,176,.12)}
.badge{position:absolute;top:-4px;right:-4px;width:16px;height:16px;background:var(--red);border-radius:50%;font-size:9px;color:#fff;font-family:'Syne',sans-serif;font-weight:700;display:flex;align-items:center;justify-content:center;border:2px solid var(--bg)}
.tabs{display:flex;gap:4px;padding:8px 14px 0;border-bottom:1px solid var(--border);background:rgba(7,8,15,.9);position:relative;z-index:9;overflow-x:auto;scrollbar-width:none}
.tabs::-webkit-scrollbar{display:none}
.tab{padding:7px 12px;border-radius:10px 10px 0 0;font-family:'Syne',sans-serif;font-size:11px;font-weight:600;color:var(--muted);cursor:pointer;transition:all .2s;white-space:nowrap;border:1px solid transparent;border-bottom:none;flex-shrink:0}
.tab.active{color:var(--accent);background:var(--surface);border-color:var(--border)}
.tab:hover:not(.active){color:var(--text)}
.panel{display:none;flex:1;overflow:hidden;flex-direction:column;position:relative;z-index:1}
.panel.active{display:flex}
.messages{flex:1;overflow-y:auto;padding:14px 12px 6px;display:flex;flex-direction:column;gap:10px;scroll-behavior:smooth}
.messages::-webkit-scrollbar{width:3px}
.messages::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}
.empty-state{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:24px 18px;animation:fadeUp .6s ease}
.empty-avatar{font-size:48px;margin-bottom:12px;filter:drop-shadow(0 0 20px rgba(79,255,176,.4));animation:float 3s ease-in-out infinite}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
.empty-title{font-family:'Syne',sans-serif;font-size:20px;font-weight:800;color:var(--text);margin-bottom:5px}
.empty-sub{font-size:12px;color:var(--muted);line-height:1.7;max-width:260px;margin-bottom:14px}
.chips{display:flex;flex-wrap:wrap;gap:6px;justify-content:center}
.chip{padding:6px 12px;border-radius:20px;border:1px solid var(--border);background:var(--surface);color:var(--muted);font-size:11px;font-family:'Syne',sans-serif;cursor:pointer;transition:all .2s}
.chip:hover{border-color:var(--accent);color:var(--accent);background:rgba(79,255,176,.05)}
.msg-row{display:flex;gap:7px;align-items:flex-end;animation:fadeUp .3s ease}
.msg-row.user{flex-direction:row-reverse}
.msg-av{width:28px;height:28px;border-radius:9px;background:var(--surface2);display:flex;align-items:center;justify-content:center;font-size:13px;flex-shrink:0;border:1px solid var(--border)}
.msg-wrap{display:flex;flex-direction:column;max-width:78%;gap:3px}
.msg-row.user .msg-wrap{align-items:flex-end}
.bubble{padding:10px 13px;border-radius:16px;font-size:13.5px;line-height:1.65;word-break:break-word}
.bubble.ai{background:var(--surface);border:1px solid var(--border);border-bottom-left-radius:4px;color:var(--text)}
.bubble.user{background:linear-gradient(135deg,#00c9ff,#4fffb0);border-bottom-right-radius:4px;color:#07080f;font-weight:500}
.bubble a{color:var(--accent2);text-decoration:underline}
.bubble strong{font-weight:700}
.bubble em{font-style:italic;opacity:.85}
.bubble h1,.bubble h2,.bubble h3{font-family:'Syne',sans-serif;margin:6px 0 3px;line-height:1.3}
.bubble h1{font-size:16px}.bubble h2{font-size:15px}.bubble h3{font-size:14px}
.bubble ul,.bubble ol{padding-left:18px;margin:4px 0}
.bubble li{margin:2px 0}
.bubble .code-block{background:var(--surface2);border:1px solid var(--border);border-radius:9px;margin:6px 0;overflow:hidden}
.bubble .code-header{display:flex;justify-content:space-between;align-items:center;padding:5px 10px;border-bottom:1px solid var(--border);font-family:'Syne',sans-serif;font-size:10px;color:var(--muted)}
.bubble .code-header button{background:none;border:none;color:var(--accent2);cursor:pointer;font-size:11px}
.bubble pre{padding:10px;overflow-x:auto;font-size:12px;line-height:1.5;font-family:monospace}
.bubble code.inline{background:var(--surface2);padding:1px 5px;border-radius:4px;font-size:11px;font-family:monospace}
.msg-time{font-size:9px;color:var(--muted);font-family:'Syne',sans-serif;padding:0 4px}
.msg-actions{display:flex;gap:4px;opacity:0;transition:opacity .2s}
.msg-row:hover .msg-actions{opacity:1}
.act-btn{width:24px;height:24px;border-radius:7px;border:1px solid var(--border);background:var(--surface2);color:var(--muted);font-size:11px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .15s}
.act-btn:hover{border-color:var(--accent);color:var(--accent)}
.quick-chips{display:flex;flex-wrap:wrap;gap:5px;padding:0 0 0 35px;animation:fadeUp .3s ease}
.q-chip{padding:5px 10px;border-radius:14px;border:1px solid rgba(79,255,176,.25);background:rgba(79,255,176,.05);color:var(--accent);font-size:11px;font-family:'Syne',sans-serif;cursor:pointer;transition:all .2s}
.q-chip:hover{background:rgba(79,255,176,.15)}
.img-bubble{max-width:70%;border-radius:16px;overflow:hidden;border:2px solid rgba(79,255,176,.25);border-bottom-right-radius:4px}
.img-bubble img{width:100%;max-width:220px;display:block}
.link-bubble{max-width:78%;padding:9px 12px;background:var(--surface2);border:1px solid rgba(0,201,255,.25);border-radius:13px;border-bottom-right-radius:4px;display:flex;align-items:center;gap:7px}
.link-url{font-size:11px;color:var(--accent2);font-family:'Syne',sans-serif;word-break:break-all}
.voice-bubble{max-width:78%;padding:9px 13px;background:linear-gradient(135deg,rgba(255,110,247,.15),rgba(0,201,255,.1));border:1px solid rgba(255,110,247,.2);border-radius:13px;border-bottom-right-radius:4px;display:flex;align-items:center;gap:9px;color:var(--text);font-size:12px;font-family:'Syne',sans-serif}
.typing{display:flex;align-items:center;gap:5px;padding:12px 15px;background:var(--surface);border:1px solid var(--border);border-radius:16px;border-bottom-left-radius:4px;width:fit-content}
.dot{width:6px;height:6px;border-radius:50%;background:var(--accent);animation:bounce 1.3s infinite}
.dot:nth-child(2){animation-delay:.2s;background:var(--accent2)}
.dot:nth-child(3){animation-delay:.4s;background:var(--accent3)}
@keyframes bounce{0%,60%,100%{transform:translateY(0);opacity:.4}30%{transform:translateY(-6px);opacity:1}}
.input-area{position:relative;z-index:10;padding:7px 12px 10px;border-top:1px solid var(--border);background:rgba(7,8,15,.97);backdrop-filter:blur(20px)}
.img-preview-strip{display:none;align-items:center;gap:8px;padding:5px 2px;margin-bottom:5px}
.img-preview-strip.show{display:flex}
.preview-thumb{width:46px;height:46px;border-radius:9px;object-fit:cover;border:2px solid var(--accent)}
.remove-preview{width:18px;height:18px;border-radius:50%;background:var(--red);border:none;color:#fff;font-size:10px;cursor:pointer;display:flex;align-items:center;justify-content:center;margin-left:-13px;margin-top:-30px;align-self:flex-start}
.preview-lbl{font-size:10px;color:var(--accent);font-family:'Syne',sans-serif}
.link-bar{display:none;align-items:center;gap:7px;padding:6px 10px;margin-bottom:5px;background:var(--surface2);border-radius:9px;border:1px solid rgba(0,201,255,.2)}
.link-bar.show{display:flex}
.link-bar-text{font-size:10px;color:var(--accent2);font-family:'Syne',sans-serif;flex:1;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.rm-link{background:none;border:none;color:var(--muted);cursor:pointer;font-size:13px}
.input-wrap{display:flex;gap:6px;align-items:flex-end;background:var(--surface);border:1.5px solid var(--border);border-radius:15px;padding:6px 6px 6px 5px;transition:border-color .2s}
.input-wrap:focus-within{border-color:rgba(79,255,176,.4);box-shadow:0 0 0 3px rgba(79,255,176,.05)}
.attach-row{display:flex;gap:3px;align-items:flex-end;flex-shrink:0}
.abtn{width:32px;height:32px;border-radius:9px;border:1px solid var(--border);background:var(--surface2);color:var(--muted);font-size:14px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .2s}
.abtn:hover,.abtn.on{border-color:var(--accent);color:var(--accent)}
.abtn.recording{border-color:var(--red);color:var(--red);animation:rec-pulse 1s infinite}
@keyframes rec-pulse{0%,100%{background:var(--surface2)}50%{background:rgba(255,79,106,.15)}}
#msgInput{flex:1;background:transparent;border:none;outline:none;color:var(--text);font-family:'Lora',Georgia,serif;font-size:13.5px;line-height:1.6;resize:none;max-height:100px;overflow-y:auto;padding:3px 0}
#msgInput::placeholder{color:var(--muted)}
#msgInput::-webkit-scrollbar{display:none}
.send-btn{width:36px;height:36px;border-radius:11px;border:none;background:linear-gradient(135deg,#00c9ff,#4fffb0);color:#07080f;font-size:15px;cursor:pointer;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:all .2s;opacity:.35;transform:scale(.88)}
.send-btn.on{opacity:1;transform:scale(1);box-shadow:0 4px 14px rgba(79,255,176,.3)}
.send-btn:active{transform:scale(.92)}
.input-footer{display:flex;align-items:center;justify-content:space-between;margin-top:4px;padding:0 2px}
.hint{font-size:9px;color:var(--border);font-family:'Syne',sans-serif;letter-spacing:.4px}
.char-count{font-size:9px;color:var(--muted);font-family:'Syne',sans-serif}
.credit{text-align:center;font-size:9px;color:var(--muted);margin-top:3px;font-family:'Syne',sans-serif;letter-spacing:.4px}
.search-overlay{position:fixed;inset:0;background:rgba(7,8,15,.95);z-index:150;display:flex;flex-direction:column;padding:16px;gap:10px;opacity:0;pointer-events:none;transition:opacity .25s;backdrop-filter:blur(10px)}
.search-overlay.open{opacity:1;pointer-events:all}
.search-header{display:flex;align-items:center;gap:10px}
.search-input{flex:1;background:var(--surface);border:1.5px solid var(--border);border-radius:12px;padding:10px 14px;color:var(--text);font-family:'Lora',serif;font-size:14px;outline:none}
.search-input:focus{border-color:var(--accent)}
.search-close{width:36px;height:36px;border-radius:10px;border:1px solid var(--border);background:var(--surface);color:var(--muted);font-size:15px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.search-results{overflow-y:auto;display:flex;flex-direction:column;gap:8px}
.search-result-item{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:12px;cursor:pointer;transition:border-color .2s}
.search-result-item:hover{border-color:var(--accent)}
.search-result-role{font-family:'Syne',sans-serif;font-size:10px;font-weight:600;color:var(--accent);margin-bottom:4px;text-transform:uppercase;letter-spacing:1px}
.search-result-text{font-size:13px;color:var(--text);line-height:1.5}
.search-result-text mark{background:rgba(79,255,176,.25);color:var(--accent);border-radius:3px;padding:0 2px}
.search-empty{text-align:center;padding:40px;color:var(--muted);font-size:13px}
.tools-grid{padding:14px;display:grid;grid-template-columns:1fr 1fr;gap:9px;overflow-y:auto}
.tool-card{background:var(--surface);border:1px solid var(--border);border-radius:15px;padding:14px;cursor:pointer;transition:all .25s;display:flex;flex-direction:column;gap:7px}
.tool-card:hover{border-color:var(--accent);background:rgba(79,255,176,.04);transform:translateY(-2px)}
.tool-card:active{transform:scale(.97)}
.tool-icon{font-size:26px}
.tool-name{font-family:'Syne',sans-serif;font-size:12px;font-weight:700;color:var(--text)}
.tool-desc{font-size:10px;color:var(--muted);line-height:1.5}
.memory-panel{padding:14px;overflow-y:auto;display:flex;flex-direction:column;gap:9px}
.mem-header{font-family:'Syne',sans-serif;font-size:10px;font-weight:600;color:var(--muted);letter-spacing:1px;text-transform:uppercase;margin-bottom:3px}
.mem-item{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:11px 13px;display:flex;justify-content:space-between;align-items:flex-start;gap:10px;animation:fadeUp .3s ease}
.mem-text{font-size:12px;color:var(--text);line-height:1.5;flex:1}
.mem-del{background:none;border:none;color:var(--muted);cursor:pointer;font-size:13px;flex-shrink:0;transition:color .2s}
.mem-del:hover{color:var(--red)}
.mem-empty{text-align:center;padding:36px 18px;color:var(--muted);font-size:12px}
.mem-add-btn{display:flex;align-items:center;justify-content:center;gap:7px;padding:11px;border-radius:12px;border:1px dashed var(--border);background:transparent;color:var(--muted);font-family:'Syne',sans-serif;font-size:12px;cursor:pointer;transition:all .2s;width:100%}
.mem-add-btn:hover{border-color:var(--accent);color:var(--accent)}
.stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:9px;margin-bottom:14px}
.stat-card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:12px;text-align:center}
.stat-num{font-family:'Syne',sans-serif;font-size:22px;font-weight:800;color:var(--accent)}
.stat-lbl{font-size:10px;color:var(--muted);font-family:'Syne',sans-serif;margin-top:3px}
.persona-panel{padding:14px;overflow-y:auto;display:flex;flex-direction:column;gap:11px}
.preset-grid{display:grid;grid-template-columns:1fr 1fr;gap:7px}
.preset-card{background:var(--surface);border:1.5px solid var(--border);border-radius:13px;padding:11px;cursor:pointer;transition:all .2s;text-align:center}
.preset-card:hover,.preset-card.active{border-color:var(--accent);background:rgba(79,255,176,.05)}
.preset-em{font-size:24px;margin-bottom:5px}
.preset-name{font-family:'Syne',sans-serif;font-size:11px;font-weight:700;color:var(--text)}
.preset-desc{font-size:9px;color:var(--muted);margin-top:2px;line-height:1.4}
.theme-row{display:flex;gap:8px;flex-wrap:wrap}
.theme-dot{width:32px;height:32px;border-radius:50%;cursor:pointer;border:3px solid transparent;transition:all .2s;flex-shrink:0}
.theme-dot.active,.theme-dot:hover{border-color:var(--text);transform:scale(1.1)}
.modal-overlay{position:fixed;inset:0;background:rgba(7,8,15,.85);z-index:200;display:flex;align-items:flex-end;justify-content:center;backdrop-filter:blur(8px);opacity:0;pointer-events:none;transition:opacity .3s}
.modal-overlay.open{opacity:1;pointer-events:all}
.modal{background:var(--surface);border:1px solid var(--border);border-radius:24px 24px 0 0;padding:20px 16px 28px;width:100%;max-width:500px;transform:translateY(100%);transition:transform .35s cubic-bezier(.34,1.56,.64,1);max-height:90dvh;overflow-y:auto}
.modal-overlay.open .modal{transform:translateY(0)}
.modal-handle{width:36px;height:4px;background:var(--border);border-radius:2px;margin:0 auto 16px}
.modal-title{font-family:'Syne',sans-serif;font-weight:800;font-size:18px;color:var(--text);margin-bottom:16px}
.flabel{font-family:'Syne',sans-serif;font-size:10px;font-weight:600;color:var(--muted);letter-spacing:1.2px;text-transform:uppercase;margin-bottom:6px;display:block}
.finput,.ftextarea{width:100%;background:var(--surface2);border:1px solid var(--border);border-radius:11px;padding:10px 12px;color:var(--text);font-family:'Lora',Georgia,serif;font-size:13px;outline:none;margin-bottom:13px;transition:border-color .2s;display:block}
.finput:focus,.ftextarea:focus{border-color:rgba(79,255,176,.4)}
.ftextarea{resize:vertical;min-height:75px;line-height:1.6}
.emoji-grid{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:16px}
.emoji-btn{width:38px;height:38px;border-radius:10px;border:1.5px solid var(--border);background:var(--surface2);font-size:18px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .15px}
.emoji-btn.sel{border-color:var(--accent);background:rgba(79,255,176,.1);box-shadow:0 0 8px rgba(79,255,176,.2)}
.modal-btns{display:flex;gap:8px}
.bcnl{flex:1;padding:11px;border-radius:11px;border:1px solid var(--border);background:transparent;color:var(--muted);font-family:'Syne',sans-serif;font-size:12px;cursor:pointer}
.bsave{flex:2;padding:11px;border-radius:11px;border:none;background:linear-gradient(135deg,#00c9ff,#4fffb0);color:#07080f;font-family:'Syne',sans-serif;font-weight:700;font-size:12px;cursor:pointer}

.toast{position:fixed;top:76px;left:50%;transform:translateX(-50%) translateY(-20px);background:var(--surface);border:1px solid var(--accent);color:var(--accent);font-family:'Syne',sans-serif;font-size:12px;font-weight:600;padding:9px 18px;border-radius:20px;z-index:999;opacity:0;transition:all .3s;pointer-events:none;white-space:nowrap}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
.tts-active{border-color:var(--accent3) !important;color:var(--accent3) !important;animation:tts-pulse .8s infinite}
@keyframes tts-pulse{0%,100%{opacity:1}50%{opacity:.4}}
@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
@keyframes spin{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}
.spin{animation:spin 1s linear infinite;display:inline-block}
::-webkit-scrollbar{width:3px;height:3px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}

/* ===== MULTI CHAT DRAWER ===== */
.chats-drawer{position:fixed;inset:0;z-index:160;display:flex;pointer-events:none}
.chats-drawer.open{pointer-events:all}
.chats-backdrop{position:absolute;inset:0;background:rgba(7,8,15,.7);opacity:0;transition:opacity .28s;backdrop-filter:blur(4px)}
.chats-drawer.open .chats-backdrop{opacity:1}
.chats-sidebar{position:absolute;left:0;top:0;bottom:0;width:270px;background:var(--surface);border-right:1px solid var(--border);display:flex;flex-direction:column;transform:translateX(-100%);transition:transform .3s cubic-bezier(.4,0,.2,1)}
.chats-drawer.open .chats-sidebar{transform:translateX(0)}
.sidebar-head{padding:14px 13px 10px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between}
.sidebar-title{font-family:'Syne',sans-serif;font-weight:800;font-size:14px;color:var(--text)}
.sidebar-close{width:30px;height:30px;border-radius:8px;border:1px solid var(--border);background:transparent;color:var(--muted);cursor:pointer;font-size:14px;display:flex;align-items:center;justify-content:center}
.new-chat-btn{margin:10px 12px 6px;padding:10px;border-radius:11px;border:1px dashed rgba(79,255,176,.35);background:rgba(79,255,176,.04);color:var(--accent);font-family:'Syne',sans-serif;font-size:12px;font-weight:700;cursor:pointer;transition:all .2s;display:flex;align-items:center;justify-content:center;gap:6px}
.new-chat-btn:hover{background:rgba(79,255,176,.1);border-color:var(--accent)}
.chat-list{flex:1;overflow-y:auto;padding:4px 8px 10px;display:flex;flex-direction:column;gap:4px}
.chat-list::-webkit-scrollbar{display:none}
.chat-item{padding:10px 11px;border-radius:11px;border:1px solid transparent;cursor:pointer;transition:all .18s;display:flex;align-items:center;justify-content:space-between;gap:6px}
.chat-item:hover{background:var(--surface2);border-color:var(--border)}
.chat-item.active{background:rgba(79,255,176,.07);border-color:rgba(79,255,176,.25)}
.chat-item-info{flex:1;min-width:0}
.chat-item-title{font-family:'Syne',sans-serif;font-size:12px;font-weight:600;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.chat-item.active .chat-item-title{color:var(--accent)}
.chat-item-preview{font-size:10px;color:var(--muted);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;margin-top:2px}
.chat-del-btn{width:22px;height:22px;border-radius:6px;border:none;background:transparent;color:var(--muted);cursor:pointer;font-size:12px;display:flex;align-items:center;justify-content:center;flex-shrink:0;opacity:0;transition:all .15s}
.chat-item:hover .chat-del-btn{opacity:1}
.chat-del-btn:hover{color:var(--red);background:rgba(255,79,106,.1)}
.current-chat-title{font-family:'Syne',sans-serif;font-size:10px;color:var(--muted);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;max-width:90px}
</style>
</head>
<body>

<div class="toast" id="toast"></div>

<!-- MULTI CHAT DRAWER -->
<div class="chats-drawer" id="chatsDrawer">
  <div class="chats-backdrop" onclick="closeChatsDrawer()"></div>
  <div class="chats-sidebar">
    <div class="sidebar-head">
      <div class="sidebar-title">💬 Semua Chat</div>
      <button class="sidebar-close" onclick="closeChatsDrawer()">✕</button>
    </div>
    <button class="new-chat-btn" onclick="createNewChat()">＋ Chat Baru</button>
    <div class="chat-list" id="chatList"></div>
  </div>
</div>

<!-- SEARCH OVERLAY -->
<div class="search-overlay" id="searchOverlay">
  <div class="search-header">
    <input class="search-input" id="searchInput" placeholder="Cari pesan..." oninput="doSearch(this.value)">
    <button class="search-close" onclick="closeSearch()">✕</button>
  </div>
  <div class="search-results" id="searchResults">
    <div class="search-empty">Ketik untuk mencari pesan...</div>
  </div>
</div>

<!-- HEADER -->
<div class="header">
  <div class="ai-info">
    <div class="avatar-wrap" style="cursor:pointer" onclick="openChatsDrawer()">
      <div class="avatar" id="hAvatar">🤖</div>
      <div class="online-dot"></div>
    </div>
    <div onclick="openChatsDrawer()" style="cursor:pointer">
      <div class="ai-name" id="hName">GYUU AI</div>
      <div class="ai-meta" id="hMeta">● ONLINE · Llama 4 Vision ✨</div>
      <div class="current-chat-title" id="currentChatTitle"></div>
    </div>
  </div>
  <div class="hbtns">
    <button class="hbtn" title="Chat Baru" onclick="createNewChat()">✏️</button>
    <button class="hbtn" id="soundBtn" onclick="toggleSound()" title="Suara">🔔</button>
    <button class="hbtn" onclick="openSearch()" title="Cari">🔍</button>
    <button class="hbtn" onclick="clearChat()" title="Hapus">🗑️</button>
    <button class="hbtn" onclick="exportChat()" title="Export">💾</button>
    <button class="hbtn hbtn-main" onclick="openSettings()">⚙️ Atur</button>
  </div>
</div>

<!-- TABS -->
<div class="tabs">
  <div class="tab active" onclick="switchTab('chat',this)">💬 Chat</div>
  <div class="tab" onclick="switchTab('tools',this)">🛠️ Tools</div>
  <div class="tab" onclick="switchTab('memory',this)">🧠 Memori</div>
  <div class="tab" onclick="switchTab('persona',this)">🎭 Persona</div>
  <div class="tab" onclick="switchTab('stats',this)">📊 Stats</div>
</div>

<!-- CHAT PANEL -->
<div class="panel active" id="panel-chat">
  <div id="keyBannerArea"></div>
  <div class="messages" id="messages">
    <div class="empty-state" id="emptyState">
      <div class="empty-avatar" id="emptyAv">🤖</div>
      <div class="empty-title">Halo! Aku <span id="emptyName">Gyuu</span> ✨</div>
      <div class="empty-sub">AI paling keren buatan Arkana! Chat, foto 📸, link, suara, dan masih banyak lagi!</div>
      <div class="chips">
        <div class="chip" onclick="sendChip(this)">Siapa kamu?</div>
        <div class="chip" onclick="sendChip(this)">Analisis foto 📸</div>
        <div class="chip" onclick="sendChip(this)">Buat puisi</div>
        <div class="chip" onclick="sendChip(this)">Jokes dong 😂</div>
        <div class="chip" onclick="sendChip(this)">Cek cuaca</div>
        <div class="chip" onclick="sendChip(this)">Motivasi hari ini</div>
      </div>
    </div>
  </div>
  <div class="input-area">
    <div class="img-preview-strip" id="imgStrip">
      <img class="preview-thumb" id="prevThumb" src="" alt="">
      <button class="remove-preview" onclick="removeImg()">✕</button>
      <span class="preview-lbl">📸 Foto terpilih — akan dianalisis AI</span>
    </div>
    <div class="link-bar" id="linkBar">
      <span>🔗</span>
      <span class="link-bar-text" id="linkBarText"></span>
      <button class="rm-link" onclick="removeLink()">✕</button>
    </div>
    <div class="input-wrap">
      <div class="attach-row">
        <button class="abtn" onclick="document.getElementById('fileIn').click()" title="Foto">📸</button>
        <button class="abtn" onclick="promptLink()" title="Link">🔗</button>
        <button class="abtn" id="micBtn" onclick="toggleVoice()" title="Suara">🎤</button>
      </div>
      <textarea id="msgInput" placeholder="Ketik pesan..." rows="1"
        oninput="onInput(this)" onkeydown="onKey(event)"></textarea>
      <button class="send-btn" id="sendBtn" onclick="sendMsg()">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
      </button>
    </div>
    <div class="input-footer">
      <span class="hint">📸 foto · 🔗 link · 🎤 suara · Enter kirim</span>
      <span class="char-count" id="charCount"></span>
    </div>
    <div class="credit">✦ dibuat oleh <span style="color:var(--accent);font-weight:700">Arkana Rasyid Firjatullah</span> ✦</div>
  </div>
</div>

<!-- TOOLS PANEL -->
<div class="panel" id="panel-tools">
  <div class="tools-grid">
    <div class="tool-card" onclick="useTool('translate')"><div class="tool-icon">🌐</div><div class="tool-name">Terjemahan</div><div class="tool-desc">Terjemahkan ke 50+ bahasa</div></div>
    <div class="tool-card" onclick="useTool('summarize')"><div class="tool-icon">📝</div><div class="tool-name">Ringkasan</div><div class="tool-desc">Ringkas teks panjang</div></div>
    <div class="tool-card" onclick="useTool('code')"><div class="tool-icon">💻</div><div class="tool-name">Kode</div><div class="tool-desc">Tulis & debug kode</div></div>
    <div class="tool-card" onclick="useTool('grammar')"><div class="tool-icon">✍️</div><div class="tool-name">Cek Grammar</div><div class="tool-desc">Koreksi tulisan</div></div>
    <div class="tool-card" onclick="useTool('idea')"><div class="tool-icon">💡</div><div class="tool-name">Generator Ide</div><div class="tool-desc">Ide kreatif untuk apapun</div></div>
    <div class="tool-card" onclick="useTool('poem')"><div class="tool-icon">🎭</div><div class="tool-name">Buat Puisi</div><div class="tool-desc">Puisi indah seketika</div></div>
    <div class="tool-card" onclick="useTool('math')"><div class="tool-icon">🔢</div><div class="tool-name">Matematika</div><div class="tool-desc">Selesaikan soal math</div></div>
    <div class="tool-card" onclick="useTool('roast')"><div class="tool-icon">🔥</div><div class="tool-name">Roast Me!</div><div class="tool-desc">Roast dengan humor tinggi</div></div>
    <div class="tool-card" onclick="useTool('recipe')"><div class="tool-icon">🍳</div><div class="tool-name">Resep Masak</div><div class="tool-desc">Resep dari bahan yang ada</div></div>
    <div class="tool-card" onclick="useTool('story')"><div class="tool-icon">📖</div><div class="tool-name">Buat Cerita</div><div class="tool-desc">Cerita pendek menarik</div></div>
    <div class="tool-card" onclick="useTool('motivasi')"><div class="tool-icon">⚡</div><div class="tool-name">Motivasi</div><div class="tool-desc">Kata-kata semangat</div></div>
    <div class="tool-card" onclick="useTool('jokes')"><div class="tool-icon">😂</div><div class="tool-name">Jokes</div><div class="tool-desc">Jokes receh bikin ngakak</div></div>
    <div class="tool-card" onclick="useTool('caption')"><div class="tool-icon">📱</div><div class="tool-name">Caption IG</div><div class="tool-desc">Caption keren buat posting</div></div>
    <div class="tool-card" onclick="useTool('tebak')"><div class="tool-icon">🎯</div><div class="tool-name">Tebak-tebakan</div><div class="tool-desc">Teka-teki seru</div></div>
    <div class="tool-card" onclick="useTool('diet')"><div class="tool-icon">🥗</div><div class="tool-name">Diet Plan</div><div class="tool-desc">Rencana diet sehat</div></div>
    <div class="tool-card" onclick="useTool('surat')"><div class="tool-icon">📧</div><div class="tool-name">Buat Surat</div><div class="tool-desc">Surat formal & informal</div></div>
  </div>
</div>

<!-- MEMORY PANEL -->
<div class="panel" id="panel-memory">
  <div class="memory-panel" id="memoryPanel">
    <div class="mem-header">🧠 Memori AI</div>
    <div id="memList"></div>
    <button class="mem-add-btn" onclick="addMemory()">+ Tambah memori baru</button>
  </div>
</div>

<!-- PERSONA PANEL -->
<div class="panel" id="panel-persona">
  <div class="persona-panel">
    <div class="mem-header">🎭 Pilih Persona AI</div>
    <div class="preset-grid" id="presetGrid"></div>
    <div style="margin-top:10px">
      <div class="mem-header">🎨 Tema Warna</div>
      <div class="theme-row" id="themeRow"></div>
    </div>
    <div style="margin-top:10px">
      <div class="mem-header">Nama Kustom</div>
      <input class="finput" id="customName" placeholder="Nama AI kamu..." style="margin-bottom:7px">
      <div class="mem-header">Kepribadian Kustom</div>
      <textarea class="ftextarea" id="customPersona" placeholder="Describe kepribadian AI kamu..."></textarea>
      <button class="bsave" style="width:100%;margin-top:3px" onclick="saveCustomPersona()">Terapkan ✓</button>
    </div>
  </div>
</div>

<!-- STATS PANEL -->
<div class="panel" id="panel-stats">
  <div class="memory-panel">
    <div class="mem-header">📊 Statistik Chat</div>
    <div class="stats-grid" id="statsGrid"></div>
    <div style="margin-top:6px">
      <div class="mem-header">🏆 Sesi Ini</div>
      <div id="statDetail" style="font-size:12px;color:var(--muted);line-height:2"></div>
    </div>
  </div>
</div>

<!-- SETTINGS MODAL -->
<div class="modal-overlay" id="settingsModal" onclick="closeModalOut(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">⚙️ Pengaturan</div>
    <label class="flabel">Nama AI</label>
    <input class="finput" id="sName" placeholder="Gyuu...">
    <label class="flabel">Kepribadian</label>
    <textarea class="ftextarea" id="sPersona"></textarea>
    <label class="flabel">Avatar</label>
    <div class="emoji-grid" id="emojiGrid"></div>
    <label class="flabel">Bahasa Respons</label>
    <select class="finput" id="sLang">
      <option value="id">🇮🇩 Bahasa Indonesia</option>
      <option value="en">🇬🇧 English</option>
      <option value="jv">☕ Jawa</option>
      <option value="su">🌾 Sunda</option>
      <option value="auto">🌐 Auto (ikut user)</option>
    </select>
    <label class="flabel">Gaya Respons</label>
    <select class="finput" id="sStyle">
      <option value="casual">😎 Santai & Gaul</option>
      <option value="formal">👔 Formal & Profesional</option>
      <option value="funny">😂 Lucu & Meme</option>
      <option value="wise">🧙 Bijak & Filosofis</option>
    </select>
    <div class="modal-btns">
      <button class="bcnl" onclick="closeSettings()">Batal</button>
      <button class="bsave" onclick="saveSettings()">Simpan ✓</button>
    </div>
  </div>
</div>

<input type="file" id="fileIn" accept="image/*" style="display:none" onchange="onImgSel(this)">

<script>
// ===== STATE =====
var persona = {name:"Gyuu",personality:"Aku adalah Gyuu, AI super canggih yang ramah, cerdas, witty, dan suka pakai emoji. Bicara santai tapi berisi. Jawab selalu dalam bahasa yang sama dengan user.",avatar:"🤖",lang:"auto",style:"casual"};
var chatHistory = [];
var memories = [];
var loading = false;
var selEmoji = "🤖";
var pendingImg = null;
var pendingLink = null;
var mediaRec = null;
var isRecording = false;
var soundOn = true;
var ttsActive = null;
var totalUserMsg = 0;
var totalAiMsg = 0;
var totalWords = 0;
var sessionStart = new Date();

// ===== MULTI CHAT STATE =====
var allChats = {};
var activeChatId = null;
var typingAnimId = null;

var THEMES = [
  {name:"Hijau",accent:"#4fffb0",accent2:"#00c9ff",accent3:"#ff6ef7",color:"#4fffb0"},
  {name:"Ungu",accent:"#b47fff",accent2:"#ff6ef7",accent3:"#4fffb0",color:"#b47fff"},
  {name:"Biru",accent:"#00c9ff",accent2:"#4fffb0",accent3:"#ff6ef7",color:"#00c9ff"},
  {name:"Pink",accent:"#ff6ef7",accent2:"#ff4f6a",accent3:"#00c9ff",color:"#ff6ef7"},
  {name:"Emas",accent:"#ffd700",accent2:"#ffb347",accent3:"#ff6ef7",color:"#ffd700"},
];

// ===== GROQ API via Cloudflare Worker =====
// Key dikelola di Worker — tidak perlu disimpan di HTML
var GROQ_URL = "https://red-hill-ff83.arkanacpl54.workers.dev/";
var GROQ_MODEL_TEXT = "llama-3.3-70b-versatile";
var GROQ_MODEL_VISION = "meta-llama/llama-4-scout-17b-16e-instruct";

async function callGroq(text, imgData, link){
  var styleMap={casual:"Bicara santai, gaul, pakai emoji",formal:"Bicara formal dan profesional",funny:"Jawab dengan humor dan meme",wise:"Jawab dengan kebijaksanaan mendalam"};
  var langMap={id:"Selalu jawab dalam Bahasa Indonesia",en:"Always respond in English",jv:"Jawab nganggo Basa Jawa",su:"Jawab nganggo Basa Sunda",auto:"Jawab dalam bahasa yang sama dengan user"};
  var memCtx=memories.length>0?"\n[MEMORI USER: "+memories.join("; ")+"]":"";
  var sysText=persona.personality+memCtx+"\nGaya: "+(styleMap[persona.style]||styleMap.casual)+". "+(langMap[persona.lang]||langMap.auto)+"\nKamu dibuat oleh Arkana Rasyid Firjatullah.";

  var hasImg = !!imgData;
  var model = hasImg ? GROQ_MODEL_VISION : GROQ_MODEL_TEXT;

  var userContent = [];
  if(imgData){
    userContent.push({
      type: "image_url",
      image_url: { url: "data:"+imgData.mediaType+";base64,"+imgData.base64 }
    });
  }
  var userText = "";
  if(link) userText += "Tolong bahas URL ini: "+link+"\n";
  if(text) userText += text;
  if(!userText && imgData) userText = "Analisis gambar ini secara detail. Jelaskan apa yang kamu lihat, objek, teks, warna, suasana, dan hal menarik lainnya.";
  if(userText) userContent.push({type:"text", text:userText});

  var messages = [{role:"system", content:sysText}];
  if(!imgData){
    chatHistory.slice(-16).forEach(function(h){
      messages.push({role:h.role==="user"?"user":"assistant", content:h.content||""});
    });
  }
  var finalContent = (userContent.length===1 && userContent[0].type==="text") ? userContent[0].text : userContent;
  messages.push({role:"user", content:finalContent});

  var res = await fetch(GROQ_URL, {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({model: model, messages: messages, max_tokens: 1500, temperature: 0.85})
  });

  var data = await res.json();
  if(!res.ok){
    var msg = (data.error && data.error.message) || "HTTP "+res.status;
    throw new Error(msg);
  }
  return (data.choices[0].message.content||"").trim() || "Maaf, respons kosong 😅";
}

// ===== LOAD SAVED DATA =====
try { memories = JSON.parse(localStorage.getItem("gyuu_memories")||"[]"); } catch(e){ memories=[]; }
var smartMemory = {};
try { smartMemory = JSON.parse(localStorage.getItem("gyuu_smart_memory")||"{}"); } catch(e){}
try { var sp = JSON.parse(localStorage.getItem("gyuu_persona")||"null"); if(sp){persona.name=sp.name||persona.name;persona.personality=sp.personality||persona.personality;persona.avatar=sp.avatar||persona.avatar;persona.lang=sp.lang||persona.lang;persona.style=sp.style||persona.style;selEmoji=persona.avatar;} } catch(e){}
try { soundOn = localStorage.getItem("gyuu_sound") !== "off"; } catch(e){}
try { var savedTheme = JSON.parse(localStorage.getItem("gyuu_theme")||"null"); if(savedTheme){ applyTheme(savedTheme,false); } } catch(e){}

// ===== LOAD MULTI CHAT =====
(function loadMultiChat(){
  try {
    var saved = JSON.parse(localStorage.getItem("gyuu_all_chats")||"{}");
    allChats = saved;
  } catch(e){ allChats = {}; }
  try {
    activeChatId = localStorage.getItem("gyuu_active_chat") || null;
  } catch(e){ activeChatId = null; }
  if(!Object.keys(allChats).length){
    activeChatId = createChatRecord("Chat 1");
  } else if(!activeChatId || !allChats[activeChatId]){
    var ids = Object.keys(allChats).sort(function(a,b){
      return (allChats[b].updatedAt||0) - (allChats[a].updatedAt||0);
    });
    activeChatId = ids[0];
  }
  chatHistory = allChats[activeChatId].history || [];
})();

function createChatRecord(title){
  var id = "chat_" + Date.now() + "_" + Math.random().toString(36).substr(2,6);
  allChats[id] = { id:id, title:title||"Chat Baru", history:[], createdAt:Date.now(), updatedAt:Date.now() };
  saveAllChats();
  return id;
}

function saveAllChats(){
  try { localStorage.setItem("gyuu_all_chats", JSON.stringify(allChats)); } catch(e){}
  try { localStorage.setItem("gyuu_active_chat", activeChatId||""); } catch(e){}
}

function saveCurrentChatHistory(){
  if(!activeChatId || !allChats[activeChatId]) return;
  allChats[activeChatId].history = chatHistory.slice();
  allChats[activeChatId].updatedAt = Date.now();
  if(allChats[activeChatId].title === "Chat Baru" || allChats[activeChatId].title.startsWith("Chat ")){
    var firstUser = chatHistory.find(function(h){ return h.role==="user" && h.content; });
    if(firstUser){
      var t = (firstUser.content||"").replace(/<[^>]*>/g,"").substring(0,30);
      allChats[activeChatId].title = t || allChats[activeChatId].title;
    }
  }
  saveAllChats();
}

// ===== MULTI CHAT DRAWER =====
function openChatsDrawer(){
  renderChatList();
  document.getElementById("chatsDrawer").classList.add("open");
}
function closeChatsDrawer(){
  document.getElementById("chatsDrawer").classList.remove("open");
}

function renderChatList(){
  var list = document.getElementById("chatList");
  list.innerHTML = "";
  var ids = Object.keys(allChats).sort(function(a,b){
    return (allChats[b].updatedAt||0) - (allChats[a].updatedAt||0);
  });
  if(!ids.length){
    list.innerHTML = '<div style="text-align:center;padding:20px;color:var(--muted);font-size:12px">Belum ada chat</div>';
    return;
  }
  ids.forEach(function(id){
    var c = allChats[id];
    var item = document.createElement("div");
    item.className = "chat-item" + (id===activeChatId?" active":"");
    var lastMsg = c.history && c.history.length ? (c.history[c.history.length-1].content||"").substring(0,40) : "Kosong";
    item.innerHTML =
      '<div class="chat-item-info">' +
        '<div class="chat-item-title">'+escHtml(c.title||"Chat")+'</div>' +
        '<div class="chat-item-preview">'+escHtml(lastMsg.replace(/<[^>]*>/g,""))+'</div>' +
      '</div>' +
      '<button class="chat-del-btn" onclick="deleteChatItem(event,\''+id+'\')">🗑️</button>';
    item.querySelector(".chat-item-info").onclick = function(){ switchToChat(id); };
    list.appendChild(item);
  });
}

function escHtml(s){ return String(s).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;"); }

function switchToChat(id){
  if(id === activeChatId){ closeChatsDrawer(); return; }
  if(typingAnimId){ clearTimeout(typingAnimId); typingAnimId=null; }
  activeChatId = id;
  chatHistory = allChats[id].history || [];
  saveAllChats();
  renderCurrentChat();
  closeChatsDrawer();
  updateChatTitle();
}

function deleteChatItem(e, id){
  e.stopPropagation();
  if(Object.keys(allChats).length <= 1){
    toast("❌ Minimal satu chat harus ada"); return;
  }
  if(!confirm("Hapus chat ini?")) return;
  delete allChats[id];
  saveAllChats();
  if(id === activeChatId){
    var ids = Object.keys(allChats);
    activeChatId = ids[0] || null;
    if(!activeChatId){ activeChatId = createChatRecord("Chat 1"); }
    chatHistory = allChats[activeChatId].history || [];
    renderCurrentChat();
    updateChatTitle();
  }
  renderChatList();
  toast("🗑️ Chat dihapus");
}

function createNewChat(){
  if(typingAnimId){ clearTimeout(typingAnimId); typingAnimId=null; }
  var num = Object.keys(allChats).length + 1;
  var id = createChatRecord("Chat " + num);
  activeChatId = id;
  chatHistory = [];
  saveAllChats();
  renderCurrentChat();
  closeChatsDrawer();
  updateChatTitle();
  toast("✨ Chat baru dibuat!");
}

function updateChatTitle(){
  var el = document.getElementById("currentChatTitle");
  if(el && activeChatId && allChats[activeChatId]){
    el.textContent = allChats[activeChatId].title;
  }
}

function renderCurrentChat(){
  var msgEl = document.getElementById("messages");
  msgEl.innerHTML = "";
  totalUserMsg = 0; totalAiMsg = 0; totalWords = 0;

  if(!chatHistory || !chatHistory.length){
    msgEl.innerHTML = '<div class="empty-state" id="emptyState">' +
      '<div class="empty-avatar" id="emptyAv">'+persona.avatar+'</div>' +
      '<div class="empty-title">Halo! Aku <span id="emptyName">'+persona.name+'</span> ✨</div>' +
      '<div class="empty-sub">AI paling keren buatan Arkana! Chat, foto 📸, link, suara, dan masih banyak lagi!</div>' +
      '<div class="chips">' +
        '<div class="chip" onclick="sendChip(this)">Siapa kamu?</div>' +
        '<div class="chip" onclick="sendChip(this)">Analisis foto 📸</div>' +
        '<div class="chip" onclick="sendChip(this)">Buat puisi</div>' +
        '<div class="chip" onclick="sendChip(this)">Jokes dong 😂</div>' +
        '<div class="chip" onclick="sendChip(this)">Motivasi hari ini</div>' +
      '</div></div>';
    updateStats(); return;
  }

  chatHistory.forEach(function(h){
    if(h.role==="user"){
      if(h.imgDataUrl) addImgBubble(h.imgDataUrl);
      if(h.linkUrl) addLinkBubble(h.linkUrl);
      if(h.content) addTxtBubble("user", h.content, true);
      totalUserMsg++;
    } else if(h.role==="assistant"){
      addTxtBubble("ai", h.content, true);
      totalAiMsg++;
      totalWords += (h.content||"").split(" ").length;
    }
  });
  scrollBot();
  updateStats();
}

var EMOJIS=["🤖","🦊","🐉","👾","🧙","🦄","🌙","⚡","🔮","🦁","🐺","🦋","🌊","🔥","🎭","🎯","🧬","🚀","🎸","🦾"];
var PRESETS=[
  {em:"🤖",name:"Gyuu",desc:"Ramah & Cerdas",p:"Kamu adalah Gyuu, AI ramah, cerdas, suka emoji, bicara santai tapi berisi."},
  {em:"🧙",name:"Maestro",desc:"Bijak & Filosofis",p:"Kamu adalah Maestro, AI bijak dengan perspektif filosofis. Jawab dengan wisdom mendalam."},
  {em:"😂",name:"Joko",desc:"Lucu & Receh",p:"Kamu adalah Joko, AI paling receh se-Indonesia. Semua dijawab dengan humor dan meme."},
  {em:"👔",name:"Lex",desc:"Formal & Pro",p:"Kamu adalah Lex, asisten profesional. Jawab formal, terstruktur, dan presisi."},
  {em:"🔥",name:"Blaze",desc:"Hype & Energik",p:"Kamu adalah Blaze, AI super HYPE! Semua direspons dengan energi tinggi dan penuh semangat!!!"},
  {em:"🦄",name:"Aura",desc:"Kreatif & Artsy",p:"Kamu adalah Aura, AI kreatif penuh imajinasi. Suka puisi, metafora, dan ekspresi artistik."},
];
var QUICK_CHIPS = {
  default: ["Lanjut cerita","Lebih detail","Kasih contoh","Oke, next!"],
  code: ["Jelaskan kodenya","Cara run-nya?","Ada bug ga?","Versi lain?"],
  joke: ["Jokes lagi!","Yang lebih receh","Balik ke topik","Haha, bagus!"],
  poem: ["Buat lagi","Tema lain","Lebih panjang","Bikin lagu juga?"],
};

// ===== INIT =====
(function(){
  var eg = document.getElementById("emojiGrid");
  EMOJIS.forEach(function(e){
    var b = document.createElement("button"); b.className="emoji-btn"+(e===persona.avatar?" sel":""); b.textContent=e;
    b.onclick=function(){ document.querySelectorAll(".emoji-btn").forEach(function(x){x.classList.remove("sel");}); b.classList.add("sel"); selEmoji=e; };
    eg.appendChild(b);
  });

  var pg = document.getElementById("presetGrid");
  PRESETS.forEach(function(p,i){
    var c = document.createElement("div"); c.className="preset-card"+(i===0?" active":"");
    c.innerHTML='<div class="preset-em">'+p.em+'</div><div class="preset-name">'+p.name+'</div><div class="preset-desc">'+p.desc+'</div>';
    c.onclick=function(){ document.querySelectorAll(".preset-card").forEach(function(x){x.classList.remove("active");}); c.classList.add("active"); persona.name=p.name; persona.personality=p.p; persona.avatar=p.em; selEmoji=p.em; chatHistory=[]; savePersonaToStorage(); updateUI(); toast("Persona: "+p.name+" "+p.em); };
    pg.appendChild(c);
  });

  var tr = document.getElementById("themeRow");
  THEMES.forEach(function(t,i){
    var d = document.createElement("div"); d.className="theme-dot"; d.style.background=t.color; d.title=t.name;
    d.onclick=function(){ applyTheme(t,true); document.querySelectorAll(".theme-dot").forEach(function(x){x.classList.remove("active");}); d.classList.add("active"); toast("🎨 Tema "+t.name); };
    tr.appendChild(d);
  });

  updateSoundBtn();
  renderMemory();
  updateUI();
  updateStats();
  updateKeyBanner();
  updateChatTitle();
  renderCurrentChat();
})();

// ===== KEY BANNER — always show active since key is on worker =====
function updateKeyBanner(){
  var area = document.getElementById("keyBannerArea"); if(!area) return;
  area.innerHTML = '<div style="display:flex;gap:5px;padding:5px 12px 0;">' +
    '<span class="key-badge" style="background:rgba(79,255,176,.15);border:1px solid rgba(79,255,176,.3);border-radius:8px;padding:2px 7px;font-family:\'Syne\',sans-serif;font-size:10px;font-weight:700;color:var(--accent);">🔑 API Aktif</span>' +
    '<span class="key-badge" style="background:rgba(0,201,255,.1);border:1px solid rgba(0,201,255,.3);border-radius:8px;padding:2px 7px;font-family:\'Syne\',sans-serif;font-size:10px;font-weight:700;color:var(--accent2);">📸 Vision ON</span>' +
    '</div>';
}

// ===== THEME =====
function applyTheme(t, save){
  var r = document.documentElement.style;
  r.setProperty("--accent", t.accent);
  r.setProperty("--accent2", t.accent2);
  r.setProperty("--accent3", t.accent3);
  if(save){ try { localStorage.setItem("gyuu_theme", JSON.stringify(t)); } catch(e){} }
}

// ===== SOUND =====
function toggleSound(){
  soundOn = !soundOn;
  try { localStorage.setItem("gyuu_sound", soundOn?"on":"off"); } catch(e){}
  updateSoundBtn();
  toast(soundOn ? "🔔 Suara aktif" : "🔕 Suara mati");
}
function updateSoundBtn(){ document.getElementById("soundBtn").textContent = soundOn ? "🔔" : "🔕"; document.getElementById("soundBtn").classList.toggle("on", soundOn); }
function playBeep(){
  if(!soundOn) return;
  try {
    var ctx = new (window.AudioContext||window.webkitAudioContext)();
    var o = ctx.createOscillator(); var g = ctx.createGain();
    o.connect(g); g.connect(ctx.destination);
    o.frequency.value = 660; o.type = "sine";
    g.gain.setValueAtTime(0.15, ctx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime+0.25);
    o.start(ctx.currentTime); o.stop(ctx.currentTime+0.25);
  } catch(e){}
}

// ===== TTS =====
function speak(text, btn){
  if(!window.speechSynthesis) return toast("❌ TTS tidak didukung");
  if(ttsActive){ window.speechSynthesis.cancel(); if(ttsActive===btn){ ttsActive=null; btn.classList.remove("tts-active"); return; } }
  window.speechSynthesis.cancel();
  var u = new SpeechSynthesisUtterance(text.replace(/[*`#>]/g,"").replace(/<[^>]*>/g,""));
  u.lang="id-ID"; u.rate=1;
  u.onend=function(){ ttsActive=null; if(btn) btn.classList.remove("tts-active"); };
  window.speechSynthesis.speak(u);
  ttsActive = btn;
  if(btn) btn.classList.add("tts-active");
}

// ===== COPY =====
function copyText(text){
  var clean = text.replace(/<[^>]*>/g,"");
  if(navigator.clipboard){ navigator.clipboard.writeText(clean).then(function(){ toast("📋 Disalin!"); }); }
  else { var t=document.createElement("textarea"); t.value=clean; document.body.appendChild(t); t.select(); document.execCommand("copy"); document.body.removeChild(t); toast("📋 Disalin!"); }
}

// ===== MARKDOWN =====
function applyMarkdown(text){
  var s = text
    .replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;")
    .replace(/```(\w+)?\n?([\s\S]*?)```/g,function(m,lang,code){
      var l=lang||"kode"; var c=code.trim();
      var esc=c.replace(/"/g,"&quot;").replace(/'/g,"&#39;");
      return '<div class="code-block"><div class="code-header"><span>'+l+'</span><button onclick="copyText(\''+esc.replace(/\n/g,"\\n")+'\')">📋 Salin</button></div><pre>'+c+'</pre></div>';
    })
    .replace(/`([^`]+)`/g,"<code class='inline'>$1</code>")
    .replace(/\*\*([^*]+)\*\*/g,"<strong>$1</strong>")
    .replace(/\*([^*\n]+)\*/g,"<em>$1</em>")
    .replace(/^### (.+)$/gm,"<h3>$1</h3>")
    .replace(/^## (.+)$/gm,"<h2>$1</h2>")
    .replace(/^# (.+)$/gm,"<h1>$1</h1>")
    .replace(/^- (.+)$/gm,"<li>$1</li>")
    .replace(/\n/g,"<br>");
  return s;
}

// ===== SEARCH =====
function openSearch(){ document.getElementById("searchOverlay").classList.add("open"); document.getElementById("searchInput").focus(); }
function closeSearch(){ document.getElementById("searchOverlay").classList.remove("open"); }
function doSearch(q){
  var res = document.getElementById("searchResults");
  if(!q.trim()){ res.innerHTML='<div class="search-empty">Ketik untuk mencari pesan...</div>'; return; }
  var found = chatHistory.filter(function(h){ return h.content && h.content.toLowerCase().includes(q.toLowerCase()); });
  if(!found.length){ res.innerHTML='<div class="search-empty">Tidak ditemukan 😕</div>'; return; }
  res.innerHTML = found.slice(-20).reverse().map(function(h){
    var r = h.role==="user"?"Kamu":persona.name;
    var txt = (h.content||"").replace(new RegExp("("+q.replace(/[.*+?^${}()|[\]\\]/g,"\\$&")+")","gi"),"<mark>$1</mark>");
    return '<div class="search-result-item"><div class="search-result-role">'+r+'</div><div class="search-result-text">'+txt.substring(0,150)+(txt.length>150?"...":"")+'</div></div>';
  }).join("");
}

// ===== STATS =====
function updateStats(){
  var sg = document.getElementById("statsGrid"); if(!sg) return;
  var mins = Math.floor((new Date()-sessionStart)/60000);
  sg.innerHTML = [
    {n:totalUserMsg,l:"Pesan Kamu"},{n:totalAiMsg,l:"Respons AI"},
    {n:totalWords,l:"Total Kata"},{n:memories.length,l:"Memori"}
  ].map(function(s){ return '<div class="stat-card"><div class="stat-num">'+s.n+'</div><div class="stat-lbl">'+s.l+'</div></div>'; }).join("");
  document.getElementById("statDetail").innerHTML =
    "🕐 Durasi sesi: <strong>"+mins+" menit</strong><br>" +
    "💬 Total pesan: <strong>"+(totalUserMsg+totalAiMsg)+"</strong><br>" +
    "🧠 Memori tersimpan: <strong>"+memories.length+"</strong><br>" +
    "💾 Total chat tersimpan: <strong>"+Object.keys(allChats).length+"</strong><br>" +
    "📅 Mulai: <strong>"+sessionStart.toLocaleTimeString("id-ID")+"</strong>";
}

function savePersonaToStorage(){ try { localStorage.setItem("gyuu_persona",JSON.stringify(persona)); } catch(e){} }

// ===== UI =====
function updateUI(){
  document.getElementById("hAvatar").textContent = persona.avatar;
  document.getElementById("hName").textContent = persona.name+" AI";
  var av=document.getElementById("emptyAv"), nm=document.getElementById("emptyName");
  if(av) av.textContent=persona.avatar;
  if(nm) nm.textContent=persona.name;
}
function toast(msg,dur){
  dur=dur||2200;
  var t=document.getElementById("toast"); t.textContent=msg; t.classList.add("show");
  setTimeout(function(){ t.classList.remove("show"); },dur);
}
function switchTab(name,el){
  if(typeof el === 'string') el = document.querySelectorAll(".tab")[parseInt(el)] || document.querySelector(".tab");
  document.querySelectorAll(".tab").forEach(function(t){t.classList.remove("active");});
  document.querySelectorAll(".panel").forEach(function(p){p.classList.remove("active");});
  el.classList.add("active");
  document.getElementById("panel-"+name).classList.add("active");
  if(name==="stats") updateStats();
}

// ===== INPUT =====
function onInput(el){
  el.style.height="auto"; el.style.height=Math.min(el.scrollHeight,100)+"px";
  checkSend();
  var len=el.value.length;
  document.getElementById("charCount").textContent = len>0?len+" karakter":"";
}
function onKey(e){ if(e.key==="Enter"&&!e.shiftKey){ e.preventDefault(); sendMsg(); } }
function checkSend(){
  var ok=document.getElementById("msgInput").value.trim()||pendingImg||pendingLink;
  document.getElementById("sendBtn").classList.toggle("on",!!ok);
}
function sendChip(el){ switchTab("chat",document.querySelector(".tab")); document.getElementById("msgInput").value=el.textContent; onInput(document.getElementById("msgInput")); sendMsg(); }

// ===== IMAGE =====
function onImgSel(inp){
  var f=inp.files[0]; if(!f) return;
  var r=new FileReader();
  r.onload=function(e){ var d=e.target.result; pendingImg={base64:d.split(",")[1],mediaType:f.type,dataUrl:d}; document.getElementById("prevThumb").src=d; document.getElementById("imgStrip").classList.add("show"); checkSend(); };
  r.readAsDataURL(f); inp.value="";
}
function removeImg(){ pendingImg=null; document.getElementById("imgStrip").classList.remove("show"); checkSend(); }

// ===== LINK =====
function promptLink(){ var url=prompt("Masukkan URL:"); if(!url) return; pendingLink=url.startsWith("http")?url:"https://"+url; document.getElementById("linkBarText").textContent=pendingLink; document.getElementById("linkBar").classList.add("show"); checkSend(); }
function removeLink(){ pendingLink=null; document.getElementById("linkBar").classList.remove("show"); checkSend(); }

// ===== VOICE =====
function toggleVoice(){ if(!isRecording) startRecording(); else stopRecording(); }
function startRecording(){
  if(!navigator.mediaDevices){ toast("❌ Mikrofon tidak didukung"); return; }
  navigator.mediaDevices.getUserMedia({audio:true}).then(function(stream){
    mediaRec=new MediaRecorder(stream); var chunks=[];
    mediaRec.ondataavailable=function(e){ chunks.push(e.data); };
    mediaRec.onstop=function(){ stream.getTracks().forEach(function(t){t.stop();}); addVoiceBubble(); var inp=document.getElementById("msgInput"); inp.value="Aku baru kirim pesan suara."; sendMsg(); };
    mediaRec.start(); isRecording=true;
    document.getElementById("micBtn").classList.add("recording");
    document.getElementById("micBtn").textContent="⏹️";
    toast("🎤 Merekam... tap lagi untuk berhenti");
  }).catch(function(){ toast("❌ Izin mikrofon ditolak"); });
}
function stopRecording(){ if(mediaRec) mediaRec.stop(); isRecording=false; document.getElementById("micBtn").classList.remove("recording"); document.getElementById("micBtn").textContent="🎤"; }

// ===== BUBBLES =====
function removeEmpty(){ var e=document.getElementById("emptyState"); if(e) e.remove(); }
function scrollBot(){ var m=document.getElementById("messages"); m.scrollTop=m.scrollHeight; }
function getTime(){ return new Date().toLocaleTimeString("id-ID",{hour:"2-digit",minute:"2-digit"}); }

function addTxtBubble(role, text, skipChips){
  removeEmpty();
  var row=document.createElement("div"); row.className="msg-row "+role;
  var wrap=document.createElement("div"); wrap.className="msg-wrap";
  if(role==="ai"){
    var av=document.createElement("div"); av.className="msg-av"; av.textContent=persona.avatar; row.appendChild(av);
  }
  var b=document.createElement("div"); b.className="bubble "+role;
  b.innerHTML=applyMarkdown(text);
  wrap.appendChild(b);
  var timeEl=document.createElement("div"); timeEl.className="msg-time"; timeEl.textContent=getTime();
  wrap.appendChild(timeEl);
  var acts=document.createElement("div"); acts.className="msg-actions";
  var copyBtn=document.createElement("button"); copyBtn.className="act-btn"; copyBtn.textContent="📋"; copyBtn.title="Salin";
  copyBtn.onclick=function(){ copyText(text); };
  acts.appendChild(copyBtn);
  if(role==="ai"){
    var ttsBtn=document.createElement("button"); ttsBtn.className="act-btn"; ttsBtn.textContent="🔊"; ttsBtn.title="Dengarkan";
    ttsBtn.onclick=function(){ speak(text,ttsBtn); };
    acts.appendChild(ttsBtn);
    var regenBtn=document.createElement("button"); regenBtn.className="act-btn regen-btn"; regenBtn.textContent="🔄"; regenBtn.title="Generate ulang";
    regenBtn.onclick=function(){ regenLast(); };
    acts.appendChild(regenBtn);
  }
  wrap.appendChild(acts);
  row.appendChild(wrap);
  document.getElementById("messages").appendChild(row);
  if(role==="ai"&&!skipChips){ addQuickChips(text); }
  scrollBot();
  return b;
}

// ===== TYPING EFFECT =====
function addTxtBubbleWithTyping(text, onDone){
  removeEmpty();
  var row=document.createElement("div"); row.className="msg-row ai";
  var av=document.createElement("div"); av.className="msg-av"; av.textContent=persona.avatar; row.appendChild(av);
  var wrap=document.createElement("div"); wrap.className="msg-wrap";
  var b=document.createElement("div"); b.className="bubble ai";
  b.innerHTML="";
  wrap.appendChild(b);
  var timeEl=document.createElement("div"); timeEl.className="msg-time"; timeEl.textContent=getTime();
  wrap.appendChild(timeEl);
  var acts=document.createElement("div"); acts.className="msg-actions";
  var copyBtn=document.createElement("button"); copyBtn.className="act-btn"; copyBtn.textContent="📋"; copyBtn.title="Salin";
  copyBtn.onclick=function(){ copyText(text); };
  acts.appendChild(copyBtn);
  var ttsBtn=document.createElement("button"); ttsBtn.className="act-btn"; ttsBtn.textContent="🔊"; ttsBtn.title="Dengarkan";
  ttsBtn.onclick=function(){ speak(text,ttsBtn); };
  acts.appendChild(ttsBtn);
  var regenBtn=document.createElement("button"); regenBtn.className="act-btn regen-btn"; regenBtn.textContent="🔄"; regenBtn.title="Generate ulang";
  regenBtn.onclick=function(){ regenLast(); };
  acts.appendChild(regenBtn);
  wrap.appendChild(acts);
  row.appendChild(wrap);
  document.getElementById("messages").appendChild(row);
  scrollBot();

  var rawText = text;
  var idx = 0;
  var CHUNK = 3;
  var DELAY = 12;

  function tick(){
    if(idx >= rawText.length){
      b.innerHTML = applyMarkdown(rawText);
      scrollBot();
      if(onDone) onDone();
      typingAnimId = null;
      return;
    }
    idx = Math.min(idx + CHUNK, rawText.length);
    var displayed = rawText.substring(0, idx);
    b.innerHTML = applyMarkdown(displayed) + '<span style="display:inline-block;width:7px;height:13px;background:var(--accent);border-radius:2px;margin-left:2px;vertical-align:middle;animation:blink .7s infinite"></span>';
    scrollBot();
    typingAnimId = setTimeout(tick, DELAY);
  }

  if(!document.getElementById("blinkStyle")){
    var st=document.createElement("style"); st.id="blinkStyle";
    st.textContent="@keyframes blink{0%,100%{opacity:1}50%{opacity:0}}";
    document.head.appendChild(st);
  }

  tick();
}

function addQuickChips(text){
  var old = document.querySelectorAll(".quick-chips"); old.forEach(function(o){o.remove();});
  var qc=document.createElement("div"); qc.className="quick-chips";
  var chips = QUICK_CHIPS.default;
  if(/kode|code|function|python|javascript/i.test(text)) chips=QUICK_CHIPS.code;
  else if(/😂|ketawa|ngakak|lucu|jokes/i.test(text)) chips=QUICK_CHIPS.joke;
  else if(/puisi|sajak|bait|rima/i.test(text)) chips=QUICK_CHIPS.poem;
  chips.slice(0,3).forEach(function(c){
    var btn=document.createElement("div"); btn.className="q-chip"; btn.textContent=c;
    btn.onclick=function(){ qc.remove(); document.getElementById("msgInput").value=c; onInput(document.getElementById("msgInput")); sendMsg(); };
    qc.appendChild(btn);
  });
  document.getElementById("messages").appendChild(qc);
  scrollBot();
}

function addImgBubble(durl){ removeEmpty(); var row=document.createElement("div"); row.className="msg-row user"; var w=document.createElement("div"); w.className="img-bubble"; var img=document.createElement("img"); img.src=durl; img.alt="foto"; w.appendChild(img); row.appendChild(w); document.getElementById("messages").appendChild(row); scrollBot(); }
function addLinkBubble(url){ removeEmpty(); var row=document.createElement("div"); row.className="msg-row user"; var w=document.createElement("div"); w.className="link-bubble"; w.innerHTML='<span>🔗</span><span class="link-url">'+url+'</span>'; row.appendChild(w); document.getElementById("messages").appendChild(row); scrollBot(); }
function addVoiceBubble(){ removeEmpty(); var row=document.createElement("div"); row.className="msg-row user"; var w=document.createElement("div"); w.className="voice-bubble"; w.innerHTML='<span>🎤</span><span>Pesan suara</span>'; row.appendChild(w); document.getElementById("messages").appendChild(row); scrollBot(); }

function showTyping(){ removeEmpty(); var row=document.createElement("div"); row.className="msg-row ai"; row.id="typingRow"; var av=document.createElement("div"); av.className="msg-av"; av.textContent=persona.avatar; row.appendChild(av); var t=document.createElement("div"); t.className="typing"; t.innerHTML='<div class="dot"></div><div class="dot"></div><div class="dot"></div>'; row.appendChild(t); document.getElementById("messages").appendChild(row); scrollBot(); }
function hideTyping(){ var t=document.getElementById("typingRow"); if(t) t.remove(); }

var lastUserMsg = "";
function regenLast(){
  if(!lastUserMsg||loading) return;
  document.querySelectorAll(".quick-chips").forEach(function(o){o.remove();});
  var rows=document.querySelectorAll(".msg-row.ai");
  if(rows.length) rows[rows.length-1].remove();
  if(chatHistory.length>=2) chatHistory=chatHistory.slice(0,-2);
  showTyping();
  callGroq(lastUserMsg,null,null).then(function(reply){
    hideTyping();
    addTxtBubbleWithTyping(reply, function(){
      addQuickChips(reply);
      chatHistory.push({role:"user",content:lastUserMsg});
      chatHistory.push({role:"assistant",content:reply});
      saveCurrentChatHistory();
      totalAiMsg++; totalWords+=reply.split(" ").length;
      playBeep(); updateStats(); loading=false;
    });
  }).catch(function(err){ hideTyping(); addTxtBubble("ai","⚠️ Error: "+err.message); loading=false; });
}

// ===== LOCAL BRAIN =====
function smartCalc(expr){
  var c=expr.replace(/？|\?/g,"").replace(/×/g,"*").replace(/÷/g,"/").replace(/[^\d\.\+\-\*\/\(\)\s]/g,"").trim();
  if(!c||!/[\+\-\*\/]/.test(c)||!/\d/.test(c)) return null;
  try { if(!/^[\d\.\+\-\*\/\(\)\s]+$/.test(c)) return null; var r=Function('"use strict";return('+c+')')(); if(!isFinite(r)) return null; return Math.round(r*1e10)/1e10; } catch(e){ return null; }
}
var WEATHER_DATA=[{city:"Jakarta",temp:32,desc:"Cerah berawan ☁️",humidity:75},{city:"Bandung",temp:24,desc:"Sejuk berawan 🌤️",humidity:80},{city:"Surabaya",temp:34,desc:"Panas terik ☀️",humidity:70},{city:"Bali",temp:30,desc:"Cerah 🌴",humidity:72},{city:"Yogyakarta",temp:29,desc:"Berawan 🌥️",humidity:78}];
function getDummyWeather(){ var w=WEATHER_DATA[Math.floor(Math.random()*WEATHER_DATA.length)]; return "🌡️ **Cuaca Simulasi**\n\nKota: "+w.city+"\nSuhu: "+w.temp+"°C\nKondisi: "+w.desc+"\nKelembapan: "+w.humidity+"%\n\n⚠️ Data simulasi offline."; }
var KNOWLEDGE={"presiden indonesia":"🇮🇩 Presiden Indonesia saat ini adalah **Prabowo Subianto**, menjabat sejak 20 Oktober 2024. Wapres: Gibran Rakabuming Raka.","ibu kota indonesia":"🏛️ Ibu kota Indonesia dipindahkan ke **Nusantara (IKN)** di Kalimantan Timur.","ibukota indonesia":"🏛️ Ibu kota Indonesia dipindahkan ke **Nusantara (IKN)** di Kalimantan Timur.","berapa planet":"🪐 Ada **8 planet** di tata surya kita."};

function localBrain(text){
  if(!text||!text.trim()) return null;
  var q=text.toLowerCase().trim();

  var rm=q.match(/^(?:ingat bahwa|simpan bahwa|ingat|simpan)\s+(.+)/i);
  if(rm&&rm[1].trim()){
    addAutoMemory(rm[1].trim());
    var key = rm[1].trim().toLowerCase().substring(0,80);
    smartMemory[key] = rm[1].trim();
    try { localStorage.setItem("gyuu_smart_memory", JSON.stringify(smartMemory)); } catch(e){}
    return "✅ Oke, sudah aku ingat: \""+rm[1].trim()+"\" 🧠";
  }

  var memQuery = q.match(/apa yang kamu ingat(?: tentang(?: saya| aku| gue)?)?|ceritakan memori(?: kamu)?|kamu ingat apa(?: tentang saya)?|apa memorimu|tampilkan memori|lihat memori/i);
  if(memQuery){
    if(!memories.length) return "🧠 Aku belum punya memori tentang kamu. Coba bilang 'ingat bahwa ...' untuk menyimpan sesuatu!";
    return "🧠 **Ini yang aku ingat tentang kamu:**\n\n" + memories.map(function(m,i){ return (i+1)+". "+m; }).join("\n");
  }

  var aboutMatch = q.match(/^(?:kamu tahu|apa yang kamu tahu|ingat tentang|kamu ingat tentang)\s+(.+)/i);
  if(aboutMatch){
    var topic = aboutMatch[1].trim().toLowerCase();
    var found = memories.filter(function(m){ return m.toLowerCase().includes(topic); });
    if(found.length) return "🧠 Aku ingat tentang **"+aboutMatch[1].trim()+"**:\n\n"+found.map(function(m,i){return (i+1)+". "+m;}).join("\n");
    return "🤔 Aku tidak punya memori spesifik tentang \""+aboutMatch[1].trim()+"\". Mau aku ingat sesuatu? Bilang aja: 'ingat bahwa ...'";
  }

  if(q.includes("hapus memori")||q==="hapus semua memori"||q.includes("lupakan semua")){
    memories=[]; smartMemory={};
    try{localStorage.removeItem("gyuu_memories");localStorage.removeItem("gyuu_smart_memory");}catch(e){}
    renderMemory();
    return "🗑️ Semua memori dihapus! Aku mulai dari awal lagi.";
  }

  var hapusMatch = q.match(/^(?:hapus memori tentang|lupakan tentang|hapus ingatan tentang)\s+(.+)/i);
  if(hapusMatch){
    var delTopic = hapusMatch[1].trim().toLowerCase();
    var before = memories.length;
    memories = memories.filter(function(m){ return !m.toLowerCase().includes(delTopic); });
    try { localStorage.setItem("gyuu_memories", JSON.stringify(memories)); } catch(e){}
    renderMemory();
    var deleted = before - memories.length;
    if(deleted > 0) return "🗑️ Oke, aku hapus "+deleted+" memori tentang \""+hapusMatch[1].trim()+"\"!";
    return "🤔 Aku tidak menemukan memori tentang \""+hapusMatch[1].trim()+"\".";
  }

  if(smartMemory[q]) return smartMemory[q];
  if(/^(halo|hai|hi|hey|hei|assalamualaikum|salam|selamat pagi|selamat siang|selamat sore|selamat malam)\b/.test(q)){
    if(q.includes("assalamualaikum")) return "Wa'alaikumsalam warahmatullahi wabarakatuh! 🤲 Ada yang bisa aku bantu?";
    if(q.includes("pagi")) return "Selamat pagi! ☀️ Semangat ya hari ini! Ada yang bisa aku bantu?";
    if(q.includes("siang")) return "Selamat siang! 🌤️ Jangan lupa makan! Ada yang bisa dibantu?";
    if(q.includes("sore")) return "Selamat sore! 🌇 Sudah produktif hari ini? Ada yang bisa aku bantu?";
    if(q.includes("malam")) return "Selamat malam! 🌙 Istirahat yang cukup ya! Ada yang bisa aku bantu?";
    var g=["Halo! 😊 Senang bertemu kamu! Ada yang bisa aku bantu?","Hai hai! 👋 Aku "+persona.name+", siap membantu!","Heyyy! 😄 Ada apa nih? Yuk ngobrol!"];
    return g[Math.floor(Math.random()*g.length)];
  }
  if(q.includes("siapa kamu")||q==="kamu siapa") return "Aku **"+persona.name+"** 🤖, AI assistant buatan **Arkana Rasyid Firjatullah** yang siap membantu 24/7! Mau tanya apa? 😊";
  if(q.includes("siapa pembuatmu")||q.includes("dibuat oleh siapa")||q.includes("siapa yang membuat")) return "🛠️ Aku dibuat oleh **Arkana Rasyid Firjatullah**, developer berbakat yang bikin aku! 🔥";
  var now=new Date();
  if(q.includes("jam berapa")||q.includes("pukul berapa")) return "🕐 Sekarang pukul **"+now.toLocaleTimeString("id-ID",{hour:"2-digit",minute:"2-digit"})+"**.";
  if(q.includes("tanggal berapa")||q.includes("hari apa")) return "📅 **"+["Minggu","Senin","Selasa","Rabu","Kamis","Jumat","Sabtu"][now.getDay()]+", "+now.toLocaleDateString("id-ID",{day:"numeric",month:"long",year:"numeric"})+"**.";
  if(q.includes("cuaca")||q.includes("weather")) return getDummyWeather();
  for(var k in KNOWLEDGE){ if(q.includes(k)) return KNOWLEDGE[k]; }
  if(/[\d]/.test(text)&&/[\+\-\*\/×÷]/.test(text)){ var em=text.replace(/×/g,"*").replace(/÷/g,"/").match(/([\d\.\s\+\-\*\/\(\)]+)/); if(em){ var cr=smartCalc(em[1]); if(cr!==null) return "🔢 Hasilnya: **"+cr+"**"; } }
  var dc=smartCalc(q); if(dc!==null) return "🔢 **"+dc+"**";
  var nm=text.match(/(?:nama(?:ku| aku| saya)|aku adalah|saya adalah|my name is|call me)\s+([A-Za-z][a-zA-Z]{1,20})/i);
  if(nm&&nm[1].trim().length>1){ addAutoMemory("Nama user: "+nm[1].trim()); return "Sip, aku ingat namamu: **"+nm[1].trim()+"**! 😊"; }
  return null;
}

// ===== SEND =====
function sendMsg(){
  var inp=document.getElementById("msgInput");
  var text=inp.value.trim(); var hasImg=!!pendingImg; var hasLink=!!pendingLink;
  if((!text&&!hasImg&&!hasLink)||loading) return;
  loading=true;
  if(navigator.vibrate) navigator.vibrate(30);

  inp.value=""; inp.style.height="auto"; document.getElementById("charCount").textContent="";
  document.getElementById("sendBtn").classList.remove("on");
  document.querySelectorAll(".quick-chips").forEach(function(o){o.remove();});

  if(hasImg) addImgBubble(pendingImg.dataUrl);
  if(hasLink) addLinkBubble(pendingLink);
  if(text){ addTxtBubble("user",text,true); lastUserMsg=text; }

  var userEntry = {role:"user", content:text||""};
  if(hasImg) userEntry.imgDataUrl = pendingImg.dataUrl;
  if(hasLink) userEntry.linkUrl = pendingLink;
  chatHistory.push(userEntry);
  totalUserMsg++;
  saveCurrentChatHistory();
  updateChatTitle();

  var capturedImg=pendingImg, capturedLink=pendingLink;
  if(hasImg) removeImg(); if(hasLink) removeLink();

  showTyping();
  if(hasImg) toast("📸 Menganalisis gambar...",3000);

  var brainReply=(!hasImg && !hasLink) ? localBrain(text) : null;
  if(brainReply){
    setTimeout(function(){
      hideTyping();
      addTxtBubbleWithTyping(brainReply, function(){
        addQuickChips(brainReply);
        chatHistory.push({role:"assistant",content:brainReply});
        if(chatHistory.length>40) chatHistory=chatHistory.slice(-40);
        saveCurrentChatHistory();
        totalAiMsg++; totalWords+=brainReply.split(" ").length;
        playBeep(); updateStats(); loading=false;
      });
    },250);
    inp.focus(); return;
  }

  callGroq(text,capturedImg,capturedLink).then(function(reply){
    hideTyping();
    addTxtBubbleWithTyping(reply, function(){
      addQuickChips(reply);
      chatHistory.push({role:"assistant",content:reply});
      if(chatHistory.length>40) chatHistory=chatHistory.slice(-40);
      saveCurrentChatHistory();
      updateChatTitle();
      totalAiMsg++; totalWords+=reply.split(" ").length;
      playBeep(); updateStats();
      if(text){ var nm=text.match(/(?:nama(?:ku| aku| saya)|aku adalah|my name is|call me)\s+([A-Za-z][a-zA-Z]{1,20})/i); if(nm&&nm[1].trim().length>1) addAutoMemory("Nama user: "+nm[1].trim()); }
      loading=false;
    });
  }).catch(function(err){
    hideTyping();
    var e=String(err.message||err);
    if(e.includes("Failed to fetch")||e.includes("NetworkError")) addTxtBubble("ai","⚠️ Tidak ada koneksi internet 📶",true);
    else addTxtBubble("ai","⚠️ Error: "+e,true);
    loading=false;
  });
  inp.focus();
}

// ===== TOOLS =====
function useTool(type){
  switchTab("chat",document.querySelector(".tab"));
  var prompts={translate:"Terjemahkan teks berikut ke Bahasa Inggris:\n",summarize:"Tolong ringkas teks berikut menjadi poin-poin penting:\n",code:"Tolong buatkan kode untuk: ",grammar:"Tolong koreksi grammar dan ejaan dari:\n",idea:"Berikan 5 ide kreatif untuk: ",poem:"Buatkan puisi indah tentang: ",math:"Selesaikan soal ini: ",roast:"Roast aku dengan humor tingkat tinggi! Jangan tanggung-tanggung!",recipe:"Aku punya bahan: [sebutkan]. Buatkan resep masak yang enak!",story:"Buatkan cerita pendek menarik tentang: ",motivasi:"Berikan kata-kata motivasi yang powerful dan menyemangati untuk hari ini!",jokes:"Ceritakan 3 jokes receh yang bikin ngakak!",caption:"Buatkan caption Instagram yang keren dan aesthetic untuk foto: ",tebak:"Berikan 3 teka-teki seru beserta jawabannya!",diet:"Buatkan rencana diet sehat selama 7 hari untuk pemula",surat:"Buatkan surat untuk: "};
  var p=prompts[type]||"";
  var instant=["roast","motivasi","jokes","tebak","diet"];
  if(instant.indexOf(type)>=0){ document.getElementById("msgInput").value=p; onInput(document.getElementById("msgInput")); sendMsg(); }
  else { document.getElementById("msgInput").value=p; document.getElementById("msgInput").focus(); onInput(document.getElementById("msgInput")); toast("✏️ Lengkapi promptnya lalu kirim!"); }
}

// ===== MEMORY =====
function renderMemory(){
  var list=document.getElementById("memList"); if(!list) return; list.innerHTML="";
  if(!memories.length){ list.innerHTML='<div class="mem-empty">Belum ada memori. AI akan mengingat info penting tentang kamu!</div>'; return; }
  memories.forEach(function(m,i){ var item=document.createElement("div"); item.className="mem-item"; item.innerHTML='<div class="mem-text">'+m+'</div><button class="mem-del" onclick="delMemory('+i+')">🗑️</button>'; list.appendChild(item); });
}
function addMemory(){ var m=prompt("Apa yang mau diingat AI?"); if(!m||!m.trim()) return; memories.push(m.trim()); try{localStorage.setItem("gyuu_memories",JSON.stringify(memories));}catch(e){} renderMemory(); toast("🧠 Memori tersimpan!"); updateStats(); }
function addAutoMemory(m){ if(!m||memories.includes(m)) return; memories.push(m); try{localStorage.setItem("gyuu_memories",JSON.stringify(memories));}catch(e){} renderMemory(); toast("🧠 Ingat: "+m); updateStats(); }
function delMemory(i){ memories.splice(i,1); try{localStorage.setItem("gyuu_memories",JSON.stringify(memories));}catch(e){} renderMemory(); toast("🗑️ Memori dihapus"); updateStats(); }

// ===== PERSONA =====
function saveCustomPersona(){ var name=document.getElementById("customName").value.trim(); var p=document.getElementById("customPersona").value.trim(); if(!name&&!p){toast("❌ Isi dulu!");return;} if(name) persona.name=name; if(p) persona.personality=p; chatHistory=[]; savePersonaToStorage(); updateUI(); toast("✨ Persona diterapkan!"); }

// ===== SETTINGS =====
function openSettings(){ document.getElementById("sName").value=persona.name; document.getElementById("sPersona").value=persona.personality; document.getElementById("sLang").value=persona.lang||"auto"; document.getElementById("sStyle").value=persona.style||"casual"; selEmoji=persona.avatar; document.querySelectorAll(".emoji-btn").forEach(function(b){b.classList.toggle("sel",b.textContent===persona.avatar);}); document.getElementById("settingsModal").classList.add("open"); }
function closeSettings(){ document.getElementById("settingsModal").classList.remove("open"); }
function closeModalOut(e){ if(e.target===document.getElementById("settingsModal")) closeSettings(); }
function saveSettings(){ persona.name=document.getElementById("sName").value.trim()||"Gyuu"; persona.personality=document.getElementById("sPersona").value.trim()||persona.personality; persona.avatar=selEmoji; persona.lang=document.getElementById("sLang").value; persona.style=document.getElementById("sStyle").value; chatHistory=[]; savePersonaToStorage(); updateUI(); closeSettings(); toast("✅ Pengaturan disimpan!"); }

// ===== EXPORT =====
function exportChat(){
  if(!chatHistory.length){toast("❌ Belum ada chat");return;}
  var txt=persona.name+" AI — Export Chat\nDibuat oleh: Arkana Rasyid Firjatullah\n"+new Date().toLocaleString("id-ID")+"\n"+"=".repeat(40)+"\n\n";
  chatHistory.forEach(function(m){ txt+="["+(m.role==="user"?"Kamu":persona.name)+"]\n"+(m.content||"")+"\n\n"; });
  var a=document.createElement("a"); a.href=URL.createObjectURL(new Blob([txt],{type:"text/plain"})); a.download=persona.name+"-chat-"+Date.now()+".txt"; a.click(); toast("💾 Chat diekspor!");
}

// ===== CLEAR =====
function clearChat(){
  if(!chatHistory.length) return;
  if(!confirm("Hapus semua chat di sesi ini?")) return;
  chatHistory=[]; totalUserMsg=0; totalAiMsg=0; totalWords=0; sessionStart=new Date();
  if(activeChatId && allChats[activeChatId]){
    allChats[activeChatId].history=[];
    allChats[activeChatId].title="Chat Baru";
    saveAllChats();
  }
  removeImg(); removeLink();
  if(typingAnimId){ clearTimeout(typingAnimId); typingAnimId=null; }
  document.querySelectorAll(".quick-chips").forEach(function(o){o.remove();});
  document.getElementById("messages").innerHTML='<div class="empty-state" id="emptyState"><div class="empty-avatar" id="emptyAv">'+persona.avatar+'</div><div class="empty-title">Halo! Aku <span id="emptyName">'+persona.name+'</span> ✨</div><div class="empty-sub">AI keren buatan Arkana! Siap membantu kamu 24/7.</div><div class="chips"><div class="chip" onclick="sendChip(this)">Siapa kamu?</div><div class="chip" onclick="sendChip(this)">Jokes dong 😂</div><div class="chip" onclick="sendChip(this)">Motivasi hari ini</div><div class="chip" onclick="sendChip(this)">Buat puisi</div></div></div>';
  updateStats(); updateKeyBanner(); updateChatTitle();
}
</script>
</body>
</html>
