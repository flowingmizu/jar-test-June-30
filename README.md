[Jar_Test_Simulator_WithQuiz-2.html](https://github.com/user-attachments/files/30250271/Jar_Test_Simulator_WithQuiz-2.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Jar Test Simulator</title>
<style>
  :root{
    --bg:#0a1822; --panel:#0f2230; --panel-2:#12293a;
    --edge:#23495f; --edge-bright:#3d7ea0;
    --ink:#dcefff; --ink-dim:#8fb4cc; --ink-faint:#5c8299;
    --accent:#46c2a8; --accent-warm:#e3b341;
    --bad:#e06a5c; --good:#5fd38a; --mid:#e3b341;
    --r:14px;
    --mono:'SFMono-Regular',ui-monospace,'Roboto Mono',Menlo,Consolas,monospace;
    --sans:'Segoe UI',Roboto,system-ui,sans-serif;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{
    background:radial-gradient(1200px 600px at 50% -10%,#123047 0%,transparent 60%),var(--bg);
    font-family:var(--sans);color:var(--ink);min-height:100vh;
    display:flex;justify-content:center;align-items:flex-start;padding:24px 16px 60px;
  }
  .wrap{width:100%;max-width:1120px}

  header{display:flex;justify-content:space-between;align-items:baseline;gap:16px;flex-wrap:wrap;
    border-bottom:1px solid var(--edge);padding-bottom:14px;margin-bottom:20px}
  .brand h1{font-weight:600;font-size:1.5rem;letter-spacing:.3px}
  .brand p{color:var(--ink-faint);font-size:.85rem;margin-top:3px}
  .source-pills{display:flex;gap:8px;flex-wrap:wrap}
  .pill{font-family:var(--mono);font-size:.76rem;background:var(--panel-2);
    border:1px solid var(--edge);color:var(--ink-dim);padding:7px 13px;border-radius:40px}
  .pill b{color:var(--accent-warm)}

  .card{background:var(--panel);border:1px solid var(--edge);border-radius:var(--r);padding:18px;margin-bottom:20px}
  .card > h2{font-size:.72rem;text-transform:uppercase;letter-spacing:1.5px;color:var(--ink-faint);
    font-weight:600;margin-bottom:14px}
  .card .h2row{display:flex;justify-content:space-between;align-items:baseline;flex-wrap:wrap;gap:8px;margin-bottom:14px}
  .card .h2row h2{font-size:.72rem;text-transform:uppercase;letter-spacing:1.5px;color:var(--ink-faint);font-weight:600}

  /* gang stirrer status */
  .stirrer-cap{font-size:.66rem;color:var(--ink-faint);text-align:center;margin:2px 0 6px;
    text-transform:uppercase;letter-spacing:1px}
  .stirrer-cap b{color:var(--ink-dim)}
  .stirrer-bar{height:14px;border-radius:8px;margin:0 6px 0;
    background:linear-gradient(90deg,#2b536b,#3d7ea0,#2b536b);
    border:1px solid var(--edge-bright);position:relative;overflow:hidden}
  .stirrer-bar::after{content:"";position:absolute;inset:0;
    background:linear-gradient(90deg,transparent,rgba(255,255,255,.25),transparent);
    transform:translateX(-100%)}
  .stirrer-bar.mixing::after{animation:sweep var(--sweep,1.1s) linear infinite}
  @keyframes sweep{to{transform:translateX(200%)}}

  /* six jars */
  .jar-bank{display:grid;grid-template-columns:repeat(var(--njars,6),1fr);gap:10px;margin-top:8px}
  @media(max-width:780px){.jar-bank{grid-template-columns:repeat(3,1fr);gap:14px}}
  @media(max-width:400px){.jar-bank{grid-template-columns:repeat(2,1fr)}}
  .jar-cell{display:flex;flex-direction:column;align-items:center;gap:6px}
  .jar-cell .jarno{font-size:.7rem;color:var(--ink-faint);font-family:var(--mono)}
  .jar-cell svg{width:100%;max-width:120px;height:auto;display:block}
  .jar-cell.clearest{position:relative}
  .jar-cell.clearest::before{content:"lowest NTU";position:absolute;top:-6px;left:50%;
    transform:translateX(-50%);background:var(--good);color:#06231c;font-size:.58rem;font-weight:700;
    padding:2px 8px;border-radius:30px;white-space:nowrap;z-index:2}

  .dose-wrap{display:flex;flex-direction:column;align-items:center;gap:3px;width:100%}
  .dose-input{width:100%;max-width:90px;background:var(--panel-2);border:1px solid var(--edge);
    border-radius:8px;padding:6px 4px;color:var(--ink);font-family:var(--mono);font-size:.95rem;
    text-align:center;outline:none}
  .dose-input:focus{border-color:var(--edge-bright);box-shadow:0 0 0 2px rgba(61,126,160,.3)}
  .dose-input:disabled{opacity:.5}
  .dose-lbl{font-size:.6rem;color:var(--ink-faint);text-transform:uppercase;letter-spacing:.5px}
  .readout-mini{font-family:var(--mono);font-size:.74rem;color:var(--ink-dim);text-align:center;line-height:1.35;min-height:30px}
  .ntu-num{font-size:.92rem}
  .ntu-num.good{color:var(--good)} .ntu-num.bad{color:var(--bad)} .ntu-num.mid{color:var(--mid)}

  /* controls layout */
  .twocol{display:grid;grid-template-columns:1fr 1fr;gap:18px}
  @media(max-width:680px){.twocol{grid-template-columns:1fr}}
  .ctrl-block{background:var(--panel-2);border:1px solid var(--edge);border-radius:12px;padding:14px 16px}
  .ctrl-block .cb-head{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px}
  .ctrl-block .cb-title{font-size:.86rem;font-weight:600;color:var(--ink)}
  .ctrl-block .cb-sub{font-size:.7rem;color:var(--ink-faint)}
  .slider-row{margin-top:12px}
  .slider-row .sr-top{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px}
  .slider-row label{font-size:.82rem;color:var(--ink-dim)}
  .slider-row .v{font-family:var(--mono);font-size:.95rem;color:var(--ink)}
  .slider-row .v small{color:var(--ink-faint);font-size:.72rem}
  input[type=range]{-webkit-appearance:none;appearance:none;width:100%;height:6px;
    background:linear-gradient(90deg,var(--edge-bright) 0 var(--fill,40%),var(--panel) var(--fill,40%) 100%);
    border-radius:10px;outline:none;border:1px solid var(--edge)}
  input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;appearance:none;width:18px;height:18px;
    border-radius:50%;background:var(--ink);border:3px solid var(--edge-bright);cursor:pointer;box-shadow:0 2px 5px rgba(0,0,0,.5)}
  input[type=range]::-moz-range-thumb{width:18px;height:18px;border-radius:50%;background:var(--ink);
    border:3px solid var(--edge-bright);cursor:pointer}
  input[type=range]:focus-visible{box-shadow:0 0 0 3px rgba(70,194,168,.4)}
  input[type=range]:disabled{opacity:.45;cursor:not-allowed}
  .gband{font-size:.68rem;color:var(--ink-faint);margin-top:5px;font-family:var(--mono)}
  .gband b{color:var(--accent)}

  .controls-row{display:flex;gap:10px;margin-top:18px;flex-wrap:wrap;align-items:center}
  .level-select{display:flex;gap:8px}
  .level-select button{background:var(--panel-2);border:1px solid var(--edge);color:var(--ink-dim);
    padding:8px 12px;border-radius:8px;font-size:.78rem;cursor:pointer;font-weight:600}
  .level-select button.active{background:var(--edge-bright);color:#fff;border-color:var(--ink)}
  .level-select button:disabled{opacity:.5;cursor:not-allowed}
  .spacer{flex:1}
  .btn{border:none;border-radius:10px;padding:12px 18px;font-size:.92rem;font-weight:600;
    cursor:pointer;transition:.12s;font-family:var(--sans)}
  .btn-primary{background:var(--accent);color:#06231c}
  .btn-primary:hover:not(:disabled){filter:brightness(1.08)}
  .btn-ghost{background:transparent;border:1px solid var(--edge);color:var(--ink-dim)}
  .btn-ghost:hover:not(:disabled){border-color:var(--edge-bright);color:var(--ink)}
  .btn:disabled{opacity:.4;cursor:not-allowed}

  .hint{font-size:.74rem;color:var(--ink-faint);margin-top:10px;line-height:1.45}
  .status-line{background:var(--panel-2);border:1px solid var(--edge);border-radius:12px;padding:12px 16px;
    margin-top:16px;min-height:48px;font-size:.9rem;color:var(--ink-dim);line-height:1.45;
    display:flex;align-items:center;gap:10px}
  .phase-dot{width:10px;height:10px;border-radius:50%;background:var(--ink-faint);flex-shrink:0}
  .phase-dot.flash{background:var(--bad);box-shadow:0 0 8px var(--bad)}
  .phase-dot.slow{background:var(--mid);box-shadow:0 0 8px var(--mid)}
  .phase-dot.settle{background:var(--accent);box-shadow:0 0 8px var(--accent)}
  .phase-dot.done{background:var(--good);box-shadow:0 0 8px var(--good)}

  /* graph + table side by side */
  .results-grid{display:grid;grid-template-columns:1.3fr 1fr;gap:18px}
  @media(max-width:780px){.results-grid{grid-template-columns:1fr}}
  #graph{width:100%;height:auto;display:block;background:var(--panel-2);border:1px solid var(--edge);border-radius:12px}
  .res-table{width:100%;border-collapse:collapse;font-family:var(--mono);font-size:.8rem}
  .res-table th{text-align:left;color:var(--ink-faint);font-weight:400;padding:6px 8px;border-bottom:1px solid var(--edge)}
  .res-table td{padding:6px 8px;border-bottom:1px solid var(--edge);color:var(--ink-dim)}
  .res-table tr.best td{color:var(--good)}
  .legend{display:flex;gap:14px;flex-wrap:wrap;font-size:.72rem;color:var(--ink-faint);margin-top:10px}
  .legend span{display:flex;align-items:center;gap:6px}
  .legend i{width:12px;height:12px;border-radius:3px;display:inline-block}

  /* step progress */
  .steps{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:18px;font-size:.72rem;color:var(--ink-faint)}
  .steps .step{background:var(--panel-2);border:1px solid var(--edge);border-radius:30px;padding:6px 14px}
  .steps .step.active{border-color:var(--accent);color:var(--accent)}
  .steps .step.done{border-color:var(--good);color:var(--good)}

  /* continue-to-quiz button */
  .continue-wrap{display:none;justify-content:center;margin:6px 0 20px}
  .continue-wrap .btn{padding:14px 26px;font-size:.95rem}

  /* scenario intro card */
  .scenario-card{border-left:4px solid var(--accent-warm)}
  .scenario-list{margin:10px 0 4px 20px;font-size:.86rem;color:var(--ink-dim);line-height:1.7}
  .scenario-list li{margin-bottom:2px}
  .scenario-list b{color:var(--accent-warm)}

  /* quiz */
  #quizContainer{display:none}
  .quiz-progress{font-size:.72rem;color:var(--ink-faint);margin-bottom:6px;text-transform:uppercase;letter-spacing:1px}
  .quiz-q{font-size:.98rem;color:var(--ink);margin:6px 0 14px;line-height:1.45}
  .quiz-options{display:flex;flex-direction:column;gap:8px;margin-bottom:12px}
  .quiz-options button{text-align:left;background:var(--panel-2);border:1px solid var(--edge);
    color:var(--ink);padding:11px 14px;border-radius:10px;cursor:pointer;font-size:.86rem;font-family:var(--sans)}
  .quiz-options button:hover:not(:disabled){border-color:var(--edge-bright)}
  .quiz-options button:disabled{cursor:not-allowed}
  .quiz-options button.correct{background:rgba(95,211,138,.14);border-color:var(--good);color:var(--good)}
  .quiz-options button.wrong{background:rgba(224,106,92,.14);border-color:var(--bad);color:var(--bad)}
  .quiz-feedback{font-size:.85rem;min-height:20px;margin-bottom:6px}
  .quiz-feedback.correct{color:var(--good)}
  .quiz-feedback.wrong{color:var(--bad)}
  .quiz-score-line{font-size:.76rem;color:var(--ink-faint)}
  .quiz-final{text-align:center;padding:10px 0}
  .quiz-final .big-score{font-size:2rem;font-weight:700;color:var(--accent-warm);margin:10px 0}
  .grade-letter{font-size:3.2rem;font-weight:800;margin:4px 0 2px;line-height:1}
  .grade-letter.grade-A{color:var(--good)}
  .grade-letter.grade-B{color:var(--mid)}
  .grade-letter.grade-Fail{color:var(--bad)}
  .raw-score{font-size:.82rem;color:var(--ink-faint);margin-bottom:6px}
  .gift-box{display:flex;flex-direction:column;align-items:center;gap:4px;margin:16px 0}
  .gift-caption{font-size:.85rem;color:var(--ink-dim)}

  /* jar2 section hidden until unlocked */
  #jar2Wrap{display:none}

  /* setup / choose-your-adventure screen */
  .setup-card{border-left:4px solid var(--accent)}
  .setup-group{margin:16px 0}
  .setup-label{font-size:.86rem;font-weight:600;color:var(--ink);margin-bottom:8px}
  .setup-options{display:flex;gap:8px;flex-wrap:wrap}
  .setup-options button{background:var(--panel-2);border:1px solid var(--edge);color:var(--ink-dim);
    padding:10px 16px;border-radius:10px;font-size:.86rem;font-weight:600;cursor:pointer;font-family:var(--sans)}
  .setup-options button:hover{border-color:var(--edge-bright)}
  .setup-options button.active{background:var(--accent);border-color:var(--accent);color:#06231c}
  .setup-note{font-size:.74rem;color:var(--ink-faint);margin-top:8px;line-height:1.5}
  .setup-summary{background:var(--panel-2);border:1px solid var(--edge);border-radius:10px;
    padding:10px 14px;margin:16px 0;font-size:.82rem;color:var(--ink-dim)}
  .setup-summary b{color:var(--accent-warm)}
  .dose-total{font-size:.62rem;color:var(--ink-faint);font-family:var(--mono)}

  /* ---------- walkable lab room ---------- */
  .lab-room{
    position:relative; width:100%; max-width:560px; height:340px; margin:14px auto 0;
    background-color:#241a10;
    background-image:
      linear-gradient(rgba(255,255,255,.035) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,.035) 1px, transparent 1px);
    background-size:32px 32px;
    border:6px solid #4a3420; border-radius:10px;
    box-shadow:inset 0 0 0 2px #1c130b, inset 0 0 24px rgba(0,0,0,.5);
    outline:none; overflow:hidden; cursor:pointer;
    touch-action:none;
  }
  .lab-room:focus{box-shadow:inset 0 0 0 2px #1c130b, inset 0 0 24px rgba(0,0,0,.5), 0 0 0 3px var(--accent)}

  .station{position:absolute; display:flex; flex-direction:column; align-items:center; gap:4px; width:74px}
  .station-label{font-size:.6rem; color:#cfe6f5; background:rgba(6,20,30,.6); padding:2px 6px; border-radius:6px; white-space:nowrap}
  .station-icon{width:56px; height:56px; position:relative; image-rendering:pixelated}

  .station-shelf{top:14px; left:14px}
  .shelf-icon{background:linear-gradient(#8a6a3f,#8a6a3f 14%,#6b4e2a 14%,#6b4e2a 18%,#8a6a3f 18%,#8a6a3f 46%,#6b4e2a 46%,#6b4e2a 50%,#8a6a3f 50%,#8a6a3f 100%); border:2px solid #3c2a17; border-radius:2px}
  .shelf-icon .bottle{position:absolute; top:6px; left:22px; width:10px; height:16px; background:#5fd38a; border:1px solid #2a5c3f; border-radius:2px}

  .station-cupboard{top:14px; right:14px}
  .cupboard-icon{background:#3d7ea0; border:2px solid #1c4058; border-radius:2px}
  .cupboard-icon::before, .cupboard-icon::after{content:""; position:absolute; top:6px; bottom:6px; width:44%; background:#2b536b; border:1px solid #1c4058}
  .cupboard-icon::before{left:4%}
  .cupboard-icon::after{right:4%}

  .station-tap{bottom:14px; left:50%; transform:translateX(-50%)}
  .tap-icon{background:#5c8299; border:2px solid #2b536b; border-radius:4px 4px 10px 10px}
  .tap-icon .spout{position:absolute; top:-8px; left:50%; transform:translateX(-50%); width:8px; height:14px; background:#7fa8c2; border:1px solid #2b536b; border-radius:2px}

  .station.near .station-icon{filter:brightness(1.35)}
  .station.done .station-label{color:var(--good)}
  .station.done .station-label::after{content:" \2713"}

  .player{position:absolute; width:22px; height:30px; left:250px; top:150px; z-index:5; transition:none}
  .player-head{width:14px; height:12px; background:#e8bd8a; border:1px solid #8a5c34; border-radius:2px; margin:0 auto}
  .player-body{width:20px; height:16px; background:#3d7ea0; border:1px solid #1c4058; border-radius:2px; margin-top:1px}

  .prompt-bubble{
    position:absolute; display:none; transform:translate(-50%,-140%);
    background:var(--ink); color:#06231c; font-size:.66rem; font-weight:700;
    padding:3px 8px; border-radius:8px; white-space:nowrap; z-index:6;
  }
  .prompt-bubble::after{content:""; position:absolute; left:50%; top:100%; transform:translateX(-50%);
    border:5px solid transparent; border-top-color:var(--ink)}

  .checklist{display:flex; gap:14px; justify-content:center; margin:14px 0 6px; font-size:.8rem; color:var(--ink-faint)}
  .check-item.done{color:var(--good); font-weight:600}

  /* station interaction modal */
  .station-modal-backdrop{
    position:fixed; inset:0; background:rgba(4,12,18,.72); z-index:50;
    display:flex; align-items:center; justify-content:center; padding:20px;
  }
  .station-modal{
    background:var(--panel); border:1px solid var(--edge-bright); border-radius:14px;
    padding:22px; max-width:360px; width:100%; text-align:center;
  }
  .station-modal h3{font-size:1rem; color:var(--ink); margin-bottom:10px}
  .station-modal p{font-size:.82rem; color:var(--ink-dim); margin-bottom:14px; line-height:1.5}
  .station-modal .setup-options{justify-content:center}

  /* PRE-QUIZ */
  .prequiz-card{border-left:4px solid var(--accent-warm)}
  .prequiz-card h2{font-size:1.1rem;color:var(--ink);margin-bottom:6px;text-transform:none;letter-spacing:0}
  .prequiz-sub{font-size:.82rem;color:var(--ink-dim);margin-bottom:20px;line-height:1.5}
  .pq-question{margin-bottom:22px}
  .pq-num{font-size:.7rem;color:var(--accent-warm);font-family:var(--mono);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px}
  .pq-text{font-size:.92rem;color:var(--ink);font-weight:600;margin-bottom:10px;line-height:1.5}
  .pq-options{display:flex;flex-direction:column;gap:6px}
  .pq-options button{background:var(--panel-2);border:1px solid var(--edge);color:var(--ink-dim);
    padding:10px 14px;border-radius:10px;font-size:.84rem;cursor:pointer;font-family:var(--sans);
    text-align:left;transition:border-color .2s,background .2s}
  .pq-options button:hover:not(:disabled){border-color:var(--edge-bright)}
  .pq-options button.correct{background:rgba(95,211,138,.15);border-color:var(--good);color:var(--good);opacity:1!important}
  .pq-options button.wrong{background:rgba(224,106,92,.1);border-color:var(--bad);color:var(--bad)}
  .pq-options button:disabled{cursor:default;opacity:.5}
  .pq-feedback{font-size:.78rem;margin-top:8px;line-height:1.5;padding:8px 12px;border-radius:8px;display:none}
  .pq-feedback.show{display:block}
  .pq-feedback.fb-right{background:rgba(95,211,138,.1);color:var(--good)}
  .pq-feedback.fb-wrong{background:rgba(224,106,92,.08);color:var(--bad)}
  .pq-score{text-align:center;margin-top:16px;font-size:.9rem;color:var(--ink-dim);display:none}
  .pq-score b{color:var(--accent-warm)}
  #continueSimBtn{display:none;margin-top:16px}

  /* REWARDS */
  .reward-card{text-align:center;margin-top:20px;padding:20px;background:var(--panel-2);border:1px solid var(--edge);border-radius:12px;display:none}
  .reward-card.show{display:block;animation:rewardPop .5s ease-out}
  @keyframes rewardPop{0%{transform:scale(0.5);opacity:0}50%{transform:scale(1.1)}100%{transform:scale(1);opacity:1}}
  .reward-icon{font-size:64px;margin-bottom:12px}
  .reward-name{font-size:1rem;font-weight:700;color:var(--accent-warm);font-family:var(--mono);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:4px}
  .reward-desc{font-size:.8rem;color:var(--ink-dim);line-height:1.5}
  .reward-card.gold{border-color:var(--good);box-shadow:0 0 20px rgba(95,211,138,.2)}
  .reward-card.silver{border-color:var(--accent-warm);box-shadow:0 0 20px rgba(227,179,65,.15)}
  .reward-card.poop-tier{border-color:var(--bad);box-shadow:0 0 20px rgba(224,106,92,.15)}

  /* ===== MY LAB COLLECTION ===== */
  .lab-btn{position:fixed;top:16px;right:16px;z-index:100;background:var(--panel);border:1px solid var(--edge-bright);
    color:var(--ink);padding:8px 16px;border-radius:30px;font-family:var(--mono);font-size:.82rem;font-weight:600;
    cursor:pointer;display:flex;align-items:center;gap:8px;transition:box-shadow .2s,transform .2s}
  .lab-btn:hover{box-shadow:0 0 16px rgba(70,194,168,.25);transform:translateY(-1px)}
  .lab-btn .lab-count{color:var(--accent);font-weight:700}
  .lab-btn .poop-badge{font-size:.72rem;color:var(--bad);margin-left:4px}
  .lab-btn.pulse{animation:labPulse .6s ease-out}
  @keyframes labPulse{0%{box-shadow:0 0 0 0 rgba(70,194,168,.5)}70%{box-shadow:0 0 0 12px rgba(70,194,168,0)}100%{box-shadow:none}}

  .lab-panel{position:fixed;top:52px;right:16px;z-index:99;background:var(--panel);border:1px solid var(--edge-bright);
    border-radius:var(--r);padding:20px;width:340px;max-height:80vh;overflow-y:auto;
    transform:translateY(-10px);opacity:0;pointer-events:none;transition:transform .25s,opacity .25s;
    box-shadow:0 8px 32px rgba(0,0,0,.4)}
  .lab-panel.open{transform:translateY(0);opacity:1;pointer-events:all}
  .lab-panel h3{font-size:.9rem;color:var(--ink);margin-bottom:4px;font-weight:600}
  .lab-panel .lab-subtitle{font-size:.72rem;color:var(--ink-faint);margin-bottom:16px}

  .lab-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:8px}
  .lab-item{display:flex;flex-direction:column;align-items:center;gap:4px;padding:10px 4px;
    background:var(--panel-2);border:1px solid var(--edge);border-radius:10px;transition:border-color .3s,box-shadow .3s}
  .lab-item.unlocked{border-color:var(--accent);box-shadow:0 0 8px rgba(70,194,168,.15)}
  .lab-item.locked{opacity:.35;filter:grayscale(1)}
  .lab-item .li-icon{font-size:28px;height:34px;display:flex;align-items:center;justify-content:center}
  .lab-item .li-name{font-size:.58rem;color:var(--ink-dim);text-align:center;line-height:1.2;font-family:var(--mono)}
  .lab-item.unlocked .li-name{color:var(--accent)}
  .lab-item.just-unlocked{animation:itemBounce .5s ease-out}
  @keyframes itemBounce{0%{transform:scale(0.5)}50%{transform:scale(1.2)}100%{transform:scale(1)}}

  .lab-complete{text-align:center;margin-top:14px;padding:12px;background:rgba(95,211,138,.08);
    border:1px solid var(--good);border-radius:10px;display:none}
  .lab-complete.show{display:block;animation:rewardPop .5s ease-out}
  .lab-complete h4{font-size:.85rem;color:var(--good);margin-bottom:4px}
  .lab-complete p{font-size:.72rem;color:var(--ink-dim)}

  .lab-poop-counter{text-align:center;margin-top:10px;font-size:.75rem;color:var(--bad);font-family:var(--mono)}

  /* Toast notification */
  .lab-toast{position:fixed;bottom:24px;right:24px;z-index:200;background:var(--panel);border:1px solid var(--accent);
    border-radius:12px;padding:14px 20px;display:flex;align-items:center;gap:12px;
    box-shadow:0 8px 28px rgba(0,0,0,.4);transform:translateY(80px);opacity:0;transition:transform .4s,opacity .4s;
    pointer-events:none}
  .lab-toast.show{transform:translateY(0);opacity:1}
  .lab-toast .toast-icon{font-size:32px}
  .lab-toast .toast-text{font-size:.82rem;color:var(--ink);font-weight:600}
  .lab-toast .toast-sub{font-size:.7rem;color:var(--ink-dim);margin-top:2px}
</style>
</head>
<body>
<!-- MY LAB COLLECTION UI -->
<button class="lab-btn" id="labBtn" onclick="window.labCollection.togglePanel()">
  🧪 My Lab: <span class="lab-count" id="labCount">0</span>/12
  <span class="poop-badge" id="poopBadge"></span>
</button>
<div class="lab-panel" id="labPanel">
  <h3>🧪 My Lab Equipment</h3>
  <div class="lab-subtitle">Collect all 12 items to become Lab Certified</div>
  <div class="lab-grid" id="labGrid"></div>
  <div class="lab-poop-counter" id="labPoopCounter"></div>
  <div class="lab-complete" id="labComplete">
    <h4>🏆 Lab Certified!</h4>
    <p>You've collected every piece of lab equipment. You're ready to treat real water.</p>
  </div>
</div>
<div class="lab-toast" id="labToast">
  <span class="toast-icon" id="toastIcon"></span>
  <div><div class="toast-text" id="toastText"></div><div class="toast-sub" id="toastSub"></div></div>
</div>





<div class="wrap">
  <header>
    <div class="brand">
      <h1>Jar Test Simulator</h1>
      <p>Operate the gang stirrer, dose each jar, and observe how mixing and settling affect the water.</p>
    </div>
    <div class="source-pills">
      <span class="pill">RAW <b id="rawNtuPill">--</b> NTU</span>
      <span class="pill">pH <b id="rawPhPill">--</b></span>
      <span class="pill">temp <b id="tempPill">20</b>&deg;C</span>
    </div>
  </header>

  <div class="steps">
    <span class="step active" id="stepJar1">① Jar Test #1</span>
    <span class="step" id="stepQuiz">② Pre-Test #2 Quiz</span>
    <span class="step" id="stepJar2">③ Jar Test #2 — Spring Snowmelt</span>
    <span class="step" id="stepQuiz3">④ Pre-Test #3 Quiz</span>
    <span class="step" id="stepJar3">⑤ Jar Test #3 — Autumn Rain</span>
    <span class="step" id="stepQuiz4">⑥ Pre-Test #4 Quiz</span>
    <span class="step" id="stepJar4">⑦ Jar Test #4 — Summer Drought</span>
  </div>

  <div id="jar1Wrap">
  <!-- SETUP -->
  <div class="card setup-card" id="setupCard1">
    <h2>Set up your jar test</h2>
    <p class="hint">Walk around the prep room. Grab alum from the shelf, pick your jars from the cupboard, and fill them at the tap — then begin.</p>
    <p class="hint" style="margin-top:2px">Move: <b>arrow keys / WASD</b> &nbsp;·&nbsp; Interact: <b>E</b> (or tap a station on touch screens)</p>
    <div class="lab-room" id="labRoom1" tabindex="0">
      <div class="station station-shelf" id="shelf1" data-station="shelf"><div class="station-icon shelf-icon"><span class="bottle"></span></div><div class="station-label">Alum shelf</div></div>
      <div class="station station-cupboard" id="cupboard1" data-station="cupboard"><div class="station-icon cupboard-icon"></div><div class="station-label">Cupboard</div></div>
      <div class="station station-tap" id="tap1" data-station="tap"><div class="station-icon tap-icon"><span class="spout"></span></div><div class="station-label">Water tap</div></div>
      <div class="player" id="player1"><div class="player-head"></div><div class="player-body"></div></div>
      <div class="prompt-bubble" id="prompt1">Press E</div>
    </div>
    <div class="checklist" id="checklist1">
      <span class="check-item" id="checkAlum1">○ Alum</span>
      <span class="check-item" id="checkJars1">○ Jars</span>
      <span class="check-item" id="checkWater1">○ Water</span>
    </div>
    <button class="btn btn-primary" id="beginJar1Btn" disabled>Begin experiment &#9654;</button>
  </div>

  <!-- PRE-QUIZ (shown after setup, before sim) -->
  <div class="card prequiz-card" id="preQuizCard" style="display:none">
    <h2>Quick knowledge check</h2>
    <p class="prequiz-sub">You've collected your equipment. Before you start dosing, let's make sure you understand the basics.</p>

    <div class="pq-question" id="pq1">
      <div class="pq-num">Question 1 of 3</div>
      <div class="pq-text">What is alum (aluminium sulfate) used for in water treatment?</div>
      <div class="pq-options" id="pq1opts">
        <button data-a="wrong">It kills bacteria by disinfecting the water</button>
        <button data-a="correct">It acts as a coagulant that destabilises particles so they clump together</button>
        <button data-a="wrong">It adjusts the pH to make the water taste better</button>
        <button data-a="wrong">It filters out large debris like leaves and sticks</button>
      </div>
      <div class="pq-feedback" id="pq1fb"></div>
    </div>

    <div class="pq-question" id="pq2">
      <div class="pq-num">Question 2 of 3</div>
      <div class="pq-text">What is the purpose of a jar test?</div>
      <div class="pq-options" id="pq2opts">
        <button data-a="wrong">To measure the temperature of raw water samples</button>
        <button data-a="wrong">To test whether water is safe to drink directly</button>
        <button data-a="correct">To determine the optimal coagulant dose by comparing different doses side by side</button>
        <button data-a="wrong">To count the number of bacteria present in the water</button>
      </div>
      <div class="pq-feedback" id="pq2fb"></div>
    </div>

    <div class="pq-question" id="pq3">
      <div class="pq-num">Question 3 of 3</div>
      <div class="pq-text">What is the difference between coagulation and flocculation?</div>
      <div class="pq-options" id="pq3opts">
        <button data-a="wrong">They are the same process, just different names</button>
        <button data-a="wrong">Coagulation removes colour and flocculation removes odour</button>
        <button data-a="correct">Coagulation destabilises particles with chemicals; flocculation gently mixes them so they form larger clumps (floc)</button>
        <button data-a="wrong">Coagulation is done with filters and flocculation is done with UV light</button>
      </div>
      <div class="pq-feedback" id="pq3fb"></div>
    </div>

    <div class="pq-score" id="pqScore">You scored <b id="pqScoreNum">0</b>/3</div>
    <div class="reward-card" id="rewardCard">
      <div class="reward-icon" id="rewardIcon"></div>
      <div class="reward-name" id="rewardName"></div>
      <div class="reward-desc" id="rewardDesc"></div>
    </div>
    <button class="btn btn-primary" id="continueSimBtn">Continue to jar test &#9654;</button>
  </div>

  <div id="jar1Sim" style="display:none">
  <!-- JARS -->
  <div class="card">
    <div class="h2row">
      <h2 id="jar1Title">Gang stirrer — six 1 L jars</h2>
      <div class="level-select" id="levelSelect">
        <button data-lvl="easy" class="active">Easy water</button>
        <button data-lvl="normal">Normal</button>
        <button data-lvl="hard">Hard water</button>
      </div>
    </div>
    <div class="stirrer-cap">paddle speed <b id="rpmCap">stopped</b> &middot; phase <b id="phaseCap">idle</b></div>
    <div class="stirrer-bar" id="stirrerBar"></div>
    <div class="jar-bank" id="jarBank"></div>
    <p class="hint">Type an alum dose under each jar (mg/L). A spread from low to high lets you find the optimal dose. Then set your mixing programme below and start the run.</p>
  </div>

  <!-- MIXING CONTROLS -->
  <div class="card">
    <h2>Mixing programme — applied to all six jars together</h2>
    <div class="twocol">
      <div class="ctrl-block">
        <div class="cb-head"><span class="cb-title">Rapid mix (flash)</span><span class="cb-sub">disperses the coagulant</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="flashRpm">Paddle speed</label><span class="v"><span id="flashRpmV">200</span> <small>rpm</small></span></div>
          <input type="range" id="flashRpm" min="50" max="300" value="200" step="10">
          <div class="gband">target band for good dispersion: <b>150–250 rpm</b></div>
        </div>
        <div class="slider-row">
          <div class="sr-top"><label for="flashTime">Duration</label><span class="v"><span id="flashTimeV">60</span> <small>s</small></span></div>
          <input type="range" id="flashTime" min="10" max="180" value="60" step="5">
          <div class="gband">target: at least <b>30 s</b> to fully disperse</div>
        </div>
      </div>

      <div class="ctrl-block">
        <div class="cb-head"><span class="cb-title">Slow mix (flocculation)</span><span class="cb-sub">grows the floc</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="slowRpm">Paddle speed</label><span class="v"><span id="slowRpmV">40</span> <small>rpm</small></span></div>
          <input type="range" id="slowRpm" min="10" max="120" value="40" step="5">
          <div class="gband">sweet spot: <b>20–50 rpm</b> — too fast shears floc apart</div>
        </div>
        <div class="slider-row">
          <div class="sr-top"><label for="slowTime">Duration</label><span class="v"><span id="slowTimeV">15</span> <small>min</small></span></div>
          <input type="range" id="slowTime" min="2" max="30" value="15" step="1">
          <div class="gband">target: <b>10–20 min</b> for floc to grow</div>
        </div>
      </div>
    </div>

    <div class="ctrl-block" style="margin-top:18px">
      <div class="cb-head"><span class="cb-title">Settling</span><span class="cb-sub">floc sinks, leaving clear water on top</span></div>
      <div class="slider-row">
        <div class="sr-top"><label for="settleTime">Settle time</label><span class="v"><span id="settleTimeV">20</span> <small>min</small></span></div>
        <input type="range" id="settleTime" min="1" max="60" value="20" step="1">
        <div class="gband">longer settling = more floc removed before you read the top water. Diminishing returns after ~<b>30 min</b>.</div>
      </div>
    </div>

    <div class="controls-row">
      <button class="btn btn-ghost" id="spreadBtn">Auto-spread doses</button>
      <button class="btn btn-ghost" id="resetBtn">New raw water</button>
      <div class="spacer"></div>
      <button class="btn btn-ghost" id="standardBtn">Standard programme</button>
      <button class="btn btn-primary" id="runBtn">Start run &#9654;</button>
    </div>

    <div class="status-line" id="statusLine"><span class="phase-dot" id="phaseDot"></span><span id="statusText">Set doses and a mixing programme, then start the run. The whole sequence is compressed to about 12 seconds.</span></div>
  </div>

  <!-- RESULTS -->
  <div class="card">
    <h2>Results — supernatant turbidity after settling</h2>
    <div class="results-grid">
      <div>
        <svg id="graph" viewBox="0 0 460 300" role="img" aria-label="Graph of final turbidity versus alum dose"></svg>
        <div class="legend">
          <span><i style="background:var(--accent)"></i> this run</span>
          <span><i style="background:#3a5d72"></i> previous runs (ghosted)</span>
          <span><i style="background:var(--good)"></i> lowest turbidity</span>
        </div>
      </div>
      <div>
        <table class="res-table">
          <thead><tr><th>Jar</th><th>Dose</th><th>Floc</th><th>NTU</th></tr></thead>
          <tbody id="resBody"><tr><td colspan="4" style="color:var(--ink-faint);text-align:center;padding:14px">Run the jars to see results.</td></tr></tbody>
        </table>
        <p class="hint" id="interpHint"></p>
      </div>
    </div>
  </div>
  </div> <!-- /jar1Sim -->
  </div> <!-- /jar1Wrap -->

  <!-- CONTINUE TO QUIZ -->
  <div class="continue-wrap" id="continueToQuizWrap">
    <button class="btn btn-primary" id="continueToQuizBtn">Continue to Pre-Test #2 Quiz →</button>
  </div>

  <!-- QUIZ GATE -->
  <div class="card" id="quizContainer"></div>

  <!-- JAR TEST 2 -->
  <div id="jar2Wrap">
    <div class="card scenario-card">
      <h2>Scenario — Spring Snowmelt Raw Water Profile</h2>
      <p style="font-size:.86rem;color:var(--ink-dim)">The plant's intake has just picked up spring snowmelt. Conditions are tougher than the water you just tested:</p>
      <ul class="scenario-list">
        <li><b>Temperature:</b> 5&deg;C (cold) — increases viscosity and slows floc formation</li>
        <li><b>pH:</b> 6.2 (low) — reduces alum effectiveness</li>
        <li><b>Turbidity:</b> 80 NTU (high)</li>
      </ul>
      <p style="font-size:.86rem;color:var(--ink-dim);margin-top:8px">Expect to need a <b style="color:var(--accent-warm)">higher alum dose</b> than usual, and a <b style="color:var(--accent-warm)">narrower slow-mix rpm window</b> — cold, viscous water shears floc apart more easily outside the sweet spot.</p>
    </div>

    <!-- SETUP 2 -->
    <div class="card setup-card" id="setupCard2">
      <h2>Set up your jar test</h2>
      <p class="hint">Same decision as before — but this water is less forgiving of a poor setup.</p>
      <p class="hint" style="margin-top:2px">Move: <b>arrow keys / WASD</b> &nbsp;·&nbsp; Interact: <b>E</b> (or tap a station on touch screens)</p>
      <div class="lab-room" id="labRoom2" tabindex="0">
        <div class="station station-shelf" id="shelf2" data-station="shelf"><div class="station-icon shelf-icon"><span class="bottle"></span></div><div class="station-label">Alum shelf</div></div>
        <div class="station station-cupboard" id="cupboard2" data-station="cupboard"><div class="station-icon cupboard-icon"></div><div class="station-label">Cupboard</div></div>
        <div class="station station-tap" id="tap2" data-station="tap"><div class="station-icon tap-icon"><span class="spout"></span></div><div class="station-label">Water tap</div></div>
        <div class="player" id="player2"><div class="player-head"></div><div class="player-body"></div></div>
        <div class="prompt-bubble" id="prompt2">Press E</div>
      </div>
      <div class="checklist" id="checklist2">
        <span class="check-item" id="checkAlum2">○ Alum</span>
        <span class="check-item" id="checkJars2">○ Jars</span>
        <span class="check-item" id="checkWater2">○ Water</span>
      </div>
      <button class="btn btn-primary" id="beginJar2Btn" disabled>Begin experiment &#9654;</button>
    </div>

    <div id="jar2Sim" style="display:none">
    <!-- JARS 2 -->
    <div class="card">
      <div class="h2row">
        <h2 id="jar2Title">Gang stirrer — six 1 L jars (Spring Snowmelt)</h2>
        <div class="source-pills">
          <span class="pill">RAW <b id="rawNtuPill2">--</b> NTU</span>
          <span class="pill">pH <b id="rawPhPill2">--</b></span>
          <span class="pill">temp <b id="tempPill2">--</b>&deg;C</span>
        </div>
      </div>
      <div class="stirrer-cap">paddle speed <b id="rpmCap2">stopped</b> &middot; phase <b id="phaseCap2">idle</b></div>
      <div class="stirrer-bar" id="stirrerBar2"></div>
      <div class="jar-bank" id="jarBank2"></div>
      <p class="hint">Cold, low-pH water usually needs more alum than you'd expect from turbidity alone. Type a dose under each jar, then set your mixing programme and start the run.</p>
    </div>

    <!-- MIXING CONTROLS 2 -->
    <div class="card">
      <h2>Mixing programme — applied to all six jars together</h2>
      <div class="twocol">
        <div class="ctrl-block">
          <div class="cb-head"><span class="cb-title">Rapid mix (flash)</span><span class="cb-sub">disperses the coagulant</span></div>
          <div class="slider-row">
            <div class="sr-top"><label for="flashRpm2">Paddle speed</label><span class="v"><span id="flashRpmV2">220</span> <small>rpm</small></span></div>
            <input type="range" id="flashRpm2" min="50" max="300" value="220" step="10">
            <div class="gband">cold water resists dispersion — target band: <b>170–260 rpm</b></div>
          </div>
          <div class="slider-row">
            <div class="sr-top"><label for="flashTime2">Duration</label><span class="v"><span id="flashTimeV2">70</span> <small>s</small></span></div>
            <input type="range" id="flashTime2" min="10" max="180" value="70" step="5">
            <div class="gband">target: at least <b>45 s</b> — viscosity slows dispersion</div>
          </div>
        </div>

        <div class="ctrl-block">
          <div class="cb-head"><span class="cb-title">Slow mix (flocculation)</span><span class="cb-sub">grows the floc</span></div>
          <div class="slider-row">
            <div class="sr-top"><label for="slowRpm2">Paddle speed</label><span class="v"><span id="slowRpmV2">30</span> <small>rpm</small></span></div>
            <input type="range" id="slowRpm2" min="10" max="120" value="30" step="5">
            <div class="gband">narrower sweet spot in cold water: <b>25–38 rpm</b></div>
          </div>
          <div class="slider-row">
            <div class="sr-top"><label for="slowTime2">Duration</label><span class="v"><span id="slowTimeV2">20</span> <small>min</small></span></div>
            <input type="range" id="slowTime2" min="2" max="30" value="20" step="1">
            <div class="gband">target: <b>15–25 min</b> — cold water floc grows more slowly</div>
          </div>
        </div>
      </div>

      <div class="ctrl-block" style="margin-top:18px">
        <div class="cb-head"><span class="cb-title">Settling</span><span class="cb-sub">floc sinks, leaving clear water on top</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="settleTime2">Settle time</label><span class="v"><span id="settleTimeV2">25</span> <small>min</small></span></div>
          <input type="range" id="settleTime2" min="1" max="60" value="25" step="1">
          <div class="gband">cold, viscous water settles more slowly. Diminishing returns after ~<b>40 min</b>.</div>
        </div>
      </div>

      <div class="controls-row">
        <button class="btn btn-ghost" id="spreadBtn2">Auto-spread doses</button>
        <button class="btn btn-ghost" id="resetBtn2">New snowmelt sample</button>
        <div class="spacer"></div>
        <button class="btn btn-ghost" id="standardBtn2">Standard programme</button>
        <button class="btn btn-primary" id="runBtn2">Start run &#9654;</button>
      </div>

      <div class="status-line" id="statusLine2"><span class="phase-dot" id="phaseDot2"></span><span id="statusText2">Set doses and a mixing programme, then start the run.</span></div>
    </div>

    <!-- RESULTS 2 -->
    <div class="card">
      <h2>Results — supernatant turbidity after settling</h2>
      <div class="results-grid">
        <div>
          <svg id="graph2" viewBox="0 0 460 300" role="img" aria-label="Graph of final turbidity versus alum dose"></svg>
          <div class="legend">
            <span><i style="background:var(--accent)"></i> this run</span>
            <span><i style="background:#3a5d72"></i> previous runs (ghosted)</span>
            <span><i style="background:var(--good)"></i> lowest turbidity</span>
          </div>
        </div>
        <div>
          <table class="res-table">
            <thead><tr><th>Jar</th><th>Dose</th><th>Floc</th><th>NTU</th></tr></thead>
            <tbody id="resBody2"><tr><td colspan="4" style="color:var(--ink-faint);text-align:center;padding:14px">Run the jars to see results.</td></tr></tbody>
          </table>
          <p class="hint" id="interpHint2"></p>
        </div>
      </div>
    </div>
    </div> <!-- /jar2Sim -->
  </div> <!-- /jar2Wrap -->

<!-- ============================================================ -->
<!-- JAR TEST 3: AUTUMN RAIN QUIZ                                  -->
<!-- ============================================================ -->
<div id="autumnQuizWrap" style="display:none">
  <div class="card prequiz-card" id="autumnQuizCard">
    <h2 style="text-transform:none;letter-spacing:0;">Autumn Rain — Knowledge Check</h2>
    <p class="prequiz-sub">Heavy autumn rain changes the source water. Before you treat it, let's check you understand why.</p>

    <div class="pq-question" id="aq1">
      <div class="pq-num">Question 1 of 3</div>
      <div class="pq-text">What happens to source water during heavy autumn rain?</div>
      <div class="pq-options aq-opts" data-q="1">
        <button data-a="wrong">The water becomes warmer and easier to treat</button>
        <button data-a="correct">Rain washes organic acids into the source, lowering pH and alkalinity</button>
        <button data-a="wrong">The turbidity drops because rain dilutes the particles</button>
        <button data-a="wrong">The water becomes more alkaline from dissolved minerals</button>
      </div>
      <div class="pq-feedback" id="aq1fb"></div>
    </div>

    <div class="pq-question" id="aq2">
      <div class="pq-num">Question 2 of 3</div>
      <div class="pq-text">Why might you need to add lime or soda ash before adding alum?</div>
      <div class="pq-options aq-opts" data-q="2">
        <button data-a="wrong">To make the water taste better</button>
        <button data-a="wrong">To kill bacteria before coagulation</button>
        <button data-a="correct">To raise the pH and alkalinity so alum can work effectively</button>
        <button data-a="wrong">To reduce the temperature of the water</button>
      </div>
      <div class="pq-feedback" id="aq2fb"></div>
    </div>

    <div class="pq-question" id="aq3">
      <div class="pq-num">Question 3 of 3</div>
      <div class="pq-text">What challenge does low pH create for alum coagulation?</div>
      <div class="pq-options aq-opts" data-q="3">
        <button data-a="wrong">Alum dissolves too slowly in acidic water</button>
        <button data-a="correct">Alum works best in a pH range of about 6 to 7.5; below that, coagulation becomes less effective</button>
        <button data-a="wrong">Low pH causes alum to evaporate from the jars</button>
        <button data-a="wrong">Low pH makes the water too clear to need treatment</button>
      </div>
      <div class="pq-feedback" id="aq3fb"></div>
    </div>

    <div class="pq-score" id="aqScore">You scored <b id="aqScoreNum">0</b>/3</div>
    <div class="reward-card" id="aqRewardCard">
      <div class="reward-icon" id="aqRewardIcon"></div>
      <div class="reward-name" id="aqRewardName"></div>
      <div class="reward-desc" id="aqRewardDesc"></div>
    </div>
    <button class="btn btn-primary" id="continueAutumnBtn" style="display:none;margin-top:16px">Continue to Autumn Jar Test &#9654;</button>
  </div>
</div>

<!-- ============================================================ -->
<!-- JAR TEST 3: AUTUMN RAIN SCENARIO                              -->
<!-- ============================================================ -->
<div id="jar3Wrap" style="display:none">
  <div class="card setup-card" id="setupCard3">
    <h2>Set up your jar test — Autumn Rain</h2>
    <p class="hint">Walk around the prep room. This time you also need to consider pH buffering — grab lime from the shelf alongside the alum.</p>
    <p class="hint" style="margin-top:2px">Move: <b>arrow keys / WASD</b> &nbsp;·&nbsp; Interact: <b>E</b> (or tap a station on touch screens)</p>
    <div class="lab-room" id="labRoom3" tabindex="0">
      <div class="station station-shelf" id="shelf3" data-station="shelf"><div class="station-icon shelf-icon"><span class="bottle"></span></div><div class="station-label">Alum + Lime</div></div>
      <div class="station station-cupboard" id="cupboard3" data-station="cupboard"><div class="station-icon cupboard-icon"></div><div class="station-label">Cupboard</div></div>
      <div class="station station-tap" id="tap3" data-station="tap"><div class="station-icon tap-icon"><span class="spout"></span></div><div class="station-label">Water tap</div></div>
      <div class="player" id="player3"><div class="player-head"></div><div class="player-body"></div></div>
      <div class="prompt-bubble" id="prompt3">Press E</div>
    </div>
    <div class="checklist" id="checklist3">
      <span class="check-item" id="checkAlum3">○ Alum + Lime</span>
      <span class="check-item" id="checkJars3">○ Jars</span>
      <span class="check-item" id="checkWater3">○ Water</span>
    </div>
    <button class="btn btn-primary" id="beginJar3Btn" disabled>Begin experiment &#9654;</button>
  </div>

  <div id="jar3Sim" style="display:none">
  <div class="card">
    <div class="h2row">
      <h2 id="jar3Title">Gang stirrer — six 1 L jars (Autumn Rain)</h2>
      <div class="source-pills">
        <span class="pill">RAW <b id="rawNtuPill3">--</b> NTU</span>
        <span class="pill">pH <b id="rawPhPill3">--</b></span>
        <span class="pill">temp <b id="tempPill3">15</b>&deg;C</span>
      </div>
    </div>

    <!-- pH BUFFER CONTROL -->
    <div class="ctrl-block" style="margin-bottom:14px;border-color:var(--accent-warm)">
      <div class="cb-head"><span class="cb-title" style="color:var(--accent-warm)">pH Buffer — Lime (CaO) dose</span><span class="cb-sub">raises pH before adding alum</span></div>
      <div class="slider-row">
        <div class="sr-top"><label for="limeDose3">Lime dose</label><span class="v"><span id="limeDoseV3">0</span> <small>mg/L</small></span></div>
        <input type="range" id="limeDose3" min="0" max="40" value="0" step="2">
        <div class="gband">effective pH: <b id="effectivePh3">5.8</b> &nbsp;·&nbsp; alum sweet spot: <b>pH 6.0–7.5</b></div>
      </div>
    </div>

    <div class="stirrer-cap">paddle speed <b id="rpmCap3">stopped</b> &middot; phase <b id="phaseCap3">idle</b></div>
    <div class="stirrer-bar" id="stirrerBar3"></div>
    <div class="jar-bank" id="jarBank3"></div>
    <p class="hint">Set your lime dose first to raise the pH, then type an alum dose under each jar (mg/L). Alum works best between pH 6.0 and 7.5.</p>
  </div>

  <div class="card">
    <h2>Mixing programme — applied to all six jars together</h2>
    <div class="twocol">
      <div class="ctrl-block">
        <div class="cb-head"><span class="cb-title">Rapid mix (flash)</span><span class="cb-sub">disperses coagulant + buffer</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="flashRpm3">Paddle speed</label><span class="v"><span id="flashRpmV3">200</span> <small>rpm</small></span></div>
          <input type="range" id="flashRpm3" min="50" max="300" value="200" step="10">
          <div class="gband">target: <b>150–250 rpm</b></div>
        </div>
        <div class="slider-row">
          <div class="sr-top"><label for="flashTime3">Duration</label><span class="v"><span id="flashTimeV3">60</span> <small>s</small></span></div>
          <input type="range" id="flashTime3" min="10" max="180" value="60" step="5">
          <div class="gband">target: at least <b>30 s</b></div>
        </div>
      </div>
      <div class="ctrl-block">
        <div class="cb-head"><span class="cb-title">Slow mix (flocculation)</span><span class="cb-sub">grows the floc</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="slowRpm3">Paddle speed</label><span class="v"><span id="slowRpmV3">40</span> <small>rpm</small></span></div>
          <input type="range" id="slowRpm3" min="10" max="120" value="40" step="5">
          <div class="gband">sweet spot: <b>20–50 rpm</b></div>
        </div>
        <div class="slider-row">
          <div class="sr-top"><label for="slowTime3">Duration</label><span class="v"><span id="slowTimeV3">15</span> <small>min</small></span></div>
          <input type="range" id="slowTime3" min="2" max="30" value="15" step="1">
          <div class="gband">target: <b>10–20 min</b></div>
        </div>
      </div>
    </div>
    <div class="ctrl-block" style="margin-top:18px">
      <div class="cb-head"><span class="cb-title">Settling</span><span class="cb-sub">floc sinks</span></div>
      <div class="slider-row">
        <div class="sr-top"><label for="settleTime3">Settle time</label><span class="v"><span id="settleTimeV3">20</span> <small>min</small></span></div>
        <input type="range" id="settleTime3" min="1" max="60" value="20" step="1">
        <div class="gband">diminishing returns after ~<b>30 min</b></div>
      </div>
    </div>
    <div class="controls-row">
      <button class="btn btn-ghost" id="spreadBtn3">Auto-spread doses</button>
      <button class="btn btn-ghost" id="resetBtn3">New autumn water</button>
      <div class="spacer"></div>
      <button class="btn btn-ghost" id="standardBtn3">Standard programme</button>
      <button class="btn btn-primary" id="runBtn3">Start run &#9654;</button>
    </div>
    <div class="status-line" id="statusLine3"><span class="phase-dot" id="phaseDot3"></span><span id="statusText3">Set lime dose to raise pH, set alum doses, configure mixing, then start.</span></div>
  </div>

  <div class="card">
    <h2>Results — supernatant turbidity after settling</h2>
    <div class="results-grid">
      <div>
        <svg id="graph3" viewBox="0 0 460 300" role="img" aria-label="Graph of final turbidity versus alum dose"></svg>
      </div>
      <div>
        <table class="res-table"><thead><tr><th>Jar</th><th>Dose mg/L</th><th>Final NTU</th><th>% Removed</th></tr></thead>
        <tbody id="resBody3"></tbody></table>
        <p class="interp-hint" id="interpHint3"></p>
      </div>
    </div>
  </div>
  </div>
</div>


<!-- ============================================================ -->
<!-- JAR TEST 4: SUMMER DROUGHT QUIZ                               -->
<!-- ============================================================ -->
<div id="summerQuizWrap" style="display:none">
  <div class="card prequiz-card" id="summerQuizCard">
    <h2 style="text-transform:none;letter-spacing:0;">Summer Drought — Knowledge Check</h2>
    <p class="prequiz-sub">Warm, clear, alkaline water. Sounds easy to treat? Not quite.</p>

    <div class="pq-question" id="sq1">
      <div class="pq-num">Question 1 of 3</div>
      <div class="pq-text">Why is low turbidity, warm water challenging to treat with alum?</div>
      <div class="pq-options sq-opts" data-q="1">
        <button data-a="wrong">It is already clean enough and does not need treatment</button>
        <button data-a="correct">The particles are very small and highly charged, making them difficult to destabilise and flocculate</button>
        <button data-a="wrong">Warm water dissolves alum too quickly for it to work</button>
        <button data-a="wrong">Low turbidity means there is no organic matter present</button>
      </div>
      <div class="pq-feedback" id="sq1fb"></div>
    </div>

    <div class="pq-question" id="sq2">
      <div class="pq-num">Question 2 of 3</div>
      <div class="pq-text">What happens if you add too much alum to low turbidity, alkaline water?</div>
      <div class="pq-options sq-opts" data-q="2">
        <button data-a="wrong">The water becomes more acidic and corrosive</button>
        <button data-a="correct">The excess alum re-stabilises the particles, reversing coagulation and making the water worse</button>
        <button data-a="wrong">The floc becomes extremely large and blocks the jars</button>
        <button data-a="wrong">Nothing happens because extra alum just settles to the bottom</button>
      </div>
      <div class="pq-feedback" id="sq2fb"></div>
    </div>

    <div class="pq-question" id="sq3">
      <div class="pq-num">Question 3 of 3</div>
      <div class="pq-text">In summer drought conditions with high pH water, what should you expect about the optimal alum dose?</div>
      <div class="pq-options sq-opts" data-q="3">
        <button data-a="wrong">A very high dose because warm water needs more chemicals</button>
        <button data-a="correct">A surprisingly low dose because fewer particles need less coagulant to destabilise</button>
        <button data-a="wrong">The same dose as any other season</button>
        <button data-a="wrong">No alum is needed because the water is already clear</button>
      </div>
      <div class="pq-feedback" id="sq3fb"></div>
    </div>

    <div class="pq-score" id="sqScore">You scored <b id="sqScoreNum">0</b>/3</div>
    <div class="reward-card" id="sqRewardCard">
      <div class="reward-icon" id="sqRewardIcon"></div>
      <div class="reward-name" id="sqRewardName"></div>
      <div class="reward-desc" id="sqRewardDesc"></div>
    </div>
    <button class="btn btn-primary" id="continueSummerBtn" style="display:none;margin-top:16px">Continue to Summer Jar Test &#9654;</button>
  </div>
</div>

<!-- ============================================================ -->
<!-- JAR TEST 4: SUMMER DROUGHT SCENARIO                           -->
<!-- ============================================================ -->
<div id="jar4Wrap" style="display:none">
  <div id="jar4Sim">
  <div class="card">
    <div class="h2row">
      <h2 id="jar4Title">Gang stirrer — six 1 L jars (Summer Drought)</h2>
      <div class="source-pills">
        <span class="pill">RAW <b id="rawNtuPill4">--</b> NTU</span>
        <span class="pill">pH <b id="rawPhPill4">--</b></span>
        <span class="pill">temp <b id="tempPill4">25</b>&deg;C</span>
      </div>
    </div>
    <div class="stirrer-cap">paddle speed <b id="rpmCap4">stopped</b> &middot; phase <b id="phaseCap4">idle</b></div>
    <div class="stirrer-bar" id="stirrerBar4"></div>
    <div class="jar-bank" id="jarBank4"></div>
    <p class="hint">Type an alum dose under each jar (mg/L). Warning: overdosing will re-stabilise particles and make turbidity worse. The optimal dose is lower than you think.</p>
  </div>
  <div class="card">
    <h2>Mixing programme</h2>
    <div class="twocol">
      <div class="ctrl-block">
        <div class="cb-head"><span class="cb-title">Rapid mix (flash)</span><span class="cb-sub">disperses coagulant</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="flashRpm4">Paddle speed</label><span class="v"><span id="flashRpmV4">200</span> <small>rpm</small></span></div>
          <input type="range" id="flashRpm4" min="50" max="300" value="200" step="10">
          <div class="gband">target: <b>150-250 rpm</b></div>
        </div>
        <div class="slider-row">
          <div class="sr-top"><label for="flashTime4">Duration</label><span class="v"><span id="flashTimeV4">60</span> <small>s</small></span></div>
          <input type="range" id="flashTime4" min="10" max="180" value="60" step="5">
          <div class="gband">target: at least <b>30 s</b></div>
        </div>
      </div>
      <div class="ctrl-block">
        <div class="cb-head"><span class="cb-title">Slow mix (flocculation)</span><span class="cb-sub">grows the floc</span></div>
        <div class="slider-row">
          <div class="sr-top"><label for="slowRpm4">Paddle speed</label><span class="v"><span id="slowRpmV4">40</span> <small>rpm</small></span></div>
          <input type="range" id="slowRpm4" min="10" max="120" value="40" step="5">
          <div class="gband">sweet spot: <b>20-50 rpm</b></div>
        </div>
        <div class="slider-row">
          <div class="sr-top"><label for="slowTime4">Duration</label><span class="v"><span id="slowTimeV4">15</span> <small>min</small></span></div>
          <input type="range" id="slowTime4" min="2" max="30" value="15" step="1">
          <div class="gband">target: <b>10-20 min</b></div>
        </div>
      </div>
    </div>
    <div class="ctrl-block" style="margin-top:18px">
      <div class="cb-head"><span class="cb-title">Settling</span><span class="cb-sub">floc sinks</span></div>
      <div class="slider-row">
        <div class="sr-top"><label for="settleTime4">Settle time</label><span class="v"><span id="settleTimeV4">20</span> <small>min</small></span></div>
        <input type="range" id="settleTime4" min="1" max="60" value="20" step="1">
        <div class="gband">diminishing returns after ~<b>30 min</b></div>
      </div>
    </div>
    <div class="controls-row">
      <button class="btn btn-ghost" id="spreadBtn4">Auto-spread doses</button>
      <button class="btn btn-ghost" id="resetBtn4">New summer water</button>
      <div class="spacer"></div>
      <button class="btn btn-ghost" id="standardBtn4">Standard programme</button>
      <button class="btn btn-primary" id="runBtn4">Start run &#9654;</button>
    </div>
    <div class="status-line" id="statusLine4"><span class="phase-dot" id="phaseDot4"></span><span id="statusText4">Set alum doses carefully. Overdosing will make it worse.</span></div>
  </div>
  <div class="card">
    <h2>Results</h2>
    <div class="results-grid">
      <div><svg id="graph4" viewBox="0 0 460 300" role="img" aria-label="Graph"></svg></div>
      <div>
        <table class="res-table"><thead><tr><th>Jar</th><th>Dose mg/L</th><th>Final NTU</th><th>% Removed</th></tr></thead>
        <tbody id="resBody4"></tbody></table>
        <p class="interp-hint" id="interpHint4"></p>
      </div>
    </div>
  </div>
  </div>
</div>


</div>

<script>
/* ============================================================
   LAB EQUIPMENT COLLECTION SYSTEM
   Persistent reward system using localStorage.
   11 items earned through quizzes, skill, and engagement.
   ============================================================ */
window.labCollection = (function(){
  const ITEMS = [
    {id:'goggles',    icon:'🥽', name:'Safety Goggles',    desc:'Jar Test 1 quiz — perfect score'},
    {id:'gloves',     icon:'🧤', name:'Nitrile Gloves',    desc:'Any quiz 2/3 or first successful run'},
    {id:'beaker',     icon:'🧪', name:'Beaker',            desc:'Jar Test 2 quiz — perfect score'},
    {id:'petri',      icon:'🧫', name:'Petri Dishes',      desc:'Autumn quiz — perfect score'},
    {id:'flask',      icon:'⚗️', name:'Erlenmeyer Flask',  desc:'Any jar below 5 NTU'},
    {id:'pipettes',   icon:'🔬', name:'Pipettes',          desc:'Dose within 0.3 mg/L of optimum'},
    {id:'burette',    icon:'📐', name:'Burette',           desc:'All mixing params in sweet spot'},
    {id:'turbidimeter',icon:'📊', name:'Turbidimeter',     desc:'Run 3 spreads on same water'},
    {id:'labcoat',    icon:'🥼', name:'Lab Coat',          desc:'Complete all 3 jar test scenarios'},
    {id:'phmeter',    icon:'🌡️', name:'pH Meter',          desc:'Buffer pH to 6.0–7.5 in Autumn'},
    {id:'imhoff',     icon:'🏗️', name:'Imhoff Cones',      desc:'Achieve 90%+ removal'},
    {id:'cylinder',   icon:'🧪', name:'Graduated Cylinder', desc:'Summer Drought quiz — perfect score'}
  ];

  let state = loadState();

  function loadState(){
    try {
      if(typeof localStorage === 'undefined') throw new Error('no storage');
      const raw = localStorage.getItem('jarTestLabCollection');
      if(raw){
        const parsed = JSON.parse(raw);
        // Ensure all items exist in state
        ITEMS.forEach(it=>{ if(!(it.id in parsed)) parsed[it.id]=false; });
        if(!('poopCount' in parsed)) parsed.poopCount=0;
        if(!('scenariosComplete' in parsed)) parsed.scenariosComplete={jar1:false,jar2:false,jar3:false};
        if(!('runCount' in parsed)) parsed.runCount={jar1:0,jar2:0,jar3:0};
        return parsed;
      }
    } catch(e){}
    const s = {poopCount:0, scenariosComplete:{jar1:false,jar2:false,jar3:false,jar4:false}, runCount:{jar1:0,jar2:0,jar3:0}};
    ITEMS.forEach(it=>s[it.id]=false);
    return s;
  }

  function saveState(){ try { localStorage.setItem('jarTestLabCollection', JSON.stringify(state)); } catch(e){} }

  function countUnlocked(){ return ITEMS.filter(it=>state[it.id]).length; }

  function renderShelf(){
    try {
    const grid = document.getElementById('labGrid');
    if(!grid) return;
    grid.innerHTML = ITEMS.map(it=>{
      const unlocked = state[it.id];
      return `<div class="lab-item ${unlocked?'unlocked':'locked'}" title="${it.name}: ${it.desc}">
        <div class="li-icon">${it.icon}</div>
        <div class="li-name">${it.name}</div>
      </div>`;
    }).join('');

    const count = countUnlocked();
    document.getElementById('labCount').textContent = count;

    // Poop counter
    const poopEl = document.getElementById('poopBadge');
    const poopPanel = document.getElementById('labPoopCounter');
    if(state.poopCount > 0){
      poopEl.textContent = '💩×' + state.poopCount;
      poopPanel.textContent = '💩 Poop collected: ' + state.poopCount;
    } else {
      poopEl.textContent = '';
      poopPanel.textContent = '';
    }

    // Lab complete check
    if(count === 12){
      document.getElementById('labComplete').classList.add('show');
    }
    } catch(e){}
  }

  function unlock(itemId){
    try {
      if(state[itemId]) return false;
      state[itemId] = true;
      saveState();
      renderShelf();
      showToast(itemId);
      pulseBtn();
      return true;
    } catch(e){ return false; }
  }

  function addPoop(){
    state.poopCount++;
    saveState();
    renderShelf();
  }

  function markScenarioComplete(scenario){
    state.scenariosComplete[scenario] = true;
    saveState();
    // Check if all 3 done -> lab coat
    if(state.scenariosComplete.jar1 && state.scenariosComplete.jar2 && state.scenariosComplete.jar3){
      unlock('labcoat');
    }
  }

  function incrementRunCount(scenario){
    state.runCount[scenario]++;
    saveState();
  }

  function getRunCount(scenario){ return state.runCount[scenario] || 0; }

  function showToast(itemId){
    try {
      const item = ITEMS.find(it=>it.id===itemId);
      if(!item) return;
      const toast = document.getElementById('labToast');
      if(!toast) return;
      document.getElementById('toastIcon').textContent = item.icon;
      document.getElementById('toastText').textContent = item.name + ' unlocked!';
      document.getElementById('toastSub').textContent = item.desc;
      toast.classList.add('show');
      setTimeout(()=>{ try{toast.classList.remove('show');}catch(e){} }, 3500);
    } catch(e){}
  }

  function pulseBtn(){
    const btn = document.getElementById('labBtn');
    btn.classList.remove('pulse');
    void btn.offsetWidth; // force reflow
    btn.classList.add('pulse');
  }

  function togglePanel(){
    document.getElementById('labPanel').classList.toggle('open');
  }

  // Close panel when clicking outside
  document.addEventListener('click', function(e){
    const panel = document.getElementById('labPanel');
    const btn = document.getElementById('labBtn');
    if(panel.classList.contains('open') && !panel.contains(e.target) && !btn.contains(e.target)){
      panel.classList.remove('open');
    }
  });

  // Public API
  const api = {
    unlock,
    addPoop,
    markScenarioComplete,
    incrementRunCount,
    getRunCount,
    togglePanel,
    isUnlocked: (id) => state[id],
    getState: () => state,
    renderShelf,
    // Check functions called after events
    checkAfterQuiz(quizId, score, total){ try {
      if(score === total){
        if(quizId === 'jar1') unlock('goggles');
        else if(quizId === 'jar2') unlock('beaker');
        else if(quizId === 'autumn') unlock('petri');
        else if(quizId === 'summer') unlock('cylinder');
      }
      if(score >= total-1 && score < total){
        unlock('gloves');
      }
      if(score <= 1 && total === 3){
        addPoop();
      }
    } catch(e){} },
    checkAfterRun(scenario, jars, optDose, rawNtu, mixParams, effectivePh){ try {
      // Gloves: first successful run (any jar below raw NTU)
      const anyBelow = jars.some(j => j.finalNtu < rawNtu);
      if(anyBelow) unlock('gloves');

      // Mark scenario run
      incrementRunCount(scenario);
      markScenarioComplete(scenario);

      // Erlenmeyer Flask: any jar below 5 NTU
      if(jars.some(j => j.finalNtu < 5)) unlock('flask');

      // Pipettes: dose within 0.3 mg/L of true optimum
      if(jars.some(j => Math.abs(j.dose - optDose) <= 0.3 && j.dose > 0)) unlock('pipettes');

      // Burette: all mixing params in sweet spots
      const flashOk = mixParams.flashRpm >= 150 && mixParams.flashRpm <= 250;
      const flashTimeOk = mixParams.flashTime >= 30;
      const slowOk = mixParams.slowRpm >= 20 && mixParams.slowRpm <= 50;
      const slowTimeOk = mixParams.slowTime >= 10 && mixParams.slowTime <= 20;
      const settleOk = mixParams.settleTime >= 15;
      if(flashOk && flashTimeOk && slowOk && slowTimeOk && settleOk) unlock('burette');

      // Turbidimeter: 3 runs on same water
      if(getRunCount(scenario) >= 3) unlock('turbidimeter');

      // Imhoff Cones: 90%+ removal
      const bestRemoval = Math.max(...jars.map(j => (1 - j.finalNtu/rawNtu)));
      if(bestRemoval >= 0.9) unlock('imhoff');

      // pH Meter: buffer pH to 6.0-7.5 in Autumn
      if(scenario === 'jar3' && effectivePh >= 6.0 && effectivePh <= 7.5) unlock('phmeter');
    } catch(e){} }
  };

  // Initial render
  renderShelf();
  return api;
})();
</script>

<script>
/* ============================================================
   SUMMER DROUGHT QUIZ
   ============================================================ */
(function(){
  let sqAnswered=0, sqCorrect=0;
  const feedback = {
    1: {
      right:"Correct! Low turbidity water has very fine, highly charged particles. They resist destabilisation because there are so few of them and they repel each other strongly.",
      wrong:"Not quite. The challenge is that the particles are extremely small and carry strong charges. With so few particles, collisions are rare and floc formation is difficult."
    },
    2: {
      right:"Correct! This is called charge reversal. Too much alum flips the particle charge from negative to positive, re-stabilising them. The U-curve becomes very steep on the overdose side.",
      wrong:"Not quite. Overdosing causes charge reversal. The alum overshoots the neutral point and gives particles a positive charge, making them repel each other again."
    },
    3: {
      right:"Correct! With only 10-20 NTU of particles, very little coagulant is needed. The trick is precision, not volume. The optimal dose window is narrow.",
      wrong:"Not quite. Low turbidity means fewer particles to destabilise, so the optimal dose is lower than normal. The challenge is finding that narrow sweet spot without overshooting."
    }
  };

  document.querySelectorAll('.sq-opts').forEach(optDiv=>{
    const qNum = +optDiv.dataset.q;
    optDiv.querySelectorAll('button').forEach(btn=>{
      btn.addEventListener('click',function(){
        const isCorrect = btn.dataset.a==='correct';
        const fb = document.getElementById('sq'+qNum+'fb');
        const buttons = optDiv.querySelectorAll('button');
        buttons.forEach(b=>{ b.disabled=true; if(b!==btn) b.style.opacity='0.5'; });
        if(isCorrect){
          btn.classList.add('correct');
          fb.className='pq-feedback show fb-right';
          fb.textContent=feedback[qNum].right;
          sqCorrect++;
        } else {
          btn.classList.add('wrong');
          fb.className='pq-feedback show fb-wrong';
          fb.textContent=feedback[qNum].wrong;
          buttons.forEach(b=>{ if(b.dataset.a==='correct'){b.classList.add('correct');b.style.opacity='1';} });
        }
        sqAnswered++;
        if(sqAnswered===3){
          document.getElementById('sqScoreNum').textContent=sqCorrect;
          document.getElementById('sqScore').style.display='block';
          document.getElementById('continueSummerBtn').style.display='inline-flex';
          const rc=document.getElementById('sqRewardCard');
          const ri=document.getElementById('sqRewardIcon');
          const rn=document.getElementById('sqRewardName');
          const rd=document.getElementById('sqRewardDesc');
          if(sqCorrect===3){
            ri.textContent='⚗️'; rn.textContent='Graduated Cylinder Unlocked!';
            rd.textContent='Perfect score! You earned a graduated cylinder for your lab collection.';
            rc.className='reward-card show gold';
          } else if(sqCorrect===2){
            ri.textContent='🧤'; rn.textContent='Nitrile Gloves Unlocked!';
            rd.textContent='Almost there! You earned nitrile gloves. Review the one you missed.';
            rc.className='reward-card show silver';
          } else {
            ri.textContent='💩'; rn.textContent='You got... poop.';
            rd.textContent='Time to study up on summer water chemistry!';
            rc.className='reward-card show poop-tier';
          }
          window.labCollection.checkAfterQuiz('summer', sqCorrect, 3);
        }
      });
    });
  });

  document.getElementById('continueSummerBtn').addEventListener('click',function(){
    document.getElementById('summerQuizWrap').style.display='none';
    document.getElementById('stepQuiz4').classList.remove('active');
    document.getElementById('stepQuiz4').classList.add('done');
    document.getElementById('stepJar4').classList.add('active');
    document.getElementById('jar4Wrap').style.display='block';
    document.getElementById('jar4Sim').style.display='block';
    window.dispatchEvent(new CustomEvent('jar4Ready', {detail:{n:6,v:1}}));
    document.getElementById('jar4Wrap').scrollIntoView({behavior:'smooth',block:'start'});
  });
})();

/* ============================================================
   JAR TEST 4 - Summer Drought
   25C, pH 8.0, NTU 15. Overdosing causes charge reversal.
   ============================================================ */
(function(){
  "use strict";
  const $ = id => document.getElementById(id+'4');
  let NJARS = 6, VOLUME_L = 1;

  const SUMMER = { ntu:[10,20], ph:[7.6,8.4], temp:[22,28], window:0.12 };

  let sample, optDose;
  let jars=[], phase='idle', running=false, history=[];
  let prog={flashRpm:200,flashTime:60,slowRpm:40,slowTime:15,settleTime:20};

  const rnd=(a,b)=>a+Math.random()*(b-a);
  const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));

  function calcOptDose(){
    optDose = clamp(0.6 + sample.rawNtu*0.04, 0.8, 2.0);
  }

  function newSample(){
    const rawNtu=+rnd(SUMMER.ntu[0],SUMMER.ntu[1]).toFixed(0);
    const ph=+rnd(SUMMER.ph[0],SUMMER.ph[1]).toFixed(1);
    const temp=+rnd(SUMMER.temp[0],SUMMER.temp[1]).toFixed(0);
    sample={rawNtu,ph,temp};
    calcOptDose();
    const topDose=3;
    const defaults=Array.from({length:NJARS},(_,i)=>+(i*(topDose/(NJARS-1))).toFixed(1));
    jars=[];
    for(let i=0;i<NJARS;i++) jars.push({dose:defaults[i],finalNtu:rawNtu,fq:0,ntuNow:rawNtu,flocNow:0,settledFrac:0});
    phase='idle'; running=false;
    $('rawNtuPill').textContent=rawNtu;
    $('rawPhPill').textContent=ph.toFixed(1);
    $('tempPill').textContent=temp;
    renderJars();
    $('resBody').innerHTML='<tr><td colspan="4" style="color:var(--ink-faint);text-align:center;padding:14px">Run the jars to see results.</td></tr>';
    $('interpHint').textContent='';
    setStatus('idle','Summer drought water: '+rawNtu+' NTU, pH '+ph+', '+temp+'\u00b0C. Careful: overdosing will re-stabilise particles.');
    drawGraph();
  }

  // Physics - with strong overdose penalty (charge reversal)
  function doseRemoval(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=SUMMER.window;
    let r;
    if(ratio<1) r=0.85*Math.pow(ratio,1.8);
    else {
      // Steep decline on overdose - charge reversal
      const overFactor = (ratio-1)/(w*2.5);
      r = 0.85*Math.exp(-Math.pow(overFactor,1.3)*2.0);
      // At very high overdose, turbidity can increase beyond raw
      if(ratio > 2.0) r = Math.max(r, 0) - (ratio-2.0)*0.15;
    }
    return clamp(r,-.3,0.88);
  }
  function flashEff(){
    const rpm=prog.flashRpm, t=prog.flashTime;
    const speedF=rpm<150?0.4+0.6*((rpm-50)/100):1.0;
    const timeF=t<30?0.5+0.5*((t-10)/20):1.0;
    return clamp(speedF*timeF,0,1);
  }
  function flocEff(){
    const rpm=prog.slowRpm, t=prog.slowTime;
    let speedF;
    if(rpm<20) speedF=0.45+0.55*((rpm-10)/10);
    else if(rpm<=50) speedF=1.0;
    else speedF=clamp(1.0-(rpm-50)/90,0.2,1.0);
    const timeF=t<10?0.5+0.5*((t-2)/8):(t>20?1.0:0.85+0.15*((t-10)/10));
    return clamp(speedF*timeF,0,1);
  }
  function settleFactor(){
    return clamp(1-Math.exp(-prog.settleTime/12),0,0.985);
  }
  function computeFinal(dose){
    const removalChem=doseRemoval(dose);
    const mixComposite=flashEff()*flocEff();
    const settle=settleFactor();
    const removal=removalChem*(0.45+0.55*mixComposite)*(0.55+0.45*settle);
    const noise=rnd(-0.008,0.008);
    const finalNtu = sample.rawNtu*(1-clamp(removal+noise,-0.3,0.9));
    return clamp(finalNtu,0.3,sample.rawNtu*1.3);
  }
  function flocQuality(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose;
    const chem=ratio<1?Math.pow(ratio,1.5):Math.exp(-Math.pow((ratio-1)/(SUMMER.window*2.5),1.3)*2.0);
    return clamp(chem*flashEff()*flocEff(),0,1);
  }

  let timer=null;
  function readProgramme(){
    prog.flashRpm=+$('flashRpm').value; prog.flashTime=+$('flashTime').value;
    prog.slowRpm=+$('slowRpm').value; prog.slowTime=+$('slowTime').value;
    prog.settleTime=+$('settleTime').value;
  }
  function phaseDurations(){
    return [
      {phase:'flash',dur:Math.max(1.2,prog.flashTime/30)},
      {phase:'slow',dur:Math.max(3.0,prog.slowTime*0.4)},
      {phase:'settle',dur:Math.max(2.5,prog.settleTime*0.25)}
    ];
  }
  function runAll(){
    if(running)return;
    readProgramme();
    jars.forEach((j,i)=>{ const v=parseFloat($('dose'+i).value); j.dose=isNaN(v)?0:clamp(v,0,4); });
    jars.forEach(j=>{ j.finalNtu=computeFinal(j.dose); j.fq=flocQuality(j.dose);
      j.ntuNow=sample.rawNtu; j.flocNow=0; j.settledFrac=0; });
    running=true; setControls(false);
    const SEQ=phaseDurations();
    let stepIdx=0, phaseT=0;
    timer=setInterval(()=>{
      phaseT+=0.07;
      const cur=SEQ[stepIdx];
      const frac=clamp(phaseT/cur.dur,0,1);
      phase=cur.phase;
      if(cur.phase==='flash'){
        $('stirrerBar').className='stirrer-bar mixing';
        $('stirrerBar').style.setProperty('--sweep','0.35s');
        $('rpmCap').textContent=prog.flashRpm+' rpm';
        $('phaseCap').textContent='rapid mix';
      } else if(cur.phase==='slow'){
        $('stirrerBar').className='stirrer-bar mixing';
        $('stirrerBar').style.setProperty('--sweep','1.8s');
        $('rpmCap').textContent=prog.slowRpm+' rpm';
        $('phaseCap').textContent='slow mix';
        jars.forEach(j=>{ j.flocNow=j.fq*frac; });
      } else {
        $('stirrerBar').className='stirrer-bar';
        $('rpmCap').textContent='stopped';
        $('phaseCap').textContent='settling';
        jars.forEach(j=>{ j.settledFrac=frac;
          j.ntuNow=sample.rawNtu-(sample.rawNtu-j.finalNtu)*frac; });
      }
      setStatus(cur.phase,(cur.phase==='flash'?'Rapid mixing...':
        (cur.phase==='slow'?'Slow mixing - floc forming...':
        'Settling... '+Math.round(frac*prog.settleTime)+'/'+prog.settleTime+' min')));
      renderJars();
      if(frac>=1){ stepIdx++; phaseT=0;
        if(stepIdx>=SEQ.length){ clearInterval(timer); phase='done'; running=false;
          $('stirrerBar').className='stirrer-bar';
          $('rpmCap').textContent='stopped'; $('phaseCap').textContent='done';
          setControls(true); fillResults(); drawGraph();
          setStatus('done','Run complete. Check the results below.');
          window.labCollection.checkAfterRun('jar4', jars, optDose, sample.rawNtu, prog, null);
          window.labCollection.markScenarioComplete('jar4');
        }
      }
    },70);
  }
  function setControls(on){
    document.querySelectorAll('#jar4Sim input, #jar4Sim button').forEach(el=>{ el.disabled=!on; });
  }
  function setStatus(type,msg){
    const dot=$('phaseDot'), txt=$('statusText');
    dot.className='phase-dot'+(type==='idle'?' idle':(type==='done'?' done':' running'));
    txt.textContent=msg;
  }

  function renderJars(){
    const bank=$('jarBank');
    bank.innerHTML=jars.slice(0,NJARS).map((j,i)=>{
      const turb=phase==='done'?j.finalNtu:j.ntuNow;
      const pctClr=clamp(1-turb/sample.rawNtu,0,1);
      const waterH=120, waterY=180-waterH, sedH=phase==='settle'||phase==='done'?j.fq*j.settledFrac*35:0;
      const flocDots=(phase==='slow'||phase==='settle'||phase==='done')?j.flocNow:0;
      const worse = j.finalNtu > sample.rawNtu;
      const wColor = worse ?
        `rgba(${Math.round(160-20*(j.finalNtu/sample.rawNtu-1))},${Math.round(170-30*(j.finalNtu/sample.rawNtu-1))},${Math.round(130)},0.85)` :
        `rgba(${Math.round(180+75*pctClr)},${Math.round(195+60*pctClr)},${Math.round(140+80*pctClr)},0.85)`;
      const ntuClass=turb<=3?'good':(turb>=sample.rawNtu?'bad':'mid');
      let svg=`<svg viewBox="0 0 100 200"><rect x="20" y="40" width="60" height="150" rx="4" fill="none" stroke="var(--edge-bright)" stroke-width="2"/>
        <rect x="22" y="${waterY}" width="56" height="${waterH}" rx="2" fill="${wColor}"/>`;
      if(sedH>0) svg+=`<rect x="22" y="${180-sedH}" width="56" height="${sedH}" rx="1" fill="rgba(120,90,50,0.5)"/>`;
      if(flocDots>0){ for(let d=0;d<Math.round(flocDots*12);d++){
        const fx=28+Math.random()*40, fy=waterY+10+Math.random()*(waterH-25);
        svg+=`<circle cx="${fx}" cy="${fy}" r="${1+flocDots*2}" fill="rgba(160,130,80,${0.15+flocDots*0.35})"/>`; }}
      svg+=`</svg>`;
      return `<div class="jar-cell"><span class="jarno">Jar ${i+1}</span>${svg}
        <div class="dose-wrap"><input class="dose-input" id="dose${i}4" type="number" min="0" max="4" step="0.1" value="${j.dose}" ${running?'disabled':''}>
        <span class="dose-lbl">mg/L alum</span></div>
        <div class="readout-mini">${phase==='done'?`<span class="ntu-num ${ntuClass}">${turb.toFixed(1)}</span> NTU${worse?' \u26a0\ufe0f':''}`:''}
        </div></div>`;
    }).join('');
  }

  function fillResults(){
    const best=jars.slice(0,NJARS).reduce((a,b)=>a.finalNtu<b.finalNtu?a:b);
    $('resBody').innerHTML=jars.slice(0,NJARS).map((j,i)=>{
      const pct=((1-j.finalNtu/sample.rawNtu)*100).toFixed(1);
      const cls=j===best?'style="color:var(--good)"':'';
      const warn=j.finalNtu>sample.rawNtu?' \u26a0\ufe0f overdosed':'';
      return `<tr ${cls}><td>${i+1}</td><td>${j.dose}</td><td>${j.finalNtu.toFixed(1)}</td><td>${pct}%${warn}</td></tr>`;
    }).join('');
    const overdosed = jars.filter(j=>j.finalNtu>sample.rawNtu).length;
    const odMsg = overdosed>0 ? ` ${overdosed} jar(s) showed charge reversal from overdosing.` : '';
    $('interpHint').textContent=`Lowest NTU: Jar with ${best.dose} mg/L alum \u2192 ${best.finalNtu.toFixed(1)} NTU (${((1-best.finalNtu/sample.rawNtu)*100).toFixed(1)}% removal).${odMsg}`;
    history.push(jars.slice(0,NJARS).map(j=>({dose:j.dose,ntu:j.finalNtu})));
  }

  function drawGraph(){
    const W=460,H=300,ml=50,mr=20,mt=20,mb=40;
    const gw=W-ml-mr, gh=H-mt-mb;
    const maxD=4, maxN=Math.max(sample.rawNtu*1.35,25);
    let svg=`<rect width="${W}" height="${H}" fill="var(--panel-2)" rx="8"/>
      <line x1="${ml}" y1="${mt}" x2="${ml}" y2="${H-mb}" stroke="var(--edge)" stroke-width="1"/>
      <line x1="${ml}" y1="${H-mb}" x2="${W-mr}" y2="${H-mb}" stroke="var(--edge)" stroke-width="1"/>`;
    for(let d=0;d<=maxD;d+=0.5){ const x=ml+d/maxD*gw; svg+=`<text x="${x}" y="${H-mb+18}" text-anchor="middle" fill="var(--ink-faint)" font-size="10">${d}</text>`; }
    svg+=`<text x="${ml+gw/2}" y="${H-2}" text-anchor="middle" fill="var(--ink-faint)" font-size="10">alum dose (mg/L)</text>`;
    // Raw NTU reference line
    const rawY=H-mb-sample.rawNtu/maxN*gh;
    svg+=`<line x1="${ml}" y1="${rawY}" x2="${W-mr}" y2="${rawY}" stroke="var(--bad)" stroke-width="1" stroke-dasharray="4" opacity="0.4"/>`;
    svg+=`<text x="${W-mr-2}" y="${rawY-4}" text-anchor="end" fill="var(--bad)" font-size="9" opacity="0.6">raw NTU</text>`;
    for(let n=0;n<=maxN;n+=5){ const y=H-mb-n/maxN*gh; svg+=`<line x1="${ml}" y1="${y}" x2="${W-mr}" y2="${y}" stroke="var(--edge)" stroke-width="0.5" opacity="0.3"/>
      <text x="${ml-8}" y="${y+4}" text-anchor="end" fill="var(--ink-faint)" font-size="10">${n}</text>`; }
    history.forEach((run,ri)=>{
      const op=ri===history.length-1?1:0.25;
      const col=ri===history.length-1?'var(--accent)':'var(--ink-faint)';
      let pts=run.map(p=>{ const x=ml+p.dose/maxD*gw, y=H-mb-clamp(p.ntu,0,maxN)/maxN*gh; return `${x},${y}`; });
      svg+=`<polyline points="${pts.join(' ')}" fill="none" stroke="${col}" stroke-width="2" opacity="${op}"/>`;
      run.forEach(p=>{ const x=ml+p.dose/maxD*gw, y=H-mb-clamp(p.ntu,0,maxN)/maxN*gh;
        svg+=`<circle cx="${x}" cy="${y}" r="4" fill="${col}" opacity="${op}"/>`; });
    });
    document.getElementById('graph4').innerHTML=svg;
  }

  function autoSpread(){
    const top=clamp(+(optDose*2.5).toFixed(1),2,4);
    const step=top/(NJARS-1);
    jars.forEach((j,i)=>{ j.dose=+(i*step).toFixed(1); });
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.value=j.dose; });
    setStatus('idle','Doses spread 0 to '+jars[NJARS-1].dose+' mg/L. Be careful of overdosing.');
  }
  function standardProgramme(){
    $('flashRpm').value=200; $('flashTime').value=60;
    $('slowRpm').value=40; $('slowTime').value=15; $('settleTime').value=20;
    ['flashRpm','flashTime','slowRpm','slowTime','settleTime'].forEach(id=>$(id).dispatchEvent(new Event('input')));
    setStatus('idle','Standard programme loaded.');
  }
  function bindSlider(id,vid){
    const el=$(id), out=$(vid);
    const upd=()=>{ out.textContent=el.value;
      const pct=(el.value-el.min)/(el.max-el.min)*100; el.style.setProperty('--fill',pct+'%'); };
    el.addEventListener('input',upd); upd();
  }

  bindSlider('flashRpm','flashRpmV');
  bindSlider('flashTime','flashTimeV');
  bindSlider('slowRpm','slowRpmV');
  bindSlider('slowTime','slowTimeV');
  bindSlider('settleTime','settleTimeV');

  $('runBtn').addEventListener('click',runAll);
  $('spreadBtn').addEventListener('click',()=>{ if(!running)autoSpread(); });
  $('standardBtn').addEventListener('click',()=>{ if(!running)standardProgramme(); });
  $('resetBtn').addEventListener('click',()=>{ if(running)return;
    if(!confirm('New summer water clears the current jars (graph history is kept). Continue?'))return; newSample(); });

  setInterval(()=>{ if(running)renderJars(); },70);

  window.addEventListener('jar4Ready', function(e){
    NJARS=e.detail.n; VOLUME_L=e.detail.v;
    $('jarBank').style.setProperty('--njars',NJARS);
    document.getElementById('jar4Title').textContent=`Gang stirrer \u2014 ${NJARS} \u00d7 ${VOLUME_L} L jars (Summer Drought)`;
    history=[];
    newSample();
  });
})();
</script>


<script>
/* ============================================================
   SHARED: quiz gate + step navigation between Jar Test 1 and 2
   ============================================================ */
(function(){
  "use strict";

  const quizBank = [
    {
      question: "What is the main challenge posed by this Spring Snowmelt raw water?",
      options: [
        "High temperature and already-clear water",
        "Cold temperature, low pH, and high turbidity",
        "High pH and low turbidity",
        "No coagulant is needed at all"
      ],
      answer: 1
    },
    {
      question: "How does the cold water (5°C) affect floc formation?",
      options: [
        "It speeds up floc formation",
        "It has no real effect on mixing or floc",
        "It increases viscosity and slows floc formation",
        "It removes the need for slow mixing"
      ],
      answer: 2
    },
    {
      question: "Compared to a normal-condition jar test, what should you expect for the alum dose and slow-mix rpm window?",
      options: [
        "Lower alum dose, wider rpm window",
        "Higher alum dose, narrower rpm window",
        "Same alum dose, no change to mixing",
        "No alum needed, only pH adjustment"
      ],
      answer: 1
    },
    {
      question: "Why does low pH (6.2) make alum less effective?",
      options: [
        "Low pH speeds up alum's chemical reaction with turbidity",
        "Low pH changes the water's color, not its chemistry",
        "It shifts alum's coagulation chemistry away from its optimal range",
        "pH has no effect on coagulant chemistry"
      ],
      answer: 2
    },
    {
      question: "In cold, viscous water, what tends to happen to floc if the slow-mix rpm is set too high?",
      options: [
        "The floc grows larger and settles faster",
        "The floc is sheared apart before it can grow properly",
        "The floc becomes more resistant to breaking",
        "Nothing — rpm doesn't affect floc breakup"
      ],
      answer: 1
    },
    {
      question: "Why might settling take longer with this Spring Snowmelt water than with normal-condition water?",
      options: [
        "Because the water is warmer than usual",
        "Because higher viscosity slows how fast floc particles sink",
        "Because there is no floc formed at all",
        "Settling time is unrelated to temperature"
      ],
      answer: 1
    }
  ];

  function shuffleArray(arr){
    const a = arr.slice();
    for (let i = a.length - 1; i > 0; i--){
      const j = Math.floor(Math.random() * (i + 1));
      [a[i], a[j]] = [a[j], a[i]];
    }
    return a;
  }

  function shuffleOptions(q){
    const order = shuffleArray(q.options.map((_, i) => i));
    return {
      question: q.question,
      options: order.map(i => q.options[i]),
      answer: order.indexOf(q.answer)
    };
  }

  function buildQuiz(){
    const chosen = shuffleArray(quizBank).slice(0, 3);
    return chosen.map(shuffleOptions);
  }

  let jar2GateQuiz = buildQuiz();
  let quizIndex = 0, quizScore = 0;

  function markStep(id, cls){
    const el = document.getElementById(id);
    if (el) { el.classList.remove('active','done'); if (cls) el.classList.add(cls); }
  }

  // Called by Jar Test 1's finishAll() once a run completes
  window.afterJar1Complete = function(){
    markStep('stepJar1','done');
    markStep('stepQuiz','active');
    const wrap = document.getElementById('continueToQuizWrap');
    wrap.style.display = 'flex';
    wrap.scrollIntoView({behavior:'smooth', block:'center'});
  };

  document.getElementById('continueToQuizBtn').addEventListener('click', showQuizBeforeJar2);

  function showQuizBeforeJar2(){
    jar2GateQuiz = buildQuiz();
    quizIndex = 0; quizScore = 0;
    document.getElementById('continueToQuizWrap').style.display = 'none';
    const qc = document.getElementById('quizContainer');
    qc.style.display = 'block';
    qc.scrollIntoView({behavior:'smooth', block:'start'});
    renderQuizQuestion();
  }

  function gradeFor(score, total){
    if (score === total) return 'A';
    if (score === total - 1) return 'B';
    return 'Fail';
  }

  function renderQuizQuestion(){
    const qc = document.getElementById('quizContainer');
    if (quizIndex >= jar2GateQuiz.length){
      let rewardIcon, rewardName, rewardDesc, rewardClass;
      if(quizScore === jar2GateQuiz.length){
        rewardIcon='🧪'; rewardName='Beaker Unlocked!'; rewardDesc='Perfect score! You earned a beaker for your lab equipment collection.'; rewardClass='gold';
      } else if(quizScore >= jar2GateQuiz.length-1){
        rewardIcon='🧤'; rewardName='Nitrile Gloves Unlocked!'; rewardDesc='Almost there! You earned nitrile gloves. Review the one you missed.'; rewardClass='silver';
      } else {
        rewardIcon='💩'; rewardName='You got... poop.'; rewardDesc='Time to study up on your water treatment knowledge!'; rewardClass='poop-tier';
      }
      qc.innerHTML = `
        <h2>Pre-Test #2 Quiz — Complete</h2>
        <div class="quiz-final">
          <div class="raw-score">${quizScore} / ${jar2GateQuiz.length} correct</div>
          <div class="reward-card show ${rewardClass}">
            <div class="reward-icon">${rewardIcon}</div>
            <div class="reward-name">${rewardName}</div>
            <div class="reward-desc">${rewardDesc}</div>
          </div>
          <button class="btn btn-primary" id="unlockJar2Btn" style="margin-top:16px">Continue to Spring Snowmelt &#9654;</button>
        </div>`;
      document.getElementById('unlockJar2Btn').addEventListener('click', unlockJar2);
      // Lab collection check
      window.labCollection.checkAfterQuiz('jar2', quizScore, jar2GateQuiz.length);
      return;
    }
    const q = jar2GateQuiz[quizIndex];
    qc.innerHTML = `
      <h2>Pre-Test #2 Quiz — Spring Snowmelt</h2>
      <div class="quiz-progress">Question ${quizIndex+1} of ${jar2GateQuiz.length}</div>
      <div class="quiz-q">${q.question}</div>
      <div class="quiz-options">
        ${q.options.map((opt,i)=>`<button data-i="${i}">${opt}</button>`).join('')}
      </div>
      <div class="quiz-feedback" id="quizFeedback"></div>
      <div class="quiz-score-line">Score so far: ${quizScore} / ${quizIndex}</div>
    `;
    qc.querySelectorAll('.quiz-options button').forEach(btn=>{
      btn.addEventListener('click', ()=>checkAnswer(+btn.dataset.i));
    });
  }

  function checkAnswer(selected){
    const q = jar2GateQuiz[quizIndex];
    const correct = selected === q.answer;
    const buttons = document.querySelectorAll('#quizContainer .quiz-options button');
    buttons.forEach((btn,i)=>{
      btn.disabled = true;
      if (i === q.answer) btn.classList.add('correct');
      else if (i === selected) btn.classList.add('wrong');
    });
    const fb = document.getElementById('quizFeedback');
    fb.textContent = correct ? 'Correct!' : 'Not quite — the correct answer is highlighted.';
    fb.className = 'quiz-feedback ' + (correct ? 'correct' : 'wrong');
    if (correct) quizScore++;
    setTimeout(()=>{ quizIndex++; renderQuizQuestion(); }, 1300);
  }

  function unlockJar2(){
    markStep('stepQuiz','done');
    markStep('stepJar2','active');
    document.getElementById('quizContainer').style.display = 'none';
    const j2 = document.getElementById('jar2Wrap');
    j2.style.display = 'block';
    // Skip setup, go straight to sim with default 6 jars x 1L
    document.getElementById('setupCard2').style.display = 'none';
    const simEl = document.getElementById('jar2Sim');
    simEl.style.display = 'block';
    // Dispatch custom event to trigger Jar Test 2 init
    window.dispatchEvent(new CustomEvent('jar2Ready', {detail:{n:6,v:1}}));
    j2.scrollIntoView({behavior:'smooth', block:'start'});
  }

  // exposed for debugging/testing only — not required for normal use
  window.showQuizBeforeJar2 = showQuizBeforeJar2;
  window.renderQuizQuestion = renderQuizQuestion;
  window.checkAnswer = checkAnswer;
  window.unlockJar2 = unlockJar2;
})();

/* ============================================================
   Reusable walkable "prep room" — shelf (grab alum), cupboard
   (choose jar count), tap (choose sample volume). Used by both
   Jar Test 1 and Jar Test 2 setup screens.
   ============================================================ */
window.initLabRoom = function(opts){
  const ns = opts.ns;
  const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
  const room=document.getElementById('labRoom'+ns);
  const player=document.getElementById('player'+ns);
  const prompt=document.getElementById('prompt'+ns);
  const beginBtn=document.getElementById('beginJar'+ns+'Btn');
  const stations={
    shelf: document.getElementById('shelf'+ns),
    cupboard: document.getElementById('cupboard'+ns),
    tap: document.getElementById('tap'+ns)
  };
  const checkEls={
    shelf: document.getElementById('checkAlum'+ns),
    cupboard: document.getElementById('checkJars'+ns),
    tap: document.getElementById('checkWater'+ns)
  };
  const ROOM_W=560, ROOM_H=340, PW=22, PH=30, SPEED=2.6;
  let px=(ROOM_W-PW)/2, py=(ROOM_H-PH)/2+20;
  const keys={};
  let nearest=null;
  const done={shelf:false,cupboard:false,tap:false};
  let pickedNJars=6, pickedVolume=1;

  function stationRect(el){
    const r=el.getBoundingClientRect(), rr=room.getBoundingClientRect();
    const left=r.left-rr.left, top=r.top-rr.top;
    return {cx:left+r.width/2, cy:top+r.height/2, left, top, right:left+r.width, bottom:top+r.height};
  }
  function updatePlayerPos(){ player.style.left=px+'px'; player.style.top=py+'px'; }
  updatePlayerPos();

  function isVisible(){ return room.offsetParent !== null; }

  function checkNearest(){
    let closest=null, closestDist=9999;
    Object.keys(stations).forEach(key=>{
      const rect=stationRect(stations[key]);
      const dx=px+PW/2-rect.cx, dy=py+PH/2-rect.cy;
      const dist=Math.sqrt(dx*dx+dy*dy);
      if(dist<62 && dist<closestDist){ closest=key; closestDist=dist; }
    });
    Object.keys(stations).forEach(key=>stations[key].classList.toggle('near', key===closest));
    nearest=closest;
    if(closest){
      const rect=stationRect(stations[closest]);
      prompt.style.left=rect.cx+'px';
      prompt.style.top=rect.top+'px';
      prompt.style.display = done[closest] ? 'none' : 'block';
    } else {
      prompt.style.display='none';
    }
  }

  function keydownHandler(e){
    if(!isVisible()) return;
    const k=e.key.toLowerCase();
    if(['arrowup','arrowdown','arrowleft','arrowright','w','a','s','d','e'].includes(k)) e.preventDefault();
    keys[k]=true;
    if(k==='e') tryInteract();
  }
  function keyupHandler(e){ keys[e.key.toLowerCase()]=false; }
  document.addEventListener('keydown', keydownHandler);
  document.addEventListener('keyup', keyupHandler);
  room.addEventListener('click', ()=>room.focus());

  Object.keys(stations).forEach(key=>{
    stations[key].addEventListener('click', (e)=>{
      e.stopPropagation();
      const rect=stationRect(stations[key]);
      px=clamp(rect.cx-PW/2, 4, ROOM_W-PW-4);
      py=clamp(rect.cy+42-PH/2, 4, ROOM_H-PH-4);
      updatePlayerPos();
      nearest=key;
      checkNearest();
      tryInteract();
    });
  });

  function loop(){
    if(isVisible()){
      let dx=0,dy=0;
      if(keys['arrowup']||keys['w']) dy-=1;
      if(keys['arrowdown']||keys['s']) dy+=1;
      if(keys['arrowleft']||keys['a']) dx-=1;
      if(keys['arrowright']||keys['d']) dx+=1;
      if(dx||dy){
        const len=Math.sqrt(dx*dx+dy*dy);
        px=clamp(px+(dx/len)*SPEED, 4, ROOM_W-PW-4);
        py=clamp(py+(dy/len)*SPEED, 4, ROOM_H-PH-4);
        updatePlayerPos();
      }
      checkNearest();
    }
    requestAnimationFrame(loop);
  }
  requestAnimationFrame(loop);

  function markDone(key){
    done[key]=true;
    stations[key].classList.add('done');
    const label = key==='shelf'?'Alum':key==='cupboard'?'Jars':'Water';
    checkEls[key].textContent='\u25CF '+label;
    checkEls[key].classList.add('done');
    if(done.shelf && done.cupboard && done.tap) beginBtn.disabled=false;
  }

  function showModal(html){
    const backdrop=document.createElement('div');
    backdrop.className='station-modal-backdrop';
    backdrop.innerHTML=`<div class="station-modal">${html}</div>`;
    document.body.appendChild(backdrop);
    return backdrop;
  }

  function tryInteract(){
    if(!nearest || done[nearest]) return;
    if(nearest==='shelf'){
      const bd=showModal(`
        <h3>Alum shelf</h3>
        <p>You grab a container of aluminium sulfate (alum) coagulant — enough for this test.</p>
        <button class="btn btn-primary" id="shelfOkBtn${ns}">Got it</button>`);
      document.getElementById('shelfOkBtn'+ns).addEventListener('click', ()=>{ bd.remove(); markDone('shelf'); });
    } else if(nearest==='cupboard'){
      const bd=showModal(`
        <h3>Cupboard</h3>
        <p>How many jars will you set up for this test?</p>
        <div class="setup-options">
          <button data-n="4">4</button><button data-n="5">5</button><button data-n="6">6</button>
          <button data-n="7">7</button><button data-n="8">8</button>
        </div>
        <p style="margin-top:10px;font-size:.72rem">More jars = finer dose steps, easier to pinpoint the optimum.</p>`);
      bd.querySelectorAll('button[data-n]').forEach(b=>{
        b.addEventListener('click', ()=>{ pickedNJars=+b.dataset.n; bd.remove(); markDone('cupboard'); });
      });
    } else if(nearest==='tap'){
      const bd=showModal(`
        <h3>Water tap</h3>
        <p>How much sample will you pour into each jar?</p>
        <div class="setup-options">
          <button data-v="0.5">0.5 L</button><button data-v="1">1 L</button>
          <button data-v="1.5">1.5 L</button><button data-v="2">2 L</button>
        </div>
        <p style="margin-top:10px;font-size:.72rem">Smaller volumes are quicker but noisier; larger volumes are steadier but slower to mix.</p>`);
      bd.querySelectorAll('button[data-v]').forEach(b=>{
        b.addEventListener('click', ()=>{ pickedVolume=+b.dataset.v; bd.remove(); markDone('tap'); });
      });
    }
  }

  beginBtn.addEventListener('click', ()=>{
    if(beginBtn.disabled) return;
    opts.onAllDone(pickedNJars, pickedVolume);
  });
};
</script>

<script>
/* ============================================================
   JAR TEST 1 — original engine, unmodified except for one
   added line in finishAll() that signals quiz-gate readiness
   ============================================================ */
(function(){
  "use strict";
  const $ = id => document.getElementById(id);
  let NJARS = 6;
  let VOLUME_L = 1;

  const LEVELS = {
    easy:   { ntu:[35,55], ph:[6.8,7.4], window:0.45, label:"easy" },
    normal: { ntu:[28,75], ph:[6.2,7.8], window:0.30, label:"normal" },
    hard:   { ntu:[20,95], ph:[5.4,8.6], window:0.22, label:"hard" }
  };
  let level='easy';
  let sample, optDose;
  let jars=[];               // {dose, finalNtu, fq, ntuNow, flocNow, settledFrac}
  let phase='idle';          // idle|flash|slow|settle|done
  let running=false;
  let history=[];            // arrays of {dose,ntu} from previous runs (for ghosting)

  // mixing programme (read from sliders at run time)
  let prog={flashRpm:200,flashTime:60,slowRpm:40,slowTime:15,settleTime:20};

  const rnd=(a,b)=>a+Math.random()*(b-a);
  const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));

  function newSample(){
    const L=LEVELS[level];
    const rawNtu=+rnd(L.ntu[0],L.ntu[1]).toFixed(0);
    const ph=+rnd(L.ph[0],L.ph[1]).toFixed(1);
    const phPenalty=Math.abs(ph-7.0)*4;
    optDose=clamp(1.2+rawNtu*0.025+phPenalty*0.1,1.5,4.0);
    sample={rawNtu,ph};
    const topDose=4;
    const defaults=Array.from({length:NJARS},(_,i)=>+(i*(topDose/(NJARS-1))).toFixed(1));
    jars=[];
    for(let i=0;i<NJARS;i++) jars.push({dose:defaults[i],finalNtu:rawNtu,fq:0,ntuNow:rawNtu,flocNow:0,settledFrac:0});
    phase='idle'; running=false;
    $('rawNtuPill').textContent=rawNtu;
    $('rawPhPill').textContent=ph.toFixed(1);
    renderJars();
    $('resBody').innerHTML='<tr><td colspan="4" style="color:var(--ink-faint);text-align:center;padding:14px">Run the jars to see results.</td></tr>';
    $('interpHint').textContent='';
    setStatus('idle','New raw water loaded: '+rawNtu+' NTU, pH '+ph.toFixed(1)+'. Set doses and a mixing programme, then start the run.');
    drawGraph();
  }

  // ---------- physics ----------
  // 1) dose response: U-shaped removal vs dose (under-dose poor, optimum best, over-dose restabilises)
  function doseRemoval(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=LEVELS[level].window; let r;
    if(ratio<1) r=0.96*Math.pow(ratio,1.6);
    else { const over=(ratio-1)/(w*3.0); r=0.96*Math.exp(-Math.pow(over,1.7)*1.1); }
    return clamp(r,0,0.965);
  }
  // 2) mixing efficiency 0..1 from the programme — same for every jar
  function flashEff(){
    // dispersion: needs enough speed AND time. Below band or too short => penalty. Very high speed fine here.
    // larger sample volumes take proportionally longer to disperse coagulant into evenly.
    const rpm=prog.flashRpm, t=prog.flashTime;
    const volFactor = 0.7 + 0.3*VOLUME_L;
    const reqTime = 30*volFactor;
    const speedF = rpm<150 ? 0.4+0.6*((rpm-50)/100) : 1.0;          // ramps up to 150rpm
    const timeF  = t<reqTime ? 0.5+0.5*((t-10)/(reqTime-10)) : 1.0;
    return clamp(speedF*timeF,0,1);
  }
  function flocEff(){
    // floc growth: optimal slow-mix band 20-50rpm; too slow under-mixes, too fast shears.
    // larger volumes need a bit more time in the slow-mix window too.
    const rpm=prog.slowRpm, t=prog.slowTime;
    const volFactor = 0.7 + 0.3*VOLUME_L;
    const lowT=10*volFactor, highT=20*volFactor;
    let speedF;
    if(rpm<20) speedF=0.45+0.55*((rpm-10)/10);                      // under-mixing
    else if(rpm<=50) speedF=1.0;                                    // sweet spot
    else speedF=clamp(1.0-(rpm-50)/90,0.2,1.0);                     // shear breakup
    const timeF = t<lowT ? 0.5+0.5*((t-2)/(lowT-2)) : (t>highT? 1.0 : 0.85+0.15*((t-lowT)/(highT-lowT)));
    return clamp(speedF*timeF,0,1);
  }
  function settleFactor(){
    // fraction of formed floc that has actually settled out of the supernatant
    const t=prog.settleTime;            // minutes
    return clamp(1-Math.exp(-t/12),0,0.985);   // ~63% by 12min, ~92% by 30min
  }

  // combine into final supernatant NTU for a jar
  function computeFinal(dose){
    const removalChem=doseRemoval(dose);              // best-case removal from chemistry
    const mixComposite=flashEff()*flocEff();          // 1.0 when programme is good
    const settle=settleFactor();                      // ~0.98 with long settling
    // effective removal = chemical potential * mixing quality * settling quality.
    // Tuned so a well-run optimal dose reaches clear water, while poor mixing/settling penalise hard.
    const removal=removalChem*(0.45+0.55*mixComposite)*(0.55+0.45*settle);
    // smaller volumes are more affected by jar-wall drag: proportionally noisier readings.
    const noiseScale=clamp(1/Math.sqrt(VOLUME_L),0.6,1.8);
    const noise=rnd(-0.012,0.012)*noiseScale;
    return clamp(sample.rawNtu*(1-clamp(removal+noise,0,0.965)),0.4,sample.rawNtu);
  }
  // floc visual quality for a jar (independent of settling)
  function flocQuality(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=LEVELS[level].window;
    const chem = ratio<1 ? Math.pow(ratio,1.3) : Math.exp(-Math.pow((ratio-1)/(w*3),1.7)*1.1);
    return clamp(chem*flashEff()*flocEff(),0,1);
  }

  // ---------- run sequence ----------
  let timer=null;
  function readProgramme(){
    prog.flashRpm=+$('flashRpm').value; prog.flashTime=+$('flashTime').value;
    prog.slowRpm=+$('slowRpm').value;   prog.slowTime=+$('slowTime').value;
    prog.settleTime=+$('settleTime').value;
  }
  // compressed display durations (seconds of real time) for each phase
  function phaseDurations(){
    return [
      {phase:'flash', dur:Math.max(1.2, prog.flashTime/30)},       // scaled
      {phase:'slow',  dur:Math.max(3.0, prog.slowTime*0.4)},
      {phase:'settle',dur:Math.max(2.5, prog.settleTime*0.25)}
    ];
  }
  function runAll(){
    if(running)return;
    readProgramme();
    jars.forEach((j,i)=>{ const v=parseFloat($('dose'+i).value); j.dose=isNaN(v)?0:clamp(v,0,5); });
    jars.forEach(j=>{ j.finalNtu=computeFinal(j.dose); j.fq=flocQuality(j.dose);
      j.ntuNow=sample.rawNtu; j.flocNow=0; j.settledFrac=0; });
    running=true; setControls(false);
    const SEQ=phaseDurations();
    let stepIdx=0, phaseT=0;
    timer=setInterval(()=>{
      const seg=SEQ[stepIdx]; phase=seg.phase; phaseT+=0.04;
      const frac=clamp(phaseT/seg.dur,0,1);
      jars.forEach(j=>{
        const startNtu=sample.rawNtu;
        if(seg.phase==='flash'){ j.flocNow=j.fq*0.35*frac; j.ntuNow=startNtu*(1-0.08*frac); }
        else if(seg.phase==='slow'){ j.flocNow=j.fq*(0.35+0.65*frac);
          j.ntuNow=startNtu-(startNtu-j.finalNtu)*0.30*frac; }
        else { j.settledFrac=frac;
          j.ntuNow=startNtu-(startNtu-j.finalNtu)*(0.30+0.70*frac); j.flocNow=j.fq*(1-0.55*frac); }
      });
      updateStirrerUI();
      renderJars();
      if(seg.phase==='flash') setStatus('flash','Rapid mix — '+prog.flashRpm+' rpm dispersing the coagulant through all six jars.');
      else if(seg.phase==='slow') setStatus('slow','Slow mix — '+prog.slowRpm+' rpm, gently growing the floc.');
      else setStatus('settle','Settling — floc sinking, clear water forming at the top.');
      if(phaseT>=seg.dur){ stepIdx++; phaseT=0; if(stepIdx>=SEQ.length) finishAll(); }
    },40);
  }
  function finishAll(){
    clearInterval(timer); timer=null;
    jars.forEach(j=>{ j.ntuNow=j.finalNtu; j.flocNow=j.fq*0.45; j.settledFrac=1; });
    phase='done'; running=false;
    updateStirrerUI(); renderJars(); renderResults();
    history.push(jars.map(j=>({dose:j.dose,ntu:j.finalNtu})));
    if(history.length>4) history.shift();
    drawGraph();
    setControls(true);
    const best=jars.reduce((a,b)=>b.finalNtu<a.finalNtu?b:a,jars[0]);
    setStatus('done','Run complete. Lowest turbidity was '+best.finalNtu.toFixed(1)+' NTU at '+best.dose+' mg/L. Compare all six on the graph.');
    // Lab collection check
    window.labCollection.checkAfterRun('jar1', jars, optDose, sample.rawNtu, prog, null);
    if (typeof window.afterJar1Complete === 'function') window.afterJar1Complete();
  }

  // ---------- stirrer status ----------
  function updateStirrerUI(){
    const bar=$('stirrerBar');
    let rpm=0,sweep=1.1;
    if(phase==='flash'){ rpm=prog.flashRpm; sweep=0.5; bar.classList.add('mixing'); }
    else if(phase==='slow'){ rpm=prog.slowRpm; sweep=1.6; bar.classList.add('mixing'); }
    else bar.classList.remove('mixing');
    bar.style.setProperty('--sweep',sweep+'s');
    $('rpmCap').textContent = (phase==='flash'||phase==='slow') ? rpm+' rpm' : 'stopped';
    $('phaseCap').textContent = {idle:'idle',flash:'rapid mix',slow:'slow mix',settle:'settling',done:'done'}[phase];
  }

  function setStatus(ph,txt){ $('phaseDot').className='phase-dot '+(ph==='idle'?'':ph); $('statusText').textContent=txt; }

  // ---------- jar drawing ----------
  function jarSVG(j,idx,isBest){
    const ntu=j.ntuNow;
    const cloud=clamp(ntu/90,0,1);
    const cC=[26,86,110],mC=[150,140,120],mix=(a,b,t)=>Math.round(a+(b-a)*t);
    const wr=mix(cC[0],mC[0],cloud),wg=mix(cC[1],mC[1],cloud),wb=mix(cC[2],mC[2],cloud);
    const W=120,H=150,top=26,bot=132,left=20,right=100;
    let parts=''; const fq=j.flocNow;
    if(fq>0.04){
      const n=Math.floor(3+fq*16), settling=phase==='settle'||phase==='done';
      for(let i=0;i<n;i++){
        const rx=left+6+((i*29+idx*7)%(right-left-12)); let ry;
        if(settling){ const s=phase==='done'?1:j.settledFrac;
          ry=top+10+(bot-top-20)*((((i*23)%100)/100)*(1-s*0.8)+s*0.85);
        } else ry=top+10+(((i*23)%100)/100)*(bot-top-20);
        const sz=1.3+fq*2.6+(i%2);
        parts+=`<circle cx="${rx.toFixed(1)}" cy="${ry.toFixed(1)}" r="${sz.toFixed(1)}" fill="rgba(196,176,120,${0.35+fq*0.4})"/>`;
      }
    }
    let sludge='';
    if((phase==='settle'||phase==='done')&&fq>0.12){
      const h=4+fq*14*(phase==='done'?1:j.settledFrac);
      sludge=`<path d="M${left} ${bot} L${right} ${bot} L${right} ${bot-h} Q${(left+right)/2} ${bot-h-5} ${left} ${bot-h} Z" fill="rgba(170,150,100,0.55)"/>`;
    }
    let paddle=''; const cx=(left+right)/2;
    if(phase==='flash'||phase==='slow'){
      const fast=phase==='flash', a=(Date.now()/(fast?70:240)+idx)%(Math.PI*2);
      const cy=(top+bot)/2+4, pw=Math.cos(a)*18;
      paddle=`<line x1="${cx}" y1="${top-14}" x2="${cx}" y2="${cy}" stroke="#9bbfd8" stroke-width="2.5"/><ellipse cx="${cx}" cy="${cy}" rx="${Math.abs(pw).toFixed(1)}" ry="4" fill="rgba(155,191,216,0.5)"/>`;
    } else paddle=`<line x1="${cx}" y1="${top-14}" x2="${cx}" y2="${(top+bot)/2+4}" stroke="#6f97b0" stroke-width="2"/>`;
    const ring=isBest?`<rect x="6" y="14" width="108" height="128" rx="14" fill="none" stroke="#5fd38a" stroke-width="2.5"/>`:'';
    return `<svg viewBox="0 0 ${W} ${H}" role="img" aria-label="Jar ${idx+1}">
      <defs><linearGradient id="w${idx}" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0" stop-color="rgb(${wr+15},${wg+15},${wb+15})"/><stop offset="1" stop-color="rgb(${wr},${wg},${wb})"/></linearGradient></defs>
      ${ring}
      <path d="M${left} ${top} L${right} ${top} L${right} ${bot} Q${(left+right)/2} ${bot+6} ${left} ${bot} Z" fill="url(#w${idx})"/>
      <ellipse cx="${(left+right)/2}" cy="${top}" rx="${(right-left)/2}" ry="5" fill="rgba(255,255,255,0.10)"/>
      ${sludge}${parts}${paddle}
      <path d="M${left-3} ${top-8} L${left} ${top-4} L${left} ${bot} Q${(left+right)/2} ${bot+8} ${right} ${bot} L${right} ${top-4} L${right+3} ${top-8}" fill="none" stroke="#a9cce0" stroke-width="2" stroke-linejoin="round"/>
    </svg>`;
  }
  function ntuClass(n){ return n<=4?'good':n<=15?'mid':'bad'; }
  function flocWord(fq){ return fq<0.05?'none':fq<0.3?'pin':fq<0.65?'medium':'large'; }

  function renderJars(){
    let bestIdx=-1;
    if(phase==='done') bestIdx=jars.reduce((bi,j,i,a)=>j.finalNtu<a[bi].finalNtu?i:bi,0);
    $('jarBank').innerHTML=jars.map((j,i)=>{
      const isBest=i===bestIdx;
      let readout='';
      if(phase==='done') readout=`<span class="ntu-num ${ntuClass(j.finalNtu)}">${j.finalNtu.toFixed(1)} NTU</span><br>floc: ${flocWord(j.fq)}`;
      else if(phase==='idle') readout='&nbsp;';
      else readout=`<span class="ntu-num">${j.ntuNow.toFixed(0)}</span> NTU`;
      return `<div class="jar-cell ${isBest?'clearest':''}">
        <span class="jarno">Jar ${i+1}</span>
        ${jarSVG(j,i,isBest)}
        <div class="dose-wrap">
          <input class="dose-input" id="dose${i}" type="number" min="0" max="5" step="0.1" value="${j.dose}" ${running?'disabled':''}>
          <span class="dose-lbl">mg/L alum</span>
          <span class="dose-total" id="doseTotal${i}">= ${(j.dose*VOLUME_L).toFixed(1)} mg total</span>
        </div>
        <div class="readout-mini">${readout}</div>
      </div>`;
    }).join('');
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.addEventListener('change',()=>{
      const v=parseFloat(el.value);
      j.dose=isNaN(v)?0:clamp(v,0,80);
      const tot=$('doseTotal'+i); if(tot) tot.textContent=`= ${(j.dose*VOLUME_L).toFixed(1)} mg total`;
    }); });
  }

  function renderResults(){
    const rows=jars.map((j,i)=>({...j,jar:i+1})).sort((a,b)=>a.dose-b.dose);
    const bestNtu=Math.min(...jars.map(j=>j.finalNtu));
    $('resBody').innerHTML=rows.map(r=>{
      const isBest=Math.abs(r.finalNtu-bestNtu)<0.001;
      return `<tr class="${isBest?'best':''}"><td>${r.jar}${isBest?' \u2605':''}</td><td>${r.dose} mg/L</td><td>${flocWord(r.fq)}</td><td>${r.finalNtu.toFixed(1)}</td></tr>`;
    }).join('');
    const best=jars.reduce((a,b)=>b.finalNtu<a.finalNtu?b:a,jars[0]);
    const removal=(1-best.finalNtu/sample.rawNtu)*100;
    let note=`Best jar removed ${removal.toFixed(0)}% of turbidity at ${best.dose} mg/L. `;
    if(flocEff()<0.6) note+='Your slow-mix settings limited floc growth — check the rpm band. ';
    else if(flashEff()<0.6) note+='Rapid mix was too weak/short to disperse the coagulant well. ';
    else if(settleFactor()<0.6) note+='Short settle time left floc suspended — try more settling. ';
    $('interpHint').textContent=note;
  }

  // ---------- graph ----------
  function drawGraph(){
    const svg=$('graph'), W=460,H=300, padL=46,padR=16,padT=16,padB=42;
    const x0=padL,x1=W-padR,y0=H-padB,y1=padT;
    const maxDose=80, maxNtu=Math.max(60, sample?Math.ceil(sample.rawNtu/10)*10:60);
    const sx=d=>x0+(d/maxDose)*(x1-x0);
    const sy=n=>y0-(n/maxNtu)*(y0-y1);
    let g=`<rect x="0" y="0" width="${W}" height="${H}" fill="transparent"/>`;
    // grid + axes
    for(let n=0;n<=maxNtu;n+=Math.max(10,Math.round(maxNtu/6/10)*10)){
      g+=`<line x1="${x0}" y1="${sy(n)}" x2="${x1}" y2="${sy(n)}" stroke="#1d3f55" stroke-width="1"/>`;
      g+=`<text x="${x0-8}" y="${sy(n)+4}" text-anchor="end" font-family="monospace" font-size="10" fill="#5c8299">${n}</text>`;
    }
    for(let d=0;d<=maxDose;d+=20){
      g+=`<line x1="${sx(d)}" y1="${y0}" x2="${sx(d)}" y2="${y1}" stroke="#16344a" stroke-width="1"/>`;
      g+=`<text x="${sx(d)}" y="${y0+18}" text-anchor="middle" font-family="monospace" font-size="10" fill="#5c8299">${d}</text>`;
    }
    g+=`<text x="${(x0+x1)/2}" y="${H-6}" text-anchor="middle" font-family="sans-serif" font-size="11" fill="#8fb4cc">Alum dose (mg/L)</text>`;
    g+=`<text transform="translate(13,${(y0+y1)/2}) rotate(-90)" text-anchor="middle" font-family="sans-serif" font-size="11" fill="#8fb4cc">Final turbidity (NTU)</text>`;

    // ghosted history
    history.slice(0,-0===0?history.length:0).forEach((run,ri)=>{
      const isLast = ri===history.length-1;
      if(isLast) return; // current run drawn separately
      const pts=run.slice().sort((a,b)=>a.dose-b.dose);
      let path=pts.map((p,k)=>(k?'L':'M')+sx(p.dose)+' '+sy(p.ntu)).join(' ');
      g+=`<path d="${path}" fill="none" stroke="#3a5d72" stroke-width="1.5" opacity="0.5"/>`;
    });

    // current run
    if(phase==='done' && jars.length){
      const pts=jars.map(j=>({dose:j.dose,ntu:j.finalNtu})).sort((a,b)=>a.dose-b.dose);
      const bestNtu=Math.min(...pts.map(p=>p.ntu));
      let path=pts.map((p,k)=>(k?'L':'M')+sx(p.dose)+' '+sy(p.ntu)).join(' ');
      g+=`<path d="${path}" fill="none" stroke="#46c2a8" stroke-width="2.5"/>`;
      pts.forEach(p=>{
        const best=Math.abs(p.ntu-bestNtu)<0.001;
        g+=`<circle cx="${sx(p.dose)}" cy="${sy(p.ntu)}" r="${best?6:4.5}" fill="${best?'#5fd38a':'#46c2a8'}" stroke="#0a1822" stroke-width="1.5"/>`;
      });
    } else {
      g+=`<text x="${(x0+x1)/2}" y="${(y0+y1)/2}" text-anchor="middle" font-family="sans-serif" font-size="12" fill="#5c8299">Run the jars to plot the dose–turbidity curve</text>`;
    }
    svg.innerHTML=g;
  }

  // ---------- ui helpers ----------
  function setControls(on){
    ['runBtn','resetBtn','spreadBtn','standardBtn','flashRpm','flashTime','slowRpm','slowTime','settleTime'].forEach(id=>{ const el=$(id); if(el)el.disabled=!on; });
    document.querySelectorAll('#levelSelect button').forEach(b=>b.disabled=!on);
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.disabled=!on; });
  }
  function bindSlider(id,vid,fmt){
    const el=$(id), out=$(vid);
    const upd=()=>{ out.textContent=fmt?fmt(el.value):el.value;
      const pct=(el.value-el.min)/(el.max-el.min)*100; el.style.setProperty('--fill',pct+'%'); };
    el.addEventListener('input',upd); upd();
  }
  function autoSpread(){
    const top=clamp(+(optDose*1.8).toFixed(1),3,5);
    const step=top/(NJARS-1);
    jars.forEach((j,i)=>{ j.dose=+(i*step).toFixed(1); });
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.value=j.dose; });
    setStatus('idle','Doses spread from 0 to '+jars[NJARS-1].dose+' mg/L. Adjust any jar, set your mixing programme, then start.');
  }
  function standardProgramme(){
    $('flashRpm').value=200; $('flashTime').value=60;
    $('slowRpm').value=40;   $('slowTime').value=15; $('settleTime').value=20;
    ['flashRpm','flashTime','slowRpm','slowTime','settleTime'].forEach(id=>$(id).dispatchEvent(new Event('input')));
    setStatus('idle','Standard programme loaded: 200 rpm / 60 s flash, 40 rpm / 15 min slow, 20 min settle.');
  }

  // ---------- wiring ----------
  bindSlider('flashRpm','flashRpmV');
  bindSlider('flashTime','flashTimeV');
  bindSlider('slowRpm','slowRpmV');
  bindSlider('slowTime','slowTimeV');
  bindSlider('settleTime','settleTimeV');

  $('runBtn').addEventListener('click',runAll);
  $('spreadBtn').addEventListener('click',()=>{ if(!running)autoSpread(); });
  $('standardBtn').addEventListener('click',()=>{ if(!running)standardProgramme(); });
  $('resetBtn').addEventListener('click',()=>{ if(running)return;
    if(!confirm('New raw water clears the current jars (graph history is kept). Continue?'))return; newSample(); });
  document.querySelectorAll('#levelSelect button').forEach(b=>{
    b.addEventListener('click',()=>{ if(running)return;
      document.querySelectorAll('#levelSelect button').forEach(x=>x.classList.remove('active'));
      b.classList.add('active'); level=b.dataset.lvl; history=[]; newSample();
    });
  });

  setInterval(()=>{ if(running)renderJars(); },70);

  // ---------- setup: walkable lab room (beaker count + volume) ----------
  window.initLabRoom({
    ns: '1',
    onAllDone(n, v){
      NJARS=n; VOLUME_L=v;
      $('setupCard1').style.display='none';
      // Show quiz instead of sim
      document.getElementById('preQuizCard').style.display='block';
      document.getElementById('preQuizCard').scrollIntoView({behavior:'smooth',block:'start'});
      // Store values for after quiz
      window._jar1Setup = {n, v};
    }
  });

  // --- PRE-QUIZ LOGIC ---
  (function(){
    let pqAnswered=0, pqCorrect=0;
    const feedback = {
      1: {
        right:"Correct! Alum is a coagulant. It neutralises the charges on suspended particles, allowing them to clump together and be removed.",
        wrong:"Not quite. Alum is a coagulant — it destabilises suspended particles by neutralising their surface charge, so they can clump together."
      },
      2: {
        right:"Correct! A jar test lets you compare multiple coagulant doses side by side to find the one that produces the clearest water.",
        wrong:"Not quite. A jar test is used to find the optimal coagulant dose. You test several doses in separate jars and compare the results."
      },
      3: {
        right:"Correct! Coagulation is the chemical step — adding a coagulant to destabilise particles. Flocculation is the gentle mixing that allows those particles to form larger clumps called floc.",
        wrong:"Not quite. Coagulation is the chemical addition that destabilises particles. Flocculation is the gentle mixing that brings them together into larger clumps (floc)."
      }
    };

    document.querySelectorAll('.pq-options').forEach((optDiv,qi)=>{
      const qNum = qi+1;
      optDiv.querySelectorAll('button').forEach(btn=>{
        btn.addEventListener('click',function(){
          const isCorrect = btn.dataset.a==='correct';
          const fb = document.getElementById('pq'+qNum+'fb');
          const buttons = optDiv.querySelectorAll('button');
          buttons.forEach(b=>{ b.disabled=true; if(b!==btn) b.style.opacity='0.5'; });
          if(isCorrect){
            btn.classList.add('correct');
            fb.className='pq-feedback show fb-right';
            fb.textContent=feedback[qNum].right;
            pqCorrect++;
          } else {
            btn.classList.add('wrong');
            fb.className='pq-feedback show fb-wrong';
            fb.textContent=feedback[qNum].wrong;
            buttons.forEach(b=>{ if(b.dataset.a==='correct'){b.classList.add('correct');b.style.opacity='1';} });
          }
          pqAnswered++;
          if(pqAnswered===3){
            document.getElementById('pqScoreNum').textContent=pqCorrect;
            document.getElementById('pqScore').style.display='block';
            document.getElementById('continueSimBtn').style.display='inline-flex';

            // Lab collection check
            window.labCollection.checkAfterQuiz('jar1', pqCorrect, 3);

            // Show reward
            const rc = document.getElementById('rewardCard');
            const ri = document.getElementById('rewardIcon');
            const rn = document.getElementById('rewardName');
            const rd = document.getElementById('rewardDesc');
            if(pqCorrect===3){
              ri.textContent='🥽';
              rn.textContent='Safety Goggles Unlocked!';
              rd.textContent='Perfect score! A true water treatment professional. You earned safety goggles for your lab equipment collection.';
              rc.className='reward-card show gold';
            } else if(pqCorrect===2){
              ri.textContent='🧤';
              rn.textContent='Nitrile Gloves Unlocked!';
              rd.textContent='Almost there! You earned nitrile gloves for your lab equipment collection. Review the one you missed and try again next time.';
              rc.className='reward-card show silver';
            } else {
              ri.textContent='💩';
              rn.textContent='You got... poop.';
              rd.textContent='Looks like you need to study up on your water treatment basics! Don\'t worry, you can still run the simulator and learn by doing.';
              rc.className='reward-card show poop-tier';
            }
          }
        });
      });
    });

    document.getElementById('continueSimBtn').addEventListener('click',function(){
      document.getElementById('preQuizCard').style.display='none';
      const s = window._jar1Setup;
      const simEl=document.getElementById('jar1Sim');
      simEl.style.display='block';
      $('jarBank').style.setProperty('--njars',s.n);
      $('jar1Title').textContent=`Gang stirrer — ${s.n} \u00d7 ${s.v} L jars`;
      history=[];
      newSample();
      simEl.scrollIntoView({behavior:'smooth',block:'start'});
    });
  })();
})();
</script>

<script>
/* ============================================================
   JAR TEST 2 — Spring Snowmelt (cold, low pH, high turbidity)
   Adapted copy of the Jar Test 1 engine: same structure, ids
   suffixed with "2", physics tuned for cold-water conditions.
   ============================================================ */
(function(){
  "use strict";
  const $ = id => document.getElementById(id+'2');
  let NJARS = 6;
  let VOLUME_L = 1;

  // Fixed profile with slight run-to-run variability, always cold/low-pH/high-turbidity
  const SPRING = { ntu:[70,90], ph:[5.8,6.5], temp:[3,7], window:0.24 };

  let sample, optDose;
  let jars=[];
  let phase='idle';
  let running=false;
  let history=[];

  let prog={flashRpm:220,flashTime:70,slowRpm:30,slowTime:20,settleTime:25};

  const rnd=(a,b)=>a+Math.random()*(b-a);
  const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));

  function newSample(){
    const rawNtu=+rnd(SPRING.ntu[0],SPRING.ntu[1]).toFixed(0);
    const ph=+rnd(SPRING.ph[0],SPRING.ph[1]).toFixed(1);
    const temp=+rnd(SPRING.temp[0],SPRING.temp[1]).toFixed(1);
    const phPenalty=Math.abs(ph-7.0)*4;
    // Cold + low pH: needs a meaningfully higher alum dose than a normal-condition jar test
    optDose=clamp((1.2+rawNtu*0.025+phPenalty*0.1)*1.4,2.5,5.5);
    sample={rawNtu,ph,temp};
    const topDose=5;
    const defaults=Array.from({length:NJARS},(_,i)=>+(i*(topDose/(NJARS-1))).toFixed(1));
    jars=[];
    for(let i=0;i<NJARS;i++) jars.push({dose:defaults[i],finalNtu:rawNtu,fq:0,ntuNow:rawNtu,flocNow:0,settledFrac:0});
    phase='idle'; running=false;
    $('rawNtuPill').textContent=rawNtu;
    $('rawPhPill').textContent=ph.toFixed(1);
    $('tempPill').textContent=temp;
    renderJars();
    $('resBody').innerHTML='<tr><td colspan="4" style="color:var(--ink-faint);text-align:center;padding:14px">Run the jars to see results.</td></tr>';
    $('interpHint').textContent='';
    setStatus('idle','Spring snowmelt water loaded: '+rawNtu+' NTU, pH '+ph.toFixed(1)+', '+temp+'\u00b0C. Cold, low-pH water is tougher to treat — set doses and a mixing programme, then start the run.');
    drawGraph();
  }

  // ---------- physics (cold-water tuned) ----------
  function doseRemoval(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=SPRING.window; let r;
    if(ratio<1) r=0.94*Math.pow(ratio,1.6);
    else r=0.94*Math.exp(-Math.pow((ratio-1)/w,1.8));
    return clamp(r,0,0.95);
  }
  function flashEff(){
    // cold water resists dispersion: needs higher speed and longer time than usual;
    // larger volumes need proportionally longer still.
    const rpm=prog.flashRpm, t=prog.flashTime;
    const volFactor = 0.7 + 0.3*VOLUME_L;
    const reqTime = 45*volFactor;
    const speedF = rpm<170 ? 0.35+0.65*((rpm-50)/120) : 1.0;
    const timeF  = t<reqTime ? 0.4+0.6*((t-10)/(reqTime-10)) : 1.0;
    return clamp(speedF*timeF,0,1);
  }
  function flocEff(){
    // narrower slow-mix sweet spot; viscosity slows floc growth even inside it
    const rpm=prog.slowRpm, t=prog.slowTime;
    const volFactor = 0.7 + 0.3*VOLUME_L;
    const lowT=15*volFactor, highT=25*volFactor;
    let speedF;
    if(rpm<25) speedF=0.32+0.55*((rpm-10)/15);
    else if(rpm<=38) speedF=1.0;
    else speedF=clamp(1.0-(rpm-38)/55,0.15,1.0);
    const timeF = t<lowT ? 0.4+0.5*((t-2)/(lowT-2)) : (t>highT ? 1.0 : 0.78+0.22*((t-lowT)/(highT-lowT)));
    return clamp(speedF*timeF*0.88,0,1); // viscosity penalty even at ideal settings
  }
  function settleFactor(){
    // cold, viscous water settles more slowly than normal
    const t=prog.settleTime;
    return clamp(1-Math.exp(-t/18),0,0.96);
  }
  function computeFinal(dose){
    const removalChem=doseRemoval(dose);
    const mixComposite=flashEff()*flocEff();
    const settle=settleFactor();
    const removal=removalChem*(0.42+0.58*mixComposite)*(0.5+0.5*settle);
    // smaller volumes are more affected by jar-wall drag: proportionally noisier readings.
    const noiseScale=clamp(1/Math.sqrt(VOLUME_L),0.6,1.8);
    const noise=rnd(-0.012,0.012)*noiseScale;
    return clamp(sample.rawNtu*(1-clamp(removal+noise,0,0.95)),0.4,sample.rawNtu);
  }
  function flocQuality(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=SPRING.window;
    const chem = ratio<1 ? Math.pow(ratio,1.3) : Math.exp(-Math.pow((ratio-1)/(w*3),1.7)*1.1);
    return clamp(chem*flashEff()*flocEff(),0,1);
  }

  // ---------- run sequence ----------
  let timer=null;
  function readProgramme(){
    prog.flashRpm=+$('flashRpm').value; prog.flashTime=+$('flashTime').value;
    prog.slowRpm=+$('slowRpm').value;   prog.slowTime=+$('slowTime').value;
    prog.settleTime=+$('settleTime').value;
  }
  function phaseDurations(){
    return [
      {phase:'flash', dur:Math.max(1.2, prog.flashTime/30)},
      {phase:'slow',  dur:Math.max(3.0, prog.slowTime*0.4)},
      {phase:'settle',dur:Math.max(2.5, prog.settleTime*0.25)}
    ];
  }
  function runAll(){
    if(running)return;
    readProgramme();
    jars.forEach((j,i)=>{ const v=parseFloat($('dose'+i).value); j.dose=isNaN(v)?0:clamp(v,0,6); });
    jars.forEach(j=>{ j.finalNtu=computeFinal(j.dose); j.fq=flocQuality(j.dose);
      j.ntuNow=sample.rawNtu; j.flocNow=0; j.settledFrac=0; });
    running=true; setControls(false);
    const SEQ=phaseDurations();
    let stepIdx=0, phaseT=0;
    timer=setInterval(()=>{
      const seg=SEQ[stepIdx]; phase=seg.phase; phaseT+=0.04;
      const frac=clamp(phaseT/seg.dur,0,1);
      jars.forEach(j=>{
        const startNtu=sample.rawNtu;
        if(seg.phase==='flash'){ j.flocNow=j.fq*0.35*frac; j.ntuNow=startNtu*(1-0.08*frac); }
        else if(seg.phase==='slow'){ j.flocNow=j.fq*(0.35+0.65*frac);
          j.ntuNow=startNtu-(startNtu-j.finalNtu)*0.30*frac; }
        else { j.settledFrac=frac;
          j.ntuNow=startNtu-(startNtu-j.finalNtu)*(0.30+0.70*frac); j.flocNow=j.fq*(1-0.55*frac); }
      });
      updateStirrerUI();
      renderJars();
      if(seg.phase==='flash') setStatus('flash','Rapid mix — '+prog.flashRpm+' rpm dispersing coagulant through cold, viscous water.');
      else if(seg.phase==='slow') setStatus('slow','Slow mix — '+prog.slowRpm+' rpm. Stay in the narrow band, or floc growth suffers.');
      else setStatus('settle','Settling — cold water floc sinks more slowly than usual.');
      if(phaseT>=seg.dur){ stepIdx++; phaseT=0; if(stepIdx>=SEQ.length) finishAll(); }
    },40);
  }
  function finishAll(){
    clearInterval(timer); timer=null;
    jars.forEach(j=>{ j.ntuNow=j.finalNtu; j.flocNow=j.fq*0.45; j.settledFrac=1; });
    phase='done'; running=false;
    updateStirrerUI(); renderJars(); renderResults();
    history.push(jars.map(j=>({dose:j.dose,ntu:j.finalNtu})));
    if(history.length>4) history.shift();
    drawGraph();
    setControls(true);
    const best=jars.reduce((a,b)=>b.finalNtu<a.finalNtu?b:a,jars[0]);
    setStatus('done','Run complete. Lowest turbidity was '+best.finalNtu.toFixed(1)+' NTU at '+best.dose+' mg/L. Compare all six on the graph.');
    // Lab collection check
    window.labCollection.checkAfterRun('jar2', jars, optDose, sample.rawNtu, prog, null);
    // Show button to proceed to Autumn scenario
    if(!document.getElementById('continueToAutumnBtn')){
      const wrap=document.createElement('div');
      wrap.style.cssText='text-align:center;padding:16px;';
      wrap.innerHTML='<button class="btn btn-primary" id="continueToAutumnBtn" style="margin-top:12px;">Continue to Jar Test #3 — Autumn Rain &#9654;</button>';
      document.getElementById('jar2Sim').appendChild(wrap);
      document.getElementById('continueToAutumnBtn').addEventListener('click',function(){
        document.getElementById('stepJar2').classList.remove('active');
        document.getElementById('stepJar2').classList.add('done');
        document.getElementById('stepQuiz3').classList.add('active');
        document.getElementById('jar2Wrap').style.display='none';
        document.getElementById('autumnQuizWrap').style.display='block';
        document.getElementById('autumnQuizWrap').scrollIntoView({behavior:'smooth',block:'start'});
      });
    }
  }

  // ---------- stirrer status ----------
  function updateStirrerUI(){
    const bar=$('stirrerBar');
    let rpm=0,sweep=1.1;
    if(phase==='flash'){ rpm=prog.flashRpm; sweep=0.5; bar.classList.add('mixing'); }
    else if(phase==='slow'){ rpm=prog.slowRpm; sweep=1.6; bar.classList.add('mixing'); }
    else bar.classList.remove('mixing');
    bar.style.setProperty('--sweep',sweep+'s');
    $('rpmCap').textContent = (phase==='flash'||phase==='slow') ? rpm+' rpm' : 'stopped';
    $('phaseCap').textContent = {idle:'idle',flash:'rapid mix',slow:'slow mix',settle:'settling',done:'done'}[phase];
  }

  function setStatus(ph,txt){ $('phaseDot').className='phase-dot '+(ph==='idle'?'':ph); $('statusText').textContent=txt; }

  // ---------- jar drawing ----------
  function jarSVG(j,idx,isBest){
    const ntu=j.ntuNow;
    const cloud=clamp(ntu/90,0,1);
    const cC=[26,86,110],mC=[150,140,120],mix=(a,b,t)=>Math.round(a+(b-a)*t);
    const wr=mix(cC[0],mC[0],cloud),wg=mix(cC[1],mC[1],cloud),wb=mix(cC[2],mC[2],cloud);
    const W=120,H=150,top=26,bot=132,left=20,right=100;
    let parts=''; const fq=j.flocNow;
    if(fq>0.04){
      const n=Math.floor(3+fq*16), settling=phase==='settle'||phase==='done';
      for(let i=0;i<n;i++){
        const rx=left+6+((i*29+idx*7)%(right-left-12)); let ry;
        if(settling){ const s=phase==='done'?1:j.settledFrac;
          ry=top+10+(bot-top-20)*((((i*23)%100)/100)*(1-s*0.8)+s*0.85);
        } else ry=top+10+(((i*23)%100)/100)*(bot-top-20);
        const sz=1.3+fq*2.6+(i%2);
        parts+=`<circle cx="${rx.toFixed(1)}" cy="${ry.toFixed(1)}" r="${sz.toFixed(1)}" fill="rgba(196,176,120,${0.35+fq*0.4})"/>`;
      }
    }
    let sludge='';
    if((phase==='settle'||phase==='done')&&fq>0.12){
      const h=4+fq*14*(phase==='done'?1:j.settledFrac);
      sludge=`<path d="M${left} ${bot} L${right} ${bot} L${right} ${bot-h} Q${(left+right)/2} ${bot-h-5} ${left} ${bot-h} Z" fill="rgba(170,150,100,0.55)"/>`;
    }
    let paddle=''; const cx=(left+right)/2;
    if(phase==='flash'||phase==='slow'){
      const fast=phase==='flash', a=(Date.now()/(fast?70:240)+idx)%(Math.PI*2);
      const cy=(top+bot)/2+4, pw=Math.cos(a)*18;
      paddle=`<line x1="${cx}" y1="${top-14}" x2="${cx}" y2="${cy}" stroke="#9bbfd8" stroke-width="2.5"/><ellipse cx="${cx}" cy="${cy}" rx="${Math.abs(pw).toFixed(1)}" ry="4" fill="rgba(155,191,216,0.5)"/>`;
    } else paddle=`<line x1="${cx}" y1="${top-14}" x2="${cx}" y2="${(top+bot)/2+4}" stroke="#6f97b0" stroke-width="2"/>`;
    const ring=isBest?`<rect x="6" y="14" width="108" height="128" rx="14" fill="none" stroke="#5fd38a" stroke-width="2.5"/>`:'';
    return `<svg viewBox="0 0 ${W} ${H}" role="img" aria-label="Jar ${idx+1}">
      <defs><linearGradient id="w2_${idx}" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0" stop-color="rgb(${wr+15},${wg+15},${wb+15})"/><stop offset="1" stop-color="rgb(${wr},${wg},${wb})"/></linearGradient></defs>
      ${ring}
      <path d="M${left} ${top} L${right} ${top} L${right} ${bot} Q${(left+right)/2} ${bot+6} ${left} ${bot} Z" fill="url(#w2_${idx})"/>
      <ellipse cx="${(left+right)/2}" cy="${top}" rx="${(right-left)/2}" ry="5" fill="rgba(255,255,255,0.10)"/>
      ${sludge}${parts}${paddle}
      <path d="M${left-3} ${top-8} L${left} ${top-4} L${left} ${bot} Q${(left+right)/2} ${bot+8} ${right} ${bot} L${right} ${top-4} L${right+3} ${top-8}" fill="none" stroke="#a9cce0" stroke-width="2" stroke-linejoin="round"/>
    </svg>`;
  }

  function ntuClass(n){ return n<=4?'good':n<=15?'mid':'bad'; }
  function flocWord(fq){ return fq<0.05?'none':fq<0.3?'pin':fq<0.65?'medium':'large'; }

  function renderJars(){
    let bestIdx=-1;
    if(phase==='done') bestIdx=jars.reduce((bi,j,i,a)=>j.finalNtu<a[bi].finalNtu?i:bi,0);
    $('jarBank').innerHTML=jars.map((j,i)=>{
      const isBest=i===bestIdx;
      let readout='';
      if(phase==='done') readout=`<span class="ntu-num ${ntuClass(j.finalNtu)}">${j.finalNtu.toFixed(1)} NTU</span><br>floc: ${flocWord(j.fq)}`;
      else if(phase==='idle') readout='&nbsp;';
      else readout=`<span class="ntu-num">${j.ntuNow.toFixed(0)}</span> NTU`;
      return `<div class="jar-cell ${isBest?'clearest':''}">
        <span class="jarno">Jar ${i+1}</span>
        ${jarSVG(j,i,isBest)}
        <div class="dose-wrap">
          <input class="dose-input" id="dose${i}2" type="number" min="0" max="6" step="0.1" value="${j.dose}" ${running?'disabled':''}>
          <span class="dose-lbl">mg/L alum</span>
          <span class="dose-total" id="doseTotal${i}2">= ${(j.dose*VOLUME_L).toFixed(1)} mg total</span>
        </div>
        <div class="readout-mini">${readout}</div>
      </div>`;
    }).join('');
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.addEventListener('change',()=>{
      const v=parseFloat(el.value);
      j.dose=isNaN(v)?0:clamp(v,0,90);
      const tot=$('doseTotal'+i); if(tot) tot.textContent=`= ${(j.dose*VOLUME_L).toFixed(1)} mg total`;
    }); });
  }

  function renderResults(){
    const rows=jars.map((j,i)=>({...j,jar:i+1})).sort((a,b)=>a.dose-b.dose);
    const bestNtu=Math.min(...jars.map(j=>j.finalNtu));
    $('resBody').innerHTML=rows.map(r=>{
      const isBest=Math.abs(r.finalNtu-bestNtu)<0.001;
      return `<tr class="${isBest?'best':''}"><td>${r.jar}${isBest?' \u2605':''}</td><td>${r.dose} mg/L</td><td>${flocWord(r.fq)}</td><td>${r.finalNtu.toFixed(1)}</td></tr>`;
    }).join('');
    const best=jars.reduce((a,b)=>b.finalNtu<a.finalNtu?b:a,jars[0]);
    const removal=(1-best.finalNtu/sample.rawNtu)*100;
    let note=`Best jar removed ${removal.toFixed(0)}% of turbidity at ${best.dose} mg/L. `;
    if(flocEff()<0.6) note+='Your slow-mix rpm is outside the narrow cold-water band — floc growth suffered. ';
    else if(flashEff()<0.6) note+='Rapid mix was too weak/short for cold, viscous water to disperse the coagulant well. ';
    else if(settleFactor()<0.6) note+='Short settle time left floc suspended — cold water needs more settling time. ';
    $('interpHint').textContent=note;
  }

  // ---------- graph ----------
  function drawGraph(){
    const svg=$('graph'), W=460,H=300, padL=46,padR=16,padT=16,padB=42;
    const x0=padL,x1=W-padR,y0=H-padB,y1=padT;
    const maxDose=90, maxNtu=Math.max(60, sample?Math.ceil(sample.rawNtu/10)*10:60);
    const sx=d=>x0+(d/maxDose)*(x1-x0);
    const sy=n=>y0-(n/maxNtu)*(y0-y1);
    let g=`<rect x="0" y="0" width="${W}" height="${H}" fill="transparent"/>`;
    for(let n=0;n<=maxNtu;n+=Math.max(10,Math.round(maxNtu/6/10)*10)){
      g+=`<line x1="${x0}" y1="${sy(n)}" x2="${x1}" y2="${sy(n)}" stroke="#1d3f55" stroke-width="1"/>`;
      g+=`<text x="${x0-8}" y="${sy(n)+4}" text-anchor="end" font-family="monospace" font-size="10" fill="#5c8299">${n}</text>`;
    }
    for(let d=0;d<=maxDose;d+=20){
      g+=`<line x1="${sx(d)}" y1="${y0}" x2="${sx(d)}" y2="${y1}" stroke="#16344a" stroke-width="1"/>`;
      g+=`<text x="${sx(d)}" y="${y0+18}" text-anchor="middle" font-family="monospace" font-size="10" fill="#5c8299">${d}</text>`;
    }
    g+=`<text x="${(x0+x1)/2}" y="${H-6}" text-anchor="middle" font-family="sans-serif" font-size="11" fill="#8fb4cc">Alum dose (mg/L)</text>`;
    g+=`<text transform="translate(13,${(y0+y1)/2}) rotate(-90)" text-anchor="middle" font-family="sans-serif" font-size="11" fill="#8fb4cc">Final turbidity (NTU)</text>`;

    history.forEach((run,ri)=>{
      const isLast = ri===history.length-1;
      if(isLast) return;
      const pts=run.slice().sort((a,b)=>a.dose-b.dose);
      let path=pts.map((p,k)=>(k?'L':'M')+sx(p.dose)+' '+sy(p.ntu)).join(' ');
      g+=`<path d="${path}" fill="none" stroke="#3a5d72" stroke-width="1.5" opacity="0.5"/>`;
    });

    if(phase==='done' && jars.length){
      const pts=jars.map(j=>({dose:j.dose,ntu:j.finalNtu})).sort((a,b)=>a.dose-b.dose);
      const bestNtu=Math.min(...pts.map(p=>p.ntu));
      let path=pts.map((p,k)=>(k?'L':'M')+sx(p.dose)+' '+sy(p.ntu)).join(' ');
      g+=`<path d="${path}" fill="none" stroke="#46c2a8" stroke-width="2.5"/>`;
      pts.forEach(p=>{
        const best=Math.abs(p.ntu-bestNtu)<0.001;
        g+=`<circle cx="${sx(p.dose)}" cy="${sy(p.ntu)}" r="${best?6:4.5}" fill="${best?'#5fd38a':'#46c2a8'}" stroke="#0a1822" stroke-width="1.5"/>`;
      });
    } else {
      g+=`<text x="${(x0+x1)/2}" y="${(y0+y1)/2}" text-anchor="middle" font-family="sans-serif" font-size="12" fill="#5c8299">Run the jars to plot the dose–turbidity curve</text>`;
    }
    svg.innerHTML=g;
  }

  // ---------- ui helpers ----------
  function setControls(on){
    ['runBtn','resetBtn','spreadBtn','standardBtn','flashRpm','flashTime','slowRpm','slowTime','settleTime'].forEach(id=>{ const el=$(id); if(el)el.disabled=!on; });
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.disabled=!on; });
  }
  function bindSlider(id,vid,fmt){
    const el=$(id), out=$(vid);
    const upd=()=>{ out.textContent=fmt?fmt(el.value):el.value;
      const pct=(el.value-el.min)/(el.max-el.min)*100; el.style.setProperty('--fill',pct+'%'); };
    el.addEventListener('input',upd); upd();
  }
  function autoSpread(){
    const top=clamp(+(optDose*1.6).toFixed(1),3.5,6);
    const step=top/(NJARS-1);
    jars.forEach((j,i)=>{ j.dose=+(i*step).toFixed(1); });
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.value=j.dose; });
    setStatus('idle','Doses spread from 0 to '+jars[NJARS-1].dose+' mg/L. Adjust any jar, set your mixing programme, then start.');
  }
  function standardProgramme(){
    $('flashRpm').value=220; $('flashTime').value=70;
    $('slowRpm').value=30;   $('slowTime').value=20; $('settleTime').value=25;
    ['flashRpm','flashTime','slowRpm','slowTime','settleTime'].forEach(id=>$(id).dispatchEvent(new Event('input')));
    setStatus('idle','Cold-water programme loaded: 220 rpm / 70 s flash, 30 rpm / 20 min slow, 25 min settle.');
  }

  // ---------- wiring ----------
  bindSlider('flashRpm','flashRpmV');
  bindSlider('flashTime','flashTimeV');
  bindSlider('slowRpm','slowRpmV');
  bindSlider('slowTime','slowTimeV');
  bindSlider('settleTime','settleTimeV');

  $('runBtn').addEventListener('click',runAll);
  $('spreadBtn').addEventListener('click',()=>{ if(!running)autoSpread(); });
  $('standardBtn').addEventListener('click',()=>{ if(!running)standardProgramme(); });
  $('resetBtn').addEventListener('click',()=>{ if(running)return;
    if(!confirm('New snowmelt sample clears the current jars (graph history is kept). Continue?'))return; newSample(); });

  setInterval(()=>{ if(running)renderJars(); },70);

  // ---------- setup: skip setup, init via event ----------
  window.addEventListener('jar2Ready', function(e){
    NJARS=e.detail.n; VOLUME_L=e.detail.v;
    $('jarBank').style.setProperty('--njars',NJARS);
    document.getElementById('jar2Title').textContent=`Gang stirrer — ${NJARS} \u00d7 ${VOLUME_L} L jars (Spring Snowmelt)`;
    history=[];
    newSample();
  });

  window.initLabRoom({
    ns: '2',
    onAllDone(n, v){
      NJARS=n; VOLUME_L=v;
      document.getElementById('setupCard2').style.display='none';
      const simEl=document.getElementById('jar2Sim');
      simEl.style.display='block';
      $('jarBank').style.setProperty('--njars',NJARS);
      document.getElementById('jar2Title').textContent=`Gang stirrer — ${NJARS} \u00d7 ${VOLUME_L} L jars (Spring Snowmelt)`;
      history=[];
      newSample();
      simEl.scrollIntoView({behavior:'smooth',block:'start'});
    }
  });
})();
</script>


