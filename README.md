<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>获取免费的自动化审计</title>
  <meta name="description" content="提交你的需求，我们回传 3 个本周可落地的自动化方案。" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="./styles.css" />
</head>
<body>
  <header class="container">
    <h1>获得免费的自动化审计</h1>
    <p class="sub">我们将审查你的工作流程，并回传 <strong>3 个本周可实施</strong> 的自动化建议。</p>
  </header>

  <main class="container card">
    <form id="lead-form" class="form">
      <div class="grid">
        <label class="field">
          <span>电子邮件*</span>
          <input type="email" name="email" required placeholder="you@company.com" />
        </label>
        <label class="field">
          <span>姓名</span>
          <input type="text" name="full_name" placeholder="付泽文" />
        </label>
      </div>

      <label class="field">
        <span>公司/网站</span>
        <input type="text" name="company" placeholder="acme.com 或 https://example.com" />
      </label>

      <label class="field">
        <span>你想修复的瓶颈是什么？</span>
        <textarea name="bottleneck" rows="4" placeholder="例如：从网站同步线索到 CRM；对未付款发票自动跟进；工单自动分派……"></textarea>
      </label>

      <input type="hidden" name="campaign" value="website" />

      <button type="submit" class="btn">请求审核</button>
      <p id="msg" class="muted"></p>
    </form>
  </main>

  <footer class="container footer">
    <a href="./privacy.html">隐私</a>
    <a href="./unsubscribe.html">取消订阅</a>
  </footer>

  <script>
    const N8N_WEBHOOK_URL = "https://fuzewen.app.n8n.cloud/webhook/hooks/audit";
    const form = document.getElementById('lead-form');
    const msg  = document.getElementById('msg');
    function validEmail(v) { return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v); }
    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      const fd = new FormData(form);
      const data = Object.fromEntries(fd.entries());
      if (!validEmail(data.email)) { msg.textContent = "邮箱格式不正确。"; msg.classList.add("error"); return; }
      data.submitted_at = new Date().toISOString();
      data.user_agent   = navigator.userAgent;
      msg.textContent = "提交中…"; msg.classList.remove("error");
      try {
        const res = await fetch(N8N_WEBHOOK_URL, { method: "POST", headers: { "Content-Type": "application/json" }, body: JSON.stringify(data) });
        if (!res.ok) throw new Error("webhook 非 200 返回");
        window.location.href = "./thanks.html";
      } catch (err) { console.error(err); msg.textContent = "提交失败，请稍后重试。"; msg.classList.add("error"); }
    });
  </script>
</body>
</html>
