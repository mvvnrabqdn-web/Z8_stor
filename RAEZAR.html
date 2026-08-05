from flask import Flask, request, jsonify, render_template_string
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from webdriver_manager.chrome import ChromeDriverManager
import threading
import time
import random

app = Flask(__name__)

# ===== المتغيرات =====
bot_status = "متوقف"
bot_thread = None
stop_flag = False
username = password = comment_text = target_hashtag = ""

# ===== واجهة الموقع (نظيفة وبسيطة) =====
HTML_PAGE = """
<!DOCTYPE html>
<html>
<head><meta charset="UTF-8"><title>بوت إنستغرام</title>
<style>
*{box-sizing:border-box;font-family:Arial,sans-serif}
body{background:#0d1117;color:#c9d1d9;display:flex;justify-content:center;align-items:center;min-height:100vh;margin:0}
.card{background:#161b22;padding:30px;border-radius:16px;border:1px solid #30363d;width:400px}
h1{color:#58a6ff;text-align:center}
.input-group{margin-bottom:15px}
label{color:#8b949e;font-size:13px;display:block;margin-bottom:5px}
input,textarea{width:100%;padding:10px;background:#0d1117;border:1px solid #30363d;border-radius:8px;color:#fff}
.btn-group{display:flex;gap:10px;margin:20px 0}
button{flex:1;padding:12px;border:none;border-radius:8px;font-weight:bold;cursor:pointer}
.btn-start{background:#238636;color:#fff}
.btn-stop{background:#da3633;color:#fff}
.status{background:#0d1117;padding:10px;border-radius:8px;text-align:center;border:1px solid #30363d}
.status span{color:#58a6ff}
</style>
</head>
<body>
<div class="card">
<h1>⚡ بوت التعليقات</h1>
<div class="input-group"><label>👤 اسم المستخدم</label><input type="text" id="username"></div>
<div class="input-group"><label>🔑 كلمة المرور</label><input type="password" id="password"></div>
<div class="input-group"><label>#️⃣ الهاشتاغ (بدون #)</label><input type="text" id="hashtag" value="photography"></div>
<div class="input-group"><label>✍️ نص التعليق</label><textarea id="comment">🔥 ريلز رائع</textarea></div>
<div class="btn-group"><button class="btn-start" onclick="startBot()">▶ تشغيل</button><button class="btn-stop" onclick="stopBot()">⏹ إيقاف</button></div>
<div class="status">📡 الحالة: <span id="statusText">متوقف</span></div>
</div>
<script>
function updateStatus(){fetch('/status').then(r=>r.json()).then(d=>document.getElementById('statusText').innerText=d.status).catch(()=>{});}
function startBot(){
const u=document.getElementById('username').value,p=document.getElementById('password').value,h=document.getElementById('hashtag').value,c=document.getElementById('comment').value;
if(!u||!p)return alert('أدخل البيانات');
fetch('/start',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({username:u,password:p,hashtag:h,comment:c})})
.then(r=>r.json()).then(d=>{alert(d.status);updateStatus();});
}
function stopBot(){fetch('/stop',{method:'POST'}).then(r=>r.json()).then(d=>{alert(d.status);updateStatus();});}
setInterval(updateStatus,2000);updateStatus();
</script>
</body>
</html>
"""