<script>
/* ============================================================
   AUTUMN QUIZ
   ============================================================ */
(function(){
  let aqAnswered=0, aqCorrect=0;
  const feedback = {
    1: {
      right:"Correct! Heavy rain washes organic acids (like humic and fulvic acids) from decomposing leaves into the source water, lowering both pH and alkalinity.",
      wrong:"Not quite. Autumn rain washes organic acids from decomposing leaves into the water, lowering the pH and alkalinity. This makes treatment more challenging."
    },
    2: {
      right:"Correct! Lime or soda ash raises the pH and restores alkalinity. Alum works best in a pH range of about 6 to 7.5, so buffering first ensures coagulation can work properly.",
      wrong:"Not quite. Lime or soda ash is added to raise the pH into the range where alum works effectively (pH 6-7.5). Without buffering, alum struggles in acidic water."
    },
    3: {
      right:"Correct! Alum's coagulation chemistry is pH-dependent. Below about pH 6, the aluminium species that form are less effective at destabilising particles.",
      wrong:"Not quite. Alum works best between pH 6 and 7.5. Below that range, the aluminium chemistry shifts and coagulation becomes much less effective."
    }
  };

  document.querySelectorAll('.aq-opts').forEach(optDiv=>{
    const qNum = +optDiv.dataset.q;
    optDiv.querySelectorAll('button').forEach(btn=>{
      btn.addEventListener('click',function(){
        const isCorrect = btn.dataset.a==='correct';
        const fb = document.getElementById('aq'+qNum+'fb');
        const buttons = optDiv.querySelectorAll('button');
        buttons.forEach(b=>{ b.disabled=true; if(b!==btn) b.style.opacity='0.5'; });
        if(isCorrect){
          btn.classList.add('correct');
          fb.className='pq-feedback show fb-right';
          fb.textContent=feedback[qNum].right;
          aqCorrect++;
        } else {
          btn.classList.add('wrong');
          fb.className='pq-feedback show fb-wrong';
          fb.textContent=feedback[qNum].wrong;
          buttons.forEach(b=>{ if(b.dataset.a==='correct'){b.classList.add('correct');b.style.opacity='1';} });
        }
        aqAnswered++;
        if(aqAnswered===3){
          document.getElementById('aqScoreNum').textContent=aqCorrect;
          document.getElementById('aqScore').style.display='block';
          document.getElementById('continueAutumnBtn').style.display='inline-flex';
          const rc=document.getElementById('aqRewardCard');
          const ri=document.getElementById('aqRewardIcon');
          const rn=document.getElementById('aqRewardName');
          const rd=document.getElementById('aqRewardDesc');
          if(aqCorrect===3){
            ri.textContent='🧫'; rn.textContent='Petri Dishes Unlocked!';
            rd.textContent='Perfect score! You earned Petri Dishes for your lab equipment collection.';
            rc.className='reward-card show gold';
          } else if(aqCorrect===2){
            ri.textContent='🧤'; rn.textContent='Nitrile Gloves Unlocked!';
            rd.textContent='Almost there! You earned nitrile gloves. Review the one you missed.';
            rc.className='reward-card show silver';
          } else {
            ri.textContent='💩'; rn.textContent='You got... poop.';
            rd.textContent='Time to study up on autumn water chemistry!';
            rc.className='reward-card show poop-tier';
          }
          // Lab collection check
          window.labCollection.checkAfterQuiz('autumn', aqCorrect, 3);
        }
      });
    });
  });

  document.getElementById('continueAutumnBtn').addEventListener('click',function(){
    document.getElementById('autumnQuizWrap').style.display='none';
    document.getElementById('stepQuiz3').classList.remove('active');
    document.getElementById('stepQuiz3').classList.add('done');
    document.getElementById('stepJar3').classList.add('active');
    document.getElementById('jar3Wrap').style.display='block';
    // Skip setup, go straight to sim
    document.getElementById('setupCard3').style.display='none';
    document.getElementById('jar3Sim').style.display='block';
    window.dispatchEvent(new CustomEvent('jar3Ready', {detail:{n:6,v:1}}));
    document.getElementById('jar3Wrap').scrollIntoView({behavior:'smooth',block:'start'});
  });
})();

