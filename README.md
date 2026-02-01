[abb-crepes-pro.html](https://github.com/user-attachments/files/24997422/abb-crepes-pro.html)
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ABB Crêpes - Gestion</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        :root{--bg:#0f0f14;--card:#1a1a24;--elevated:#222230;--purple:#8b5cf6;--purple-light:#a78bfa;--purple-dark:#6d28d9;--glow:rgba(139,92,246,0.25);--green:#10b981;--red:#ef4444;--orange:#f59e0b;--blue:#3b82f6;--text:#f8fafc;--text2:#94a3b8;--muted:#64748b;--border:rgba(139,92,246,0.15);--border2:rgba(255,255,255,0.08)}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--text);min-height:100vh}
        
        .login-overlay{position:fixed;inset:0;background:linear-gradient(135deg,#0f0f14,#1a1025);z-index:10000;display:flex;align-items:center;justify-content:center;padding:20px}
        .login-overlay.hidden{display:none}
        .login-box{background:var(--card);border:1px solid var(--border);border-radius:24px;padding:40px;width:100%;max-width:420px;text-align:center}
        .login-logo{width:80px;height:80px;background:linear-gradient(135deg,var(--purple),var(--purple-dark));border-radius:20px;display:flex;align-items:center;justify-content:center;font-size:40px;margin:0 auto 20px;box-shadow:0 10px 40px var(--glow)}
        .login-title{font-size:1.8rem;font-weight:800;background:linear-gradient(135deg,var(--purple-light),var(--text));-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:8px}
        .login-subtitle{color:var(--text2);margin-bottom:30px}
        .login-tabs{display:flex;gap:10px;margin-bottom:24px}
        .login-tab{flex:1;padding:14px;background:var(--elevated);border:1px solid var(--border2);border-radius:12px;color:var(--text2);font-family:inherit;font-weight:600;cursor:pointer;transition:all 0.3s}
        .login-tab:hover{border-color:var(--purple)}
        .login-tab.active{background:linear-gradient(135deg,var(--purple),var(--purple-dark));color:white;border-color:transparent;box-shadow:0 5px 20px var(--glow)}
        .login-select-wrap{display:none;margin-bottom:16px}.login-select-wrap.show{display:block}
        .login-select,.login-input{width:100%;padding:16px 20px;background:var(--bg);border:1px solid var(--border2);border-radius:12px;color:var(--text);font-family:inherit;font-size:1rem;outline:none;transition:all 0.3s}
        .login-select:focus,.login-input:focus{border-color:var(--purple);box-shadow:0 0 0 4px var(--glow)}
        .login-input{margin-bottom:16px}
        .login-btn{width:100%;padding:16px;background:linear-gradient(135deg,var(--purple),var(--purple-dark));border:none;border-radius:12px;color:white;font-family:inherit;font-size:1rem;font-weight:700;cursor:pointer;transition:all 0.3s;box-shadow:0 5px 20px var(--glow)}
        .login-btn:hover{transform:translateY(-2px)}
        .login-error{color:var(--red);font-size:0.9rem;margin-top:16px;display:none}.login-error.show{display:block}
        
        .app{display:none;min-height:100vh}
        .app.active{display:grid;grid-template-columns:260px 1fr 380px;grid-template-rows:70px 1fr}
        .app.livreur-mode{grid-template-columns:1fr 380px}
        .app.livreur-mode .sidebar{display:none}
        
        .header{grid-column:1/-1;background:var(--card);border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;padding:0 24px;z-index:100}
        .logo{display:flex;align-items:center;gap:12px}
        .logo-icon{width:44px;height:44px;background:linear-gradient(135deg,var(--purple),var(--purple-dark));border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:24px}
        .logo-text{font-size:1.25rem;font-weight:800;background:linear-gradient(135deg,var(--purple-light),var(--text));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
        .header-right{display:flex;align-items:center;gap:16px}
        .status-pill{display:flex;align-items:center;gap:8px;padding:8px 16px;background:rgba(16,185,129,0.1);border:1px solid rgba(16,185,129,0.2);border-radius:50px;font-size:0.85rem;font-weight:600;color:var(--green)}
        .status-dot{width:8px;height:8px;background:var(--green);border-radius:50%;animation:pulse 2s infinite}
        @keyframes pulse{0%,100%{box-shadow:0 0 0 0 rgba(16,185,129,0.4)}50%{box-shadow:0 0 0 8px rgba(16,185,129,0)}}
        .role-badge{padding:8px 16px;border-radius:50px;font-size:0.8rem;font-weight:700}
        .role-badge.admin{background:linear-gradient(135deg,var(--purple),var(--purple-dark));color:white}
        .role-badge.livreur{background:linear-gradient(135deg,var(--green),#059669);color:white}
        .user-info{display:flex;align-items:center;gap:12px}
        .user-avatar{width:40px;height:40px;background:linear-gradient(135deg,var(--purple),var(--blue));border-radius:10px;display:flex;align-items:center;justify-content:center;font-weight:700}
        .user-details{text-align:right}
        .user-name{font-weight:600}
        .user-role{font-size:0.75rem;color:var(--text2)}
        .logout-btn{background:rgba(239,68,68,0.1);border:1px solid rgba(239,68,68,0.2);color:var(--red);padding:10px 18px;border-radius:10px;font-family:inherit;font-weight:600;cursor:pointer;transition:all 0.3s}
        .logout-btn:hover{background:var(--red);color:white}
        
        .sidebar{background:var(--card);border-right:1px solid var(--border);padding:24px 16px;overflow-y:auto}
        .nav-section{margin-bottom:28px}
        .nav-title{font-size:0.7rem;text-transform:uppercase;letter-spacing:1.5px;color:var(--muted);padding:0 16px 12px;font-weight:600}
        .nav-item{display:flex;align-items:center;gap:12px;padding:14px 16px;border-radius:10px;cursor:pointer;color:var(--text2);font-weight:500;transition:all 0.3s;margin-bottom:4px}
        .nav-item:hover{background:rgba(139,92,246,0.08);color:var(--text)}
        .nav-item.active{background:rgba(139,92,246,0.15);color:var(--purple-light)}
        .nav-icon{font-size:1.15rem;width:24px;text-align:center}
        .nav-badge{margin-left:auto;background:var(--purple);color:white;font-size:0.7rem;padding:3px 8px;border-radius:10px;font-weight:700;min-width:22px;text-align:center}
        
        .main{background:var(--bg);padding:24px;overflow-y:auto}
        
        .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:16px;margin-bottom:24px}
        .stat-card{background:var(--card);border:1px solid var(--border);border-radius:16px;padding:20px;position:relative;overflow:hidden;transition:all 0.3s}
        .stat-card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(90deg,var(--purple),var(--purple-light))}
        .stat-card:hover{transform:translateY(-4px);box-shadow:0 12px 40px rgba(0,0,0,0.3)}
        .stat-card.green::before{background:linear-gradient(90deg,var(--green),#34d399)}
        .stat-card.orange::before{background:linear-gradient(90deg,var(--orange),#fbbf24)}
        .stat-card.blue::before{background:linear-gradient(90deg,var(--blue),#60a5fa)}
        .stat-icon{width:48px;height:48px;background:rgba(139,92,246,0.15);border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:1.4rem;margin-bottom:16px}
        .stat-card.green .stat-icon{background:rgba(16,185,129,0.15)}
        .stat-card.orange .stat-icon{background:rgba(245,158,11,0.15)}
        .stat-card.blue .stat-icon{background:rgba(59,130,246,0.15)}
        .stat-value{font-size:2rem;font-weight:800;margin-bottom:4px}
        .stat-label{color:var(--text2);font-size:0.85rem;font-weight:500}
        
        .section{background:var(--card);border:1px solid var(--border);border-radius:16px;overflow:hidden;margin-bottom:24px}
        .section-header{display:flex;justify-content:space-between;align-items:center;padding:18px 24px;border-bottom:1px solid var(--border);flex-wrap:wrap;gap:12px}
        .section-title{font-size:1.1rem;font-weight:700;display:flex;align-items:center;gap:10px}
        .section-actions{display:flex;gap:10px;flex-wrap:wrap}
        
        .btn{padding:10px 18px;border-radius:10px;border:none;font-family:inherit;font-weight:600;font-size:0.9rem;cursor:pointer;transition:all 0.3s;display:inline-flex;align-items:center;gap:8px}
        .btn-primary{background:linear-gradient(135deg,var(--purple),var(--purple-dark));color:white;box-shadow:0 4px 15px var(--glow)}
        .btn-primary:hover{transform:translateY(-2px);box-shadow:0 8px 25px var(--glow)}
        .btn-secondary{background:var(--elevated);color:var(--text2);border:1px solid var(--border2)}
        .btn-secondary:hover{border-color:var(--purple);color:var(--text)}
        .btn-danger{background:rgba(239,68,68,0.15);color:var(--red)}
        .btn-danger:hover{background:var(--red);color:white}
        .btn-success{background:rgba(16,185,129,0.15);color:var(--green)}
        .btn-success:hover{background:var(--green);color:white}
        
        .table-row{display:grid;grid-template-columns:80px 1.5fr 1fr 120px 80px 100px;padding:16px 24px;align-items:center;border-bottom:1px solid var(--border2);transition:all 0.3s}
        .table-row:last-child{border-bottom:none}
        .table-row.header{background:rgba(139,92,246,0.05);font-size:0.75rem;text-transform:uppercase;letter-spacing:0.5px;color:var(--muted);font-weight:600}
        .table-row:not(.header):hover{background:rgba(139,92,246,0.05)}
        .order-id{font-weight:700;color:var(--purple-light)}
        .customer-name{font-weight:600}
        .customer-address{font-size:0.8rem;color:var(--text2)}
        
        .status-badge{display:inline-flex;align-items:center;gap:6px;padding:6px 12px;border-radius:20px;font-size:0.8rem;font-weight:600;cursor:pointer;transition:all 0.3s}
        .status-badge:hover{transform:scale(1.05)}
        .status-badge.pending{background:rgba(245,158,11,0.15);color:var(--orange)}
        .status-badge.progress{background:rgba(59,130,246,0.15);color:var(--blue)}
        .status-badge.completed{background:rgba(16,185,129,0.15);color:var(--green)}
        .status-badge.cancelled{background:rgba(239,68,68,0.15);color:var(--red)}
        
        .table-actions{display:flex;gap:8px}
        .action-btn{width:36px;height:36px;border-radius:8px;border:none;background:var(--elevated);color:var(--text2);cursor:pointer;transition:all 0.3s;display:flex;align-items:center;justify-content:center;font-size:0.9rem}
        .action-btn:hover{background:var(--purple);color:white}
        .action-btn.delete:hover{background:var(--red)}
        .action-btn.success:hover{background:var(--green)}
        
        .empty-state{padding:60px 20px;text-align:center;color:var(--muted)}
        .empty-icon{font-size:3rem;margin-bottom:16px;opacity:0.5}
        .empty-text{font-size:1rem;margin-bottom:8px}
        .empty-hint{font-size:0.85rem}
        
        .livreurs-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:16px;padding:20px}
        .livreur-card{background:var(--bg);border:1px solid var(--border2);border-radius:14px;padding:24px;text-align:center;transition:all 0.3s;position:relative}
        .livreur-card:hover{border-color:var(--purple);transform:translateY(-4px);box-shadow:0 10px 30px rgba(0,0,0,0.2)}
        .livreur-avatar{width:60px;height:60px;border-radius:16px;margin:0 auto 16px;display:flex;align-items:center;justify-content:center;font-size:1.4rem;font-weight:700}
        .livreur-avatar.purple{background:linear-gradient(135deg,var(--purple),var(--purple-dark))}
        .livreur-avatar.blue{background:linear-gradient(135deg,var(--blue),#2563eb)}
        .livreur-avatar.green{background:linear-gradient(135deg,var(--green),#059669)}
        .livreur-avatar.orange{background:linear-gradient(135deg,var(--orange),#d97706)}
        .livreur-avatar.red{background:linear-gradient(135deg,var(--red),#dc2626)}
        .livreur-name{font-weight:600;font-size:1rem;margin-bottom:8px}
        .livreur-status{display:inline-flex;align-items:center;gap:6px;font-size:0.8rem;padding:6px 14px;border-radius:20px;cursor:pointer;transition:all 0.3s}
        .livreur-status.online{background:rgba(16,185,129,0.15);color:var(--green)}
        .livreur-status.busy{background:rgba(245,158,11,0.15);color:var(--orange)}
        .livreur-status.offline{background:rgba(100,116,139,0.15);color:var(--muted)}
        .livreur-actions{position:absolute;top:12px;right:12px;display:flex;gap:6px;opacity:0;transition:all 0.3s}
        .livreur-card:hover .livreur-actions{opacity:1}
        
        .reminders-container{padding:20px}
        .reminder{display:flex;align-items:center;gap:16px;padding:16px;background:var(--bg);border-radius:12px;margin-bottom:12px;border-left:4px solid var(--purple);transition:all 0.3s;position:relative;flex-wrap:wrap}
        .reminder:hover{background:rgba(139,92,246,0.05)}
        .reminder.urgent{border-left-color:var(--red)}
        .reminder.warning{border-left-color:var(--orange)}
        .reminder.info{border-left-color:var(--blue)}
        .reminder-icon{width:44px;height:44px;background:rgba(139,92,246,0.15);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:1.2rem;flex-shrink:0}
        .reminder.urgent .reminder-icon{background:rgba(239,68,68,0.15)}
        .reminder.warning .reminder-icon{background:rgba(245,158,11,0.15)}
        .reminder.info .reminder-icon{background:rgba(59,130,246,0.15)}
        .reminder-content{flex:1;min-width:0}
        .reminder-title{font-weight:600;margin-bottom:4px}
        .reminder-desc{font-size:0.85rem;color:var(--text2)}
        .reminder-time{font-size:0.8rem;color:var(--muted);white-space:nowrap}
        .reminder-actions{display:flex;gap:6px;opacity:0;transition:all 0.3s}
        .reminder:hover .reminder-actions{opacity:1}
        .reminder-btn{width:32px;height:32px;border-radius:8px;border:none;background:var(--elevated);color:var(--text2);cursor:pointer;font-size:0.8rem;transition:all 0.3s}
        .reminder-btn:hover{background:var(--purple);color:white}
        .reminder-btn.delete:hover{background:var(--red)}
        
        .chat{background:var(--card);border-left:1px solid var(--border);display:flex;flex-direction:column;height:calc(100vh - 70px)}
        .chat-header{padding:20px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between}
        .chat-title{font-size:1.1rem;font-weight:700;display:flex;align-items:center;gap:10px}
        .chat-online{display:flex;align-items:center;gap:10px}
        .online-avatars{display:flex}
        .online-avatar{width:32px;height:32px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:0.7rem;font-weight:700;margin-left:-8px;border:3px solid var(--card);transition:all 0.3s}
        .online-avatar:first-child{margin-left:0}
        .online-avatar:hover{transform:scale(1.1);z-index:1}
        .online-count{background:var(--green);color:white;font-size:0.75rem;padding:4px 10px;border-radius:12px;font-weight:700}
        
        .chat-messages{flex:1;overflow-y:auto;padding:20px;display:flex;flex-direction:column;gap:16px}
        .chat-msg{display:flex;gap:12px;position:relative;animation:msgIn 0.3s ease}
        @keyframes msgIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
        .chat-msg:hover .msg-delete{opacity:1}
        .chat-msg.own{flex-direction:row-reverse}
        .chat-msg.own .msg-bubble{background:linear-gradient(135deg,var(--purple),var(--purple-dark));border-radius:16px 16px 4px 16px}
        .chat-msg.own .msg-meta{justify-content:flex-end}
        .msg-avatar{width:40px;height:40px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:0.85rem;flex-shrink:0}
        .msg-content{flex:1;max-width:280px}
        .msg-meta{display:flex;align-items:center;gap:8px;margin-bottom:6px}
        .msg-sender{font-weight:600;font-size:0.85rem}
        .msg-time{font-size:0.7rem;color:var(--muted)}
        .msg-bubble{background:var(--elevated);padding:12px 16px;border-radius:4px 16px 16px 16px;font-size:0.9rem;line-height:1.5;word-wrap:break-word}
        .msg-delete{position:absolute;top:0;right:0;width:24px;height:24px;background:var(--red);border:none;border-radius:50%;color:white;font-size:0.7rem;cursor:pointer;opacity:0;transition:all 0.3s}
        
        .chat-input-area{padding:20px;border-top:1px solid var(--border)}
        .chat-input-wrap{display:flex;gap:10px;background:var(--bg);border:1px solid var(--border2);border-radius:14px;padding:8px;transition:all 0.3s}
        .chat-input-wrap:focus-within{border-color:var(--purple);box-shadow:0 0 0 4px var(--glow)}
        .chat-input{flex:1;background:none;border:none;color:var(--text);font-family:inherit;font-size:0.95rem;padding:10px 14px;outline:none}
        .chat-input::placeholder{color:var(--muted)}
        .chat-send{width:44px;height:44px;border-radius:10px;border:none;background:linear-gradient(135deg,var(--purple),var(--purple-dark));color:white;cursor:pointer;transition:all 0.3s;display:flex;align-items:center;justify-content:center;font-size:1.1rem}
        .chat-send:hover{transform:scale(1.05);box-shadow:0 5px 15px var(--glow)}
        
        .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.85);backdrop-filter:blur(4px);z-index:9999;display:flex;align-items:center;justify-content:center;opacity:0;visibility:hidden;transition:all 0.3s;padding:20px}
        .modal-overlay.active{opacity:1;visibility:visible}
        .modal{background:var(--card);border:1px solid var(--border);border-radius:20px;padding:28px;width:100%;max-width:480px;max-height:90vh;overflow-y:auto;transform:scale(0.9);transition:all 0.3s}
        .modal-overlay.active .modal{transform:scale(1)}
        .modal-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:24px}
        .modal-title{font-size:1.25rem;font-weight:700}
        .modal-close{background:none;border:none;color:var(--text2);font-size:1.5rem;cursor:pointer;width:40px;height:40px;border-radius:10px;display:flex;align-items:center;justify-content:center;transition:all 0.3s}
        .modal-close:hover{background:var(--elevated);color:var(--red)}
        
        .form-group{margin-bottom:20px}
        .form-label{display:block;margin-bottom:8px;font-weight:600;color:var(--text2);font-size:0.9rem}
        .form-input,.form-select,.form-textarea{width:100%;padding:14px 18px;background:var(--bg);border:1px solid var(--border2);border-radius:10px;color:var(--text);font-family:inherit;font-size:0.95rem;outline:none;transition:all 0.3s}
        .form-textarea{resize:vertical;min-height:100px}
        .form-input:focus,.form-select:focus,.form-textarea:focus{border-color:var(--purple);box-shadow:0 0 0 4px var(--glow)}
        .form-row{display:grid;grid-template-columns:1fr 1fr;gap:16px}
        .modal-actions{display:flex;gap:12px;justify-content:flex-end;margin-top:28px}
        
        .settings-section{padding:24px;border-bottom:1px solid var(--border2)}
        .settings-section:last-child{border-bottom:none}
        .settings-title{font-size:1rem;font-weight:700;margin-bottom:16px;display:flex;align-items:center;gap:10px}
        
        .status-selector{display:flex;gap:12px;padding:20px;background:var(--bg);border-radius:14px}
        .status-opt{flex:1;padding:16px;border-radius:12px;border:2px solid var(--border2);background:transparent;color:var(--text2);font-family:inherit;font-weight:600;font-size:0.95rem;cursor:pointer;text-align:center;transition:all 0.3s}
        .status-opt:hover{border-color:var(--purple)}
        .status-opt.active.online{background:var(--green);color:white;border-color:var(--green)}
        .status-opt.active.busy{background:var(--orange);color:white;border-color:var(--orange)}
        .status-opt.active.offline{background:var(--muted);color:white;border-color:var(--muted)}
        
        .livreur-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:16px;margin-bottom:24px}
        .table-row.livreur-row{grid-template-columns:80px 1.5fr 120px 80px 100px}
        
        .toast{position:fixed;bottom:24px;right:24px;background:var(--card);border:1px solid var(--green);border-radius:14px;padding:16px 24px;display:flex;align-items:center;gap:12px;z-index:10001;transform:translateY(100px);opacity:0;transition:all 0.3s;box-shadow:0 10px 40px rgba(0,0,0,0.3)}
        .toast.show{transform:translateY(0);opacity:1}
        .toast.error{border-color:var(--red)}
        
        .view{display:none}.view.active{display:block}
        .content-grid{display:grid;grid-template-columns:1fr 380px;gap:24px}
        
        @media(max-width:1400px){.app.active{grid-template-columns:240px 1fr 340px}.content-grid{grid-template-columns:1fr}}
        @media(max-width:1200px){.app.active,.app.livreur-mode{grid-template-columns:1fr}.sidebar,.chat{display:none}.header-right{gap:10px}.status-pill span:not(.status-dot){display:none}.user-details{display:none}.stats{grid-template-columns:repeat(2,1fr)}.mobile-nav{display:flex;justify-content:space-around}}
        @media(max-width:768px){.header{padding:0 16px}.logo-text{display:none}.main{padding:16px;padding-bottom:100px}.stats{grid-template-columns:repeat(2,1fr);gap:12px}.stat-card{padding:16px}.stat-value{font-size:1.5rem}.table-row{grid-template-columns:1fr;gap:8px;padding:16px}.table-row.header{display:none}.table-actions{justify-content:flex-start;margin-top:8px}.section-header{flex-direction:column;gap:12px;align-items:stretch}.section-actions{justify-content:stretch}.section-actions .btn{flex:1;justify-content:center}.livreurs-grid{grid-template-columns:repeat(2,1fr);gap:12px;padding:16px}.livreur-card{padding:16px}.livreur-avatar{width:50px;height:50px}.reminder{flex-wrap:wrap}.reminder-time{width:100%;margin-top:8px}.status-selector{flex-direction:column}.livreur-stats{grid-template-columns:repeat(3,1fr);gap:10px}.form-row{grid-template-columns:1fr}.modal{padding:20px}}
        
        .mobile-nav{display:none;position:fixed;bottom:0;left:0;right:0;background:var(--card);border-top:1px solid var(--border);padding:8px 0;z-index:1000;padding-bottom:env(safe-area-inset-bottom,8px)}
        .mobile-nav-item{display:flex;flex-direction:column;align-items:center;gap:4px;padding:8px 16px;border-radius:12px;color:var(--muted);font-size:0.7rem;font-weight:600;cursor:pointer;transition:all 0.3s;background:none;border:none;font-family:inherit;position:relative}
        .mobile-nav-item:hover,.mobile-nav-item.active{color:var(--purple-light);background:rgba(139,92,246,0.1)}
        .mobile-nav-icon{font-size:1.4rem}
        .mobile-nav-badge{position:absolute;top:2px;right:10px;background:var(--purple);color:white;font-size:0.6rem;padding:2px 5px;border-radius:8px;font-weight:700}
        
        .chat-overlay{display:none;position:fixed;inset:0;background:var(--bg);z-index:9998;flex-direction:column}
        .chat-overlay.active{display:flex}
        .chat-overlay .chat{display:flex;flex:1;height:100%;border:none}
        .chat-back{display:flex;align-items:center;gap:8px;padding:16px;border-bottom:1px solid var(--border);font-weight:600;cursor:pointer;background:var(--card)}
    </style>
</head>
<body>
<div class="login-overlay" id="loginOverlay">
    <div class="login-box">
        <div class="login-logo">🥞</div>
        <h1 class="login-title">ABB CRÊPES</h1>
        <p class="login-subtitle">Connectez-vous pour continuer</p>
        <div class="login-tabs">
            <button class="login-tab active" onclick="setLoginMode('admin')">👑 Admin</button>
            <button class="login-tab" onclick="setLoginMode('livreur')">🚴 Livreur</button>
        </div>
        <div class="login-select-wrap" id="livreurSelectWrap">
            <select class="login-select" id="livreurSelect"><option value="">-- Sélectionner --</option></select>
        </div>
        <input type="password" class="login-input" id="loginInput" placeholder="Mot de passe">
        <button class="login-btn" onclick="login()">CONNEXION</button>
        <p class="login-error" id="loginError">Mot de passe incorrect</p>
    </div>
</div>

<div class="toast" id="toast"><span id="toastIcon">✅</span><span id="toastMsg">Message</span></div>

<div class="app" id="app">
    <header class="header">
        <div class="logo"><div class="logo-icon">🥞</div><span class="logo-text">ABB CRÊPES</span></div>
        <div class="header-right">
            <div class="status-pill"><span class="status-dot"></span><span>Service Actif</span></div>
            <div class="role-badge admin" id="roleBadge">👑 ADMIN</div>
            <div class="user-info">
                <div class="user-details"><div class="user-name" id="userName">Admin</div><div class="user-role" id="userRoleText">Administrateur</div></div>
                <div class="user-avatar" id="userAvatar">AD</div>
            </div>
            <button class="logout-btn" onclick="logout()">Déconnexion</button>
        </div>
    </header>
    
    <nav class="sidebar" id="sidebar">
        <div class="nav-section">
            <div class="nav-title">Principal</div>
            <div class="nav-item active" onclick="showView('dashboard')"><span class="nav-icon">📊</span>Dashboard</div>
            <div class="nav-item" onclick="showView('orders')"><span class="nav-icon">📦</span>Commandes<span class="nav-badge" id="ordersBadge">0</span></div>
            <div class="nav-item" onclick="showView('livreurs')"><span class="nav-icon">🚴</span>Livreurs<span class="nav-badge" id="livreursBadge">0</span></div>
        </div>
        <div class="nav-section">
            <div class="nav-title">Gestion</div>
            <div class="nav-item" onclick="showView('reminders')"><span class="nav-icon">📌</span>Rappels</div>
            <div class="nav-item" onclick="showView('settings')"><span class="nav-icon">⚙️</span>Paramètres</div>
        </div>
    </nav>
    
    <main class="main" id="main">
        <div class="view active" id="dashboardView">
            <div class="stats" id="statsContainer"></div>
            <div class="content-grid">
                <div class="section">
                    <div class="section-header"><h2 class="section-title">📦 Livraisons récentes</h2><div class="section-actions"><button class="btn btn-primary" onclick="openModal('order')">+ Nouvelle</button></div></div>
                    <div id="ordersTable"></div>
                </div>
                <div class="section">
                    <div class="section-header"><h2 class="section-title">📌 À faire</h2><button class="btn btn-secondary" onclick="openModal('reminder')">+ Ajouter</button></div>
                    <div class="reminders-container" id="remindersContainer"></div>
                </div>
            </div>
        </div>
        
        <div class="view" id="ordersView">
            <div class="section">
                <div class="section-header"><h2 class="section-title">📦 Toutes les commandes</h2><div class="section-actions"><button class="btn btn-danger" onclick="clearCompleted()">🗑️ Nettoyer</button><button class="btn btn-primary" onclick="openModal('order')">+ Nouvelle</button></div></div>
                <div id="allOrdersTable"></div>
            </div>
        </div>
        
        <div class="view" id="livreursView">
            <div class="section">
                <div class="section-header"><h2 class="section-title">🚴 Équipe de livreurs</h2><button class="btn btn-primary" onclick="openModal('livreur')">+ Ajouter</button></div>
                <div class="livreurs-grid" id="livreursGrid"></div>
            </div>
        </div>
        
        <div class="view" id="remindersView">
            <div class="section">
                <div class="section-header"><h2 class="section-title">📌 Tous les rappels</h2><button class="btn btn-primary" onclick="openModal('reminder')">+ Nouveau</button></div>
                <div class="reminders-container" id="allRemindersContainer"></div>
            </div>
        </div>
        
        <div class="view" id="settingsView">
            <div class="section">
                <div class="section-header"><h2 class="section-title">⚙️ Paramètres</h2></div>
                <div class="settings-section">
                    <div class="settings-title">👤 Profil Admin</div>
                    <div class="form-group"><label class="form-label">Nom de l'administrateur</label><input type="text" class="form-input" id="settingAdminName" placeholder="Admin"></div>
                    <div class="form-group"><label class="form-label">Mot de passe Admin</label><input type="password" class="form-input" id="settingAdminPass" placeholder="Nouveau mot de passe"></div>
                    <div class="form-group"><label class="form-label">Mot de passe Livreurs</label><input type="password" class="form-input" id="settingLivreurPass" placeholder="Mot de passe livreurs"></div>
                    <button class="btn btn-primary" onclick="saveSettings()">💾 Sauvegarder</button>
                </div>
                <div class="settings-section">
                    <div class="settings-title">🗄️ Données</div>
                    <p style="color:var(--text2);margin-bottom:16px;font-size:0.9rem">Supprimer toutes les données et repartir à zéro.</p>
                    <button class="btn btn-danger" onclick="resetData()">🗑️ Réinitialiser tout</button>
                </div>
            </div>
        </div>
        
        <div class="view" id="livreurDashView">
            <div class="section" style="margin-bottom:24px">
                <div class="section-header"><h2 class="section-title">🚴 Mon statut</h2></div>
                <div style="padding:20px"><div class="status-selector" id="statusSelector"></div></div>
            </div>
            <div class="livreur-stats" id="livreurStats"></div>
            <div class="section">
                <div class="section-header"><h2 class="section-title">📦 Mes livraisons</h2></div>
                <div id="myOrdersTable"></div>
            </div>
        </div>
    </main>
    
    <aside class="chat" id="chatPanel">
        <div class="chat-header">
            <h3 class="chat-title">💬 Chat Équipe</h3>
            <div class="chat-online"><div class="online-avatars" id="onlineAvatars"></div><span class="online-count" id="onlineCount">0</span></div>
        </div>
        <div class="chat-messages" id="chatMessages"></div>
        <div class="chat-input-area">
            <div class="chat-input-wrap"><input type="text" class="chat-input" id="chatInput" placeholder="Écrire un message..."><button class="chat-send" onclick="sendMessage()">➤</button></div>
        </div>
    </aside>
</div>

<nav class="mobile-nav" id="mobileNav">
    <button class="mobile-nav-item active" onclick="showView('dashboard');updateMobileNav(this)"><span class="mobile-nav-icon">📊</span><span>Dashboard</span></button>
    <button class="mobile-nav-item" onclick="showView('orders');updateMobileNav(this)" style="position:relative"><span class="mobile-nav-icon">📦</span><span>Commandes</span><span class="mobile-nav-badge" id="mobileOrdersBadge">0</span></button>
    <button class="mobile-nav-item" onclick="showView('livreurs');updateMobileNav(this)"><span class="mobile-nav-icon">🚴</span><span>Livreurs</span></button>
    <button class="mobile-nav-item" onclick="openMobileChat()"><span class="mobile-nav-icon">💬</span><span>Chat</span></button>
    <button class="mobile-nav-item" onclick="showView('settings');updateMobileNav(this)"><span class="mobile-nav-icon">⚙️</span><span>Plus</span></button>
</nav>

<div class="chat-overlay" id="chatOverlay">
    <div class="chat-back" onclick="closeMobileChat()">← Retour</div>
    <aside class="chat" id="mobileChatPanel">
        <div class="chat-header">
            <h3 class="chat-title">💬 Chat Équipe</h3>
            <div class="chat-online"><div class="online-avatars" id="mobileOnlineAvatars"></div><span class="online-count" id="mobileOnlineCount">0</span></div>
        </div>
        <div class="chat-messages" id="mobileChatMessages"></div>
        <div class="chat-input-area">
            <div class="chat-input-wrap"><input type="text" class="chat-input" id="mobileChatInput" placeholder="Écrire un message..."><button class="chat-send" onclick="sendMessage(true)">➤</button></div>
        </div>
    </aside>
</div>

<div class="modal-overlay" id="modalOverlay" onclick="if(event.target===this)closeModal()"><div class="modal" id="modalContent"></div></div>

<script>
const DEFAULT_ADMIN_PASS='PoraUHQ',DEFAULT_LIVREUR_PASS='ABB Crêpes';
let currentUser=null,loginMode='admin';
const defaultData={settings:{adminName:'Admin',adminPass:DEFAULT_ADMIN_PASS,livreurPass:DEFAULT_LIVREUR_PASS},stats:{completed:0,pending:0,revenue:0},orders:[],livreurs:[],reminders:[],messages:[],nextOrderId:1000};
let data=JSON.parse(JSON.stringify(defaultData));

function save(){localStorage.setItem('abbCrepes',JSON.stringify(data))}
function load(){const s=localStorage.getItem('abbCrepes');if(s){data=JSON.parse(s);if(!data.settings.adminPass)data.settings.adminPass=DEFAULT_ADMIN_PASS;if(!data.settings.livreurPass)data.settings.livreurPass=DEFAULT_LIVREUR_PASS}}

function setLoginMode(m){loginMode=m;document.querySelectorAll('.login-tab').forEach((t,i)=>t.classList.toggle('active',(m==='admin'&&i===0)||(m==='livreur'&&i===1)));document.getElementById('livreurSelectWrap').classList.toggle('show',m==='livreur');document.getElementById('loginError').classList.remove('show')}
function populateLivreurSelect(){const s=document.getElementById('livreurSelect');s.innerHTML=data.livreurs.length===0?'<option value="">Aucun livreur créé</option>':data.livreurs.map(l=>`<option value="${l.id}">${l.name}</option>`).join('')}

function login(){const pass=document.getElementById('loginInput').value,err=document.getElementById('loginError');if(loginMode==='admin'&&pass===data.settings.adminPass){currentUser={type:'admin',name:data.settings.adminName};err.classList.remove('show');startApp()}else if(loginMode==='livreur'&&pass===data.settings.livreurPass){const lid=parseInt(document.getElementById('livreurSelect').value),liv=data.livreurs.find(l=>l.id===lid);if(liv){currentUser={type:'livreur',id:lid,name:liv.name,initials:liv.initials,color:liv.color};liv.status='online';save();err.classList.remove('show');startApp()}else{err.textContent='Veuillez sélectionner un livreur';err.classList.add('show')}}else{err.textContent='Mot de passe incorrect';err.classList.add('show')}}

function logout(){if(currentUser?.type==='livreur'){const liv=data.livreurs.find(l=>l.id===currentUser.id);if(liv){liv.status='offline';save()}}currentUser=null;document.getElementById('app').classList.remove('active');document.getElementById('loginOverlay').classList.remove('hidden');document.getElementById('loginInput').value=''}

function startApp(){document.getElementById('loginOverlay').classList.add('hidden');document.getElementById('app').classList.add('active');if(currentUser.type==='admin'){document.getElementById('app').classList.remove('livreur-mode');document.getElementById('roleBadge').className='role-badge admin';document.getElementById('roleBadge').textContent='👑 ADMIN';document.getElementById('userName').textContent=data.settings.adminName;document.getElementById('userRoleText').textContent='Administrateur';document.getElementById('userAvatar').textContent='AD';showView('dashboard')}else{document.getElementById('app').classList.add('livreur-mode');document.getElementById('roleBadge').className='role-badge livreur';document.getElementById('roleBadge').textContent='🚴 LIVREUR';document.getElementById('userName').textContent=currentUser.name;document.getElementById('userRoleText').textContent='Livreur';document.getElementById('userAvatar').textContent=currentUser.initials;showView('livreurDash')}renderAll()}

function showView(v){document.querySelectorAll('.view').forEach(x=>x.classList.remove('active'));document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));document.getElementById(v+'View').classList.add('active');const navIndex={dashboard:0,orders:1,livreurs:2,reminders:3,settings:4};const items=document.querySelectorAll('.sidebar .nav-item');if(items[navIndex[v]])items[navIndex[v]].classList.add('active');renderAll()}
function updateMobileNav(item){document.querySelectorAll('.mobile-nav-item').forEach(i=>i.classList.remove('active'));item.classList.add('active')}
function openMobileChat(){document.getElementById('chatOverlay').classList.add('active');renderChat(true)}
function closeMobileChat(){document.getElementById('chatOverlay').classList.remove('active')}

function renderAll(){renderStats();renderOrders();renderReminders();renderLivreurs();renderChat();updateBadges();if(currentUser?.type==='livreur')renderLivreurDash()}

function renderStats(){const onlineLivreurs=data.livreurs.filter(l=>l.status!=='offline').length,pendingOrders=data.orders.filter(o=>o.status==='pending'||o.status==='progress').length,completedOrders=data.orders.filter(o=>o.status==='completed').length;document.getElementById('statsContainer').innerHTML=`<div class="stat-card green"><div class="stat-icon">✅</div><div class="stat-value">${completedOrders}</div><div class="stat-label">Livraisons effectuées</div></div><div class="stat-card orange"><div class="stat-icon">⏳</div><div class="stat-value">${pendingOrders}</div><div class="stat-label">En cours</div></div><div class="stat-card blue"><div class="stat-icon">🚴</div><div class="stat-value">${onlineLivreurs}</div><div class="stat-label">Livreurs actifs</div></div><div class="stat-card"><div class="stat-icon">💰</div><div class="stat-value">€${data.stats.revenue}</div><div class="stat-label">Revenus du jour</div></div>`}

function renderOrders(){const header=`<div class="table-row header"><div>ID</div><div>Client</div><div>Livreur</div><div>Statut</div><div>Heure</div><div>Actions</div></div>`;if(data.orders.length===0){const empty=`<div class="empty-state"><div class="empty-icon">📦</div><div class="empty-text">Aucune commande</div><div class="empty-hint">Créez votre première commande</div></div>`;document.getElementById('ordersTable').innerHTML=empty;document.getElementById('allOrdersTable').innerHTML=empty;return}const rows=data.orders.map(o=>{const liv=data.livreurs.find(l=>l.id===o.livreurId);return`<div class="table-row"><div class="order-id">#${o.id}</div><div><div class="customer-name">${o.customer}</div><div class="customer-address">${o.address}</div></div><div>${liv?.name||'Non assigné'}</div><div><span class="status-badge ${o.status}" onclick="cycleStatus(${o.id})">${statusIcon(o.status)} ${statusText(o.status)}</span></div><div>${o.time}</div><div class="table-actions"><button class="action-btn" onclick="openModal('order',${o.id})">✏️</button><button class="action-btn delete" onclick="deleteOrder(${o.id})">🗑️</button></div></div>`}).join('');document.getElementById('ordersTable').innerHTML=header+rows;document.getElementById('allOrdersTable').innerHTML=header+rows}

function renderReminders(){if(data.reminders.length===0){const empty=`<div class="empty-state"><div class="empty-icon">📌</div><div class="empty-text">Aucun rappel</div><div class="empty-hint">Ajoutez des rappels</div></div>`;document.getElementById('remindersContainer').innerHTML=empty;document.getElementById('allRemindersContainer').innerHTML=empty;return}const html=data.reminders.map(r=>`<div class="reminder ${r.type}"><div class="reminder-icon">${r.icon}</div><div class="reminder-content"><div class="reminder-title">${r.title}</div><div class="reminder-desc">${r.desc}</div></div><div class="reminder-time">${r.time}</div><div class="reminder-actions"><button class="reminder-btn" onclick="openModal('reminder',${r.id})">✏️</button><button class="reminder-btn delete" onclick="deleteReminder(${r.id})">🗑️</button></div></div>`).join('');document.getElementById('remindersContainer').innerHTML=html;document.getElementById('allRemindersContainer').innerHTML=html}

function renderLivreurs(){if(data.livreurs.length===0){document.getElementById('livreursGrid').innerHTML=`<div class="empty-state" style="grid-column:1/-1"><div class="empty-icon">🚴</div><div class="empty-text">Aucun livreur</div><div class="empty-hint">Ajoutez des livreurs</div></div>`;return}document.getElementById('livreursGrid').innerHTML=data.livreurs.map(l=>`<div class="livreur-card"><div class="livreur-actions"><button class="action-btn" onclick="openModal('livreur',${l.id})">✏️</button><button class="action-btn delete" onclick="deleteLivreur(${l.id})">🗑️</button></div><div class="livreur-avatar ${l.color}">${l.initials}</div><div class="livreur-name">${l.name}</div><span class="livreur-status ${l.status}" onclick="cycleLivreurStatus(${l.id})">${statusLabel(l.status)}</span></div>`).join('')}

function renderChat(mobile=false){const isAdmin=currentUser?.type==='admin',msgContainer=mobile?document.getElementById('mobileChatMessages'):document.getElementById('chatMessages'),avatarsContainer=mobile?document.getElementById('mobileOnlineAvatars'):document.getElementById('onlineAvatars'),countContainer=mobile?document.getElementById('mobileOnlineCount'):document.getElementById('onlineCount');if(data.messages.length===0){msgContainer.innerHTML=`<div class="empty-state"><div class="empty-icon">💬</div><div class="empty-text">Aucun message</div><div class="empty-hint">Commencez la conversation!</div></div>`}else{msgContainer.innerHTML=data.messages.map(m=>{const sender=m.senderId===0?{name:data.settings.adminName,initials:'AD',color:'purple'}:data.livreurs.find(l=>l.id===m.senderId)||{name:'?',initials:'?',color:'purple'};const isOwn=(currentUser?.type==='admin'&&m.senderId===0)||(currentUser?.type==='livreur'&&m.senderId===currentUser.id);return`<div class="chat-msg ${isOwn?'own':''}">${isAdmin?`<button class="msg-delete" onclick="deleteMessage(${m.id})">×</button>`:''}<div class="msg-avatar ${sender.color}">${sender.initials}</div><div class="msg-content"><div class="msg-meta"><span class="msg-sender">${sender.name}</span><span class="msg-time">${m.time}</span></div><div class="msg-bubble">${escapeHtml(m.text)}</div></div></div>`}).join('')}msgContainer.scrollTop=msgContainer.scrollHeight;const online=data.livreurs.filter(l=>l.status!=='offline');avatarsContainer.innerHTML=online.slice(0,5).map(l=>`<div class="online-avatar ${l.color}" title="${l.name}">${l.initials}</div>`).join('');countContainer.textContent=online.length+' en ligne'}

function renderLivreurDash(){const liv=data.livreurs.find(l=>l.id===currentUser.id);if(!liv)return;document.getElementById('statusSelector').innerHTML=['online','busy','offline'].map(s=>`<button class="status-opt ${s} ${liv.status===s?'active':''}" onclick="setMyStatus('${s}')">${statusLabel(s)}</button>`).join('');const myOrders=data.orders.filter(o=>o.livreurId===currentUser.id),completed=myOrders.filter(o=>o.status==='completed').length,inProgress=myOrders.filter(o=>o.status==='pending'||o.status==='progress').length;document.getElementById('livreurStats').innerHTML=`<div class="stat-card green"><div class="stat-icon">✅</div><div class="stat-value">${completed}</div><div class="stat-label">Effectuées</div></div><div class="stat-card orange"><div class="stat-icon">⏳</div><div class="stat-value">${inProgress}</div><div class="stat-label">En cours</div></div><div class="stat-card blue"><div class="stat-icon">📦</div><div class="stat-value">${myOrders.length}</div><div class="stat-label">Total</div></div>`;if(myOrders.length===0){document.getElementById('myOrdersTable').innerHTML=`<div class="empty-state"><div class="empty-icon">📦</div><div class="empty-text">Aucune livraison assignée</div></div>`;return}const header=`<div class="table-row livreur-row header"><div>ID</div><div>Client</div><div>Statut</div><div>Heure</div><div>Actions</div></div>`;const rows=myOrders.map(o=>`<div class="table-row livreur-row" style="${o.status==='progress'?'border-left:3px solid var(--purple);':''}"><div class="order-id">#${o.id}</div><div><div class="customer-name">${o.customer}</div><div class="customer-address">${o.address}</div></div><div><span class="status-badge ${o.status}" onclick="cycleMyOrder(${o.id})">${statusIcon(o.status)} ${statusText(o.status)}</span></div><div>${o.time}</div><div class="table-actions">${o.status==='pending'?`<button class="action-btn success" onclick="startOrder(${o.id})">▶️</button>`:''}${o.status==='progress'?`<button class="action-btn success" onclick="completeOrder(${o.id})">✅</button>`:''}</div></div>`).join('');document.getElementById('myOrdersTable').innerHTML=header+rows}

function updateBadges(){const pending=data.orders.filter(o=>o.status!=='completed'&&o.status!=='cancelled').length,online=data.livreurs.filter(l=>l.status!=='offline').length;document.getElementById('ordersBadge').textContent=pending;document.getElementById('livreursBadge').textContent=online;const mobileBadge=document.getElementById('mobileOrdersBadge');if(mobileBadge){mobileBadge.textContent=pending;mobileBadge.style.display=pending>0?'':'none'}}

function statusIcon(s){return{pending:'⏳',progress:'🚴',completed:'✅',cancelled:'❌'}[s]||'📦'}
function statusText(s){return{pending:'Préparation',progress:'En route',completed:'Livrée',cancelled:'Annulée'}[s]||s}
function statusLabel(s){return{online:'🟢 En ligne',busy:'🟠 Occupé',offline:'⚫ Hors ligne'}[s]||s}
function getTime(){const n=new Date();return n.getHours().toString().padStart(2,'0')+':'+n.getMinutes().toString().padStart(2,'0')}
function escapeHtml(text){const div=document.createElement('div');div.textContent=text;return div.innerHTML}
function toast(message,isError=false){const t=document.getElementById('toast');document.getElementById('toastIcon').textContent=isError?'❌':'✅';document.getElementById('toastMsg').textContent=message;t.className='toast show'+(isError?' error':'');setTimeout(()=>t.classList.remove('show'),3000)}

function cycleStatus(id){const o=data.orders.find(x=>x.id===id),statuses=['pending','progress','completed','cancelled'];o.status=statuses[(statuses.indexOf(o.status)+1)%statuses.length];save();renderAll();toast('Statut mis à jour')}
function cycleMyOrder(id){const o=data.orders.find(x=>x.id===id);if(o.status==='pending')o.status='progress';else if(o.status==='progress')o.status='completed';save();renderAll();toast('Statut mis à jour')}
function startOrder(id){data.orders.find(x=>x.id===id).status='progress';save();renderAll();toast('Livraison démarrée 🚴')}
function completeOrder(id){data.orders.find(x=>x.id===id).status='completed';save();renderAll();toast('Livraison terminée 🎉')}
function cycleLivreurStatus(id){const l=data.livreurs.find(x=>x.id===id),statuses=['online','busy','offline'];l.status=statuses[(statuses.indexOf(l.status)+1)%statuses.length];save();renderAll();toast(`${l.name}: ${statusLabel(l.status)}`)}
function setMyStatus(s){data.livreurs.find(x=>x.id===currentUser.id).status=s;save();renderAll();toast(`Statut: ${statusLabel(s)}`)}
function deleteOrder(id){if(confirm('Supprimer cette commande?')){data.orders=data.orders.filter(o=>o.id!==id);save();renderAll();toast('Commande supprimée')}}
function deleteReminder(id){if(confirm('Supprimer ce rappel?')){data.reminders=data.reminders.filter(r=>r.id!==id);save();renderAll();toast('Rappel supprimé')}}
function deleteLivreur(id){if(confirm('Supprimer ce livreur?')){data.livreurs=data.livreurs.filter(l=>l.id!==id);save();renderAll();populateLivreurSelect();toast('Livreur supprimé')}}
function deleteMessage(id){data.messages=data.messages.filter(m=>m.id!==id);save();renderChat();renderChat(true)}
function clearCompleted(){if(confirm('Supprimer les commandes terminées?')){data.orders=data.orders.filter(o=>o.status!=='completed'&&o.status!=='cancelled');save();renderAll();toast('Historique nettoyé')}}

function sendMessage(mobile=false){const input=mobile?document.getElementById('mobileChatInput'):document.getElementById('chatInput'),text=input.value.trim();if(!text)return;data.messages.push({id:Date.now(),senderId:currentUser.type==='admin'?0:currentUser.id,text:text,time:getTime()});save();renderChat();renderChat(true);input.value=''}

function saveSettings(){data.settings.adminName=document.getElementById('settingAdminName').value||'Admin';const newAdminPass=document.getElementById('settingAdminPass').value,newLivreurPass=document.getElementById('settingLivreurPass').value;if(newAdminPass)data.settings.adminPass=newAdminPass;if(newLivreurPass)data.settings.livreurPass=newLivreurPass;save();if(currentUser?.type==='admin')document.getElementById('userName').textContent=data.settings.adminName;document.getElementById('settingAdminPass').value='';document.getElementById('settingLivreurPass').value='';toast('Paramètres sauvegardés')}
function resetData(){if(confirm('⚠️ Supprimer TOUTES les données?')){localStorage.removeItem('abbCrepes');location.reload()}}

function openModal(type,id=null){const modal=document.getElementById('modalContent');let html='';if(type==='order'){const o=id?data.orders.find(x=>x.id===id):null,livreurOptions=data.livreurs.length===0?'<option value="">Aucun livreur</option>':data.livreurs.map(l=>`<option value="${l.id}" ${o?.livreurId===l.id?'selected':''}>${l.name}</option>`).join('');html=`<div class="modal-header"><h3 class="modal-title">${o?'Modifier':'Nouvelle'} Commande</h3><button class="modal-close" onclick="closeModal()">×</button></div><div class="form-group"><label class="form-label">Nom du client</label><input class="form-input" id="mCustomer" value="${o?.customer||''}" placeholder="Ex: Marie Dupont"></div><div class="form-group"><label class="form-label">Adresse</label><input class="form-input" id="mAddress" value="${o?.address||''}" placeholder="Ex: 12 Rue de la Paix"></div><div class="form-group"><label class="form-label">Articles</label><input class="form-input" id="mItems" value="${o?.items||''}" placeholder="Ex: Crêpe Nutella x2"></div><div class="form-row"><div class="form-group"><label class="form-label">Livreur</label><select class="form-select" id="mLivreur">${livreurOptions}</select></div><div class="form-group"><label class="form-label">Statut</label><select class="form-select" id="mStatus"><option value="pending" ${o?.status==='pending'?'selected':''}>⏳ Préparation</option><option value="progress" ${o?.status==='progress'?'selected':''}>🚴 En route</option><option value="completed" ${o?.status==='completed'?'selected':''}>✅ Livrée</option><option value="cancelled" ${o?.status==='cancelled'?'selected':''}>❌ Annulée</option></select></div></div><div class="modal-actions"><button class="btn btn-secondary" onclick="closeModal()">Annuler</button><button class="btn btn-primary" onclick="saveOrder(${id})">💾 Sauvegarder</button></div>`}else if(type==='reminder'){const r=id?data.reminders.find(x=>x.id===id):null;html=`<div class="modal-header"><h3 class="modal-title">${r?'Modifier':'Nouveau'} Rappel</h3><button class="modal-close" onclick="closeModal()">×</button></div><div class="form-group"><label class="form-label">Titre</label><input class="form-input" id="mTitle" value="${r?.title||''}" placeholder="Ex: Stock bas"></div><div class="form-group"><label class="form-label">Description</label><textarea class="form-textarea" id="mDesc" placeholder="Détails...">${r?.desc||''}</textarea></div><div class="form-row"><div class="form-group"><label class="form-label">Échéance</label><input class="form-input" id="mTime" value="${r?.time||''}" placeholder="Ex: Demain"></div><div class="form-group"><label class="form-label">Priorité</label><select class="form-select" id="mType"><option value="" ${r?.type===''?'selected':''}>💡 Normal</option><option value="info" ${r?.type==='info'?'selected':''}>🔵 Info</option><option value="warning" ${r?.type==='warning'?'selected':''}>🟠 Attention</option><option value="urgent" ${r?.type==='urgent'?'selected':''}>🔴 Urgent</option></select></div></div><div class="modal-actions"><button class="btn btn-secondary" onclick="closeModal()">Annuler</button><button class="btn btn-primary" onclick="saveReminder(${id})">💾 Sauvegarder</button></div>`}else if(type==='livreur'){const l=id?data.livreurs.find(x=>x.id===id):null;html=`<div class="modal-header"><h3 class="modal-title">${l?'Modifier':'Nouveau'} Livreur</h3><button class="modal-close" onclick="closeModal()">×</button></div><div class="form-group"><label class="form-label">Nom complet</label><input class="form-input" id="mName" value="${l?.name||''}" placeholder="Ex: Lucas Martin"></div><div class="form-row"><div class="form-group"><label class="form-label">Initiales (2 lettres)</label><input class="form-input" id="mInitials" value="${l?.initials||''}" maxlength="2" placeholder="LM" style="text-transform:uppercase"></div><div class="form-group"><label class="form-label">Couleur</label><select class="form-select" id="mColor"><option value="purple" ${l?.color==='purple'?'selected':''}>🟣 Violet</option><option value="blue" ${l?.color==='blue'?'selected':''}>🔵 Bleu</option><option value="green" ${l?.color==='green'?'selected':''}>🟢 Vert</option><option value="orange" ${l?.color==='orange'?'selected':''}>🟠 Orange</option><option value="red" ${l?.color==='red'?'selected':''}>🔴 Rouge</option></select></div></div><div class="form-group"><label class="form-label">Statut initial</label><select class="form-select" id="mLivStatus"><option value="offline" ${l?.status==='offline'||!l?'selected':''}>⚫ Hors ligne</option><option value="online" ${l?.status==='online'?'selected':''}>🟢 En ligne</option><option value="busy" ${l?.status==='busy'?'selected':''}>🟠 Occupé</option></select></div><div class="modal-actions"><button class="btn btn-secondary" onclick="closeModal()">Annuler</button><button class="btn btn-primary" onclick="saveLivreur(${id})">💾 Sauvegarder</button></div>`}modal.innerHTML=html;document.getElementById('modalOverlay').classList.add('active')}
function closeModal(){document.getElementById('modalOverlay').classList.remove('active')}

function saveOrder(id){const customer=document.getElementById('mCustomer').value.trim(),address=document.getElementById('mAddress').value.trim();if(!customer||!address){toast('Veuillez remplir le client et l\'adresse',true);return}const o={id:id||data.nextOrderId++,customer:customer,address:address,items:document.getElementById('mItems').value.trim(),livreurId:parseInt(document.getElementById('mLivreur').value)||null,status:document.getElementById('mStatus').value,time:getTime()};if(id){const i=data.orders.findIndex(x=>x.id===id);data.orders[i]=o}else{data.orders.unshift(o)}save();renderAll();closeModal();toast('Commande sauvegardée')}
function saveReminder(id){const title=document.getElementById('mTitle').value.trim();if(!title){toast('Veuillez entrer un titre',true);return}const icons={'':'💡',info:'🔵',warning:'🟠',urgent:'🔴'},type=document.getElementById('mType').value,r={id:id||Date.now(),title:title,desc:document.getElementById('mDesc').value.trim(),time:document.getElementById('mTime').value.trim()||'Sans échéance',type:type,icon:icons[type]};if(id){const i=data.reminders.findIndex(x=>x.id===id);data.reminders[i]=r}else{data.reminders.unshift(r)}save();renderAll();closeModal();toast('Rappel sauvegardé')}
function saveLivreur(id){const name=document.getElementById('mName').value.trim(),initials=document.getElementById('mInitials').value.trim().toUpperCase();if(!name||!initials){toast('Veuillez remplir le nom et les initiales',true);return}if(initials.length!==2){toast('Les initiales doivent faire 2 caractères',true);return}const l={id:id||Date.now(),name:name,initials:initials,color:document.getElementById('mColor').value,status:document.getElementById('mLivStatus').value};if(id){const i=data.livreurs.findIndex(x=>x.id===id);data.livreurs[i]=l}else{data.livreurs.push(l)}save();renderAll();populateLivreurSelect();closeModal();toast('Livreur sauvegardé')}

load();populateLivreurSelect();document.getElementById('settingAdminName').value=data.settings.adminName;
document.getElementById('loginInput').addEventListener('keypress',e=>{if(e.key==='Enter')login()});
document.getElementById('chatInput').addEventListener('keypress',e=>{if(e.key==='Enter')sendMessage()});
document.getElementById('mobileChatInput').addEventListener('keypress',e=>{if(e.key==='Enter')sendMessage(true)});
setInterval(()=>{if(currentUser){load();renderAll()}},5000);
</script>
</body>
</html>