# ===== منطق البوت (مرتب ومضمون) =====
def run_bot():
    global bot_status, stop_flag, username, password, comment_text, target_hashtag
    driver = None
    try:
        bot_status = "🔄 تهيئة المتصفح..."
        
        # إعدادات Chrome
        options = Options()
        options.add_argument("--start-maximized")
        options.add_argument("--disable-blink-features=AutomationControlled")
        options.add_experimental_option("excludeSwitches", ["enable-automation"])
        options.add_experimental_option('useAutomationExtension', False)
        
        # تشغيل المتصفح
        service = Service(ChromeDriverManager().install())
        driver = webdriver.Chrome(service=service, options=options)
        driver.execute_script("Object.defineProperty(navigator, 'webdriver', {get: () => undefined})")

        # 1. تسجيل الدخول
        bot_status = "🔓 جاري تسجيل الدخول..."
        driver.get("https://www.instagram.com/accounts/login/")
        time.sleep(3)
        
        WebDriverWait(driver, 15).until(EC.presence_of_element_located((By.NAME, "username"))).send_keys(username)
        driver.find_element(By.NAME, "password").send_keys(password + Keys.RETURN)
        time.sleep(5)
        
        # تخطي "حفظ المعلومات"
        try:
            not_now = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.XPATH, "//div[contains(text(),'ليس الآن')]")))
            not_now.click()
            time.sleep(2)
        except: pass

        # 2. الدخول للريلز
        bot_status = f"🔍 جاري البحث عن #{
  target_hashtag}..."
        driver.get(f"https://www.instagram.com/explore/tags/{target_hashtag}/")
        time.sleep(4)
        
        # النقر على أول ريلز
        try:
            first_reel = WebDriverWait(driver, 15).until(EC.element_to_be_clickable((By.XPATH, "//div[@class='_aagv']//a[contains(@href, '/reel/')]")))
            first_reel.click()
            time.sleep(4)
        except:
            bot_status = f"⚠️ ماكو ريلزات لهاشتاغ #{
  target_hashtag}"
            return

        # 3. الحلقة الرئيسية
        counter = 0
        while not stop_flag:
            counter += 1
            bot_status = f"🔄 جولة {counter}..."
            
            try:
                # --- إعجاب ---
                try:
                    like_btn = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.XPATH, "//*[local-name()='svg' and @aria-label='إعجاب']")))
                    like_btn.click()
                    time.sleep(random.uniform(1, 2))
                    bot_status = f"❤️ جولة {counter}: تم الإعجاب!"
                except:
                    bot_status = f"⚠️ جولة {counter}: ماعجبته (مكرر)"
                
                # --- تعليق ---
                try:
                    comment_input = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, "//textarea[@placeholder='أضف تعليقًا...']")))
                    comment_input.click()
                    time.sleep(1)
                    comment_input.send_keys(comment_text)
                    time.sleep(1)
                    
                    # زر النشر
                    post_btn = driver.find_element(By.XPATH, "//div[contains(text(),'نشر')]")
                    post_btn.click()
                    bot_status = f"💬 جولة {counter}: تم التعليق!"
                    time.sleep(random.uniform(2, 4))
                except Exception as e:
                    bot_status = f"⚠️ جولة {counter}: فشل التعليق"

                # --- الانتقال للريلز التالي ---
                driver.find_element(By.TAG_NAME, 'body').send_keys(Keys.ARROW_DOWN)
                time.sleep(random.uniform(3, 6))
                
                # انتظار طويل عشان ما ينحظر
                wait_time = random.randint(10, 20)
                bot_status = f"⏳ انتظار {wait_time} ثانية..."
                for _ in range(wait_time):
                    if stop_flag: break
                    time.sleep(1)
                    
            except Exception as e:
                bot_status = f"❌ خطأ: حاول إعادة التشغيل"
                time.sleep(5)
                # محاولة العودة للريلزات
                try:
                    driver.get(f"https://www.instagram.com/explore/tags/{target_hashtag}/")
                    time.sleep(4)
                    first_reel = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, "//div[@class='_aagv']//a[contains(@href, '/reel/')]")))
                    first_reel.click()
                    time.sleep(4)
                except: pass
                
        bot_status = "🛑 تم الإيقاف"
        
    except Exception as e:
        bot_status = f"💀 خطأ: {str(e)[:70]}"
        print("="*40)
        import traceback
        traceback.print_exc()
    finally:
        if driver: driver.quit()

# ===== مسارات Flask =====
@app.route('/')
def index():
    return render_template_string(HTML_PAGE)

@app.route('/start', methods=['POST'])
def start():
    global bot_thread, stop_flag, bot_status, username, password, comment_text, target_hashtag
    if bot_thread and bot_thread.is_alive():
        return jsonify({"status": "⚠️ البوت شغال"})
    
    data = request.json
    username = data.get('username')
    password = data.get('password')
    target_hashtag = data.get('hashtag', 'photography')
    comment_text = data.get('comment', '🔥 رائع')
    
    if not username or not password:
        return jsonify({"status": "❌ اكتب اسم المستخدم وكلمة المرور"})
    
    stop_flag = False
    bot_status = "🚀 يتم التشغيل..."
    bot_thread = threading.Thread(target=run_bot)
    bot_thread.start()
    return jsonify({"status": f"✅ تم التشغيل على #{
  target_hashtag}"})

@app.route('/stop', methods=['POST'])
def stop():
    global stop_flag, bot_status
    stop_flag = True
    bot_status = "⏳ جاري الإيقاف..."
    return jsonify({"status": "⏹ تم إرسال أمر الإيقاف"})

@app.route('/status')
def status():
    return jsonify({"status": bot_status})

if __name__ == '__main__':
    print("🔥 Shadow Bot جاهز - http://127.0.0.1:5000")
    app.run(debug=True, host='0.0.0.0', port=5000)