/* ============================================================
   JAR TEST 3 — Heavy Autumn Rain
   15°C, pH 5.8, NTU 45, organic acids from decomposing leaves.
   New mechanic: pH buffer (lime) slider before alum dosing.
   ============================================================ */
(function(){
  "use strict";
  const $ = id => document.getElementById(id+'3');
  let NJARS = 6;
  let VOLUME_L = 1;

  const AUTUMN = { ntu:[35,55], ph:[5.4,6.2], temp:[12,18], window:0.22 };
  const BASE_PH = 5.8;

  let sample, optDose, limeDose=0, effectivePh=BASE_PH;
  let jars=[];
  let phase='idle';
  let running=false;
  let history=[];

  let prog={flashRpm:200,flashTime:60,slowRpm:40,slowTime:15,settleTime:20};

  const rnd=(a,b)=>a+Math.random()*(b-a);
  const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));

  function calcEffectivePh(){
    effectivePh = clamp(sample.ph + limeDose*0.04, sample.ph, 8.5);
    $('effectivePh').textContent = effectivePh.toFixed(1);
  }

  function calcOptDose(){
    const phPenalty = effectivePh<6.0 ? (6.0-effectivePh)*8 : (effectivePh>7.5 ? (effectivePh-7.5)*3 : 0);
    const phBonus = (effectivePh>=6.0 && effectivePh<=7.5) ? 1.0 : clamp(1.0-phPenalty*0.08,0.3,1.0);
    optDose = clamp((1.5+sample.rawNtu*0.03)/phBonus, 2.0, 5.0);
  }

  function newSample(){
    const rawNtu=+rnd(AUTUMN.ntu[0],AUTUMN.ntu[1]).toFixed(0);
    const ph=+rnd(AUTUMN.ph[0],AUTUMN.ph[1]).toFixed(1);
    const temp=+rnd(AUTUMN.temp[0],AUTUMN.temp[1]).toFixed(0);
    sample={rawNtu,ph,temp};
    limeDose=+$('limeDose').value;
    calcEffectivePh();
    calcOptDose();
    const topDose=5;
    const defaults=Array.from({length:NJARS},(_,i)=>+(i*(topDose/(NJARS-1))).toFixed(1));
    jars=[];
    for(let i=0;i<NJARS;i++) jars.push({dose:defaults[i],finalNtu:rawNtu,fq:0,ntuNow:rawNtu,flocNow:0,settledFrac:0});
    phase='idle'; running=false;
    $('rawNtuPill').textContent=rawNtu;
    $('rawPhPill').textContent=ph.toFixed(1);
    $('tempPill').textContent=temp;
    renderJars();
    $('resBody').innerHTML='<tr><td colspan="4" style="color:var(--ink-faint);text-align:center;padding:14px">Run the jars to see results.</td></tr>';
    $('interpHint').textContent='';
    setStatus('idle','Autumn rain water: '+rawNtu+' NTU, pH '+ph+', '+temp+'°C. Set lime dose to buffer pH, then set alum doses and run.');
    drawGraph();
  }

  // lime slider updates effective pH in real time
  const limeSlider = $('limeDose');
  limeSlider.addEventListener('input',()=>{
    limeDose=+limeSlider.value;
    $('limeDoseV').textContent=limeDose;
    if(sample){ calcEffectivePh(); calcOptDose(); }
  });

  // ---------- physics ----------
  function doseRemoval(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=AUTUMN.window; let r;
    // pH penalty: if effective pH is outside sweet spot, reduce max removal
    const phEff = (effectivePh>=6.0 && effectivePh<=7.5) ? 1.0 :
                  clamp(1.0 - Math.abs(effectivePh<6.0 ? 6.0-effectivePh : effectivePh-7.5)*0.25, 0.2, 1.0);
    if(ratio<1) r=0.94*Math.pow(ratio,1.6)*phEff;
    else r=0.94*Math.exp(-Math.pow((ratio-1)/(w*3.0),1.7)*1.1)*phEff;
    return clamp(r,0,0.95);
  }
  function flashEff(){
    const rpm=prog.flashRpm, t=prog.flashTime;
    const volFactor=0.7+0.3*VOLUME_L;
    const reqTime=30*volFactor;
    const speedF=rpm<150?0.4+0.6*((rpm-50)/100):1.0;
    const timeF=t<reqTime?0.5+0.5*((t-10)/(reqTime-10)):1.0;
    return clamp(speedF*timeF,0,1);
  }
  function flocEff(){
    const rpm=prog.slowRpm, t=prog.slowTime;
    const volFactor=0.7+0.3*VOLUME_L;
    const lowT=10*volFactor, highT=20*volFactor;
    let speedF;
    if(rpm<20) speedF=0.45+0.55*((rpm-10)/10);
    else if(rpm<=50) speedF=1.0;
    else speedF=clamp(1.0-(rpm-50)/90,0.2,1.0);
    const timeF=t<lowT?0.5+0.5*((t-2)/(lowT-2)):(t>highT?1.0:0.85+0.15*((t-lowT)/(highT-lowT)));
    return clamp(speedF*timeF,0,1);
  }
  function settleFactor(){
    const t=prog.settleTime;
    return clamp(1-Math.exp(-t/12),0,0.985);
  }
  function computeFinal(dose){
    const removalChem=doseRemoval(dose);
    const mixComposite=flashEff()*flocEff();
    const settle=settleFactor();
    const removal=removalChem*(0.45+0.55*mixComposite)*(0.55+0.45*settle);
    const noiseScale=clamp(1/Math.sqrt(VOLUME_L),0.6,1.8);
    const noise=rnd(-0.012,0.012)*noiseScale;
    return clamp(sample.rawNtu*(1-clamp(removal+noise,0,0.96)),0.4,sample.rawNtu);
  }
  function flocQuality(dose){
    if(dose<=0) return 0;
    const ratio=dose/optDose, w=AUTUMN.window;
    const chem=ratio<1?Math.pow(ratio,1.3):Math.exp(-Math.pow((ratio-1)/(w*3),1.7)*1.1);
    return clamp(chem*flashEff()*flocEff(),0,1);
  }

  let timer=null;
  function readProgramme(){
    prog.flashRpm=+$('flashRpm').value; prog.flashTime=+$('flashTime').value;
    prog.slowRpm=+$('slowRpm').value; prog.slowTime=+$('slowTime').value;
    prog.settleTime=+$('settleTime').value;
  }
  function phaseDurations(){
    return [
      {phase:'flash',dur:Math.max(1.2,prog.flashTime/30)},
      {phase:'slow',dur:Math.max(3.0,prog.slowTime*0.4)},
      {phase:'settle',dur:Math.max(2.5,prog.settleTime*0.25)}
    ];
  }
  function runAll(){
    if(running)return;
    readProgramme();
    calcEffectivePh();
    calcOptDose();
    jars.forEach((j,i)=>{ const v=parseFloat($('dose'+i).value); j.dose=isNaN(v)?0:clamp(v,0,6); });
    jars.forEach(j=>{ j.finalNtu=computeFinal(j.dose); j.fq=flocQuality(j.dose);
      j.ntuNow=sample.rawNtu; j.flocNow=0; j.settledFrac=0; });
    running=true; setControls(false);
    const SEQ=phaseDurations();
    let stepIdx=0, phaseT=0;
    timer=setInterval(()=>{
      phaseT+=0.07;
      const cur=SEQ[stepIdx];
      const frac=clamp(phaseT/cur.dur,0,1);
      phase=cur.phase;
      if(cur.phase==='flash'){
        $('stirrerBar').className='stirrer-bar mixing';
        $('stirrerBar').style.setProperty('--sweep','0.35s');
        $('rpmCap').textContent=prog.flashRpm+' rpm';
        $('phaseCap').textContent='rapid mix';
      } else if(cur.phase==='slow'){
        $('stirrerBar').className='stirrer-bar mixing';
        $('stirrerBar').style.setProperty('--sweep','1.8s');
        $('rpmCap').textContent=prog.slowRpm+' rpm';
        $('phaseCap').textContent='slow mix';
        jars.forEach(j=>{ j.flocNow=j.fq*frac; });
      } else {
        $('stirrerBar').className='stirrer-bar';
        $('rpmCap').textContent='stopped';
        $('phaseCap').textContent='settling';
        jars.forEach(j=>{ j.settledFrac=frac;
          j.ntuNow=sample.rawNtu-(sample.rawNtu-j.finalNtu)*frac; });
      }
      setStatus(cur.phase,(cur.phase==='flash'?'Rapid mixing…':(cur.phase==='slow'?'Slow mixing — floc forming…':'Settling… '+ Math.round(frac*prog.settleTime)+'/'+prog.settleTime+' min')));
      renderJars();
      if(frac>=1){ stepIdx++; phaseT=0;
        if(stepIdx>=SEQ.length){ clearInterval(timer); phase='done'; running=false;
          $('stirrerBar').className='stirrer-bar';
          $('rpmCap').textContent='stopped'; $('phaseCap').textContent='done';
          setControls(true); fillResults(); drawGraph();
          setStatus('done','Run complete. Check the results below.');
          // Lab collection check
          window.labCollection.checkAfterRun('jar3', jars, optDose, sample.rawNtu, prog, effectivePh);
          // Show button to proceed to Summer Drought
          if(!document.getElementById('continueToSummerBtn')){
            const wrap=document.createElement('div');
            wrap.style.cssText='text-align:center;padding:16px;';
            wrap.innerHTML='<button class="btn btn-primary" id="continueToSummerBtn" style="margin-top:12px;">Continue to Jar Test #4 — Summer Drought &#9654;</button>';
            document.getElementById('jar3Sim').appendChild(wrap);
            document.getElementById('continueToSummerBtn').addEventListener('click',function(){
              document.getElementById('stepJar3').classList.remove('active');
              document.getElementById('stepJar3').classList.add('done');
              document.getElementById('stepQuiz4').classList.add('active');
              document.getElementById('jar3Wrap').style.display='none';
              document.getElementById('summerQuizWrap').style.display='block';
              document.getElementById('summerQuizWrap').scrollIntoView({behavior:'smooth',block:'start'});
            });
          }
        }
      }
    },70);
  }
  function setControls(on){
    document.querySelectorAll('#jar3Sim input, #jar3Sim button').forEach(el=>{ if(el.id!=='limeDose3') el.disabled=!on; });
  }
  function setStatus(type,msg){
    const dot=$('phaseDot'), txt=$('statusText');
    dot.className='phase-dot'+(type==='idle'?' idle':(type==='done'?' done':' running'));
    txt.textContent=msg;
  }

  function renderJars(){
    const bank=$('jarBank');
    bank.innerHTML=jars.slice(0,NJARS).map((j,i)=>{
      const turb=phase==='done'?j.finalNtu:j.ntuNow;
      const pctClr=clamp(1-turb/sample.rawNtu,0,1);
      const waterH=120, waterY=180-waterH, sedH=phase==='settle'||phase==='done'?j.fq*j.settledFrac*35:0;
      const flocDots=(phase==='slow'||phase==='settle'||phase==='done')?j.flocNow:0;
      const wColor=`rgba(${Math.round(180+75*pctClr)},${Math.round(195+60*pctClr)},${Math.round(140+80*pctClr)},0.85)`;
      const ntuClass=turb<=5?'good':(turb>=20?'bad':'mid');
      let svg=`<svg viewBox="0 0 100 200"><rect x="20" y="40" width="60" height="150" rx="4" fill="none" stroke="var(--edge-bright)" stroke-width="2"/>
        <rect x="22" y="${waterY}" width="56" height="${waterH}" rx="2" fill="${wColor}"/>`;
      if(sedH>0) svg+=`<rect x="22" y="${180-sedH}" width="56" height="${sedH}" rx="1" fill="rgba(120,90,50,0.5)"/>`;
      if(flocDots>0){ for(let d=0;d<Math.round(flocDots*12);d++){
        const fx=28+Math.random()*40, fy=waterY+10+Math.random()*(waterH-25);
        svg+=`<circle cx="${fx}" cy="${fy}" r="${1+flocDots*2}" fill="rgba(160,130,80,${0.15+flocDots*0.35})"/>`; }}
      svg+=`</svg>`;
      return `<div class="jar-cell"><span class="jarno">Jar ${i+1}</span>${svg}
        <div class="dose-wrap"><input class="dose-input" id="dose${i}3" type="number" min="0" max="6" step="0.1" value="${j.dose}" ${running?'disabled':''}>
        <span class="dose-lbl">mg/L alum</span></div>
        <div class="readout-mini">${phase==='done'?`<span class="ntu-num ${ntuClass}">${turb.toFixed(1)}</span> NTU`:''}</div></div>`;
    }).join('');
  }

  function fillResults(){
    const best=jars.slice(0,NJARS).reduce((a,b)=>a.finalNtu<b.finalNtu?a:b);
    $('resBody').innerHTML=jars.slice(0,NJARS).map((j,i)=>{
      const pct=((1-j.finalNtu/sample.rawNtu)*100).toFixed(1);
      const cls=j===best?'style="color:var(--good)"':'';
      return `<tr ${cls}><td>${i+1}</td><td>${j.dose}</td><td>${j.finalNtu.toFixed(1)}</td><td>${pct}%</td></tr>`;
    }).join('');
    const phMsg=effectivePh<6.0?' Warning: pH is still below 6.0 — alum is not working at full effectiveness. Try adding more lime.':
               (effectivePh>7.5?' Note: pH is above 7.5 — you may have over-buffered.':'');
    $('interpHint').textContent=`Lowest NTU: Jar with ${best.dose} mg/L alum → ${best.finalNtu.toFixed(1)} NTU (${((1-best.finalNtu/sample.rawNtu)*100).toFixed(1)}% removal). Lime: ${limeDose} mg/L, effective pH: ${effectivePh.toFixed(1)}.${phMsg}`;
    history.push(jars.slice(0,NJARS).map(j=>({dose:j.dose,ntu:j.finalNtu})));
  }

  function drawGraph(){
    const W=460,H=300,ml=50,mr=20,mt=20,mb=40;
    const gw=W-ml-mr, gh=H-mt-mb;
    const maxD=6, maxN=Math.max(sample.rawNtu*1.05,60);
    let svg=`<rect width="${W}" height="${H}" fill="var(--panel-2)" rx="8"/>
      <line x1="${ml}" y1="${mt}" x2="${ml}" y2="${H-mb}" stroke="var(--edge)" stroke-width="1"/>
      <line x1="${ml}" y1="${H-mb}" x2="${W-mr}" y2="${H-mb}" stroke="var(--edge)" stroke-width="1"/>`;
    for(let d=0;d<=maxD;d+=1){ const x=ml+d/maxD*gw; svg+=`<text x="${x}" y="${H-mb+18}" text-anchor="middle" fill="var(--ink-faint)" font-size="10">${d}</text>`; }
    svg+=`<text x="${ml+gw/2}" y="${H-2}" text-anchor="middle" fill="var(--ink-faint)" font-size="10">alum dose (mg/L)</text>`;
    for(let n=0;n<=maxN;n+=10){ const y=H-mb-n/maxN*gh; svg+=`<line x1="${ml}" y1="${y}" x2="${W-mr}" y2="${y}" stroke="var(--edge)" stroke-width="0.5" opacity="0.3"/>
      <text x="${ml-8}" y="${y+4}" text-anchor="end" fill="var(--ink-faint)" font-size="10">${n}</text>`; }
    history.forEach((run,ri)=>{
      const op=ri===history.length-1?1:0.25;
      const col=ri===history.length-1?'var(--accent)':'var(--ink-faint)';
      let pts=run.map(p=>{ const x=ml+p.dose/maxD*gw, y=H-mb-p.ntu/maxN*gh; return `${x},${y}`; });
      svg+=`<polyline points="${pts.join(' ')}" fill="none" stroke="${col}" stroke-width="2" opacity="${op}"/>`;
      run.forEach(p=>{ const x=ml+p.dose/maxD*gw, y=H-mb-p.ntu/maxN*gh;
        svg+=`<circle cx="${x}" cy="${y}" r="4" fill="${col}" opacity="${op}"/>`; });
    });
    document.getElementById('graph3').innerHTML=svg;
  }

  function autoSpread(){
    const top=clamp(+(optDose*1.8).toFixed(1),3,6);
    const step=top/(NJARS-1);
    jars.forEach((j,i)=>{ j.dose=+(i*step).toFixed(1); });
    jars.forEach((j,i)=>{ const el=$('dose'+i); if(el)el.value=j.dose; });
    setStatus('idle','Doses spread 0 to '+jars[NJARS-1].dose+' mg/L. Lime: '+limeDose+' mg/L (pH '+effectivePh.toFixed(1)+'). Adjust and run.');
  }
  function standardProgramme(){
    $('flashRpm').value=200; $('flashTime').value=60;
    $('slowRpm').value=40; $('slowTime').value=15; $('settleTime').value=20;
    ['flashRpm','flashTime','slowRpm','slowTime','settleTime'].forEach(id=>$(id).dispatchEvent(new Event('input')));
    setStatus('idle','Standard programme loaded.');
  }
  function bindSlider(id,vid,fmt){
    const el=$(id), out=$(vid);
    const upd=()=>{ out.textContent=fmt?fmt(el.value):el.value;
      const pct=(el.value-el.min)/(el.max-el.min)*100; el.style.setProperty('--fill',pct+'%'); };
    el.addEventListener('input',upd); upd();
  }

  bindSlider('flashRpm','flashRpmV');
  bindSlider('flashTime','flashTimeV');
  bindSlider('slowRpm','slowRpmV');
  bindSlider('slowTime','slowTimeV');
  bindSlider('settleTime','settleTimeV');

  $('runBtn').addEventListener('click',runAll);
  $('spreadBtn').addEventListener('click',()=>{ if(!running)autoSpread(); });
  $('standardBtn').addEventListener('click',()=>{ if(!running)standardProgramme(); });
  $('resetBtn').addEventListener('click',()=>{ if(running)return;
    if(!confirm('New autumn water clears the current jars (graph history is kept). Continue?'))return; newSample(); });

  setInterval(()=>{ if(running)renderJars(); },70);

  // Init via event when skipping setup
  window.addEventListener('jar3Ready', function(e){
    NJARS=e.detail.n; VOLUME_L=e.detail.v;
    $('jarBank').style.setProperty('--njars',NJARS);
    document.getElementById('jar3Title').textContent=`Gang stirrer — ${NJARS} \u00d7 ${VOLUME_L} L jars (Autumn Rain)`;
    history=[];
    newSample();
  });

  window.initLabRoom({
    ns: '3',
    onAllDone(n, v){
      NJARS=n; VOLUME_L=v;
      document.getElementById('setupCard3').style.display='none';
      const simEl=document.getElementById('jar3Sim');
      simEl.style.display='block';
      $('jarBank').style.setProperty('--njars',NJARS);
      document.getElementById('jar3Title').textContent=`Gang stirrer — ${NJARS} \u00d7 ${VOLUME_L} L jars (Autumn Rain)`;
      history=[];
      newSample();
      simEl.scrollIntoView({behavior:'smooth',block:'start'});
    }
  });
})();
</script>


</body>
</html>
