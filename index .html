// ============ SMART PIGGY BANK PRO - ATM STYLE ============
// Исправлено: Telegram Bot теперь работает корректно
// Изменения: исправлена инициализация WiFiClientSecure, добавлены проверки подключения

#include <WiFi.h>
#include <WebServer.h>
#include <Preferences.h>
#include <time.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>

const char* ssid = "Ucom8137_2.4GHz";
const char* password = "123581321";

#define COIN_PIN 14

WebServer server(80);
Preferences prefs;

volatile int pulseCount = 0;
volatile unsigned long lastPulseTime = 0;
volatile unsigned long pulseId = 0;

String lastCoin = "-";
int total = 0;
int lastPulses = 0;
int goalAmount = 5000;
String goalName = "Այս ամիս ✅";

const char* BG_SCREENSAVER = "https://i.postimg.cc/Bvjq68kR/Chat-GPT-Image-13-iul-2026-g-11-24-03.png";
const char* BG_MENU = "https://i.postimg.cc/13syryJh/5373067618912248599.jpg";

int totalCoins50 = 0;
int totalCoins100 = 0;
int totalCoins200 = 0;
int totalCoins500 = 0;
int totalCoinsUnknown = 0;
int totalDeposits = 0;
int totalCollections = 0;
int lastCollectionAmount = 0;
String lastCollectionDate = "Երբեք";
int totalCollectedAllTime = 0;

int sessionAmount = 0;

String todayDate = "";
int todayDeposits = 0;
int todayAmount = 0;
int monthDeposits = 0;
int monthAmount = 0;
String collectionLog = "";

const char* ntpServer = "pool.ntp.org";
const long gmtOffset_sec = 14400;
const int daylightOffset_sec = 0;
bool timeInitialized = false;

struct InventoryItem {
  String name;
  int value;
};
InventoryItem inventory[20];
int inventoryCount = 0;

// ============ TELEGRAM BOT CONFIGURATION ============
#define BOT_TOKEN "8913123028:AAFD_HJjBGrp_77STH3s5IAVERtJlsXicSI"
#define CHAT_ID "7011850734"

WiFiClientSecure secured_client;
UniversalTelegramBot bot(BOT_TOKEN, secured_client);

unsigned long bot_lastcheck = 0;
const unsigned long BOT_MTBS = 1000;
bool bot_initialized = false;

void IRAM_ATTR coinISR() {
  pulseCount++;
  lastPulseTime = millis();
}

void saveData() {
  prefs.begin("bank", false);
  prefs.putInt("total", total);
  prefs.putInt("goal", goalAmount);
  prefs.putString("goalName", goalName);
  prefs.putInt("coins50", totalCoins50);
  prefs.putInt("coins100", totalCoins100);
  prefs.putInt("coins200", totalCoins200);
  prefs.putInt("coins500", totalCoins500);
  prefs.putInt("coinsUnk", totalCoinsUnknown);
  prefs.putInt("deposits", totalDeposits);
  prefs.putInt("collections", totalCollections);
  prefs.putInt("lastCollAmt", lastCollectionAmount);
  prefs.putString("lastCollDate", lastCollectionDate);
  prefs.putInt("totalCollAll", totalCollectedAllTime);
  prefs.putString("todayDate", todayDate);
  prefs.putInt("todayDeposits", todayDeposits);
  prefs.putInt("todayAmount", todayAmount);
  prefs.putInt("monthDeposits", monthDeposits);
  prefs.putInt("monthAmount", monthAmount);
  prefs.putString("collLog", collectionLog);
  prefs.putInt("invCount", inventoryCount);
  for (int i = 0; i < inventoryCount; i++) {
    prefs.putString(("invN" + String(i)).c_str(), inventory[i].name);
    prefs.putInt(("invV" + String(i)).c_str(), inventory[i].value);
  }
  prefs.end();
}

String getCurrentDateString() {
  if (!timeInitialized) {
    configTime(gmtOffset_sec, daylightOffset_sec, ntpServer);
    timeInitialized = true;
    delay(100);
  }
  struct tm timeinfo;
  if (!getLocalTime(&timeinfo)) {
    unsigned long daysSinceStart = millis() / 86400000UL;
    return String(daysSinceStart);
  }
  char buf[16];
  strftime(buf, sizeof(buf), "%d.%m.%Y", &timeinfo);
  return String(buf);
}

String getCurrentMonthString() {
  if (!timeInitialized) {
    configTime(gmtOffset_sec, daylightOffset_sec, ntpServer);
    timeInitialized = true;
    delay(100);
  }
  struct tm timeinfo;
  if (!getLocalTime(&timeinfo)) {
    return String(millis() / 2592000000UL);
  }
  char buf[16];
  strftime(buf, sizeof(buf), "%m.%Y", &timeinfo);
  return String(buf);
}

String getFormattedDateTime() {
  if (!timeInitialized) {
    configTime(gmtOffset_sec, daylightOffset_sec, ntpServer);
    timeInitialized = true;
    delay(100);
  }
  struct tm timeinfo;
  if (!getLocalTime(&timeinfo)) {
    return String(millis() / 1000) + " վրկ առաջ";
  }
  char buf[64];
  strftime(buf, sizeof(buf), "%d.%m.%Y %H:%M", &timeinfo);
  return String(buf);
}

void checkAndResetDailyStats() {
  String currentDate = getCurrentDateString();
  String currentMonth = getCurrentMonthString();
  if (todayDate != currentDate) {
    todayDate = currentDate;
    todayDeposits = 0;
    todayAmount = 0;
    prefs.begin("bank", false);
    prefs.putString("todayDate", todayDate);
    prefs.putInt("todayDeposits", todayDeposits);
    prefs.putInt("todayAmount", todayAmount);
    prefs.end();
  }
  static String lastCheckedMonth = "";
  if (lastCheckedMonth != currentMonth) {
    lastCheckedMonth = currentMonth;
    monthDeposits = 0;
    monthAmount = 0;
    prefs.begin("bank", false);
    prefs.putInt("monthDeposits", monthDeposits);
    prefs.putInt("monthAmount", monthAmount);
    prefs.end();
  }
}

void loadData() {
  prefs.begin("bank", true);
  total = prefs.getInt("total", 0);
  goalAmount = prefs.getInt("goal", 5000);
  goalName = prefs.getString("goalName", "Այս ամիս ✅");
  totalCoins50 = prefs.getInt("coins50", 0);
  totalCoins100 = prefs.getInt("coins100", 0);
  totalCoins200 = prefs.getInt("coins200", 0);
  totalCoins500 = prefs.getInt("coins500", 0);
  totalCoinsUnknown = prefs.getInt("coinsUnk", 0);
  totalDeposits = prefs.getInt("deposits", 0);
  totalCollections = prefs.getInt("collections", 0);
  lastCollectionAmount = prefs.getInt("lastCollAmt", 0);
  lastCollectionDate = prefs.getString("lastCollDate", "Երբեք");
  totalCollectedAllTime = prefs.getInt("totalCollAll", 0);
  todayDate = prefs.getString("todayDate", "");
  todayDeposits = prefs.getInt("todayDeposits", 0);
  todayAmount = prefs.getInt("todayAmount", 0);
  monthDeposits = prefs.getInt("monthDeposits", 0);
  monthAmount = prefs.getInt("monthAmount", 0);
  collectionLog = prefs.getString("collLog", "");
  inventoryCount = prefs.getInt("invCount", 0);
  if (inventoryCount > 20) inventoryCount = 20;
  for (int i = 0; i < inventoryCount; i++) {
    inventory[i].name = prefs.getString(("invN" + String(i)).c_str(), "Արտադրանք");
    inventory[i].value = prefs.getInt(("invV" + String(i)).c_str(), 0);
  }
  prefs.end();
}

// ============ TELEGRAM BOT FUNCTIONS ============
void sendDepositNotification(int amount, int newTotal) {
  if (!bot_initialized) return;
  String msg = "Մուտք է կատարվել *+" + String(amount) + " դրամ*\n";
  msg += "💳 Ընթացիկ մնացորդ: " + String(newTotal) + " դրամ";
  bot.sendMessage(CHAT_ID, msg, "Markdown");
}

void sendGoalReachedNotification() {
  if (!bot_initialized) return;
  String msg = "🎉 *Շնորհավորում ենք!*\n";
  msg += "Դուք հասել եք ձեր նպատակին:\n";
  msg += goalName + "\n";
  msg += "Գումար: " + String(total) + " դրամ";
  bot.sendMessage(CHAT_ID, msg, "Markdown");
}

void sendCollectionNotification(int amount) {
  if (!bot_initialized) return;
  String msg = "📤 *Կատարվել է ինկասացիա։ Հաշվետվությունը հասանելի է պաշտոնական կայքում։*\n";
  msg += "Կատարվել է ելք: " + String(amount) + " դրամ";
  bot.sendMessage(CHAT_ID, msg, "Markdown");
}

void handleTelegramMessages() {
  if (millis() - bot_lastcheck > BOT_MTBS) {
    int numNewMessages = bot.getUpdates(bot.last_message_received + 1);
    while (numNewMessages) {
      for (int i = 0; i < numNewMessages; i++) {
        String chat_id = String(bot.messages[i].chat_id);
        String text = bot.messages[i].text;
        String from_name = bot.messages[i].from_name;

        // Reply Keyboard (постоянная клавиатура под полем ввода)
        String replyKeyboard = "[[\"💳 Ընթացիկ Մնացորդ\",\"📊 Վիճակագրություն\"],[\"🎯 Նպատակ\",\"❓ Օգնություն\"]]";

        if (text == "/start") {
          String welcome = "👋 Բարև, " + from_name + "!\n\n";
          welcome += "🐷 *Խելացի Խնայատուփ PRO*\n\n";
          welcome += "💰 Ընթացիկ մնացորդ: *" + String(total) + " դրամ*\n";
          welcome += "🎯 Նպատակ: " + goalName;
          bot.sendMessageWithReplyKeyboard(chat_id, welcome, "Markdown", replyKeyboard, true);
        }
        else if (text == "💳 Ընթացիկ Մնացորդ" || text == "/balance") {
          String msg = "💳 *Ընթացիկ Մնացորդ*\n\n🏦 Մնացորդ: *" + String(total) + " դրամ*";
          bot.sendMessage(chat_id, msg, "Markdown");
        }
        else if (text == "📊 Վիճակագրություն" || text == "/stats") {
          String msg = "📊 *Վիճակագրություն*\n\n";
          msg += "📅 Այսօր: *" + String(todayDeposits) + "* անգամ / *" + String(todayAmount) + "* դրամ\n";
          msg += "📆 Այս ամիս: *" + String(monthDeposits) + "* անգամ / *" + String(monthAmount) + "* դրամ";
          bot.sendMessage(chat_id, msg, "Markdown");
        }
        else if (text == "🎯 Նպատակ" || text == "/goal") {
          int pct = (total * 100) / goalAmount;
          if (pct > 100) pct = 100;
          int filled = (pct * 10) / 100;
          String bar = "";
          for (int j = 0; j < 10; j++) {
            if (j < filled) bar += "🟩";
            else bar += "⬜";
          }
          String msg = "🎯 *Նպատակ:* " + goalName + "\n\n";
          msg += "📊 *Կատարման առաջընթաց: " + String(pct) + "%*\n" + bar + "\n";
          msg += "💰 " + String(total) + " / " + String(goalAmount) + " դրամ";
          bot.sendMessage(chat_id, msg, "Markdown");
        }
        else if (text == "❓ Օգնություն" || text == "/help") {
          String msg = "📋 *Հրամաններ*\n\n💳 Ընթացիկ Մնացորդ\n📊 Վիճակագրություն\n🎯 Նպատակ\n🔄 /start - Գլխավոր էջ";
          bot.sendMessage(chat_id, msg, "Markdown");
        }
      }
      numNewMessages = bot.getUpdates(bot.last_message_received + 1);
    }
    bot_lastcheck = millis();
  }
}

// ============ HTML PAGE GENERATORS ============
String getScreensaverHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_SCREENSAVER);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;justify-content:center;overflow:hidden}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.65);z-index:0}
.content{position:relative;z-index:1;text-align:center;animation:fadeIn 1.5s ease}
@keyframes fadeIn{from{opacity:0;transform:translateY(30px)}to{opacity:1;transform:translateY(0)}}
.clock-container{position:relative;width:220px;height:220px;margin:0 auto 30px auto}
.clock-face{position:absolute;width:100%;height:100%;border-radius:50%;background:rgba(255,255,255,0.05);border:3px solid rgba(255,255,255,0.15);box-shadow:0 0 60px rgba(255,215,0,0.15),inset 0 0 60px rgba(255,215,0,0.05),0 0 120px rgba(255,215,0,0.08);backdrop-filter:blur(5px)}
.clock-face::before{content:'';position:absolute;top:50%;left:50%;width:14px;height:14px;background:#ffd700;border-radius:50%;transform:translate(-50%,-50%);box-shadow:0 0 20px rgba(255,215,0,0.6);z-index:10}
.clock-tick{position:absolute;width:2px;height:10px;background:rgba(255,255,255,0.3);left:50%;top:8px;transform-origin:center 102px;border-radius:1px}
.clock-tick.major{height:16px;width:3px;background:rgba(255,215,0,0.7);top:5px;transform-origin:center 105px}
.clock-number{position:absolute;font-size:14px;font-weight:700;color:rgba(255,255,255,0.6);width:30px;height:30px;display:flex;align-items:center;justify-content:center;left:50%;top:50%;transform-origin:center}
.hand{position:absolute;bottom:50%;left:50%;transform-origin:bottom center;border-radius:4px;z-index:5;transition:transform 0.3s cubic-bezier(0.4,2.3,0.3,1)}
.hand-hour{width:5px;height:55px;background:#ffd700;margin-left:-2.5px;box-shadow:0 0 15px rgba(255,215,0,0.4)}
.hand-minute{width:3px;height:80px;background:rgba(255,255,255,0.9);margin-left:-1.5px;box-shadow:0 0 10px rgba(255,255,255,0.3)}
.hand-second{width:2px;height:90px;background:#ff6b6b;margin-left:-1px;box-shadow:0 0 12px rgba(255,107,107,0.5);transition:transform 0.1s cubic-bezier(0.4,2.3,0.3,1)}
.hand-second::after{content:'';position:absolute;bottom:-15px;left:50%;width:4px;height:20px;background:#ff6b6b;transform:translateX(-50%);border-radius:2px}
.date-display{margin-bottom:20px;animation:slideUp 1s ease 0.5s both}
.date-day-name{font-size:18px;color:rgba(255,215,0,0.8);font-weight:600;letter-spacing:3px;text-transform:uppercase;margin-bottom:4px;text-shadow:0 0 20px rgba(255,215,0,0.3)}
.date-full{font-size:22px;font-weight:700;color:rgba(255,255,255,0.9);letter-spacing:2px;text-shadow:0 2px 10px rgba(0,0,0,0.5)}
@keyframes slideUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
.title{font-size:48px;font-weight:800;letter-spacing:4px;text-transform:uppercase;margin-bottom:10px;text-shadow:0 0 40px rgba(255,215,0,0.4);animation:slideUp 1s ease 0.7s both}
.subtitle{font-size:18px;color:rgba(255,255,255,0.6);margin-bottom:40px;letter-spacing:2px;animation:slideUp 1s ease 0.9s both}
.tap-hint{display:inline-block;font-size:16px;color:#fff;background:rgba(255,255,255,0.12);border:2px solid rgba(255,255,255,0.3);border-radius:16px;padding:14px 28px;margin-top:30px;backdrop-filter:blur(10px);text-shadow:0 2px 8px rgba(0,0,0,0.5);letter-spacing:1px;animation:slideUp 1s ease 1.1s both,pulse 2s ease-in-out 2s infinite}
@keyframes pulse{0%,100%{opacity:0.4}50%{opacity:1}}
a{text-decoration:none;color:inherit}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="clock-container"><div class="clock-face" id="clockFace"></div><div class="hand hand-hour" id="hourHand"></div><div class="hand hand-minute" id="minuteHand"></div><div class="hand hand-second" id="secondHand"></div></div>
<div class="date-display"><div class="date-day-name" id="dayName"></div><div class="date-full" id="fullDate"></div></div>
<div class="title">Խելացի Խնայատուփ</div><div class="subtitle">Թվային նորարարությունների դարաշրջան</div>
<a href="/menu"><div class="tap-hint">&#128073; Բացեք ձեր խնայողության ճանապարհը</div></a>
</div>
<script>
const armMonths=['Հունվար','Փետրվար','Մարտ','Ապրիլ','Մայիս','Հունիս','Հուլիս','Օգոստոս','Սեպտեմբեր','Հոկտեմբեր','Նոյեմբեր','Դեկտեմբեր'];
const armDays=['Կիրակի','Երկուշաբթի','Երեքշաբթի','Չորեքշաբթի','Հինգշաբթի','Ուրբաթ','Շաբաթ'];
function createClockFace(){const face=document.getElementById('clockFace');for(let i=0;i<60;i++){const tick=document.createElement('div');tick.className=i%5===0?'clock-tick major':'clock-tick';tick.style.transform=`rotate(${i*6}deg)`;face.appendChild(tick)}const numbers=[12,1,2,3,4,5,6,7,8,9,10,11];numbers.forEach((num,i)=>{const el=document.createElement('div');el.className='clock-number';el.textContent=num;const angle=(i*30)*Math.PI/180;const radius=78;const x=Math.sin(angle)*radius;const y=-Math.cos(angle)*radius;el.style.transform=`translate(calc(-50% + ${x}px),calc(-50% + ${y}px))`;face.appendChild(el)})}
function updateClock(){const now=new Date();const h=now.getHours(),m=now.getMinutes(),s=now.getSeconds(),ms=now.getMilliseconds();const smoothS=s+ms/1000;document.getElementById('secondHand').style.transform=`rotate(${smoothS*6}deg)`;document.getElementById('minuteHand').style.transform=`rotate(${m*6+s*0.1}deg)`;document.getElementById('hourHand').style.transform=`rotate(${(h%12)*30+m*0.5}deg)`;document.getElementById('dayName').textContent=armDays[now.getDay()];document.getElementById('fullDate').textContent=now.getDate()+' '+armMonths[now.getMonth()]+', '+now.getFullYear()+' թ.'}
createClockFace();updateClock();setInterval(updateClock,50);
</script></body></html>
)rawliteral";
  return page;
}

String getMenuHTML() {
  int percent = (total * 100) / goalAmount;
  if (percent > 100) percent = 100;
  int overAmount = total > goalAmount ? total - goalAmount : 0;

  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Գլխավոր էջ - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:1100px}
.header{text-align:center;margin-bottom:25px}
.header h1{font-size:28px;font-weight:800;letter-spacing:3px;text-transform:uppercase;margin-bottom:6px}
.header p{color:rgba(255,255,255,0.5);font-size:13px;letter-spacing:2px}
.main-row{display:flex;gap:20px;align-items:stretch}
.side-card{flex:0 0 220px;border:1px solid rgba(255,255,255,0.15);border-radius:20px;padding:25px 18px;text-align:center;min-height:380px;display:flex;flex-direction:column;justify-content:center;position:relative;overflow:hidden;text-decoration:none;color:#fff}
.side-card::before{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.6);z-index:0}
.side-card>*{position:relative;z-index:1}
.side-card.goal-card{border-color:rgba(0,255,136,0.3);background:url('https://i.postimg.cc/d0zmNxCd/Chat-GPT-Image-13-iul-2026-g-12-04-21.png')center/cover no-repeat;justify-content:flex-start;padding-top:25px}
.goal-card .side-label{margin-bottom:30px;font-size:18px;color:#fff;font-weight:700;text-shadow:0 2px 8px rgba(0,0,0,0.5)}
.goal-card .goal-content{margin-top:auto;margin-bottom:auto}
.side-card.balance-card{border-color:rgba(255,215,0,0.3);background:url('https://i.postimg.cc/PqBzVRDw/Chat-GPT-Image-13-iul-2026-g-11-49-27.png')center/cover no-repeat;justify-content:flex-start;padding-top:25px}
.balance-card .side-label{margin-bottom:30px;font-size:18px;color:#fff;font-weight:700;text-shadow:0 2px 8px rgba(0,0,0,0.5)}
.balance-card .side-value{margin-top:auto;margin-bottom:auto}
.side-label{font-size:12px;color:rgba(255,255,255,0.6);text-transform:uppercase;letter-spacing:2px;margin-bottom:12px}
.side-value{font-size:32px;font-weight:800;margin-bottom:8px;word-break:break-word}
.side-value.goal{color:#00ff88}
.side-value.balance{color:#ffd700}
.side-sub{font-size:14px;color:rgba(255,255,255,0.6);margin-bottom:15px}
.goal-bar-bg{width:100%;height:8px;background:rgba(255,255,255,0.15);border-radius:4px;overflow:hidden}
.goal-bar{height:100%;background:linear-gradient(90deg,#00ff88,#00cc6a);border-radius:4px;transition:width 0.8s ease}
.goal-percent{font-size:14px;color:#00ff88;margin-top:10px;font-weight:600}
.over-card{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat;border:1px solid rgba(255,107,107,0.4);border-radius:20px;padding:20px 18px;text-align:center;margin-top:15px;position:relative;overflow:hidden;animation:slideDown 0.5s ease}
.over-card::before{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background-image:url('https://i.postimg.cc/HL8T2tF8/Chat-GPT-Image-14-iul-2026-g-11-51-11.png');background-size:cover;background-position:center;background-repeat:no-repeat;z-index:0}
.over-card>*{position:relative;z-index:1}
@keyframes slideDown{from{opacity:0;transform:translateY(-20px)}to{opacity:1;transform:translateY(0)}}
.over-label{font-size:12px;color:rgba(255,255,255,0.6);text-transform:uppercase;letter-spacing:2px;margin-bottom:8px}
.over-value{font-size:28px;font-weight:800;color:#ff6b6b;margin-bottom:8px}
.over-text{font-size:13px;color:rgba(255,255,255,0.7);line-height:1.5}
.over-link{display:inline-block;margin-top:10px;padding:10px 20px;background:linear-gradient(135deg,#ff6b6b,#ee5a5a);border-radius:10px;color:#fff;text-decoration:none;font-size:13px;font-weight:600;transition:all 0.3s}
.over-link:hover{transform:translateY(-2px);box-shadow:0 4px 15px rgba(255,107,107,0.4)}
.center-area{flex:1;display:flex;flex-direction:column;gap:16px}
.menu-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.menu-item{display:flex;flex-direction:column;align-items:center;gap:10px}
.menu-btn{border:2px solid rgba(255,255,255,0.2);border-radius:24px;padding:0;text-align:center;color:#fff;text-decoration:none;transition:all 0.3s;cursor:pointer;position:relative;overflow:hidden;display:block;min-height:180px;width:100%;background-size:cover;background-position:center;box-shadow:0 4px 20px rgba(0,0,0,0.3)}
.menu-btn::before{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.25);z-index:0;transition:all 0.3s}
.menu-btn:hover::before{background:rgba(0,0,0,0.1)}
.menu-btn:hover{transform:translateY(-6px) scale(1.02);box-shadow:0 15px 40px rgba(0,0,0,0.4);border-color:rgba(255,255,255,0.5)}
.menu-label{font-size:15px;font-weight:600;color:#fff;text-align:center;letter-spacing:1px;text-shadow:0 2px 8px rgba(0,0,0,0.8)}
.back-btn{display:block;text-align:center;padding:16px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:16px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:14px;letter-spacing:2px;transition:all 0.3s}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
@media(max-width:768px){.main-row{flex-direction:column}.side-card{flex:none;width:100%;min-height:auto;padding:20px}.side-value{font-size:24px}}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="header"><h1>Ողջույն, բարի գալուստ գլխավոր էջ</h1><p>Ընտրեք ցանկալի գործողությունը</p></div>
<div class="main-row">
<div class="side-card goal-card"><div class="side-label">Նպատակ</div><div class="goal-content"><div class="side-value goal">)rawliteral";
  page += goalName;
  page += R"rawliteral(</div><div class="side-sub">)rawliteral";
  page += String(total) + " / " + String(goalAmount) + " դրամ";
  page += R"rawliteral(</div><div class="goal-bar-bg"><div class="goal-bar" style="width:)rawliteral";
  page += String(percent);
  page += R"rawliteral(%"></div></div><div class="goal-percent">)rawliteral";
  page += String(percent) + "%";
  page += R"rawliteral(</div></div></div>
<div class="center-area">
<div class="menu-grid">
<div class="menu-item"><a href="/deposit" class="menu-btn" style="background:url('https://i.postimg.cc/9FFhDLMT/5373067618912248628.jpg')center/cover no-repeat"></a><div class="menu-label">Լիցքավորել</div></div>
<div class="menu-item"><a href="/stats" class="menu-btn" style="background:url('https://i.postimg.cc/MZPY08PM/Chat-GPT-Image-7-iul-2026-g-14-07-29.png')center/cover no-repeat"></a><div class="menu-label">Վիճակագրություն</div></div>
<div class="menu-item"><a href="/collection" class="menu-btn" style="background:url('https://i.postimg.cc/h4y08qyQ/Chat-GPT-Image-7-iul-2026-g-14-07-14.png')center/cover no-repeat"></a><div class="menu-label">Ինկասացիա</div></div>
<div class="menu-item"><a href="/inventory" class="menu-btn" style="background:url('https://i.postimg.cc/X7ygkbp0/5373067618912248634.jpg')center/cover no-repeat"></a><div class="menu-label">Մետաղադրամների պահոց</div></div>
<div class="menu-item"><a href="/goal" class="menu-btn" style="background:url('https://i.postimg.cc/SN0g5R0F/Chat-GPT-Image-12-iul-2026-g-00-59-39.png')center/cover no-repeat"></a><div class="menu-label">Փոխել նպատակը</div></div>
<div class="menu-item"><a href="/settings" class="menu-btn" style="background:url('https://i.postimg.cc/dttYh51r/5373067618912248629.jpg')center/cover no-repeat"></a><div class="menu-label">Կարգավորումներ</div></div>
</div>
<a href="/" class="back-btn">&#9664; Մեկնարկային էկրան</a>
</div>
<div class="side-card balance-card" style="justify-content:center"><div class="side-label">Մնացորդ</div><div class="side-value balance" id="menuBal">)rawliteral";
  page += String(total);
  page += R"rawliteral(</div><div class="side-sub">դրամ</div><div style="font-size:12px;color:rgba(255,255,255,0.4);margin-top:10px">Թարմացվում է ավտոմատ</div></div>
</div>
)rawliteral";

  if (overAmount > 0) {
    page += R"rawliteral(<div class="over-card"><div class="over-label">&#9888; Ուշադրություն</div><div class="over-value">+ )rawliteral";
    page += String(overAmount);
    page += R"rawliteral( դրամ</div><div class="over-text">Դուք գերազանցել եք ընթացիկ նպատակը!<br>Կատարեք ինկասացիա։</div><a href="/collection" class="over-link">&#128235; Ինկասացիա</a></div>)rawliteral";
  }

  page += R"rawliteral(</div><script>function updateMenuBal(){fetch('/api').then(r=>r.json()).then(d=>{document.getElementById('menuBal').textContent=d.total.toLocaleString()}).catch(()=>{})}setInterval(updateMenuBal,1000);updateMenuBal();</script></body></html>
)rawliteral";
  return page;
}
String getDepositHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Լիցքավորում - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:40px 20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:500px}
.card{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:24px;padding:35px;margin-bottom:20px;backdrop-filter:blur(10px);text-align:center}
.label{font-size:13px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:3px;margin-bottom:10px}
.session-total{font-size:80px;font-weight:800;color:#00ff88;text-shadow:0 0 40px rgba(0,255,136,0.3);min-height:100px;display:flex;align-items:center;justify-content:center}
.hint{font-size:16px;color:rgba(255,255,255,0.4);margin-top:10px}
.manual-section{margin-top:20px;padding-top:20px;border-top:1px solid rgba(255,255,255,0.1)}
.manual-label{font-size:12px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:15px}
.manual-row{display:flex;align-items:center;justify-content:center;gap:12px;background:rgba(255,255,255,0.04);border:1px solid rgba(255,255,255,0.1);border-radius:16px;padding:15px 20px}
.manual-name{font-size:16px;font-weight:600;color:rgba(255,255,255,0.8);min-width:70px;text-align:left}
.manual-controls{display:flex;align-items:center;gap:8px}
.manual-btn-sm{width:36px;height:36px;border:none;border-radius:10px;background:rgba(255,255,255,0.1);color:#fff;font-size:20px;font-weight:700;cursor:pointer;transition:all 0.2s;display:flex;align-items:center;justify-content:center}
.manual-btn-sm:hover{background:rgba(255,255,255,0.2)}
.manual-btn-sm:active{transform:scale(0.9)}
.manual-amount{width:60px;height:40px;background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.15);border-radius:10px;color:#fff;font-size:18px;font-weight:700;text-align:center;outline:none}
.manual-sum{font-size:14px;color:#ffd700;font-weight:600;min-width:80px;text-align:right}
.btn-row{display:flex;gap:12px;margin-top:25px}
.btn{flex:1;padding:22px;border:none;border-radius:16px;font-size:18px;font-weight:700;letter-spacing:1px;cursor:pointer;transition:all 0.3s;text-transform:uppercase;text-align:center;text-decoration:none;display:flex;align-items:center;justify-content:center}
.btn-deposit{background:linear-gradient(135deg,#00ff88,#00cc6a);color:#000;box-shadow:0 4px 20px rgba(0,255,136,0.3)}
.btn-deposit:hover{transform:translateY(-2px);box-shadow:0 6px 25px rgba(0,255,136,0.5)}
.btn-deposit:disabled{background:rgba(255,255,255,0.1);color:rgba(255,255,255,0.3);box-shadow:none;cursor:not-allowed;transform:none}
.btn-cancel{background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.2);color:#fff}
.btn-cancel:hover{background:rgba(255,255,255,0.15)}
.back-btn{display:block;text-align:center;padding:14px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:14px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:13px;letter-spacing:2px;transition:all 0.3s}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
.modal-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.8);z-index:100;display:none;align-items:center;justify-content:center;animation:fadeIn 0.3s ease}
.modal-overlay.active{display:flex}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
.modal-box{background:linear-gradient(135deg,#1a1a2e,#16213e);border:1px solid rgba(255,215,0,0.3);border-radius:24px;padding:40px 30px;text-align:center;max-width:400px;width:90%;animation:popIn 0.4s ease}
@keyframes popIn{from{opacity:0;transform:scale(0.7)}to{opacity:1;transform:scale(1)}}
.modal-icon{font-size:60px;margin-bottom:15px}
.modal-text{font-size:24px;font-weight:700;color:#ffd700;margin-bottom:10px}
.modal-sub{font-size:14px;color:rgba(255,255,255,0.5);margin-bottom:25px}
.modal-close{padding:14px 40px;background:linear-gradient(135deg,#ffd700,#ffaa00);border:none;border-radius:12px;color:#000;font-size:16px;font-weight:700;cursor:pointer;transition:all 0.3s}
.modal-close:hover{transform:translateY(-2px);box-shadow:0 6px 20px rgba(255,215,0,0.4)}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="card"><div class="label">Ընթացիկ գումարը</div><div class="session-total" id="sessionTotal">0 դրամ</div><div class="hint" id="hint">Մուտքագրեք մետաղադրամները և սեղմեք «Փոխանցել հաշվին» կոճակը։</div>
<div class="manual-section"><div class="manual-label">&#9997; Մուտքագրեք 50 դրամանոց մետաղադրամների քանակը</div>
<div class="manual-row"><div class="manual-name">50 դրամ</div><div class="manual-controls"><button class="manual-btn-sm" onclick="change50(-1)">&#8722;</button><input type="text" class="manual-amount" id="cnt50" value="0" readonly><button class="manual-btn-sm" onclick="change50(1)">+</button></div><div class="manual-sum" id="sum50">0 դրամ</div></div></div>
<div class="btn-row"><button class="btn btn-deposit" id="depositBtn" onclick="doDeposit()">Փոխանցել հաշվին</button><a href="/menu" class="btn btn-cancel">Չեղարկել</a></div></div>
<a href="/menu" class="back-btn">&#9664; Վերադառնալ Գլխավոր էջ</a>
</div>
<div class="modal-overlay" id="thankModal"><div class="modal-box"><div class="modal-icon">&#9989;</div><div class="modal-text" id="thankText">Ձեր ապագա «ես»-ը շնորհակալ կլինի այսօրվա խնայողությունների համար!</div><div class="modal-sub" id="thankSub">Ձեր ապագա «ես»-ը շնորհակալ կլինի այսօրվա խնայողությունների համար։</div><button class="modal-close" onclick="closeModal()">Շարունակե՛ք նույն ոգով!</button></div></div>
<script>
let sessionSum=0,lastPulseId=0,manual50=0;
function update(){fetch('/api/coin').then(r=>r.json()).then(d=>{if(d.coin&&d.coin!=='-'&&d.pulseId&&d.pulseId!==lastPulseId){lastPulseId=d.pulseId;const val=parseInt(d.coin);if(!isNaN(val)){sessionSum+=val;document.getElementById('sessionTotal').textContent=sessionSum.toLocaleString()+' դրամ';document.getElementById('hint').textContent='Հաստատելու համար սեղմեք "Մուտքագրել գումար"';document.getElementById('depositBtn').disabled=false}}}).catch(()=>{})}
function change50(delta){manual50=Math.max(0,manual50+delta);document.getElementById('cnt50').value=manual50;document.getElementById('sum50').textContent=(manual50*50).toLocaleString()+' դրամ';if(manual50>0||sessionSum>0)document.getElementById('depositBtn').disabled=false}
function doDeposit(){if(sessionSum===0&&manual50===0){alert('Նախ գցեք մետաղադրամներ կամ ավելացրեք 50 դրամանոցներ');return}const depositAmount=sessionSum+(manual50*50);if(manual50>0){fetch('/api/manual',{method:'POST',headers:{'Content-Type':'application/x-www-form-urlencoded'},body:'amount=50&count='+manual50}).then(()=>{manual50=0;document.getElementById('cnt50').value='0';document.getElementById('sum50').textContent='0 դրամ';return fetch('/api/deposit',{method:'POST'})}).then(r=>r.json()).then(d=>{if(d.success)finishDeposit(depositAmount)}).catch(()=>{})}else{fetch('/api/deposit',{method:'POST'}).then(r=>r.json()).then(d=>{if(d.success)finishDeposit(depositAmount)}).catch(()=>{})}}
function finishDeposit(amount){sessionSum=0;lastPulseId=0;document.getElementById('sessionTotal').textContent='0 դրամ';document.getElementById('hint').textContent='Գցեք մետաղադրամները, ապա սեղմեք կոճակը';document.getElementById('depositBtn').disabled=true;document.getElementById('thankText').textContent='Ձեր հաշվեկշիռն ավելացել է '+amount.toLocaleString()+' դրամով';document.getElementById('thankSub').textContent='Ձեր ապագա «ես»-ը շնորհակալ կլինի այսօրվա խնայողությունների համար!';document.getElementById('thankModal').classList.add('active')}
function closeModal(){document.getElementById('thankModal').classList.remove('active')}
setInterval(update,300);update();
</script></body></html>
)rawliteral";
  return page;
}
String getStatsHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Վիճակագրություն - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:40px 20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:900px}
.header{text-align:center;margin-bottom:30px}
.header h1{font-size:28px;font-weight:800;letter-spacing:3px;text-transform:uppercase}
.stats-layout{display:flex;gap:16px;align-items:flex-start}
.stats-left{flex:1}
.stats-right{flex:0 0 240px}
.card{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:20px;padding:25px;margin-bottom:16px;backdrop-filter:blur(10px)}
.card-title{font-size:13px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:15px}
.stat-row{display:flex;justify-content:space-between;align-items:center;padding:12px 0;border-bottom:1px solid rgba(255,255,255,0.05)}
.stat-row:last-child{border-bottom:none}
.stat-label{font-size:15px;color:rgba(255,255,255,0.7)}
.stat-value{font-size:18px;font-weight:700}
.stat-value.gold{color:#ffd700}
.stat-value.green{color:#00ff88}
.stat-value.blue{color:#6bb5ff}
.stat-value.red{color:#ff6b6b}
.collection-card{background:rgba(255,215,0,0.08);border:1px solid rgba(255,215,0,0.2);border-radius:20px;padding:25px 18px;text-align:center;position:sticky;top:20px}
.collection-card .card-title{color:rgba(255,215,0,0.7);margin-bottom:20px}
.collection-icon{font-size:48px;margin-bottom:15px}
.collection-total{font-size:32px;font-weight:800;color:#ffd700;text-shadow:0 0 30px rgba(255,215,0,0.3);margin-bottom:5px}
.collection-label{font-size:13px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:20px}
.collection-count{font-size:14px;color:rgba(255,255,255,0.4);padding-top:15px;border-top:1px solid rgba(255,255,255,0.1)}
.collection-count span{color:#ffd700;font-weight:700}
.log-entry{background:rgba(255,255,255,0.04);border:1px solid rgba(255,255,255,0.08);border-radius:10px;padding:12px 14px;margin-bottom:8px;font-size:14px;color:rgba(255,255,255,0.7)}
.log-empty{text-align:center;padding:20px;color:rgba(255,255,255,0.3);font-size:14px}
.back-btn{display:block;text-align:center;padding:14px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:14px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:13px;letter-spacing:2px;transition:all 0.3s;margin-top:10px}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
@media(max-width:768px){.stats-layout{flex-direction:column}.stats-right{flex:none;width:100%}.collection-card{position:relative;top:0}}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="header"><h1>&#128200; Վիճակագրություն</h1></div>
<div class="stats-layout">
<div class="stats-left">
<div class="card"><div class="card-title">&#128197; Լրացումներ</div>
<div class="stat-row"><span class="stat-label">Այսօր</span><span class="stat-value green">)rawliteral";
  page += String(todayDeposits) + " անգամ / " + String(todayAmount) + " դրամ";
  page += R"rawliteral(</span></div>
<div class="stat-row"><span class="stat-label">Այս ամիս</span><span class="stat-value blue">)rawliteral";
  page += String(monthDeposits) + " անգամ / " + String(monthAmount) + " դրամ";
  page += R"rawliteral(</span></div></div>
<div class="card"><div class="card-title">&#129689; Մետաղադրամներ ըստ արժեքի</div>
<div class="stat-row"><span class="stat-label">50 դրամ</span><span class="stat-value">)rawliteral";
  page += String(totalCoins50);
  page += R"rawliteral( հատ</span></div>
<div class="stat-row"><span class="stat-label">100 դրամ</span><span class="stat-value gold">)rawliteral";
  page += String(totalCoins100);
  page += R"rawliteral( հատ</span></div>
<div class="stat-row"><span class="stat-label">200 դրամ</span><span class="stat-value gold">)rawliteral";
  page += String(totalCoins200);
  page += R"rawliteral( հատ</span></div>
<div class="stat-row"><span class="stat-label">500 դրամ</span><span class="stat-value gold">)rawliteral";
  page += String(totalCoins500);
  page += R"rawliteral( հատ</span></div></div>
<div class="card"><div class="card-title">&#128235; Ինկասացիաների մատյան</div><div id="collLog">)rawliteral";
  if (collectionLog.length() > 0) {
    page += collectionLog;
  } else {
    page += "<div class=\"log-empty\">Ինկասացիաներ դեռ չեն եղել</div>";
  }
  page += R"rawliteral(</div></div></div>
<div class="stats-right">
<div class="collection-card"><div class="card-title">&#128176; Ընդհանուր ինկասացիա</div><div class="collection-icon">&#128235;</div><div class="collection-total">)rawliteral";
  page += String(totalCollectedAllTime);
  page += R"rawliteral( դրամ</div><div class="collection-label">հանել ենք ընդհանուր</div><div class="collection-count">Ինկասացիաներ՝ <span>)rawliteral";
  page += String(totalCollections);
  page += R"rawliteral(</span> անգամ</div></div></div></div>
<a href="/menu" class="back-btn">&#9664; Վերադառնալ Գլխավոր էջ</a>
</div></body></html>
)rawliteral";
  return page;
}

String getCollectionHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ինկասացիա - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:40px 20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:500px}
.header{text-align:center;margin-bottom:30px}
.header h1{font-size:28px;font-weight:800;letter-spacing:3px;text-transform:uppercase}
.card{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:20px;padding:30px;margin-bottom:20px;backdrop-filter:blur(10px);text-align:center}
.balance-display{font-size:56px;font-weight:800;color:#ffd700;text-shadow:0 0 40px rgba(255,215,0,0.3);margin-bottom:8px}
.balance-label{font-size:13px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:3px;margin-bottom:30px}
.warning{background:rgba(255,107,107,0.1);border:1px solid rgba(255,107,107,0.3);border-radius:12px;padding:15px;margin-bottom:25px;font-size:14px;color:#ff6b6b}
.pin-row{display:flex;gap:10px;justify-content:center;margin-bottom:20px}
.pin-digit{width:60px;height:70px;background:rgba(255,255,255,0.08);border:2px solid rgba(255,255,255,0.15);border-radius:14px;color:#fff;font-size:28px;font-weight:700;text-align:center;outline:none;transition:all 0.3s}
.pin-digit:focus{border-color:rgba(255,215,0,0.5);background:rgba(255,255,255,0.12)}
.pin-digit.filled{border-color:#00ff88;color:#00ff88}
.pin-digit.error{border-color:#ff6b6b;color:#ff6b6b;animation:shake 0.4s ease}
@keyframes shake{0%,100%{transform:translateX(0)}20%{transform:translateX(-8px)}40%{transform:translateX(8px)}60%{transform:translateX(-4px)}80%{transform:translateX(4px)}}
.pin-label{font-size:12px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:15px}
.btn{display:block;width:100%;padding:18px;border:none;border-radius:14px;font-size:16px;font-weight:700;letter-spacing:2px;cursor:pointer;transition:all 0.3s;margin-bottom:12px;text-transform:uppercase}
.btn-collect{background:linear-gradient(135deg,#ff6b6b,#ee5a5a);color:#fff;box-shadow:0 4px 20px rgba(255,107,107,0.3)}
.btn-collect:hover{transform:translateY(-2px);box-shadow:0 6px 25px rgba(255,107,107,0.5)}
.btn-collect:disabled{background:rgba(255,255,255,0.1);color:rgba(255,255,255,0.3);box-shadow:none;cursor:not-allowed;transform:none}
.back-btn{display:block;text-align:center;padding:14px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:14px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:13px;letter-spacing:2px;transition:all 0.3s}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
.success-msg{background:rgba(0,255,136,0.1);border:1px solid rgba(0,255,136,0.3);border-radius:12px;padding:15px;margin-bottom:20px;font-size:16px;color:#00ff88;display:none}
.lock-icon{font-size:48px;margin-bottom:15px}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="header"><h1>&#128235; Ինկասացիա</h1></div>
<div class="card"><div class="balance-display" id="bal">)rawliteral";
  page += String(total);
  page += R"rawliteral( դրամ</div><div class="balance-label">Հասանելի է դուրսբերման համար</div><div class="lock-icon">&#128274;</div><div class="pin-label">Մուտքագրեք PIN-կոդ</div>
<div class="pin-row"><input type="password" class="pin-digit" id="p1" maxlength="1" inputmode="numeric" oninput="nextPin(1)" onkeydown="prevPin(event,1)"><input type="password" class="pin-digit" id="p2" maxlength="1" inputmode="numeric" oninput="nextPin(2)" onkeydown="prevPin(event,2)"><input type="password" class="pin-digit" id="p3" maxlength="1" inputmode="numeric" oninput="nextPin(3)" onkeydown="prevPin(event,3)"><input type="password" class="pin-digit" id="p4" maxlength="1" inputmode="numeric" oninput="checkPin()" onkeydown="prevPin(event,4)"></div>
<div class="warning">&#9888; Մնացորդը կզրոյացվի։ Վիճակագրությունը կպահպանվի։</div><div class="success-msg" id="success">&#9989; Ինկասացիան կատարված է!</div><button class="btn btn-collect" id="collectBtn" onclick="doCollect()" disabled>Դուրս բերել</button></div>
<a href="/menu" class="back-btn">&#9664; Վերադառնալ Գլխավոր էջ</a>
</div>
<script>
const CORRECT_PIN="4776";let pinVerified=false;
function nextPin(n){const el=document.getElementById('p'+n);if(el.value.length===1){el.classList.add('filled');if(n<4)document.getElementById('p'+(n+1)).focus()}checkPin()}
function prevPin(e,n){if(e.key==='Backspace'&&!e.target.value&&n>1)document.getElementById('p'+(n-1)).focus()}
function checkPin(){const pin=document.getElementById('p1').value+document.getElementById('p2').value+document.getElementById('p3').value+document.getElementById('p4').value;if(pin.length===4){if(pin===CORRECT_PIN){pinVerified=true;document.getElementById('collectBtn').disabled=false;for(let i=1;i<=4;i++)document.getElementById('p'+i).classList.remove('error')}else{pinVerified=false;document.getElementById('collectBtn').disabled=true;for(let i=1;i<=4;i++){document.getElementById('p'+i).classList.add('error');document.getElementById('p'+i).value='';document.getElementById('p'+i).classList.remove('filled')}setTimeout(()=>{for(let i=1;i<=4;i++)document.getElementById('p'+i).classList.remove('error');document.getElementById('p1').focus()},500)}}}
function doCollect(){if(!pinVerified)return;if(!confirm('Հաստատո՞ւմ եք գումարի դուրսբերումը'))return;fetch('/api/collect',{method:'POST'}).then(r=>r.json()).then(d=>{if(d.success){document.getElementById('success').style.display='block';document.getElementById('bal').textContent='0 դրամ';document.getElementById('collectBtn').disabled=true;pinVerified=false;for(let i=1;i<=4;i++){document.getElementById('p'+i).value='';document.getElementById('p'+i).classList.remove('filled')}setTimeout(()=>{document.getElementById('success').style.display='none'},3000)}}).catch(()=>{})}
</script></body></html>
)rawliteral";
  return page;
}

String getInventoryHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Մետաղադրամների պահոց - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:40px 20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:500px}
.header{text-align:center;margin-bottom:25px}
.header h1{font-size:28px;font-weight:800;letter-spacing:3px;text-transform:uppercase}
.counters-row{display:flex;gap:12px;margin-bottom:20px}
.counter-card{flex:1;border:1px solid rgba(255,255,255,0.1);border-radius:24px;padding:20px;text-align:center;position:relative;overflow:hidden;transition:all 0.3s;display:flex;flex-direction:column;justify-content:flex-end;min-height:200px}
.counter-card::before{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:linear-gradient(to top,rgba(0,0,0,0.85) 0%,rgba(0,0,0,0.3) 50%,rgba(0,0,0,0.1) 100%);z-index:0}
.counter-card>*{position:relative;z-index:1}
.counter-card:hover{transform:translateY(-5px) scale(1.02);border-color:rgba(255,215,0,0.4);box-shadow:0 10px 30px rgba(0,0,0,0.4)}
.counter-top{display:flex;justify-content:flex-end;align-items:center;gap:8px;margin-bottom:auto;padding-bottom:20px}
.counter-value{font-size:38px;font-weight:800;color:#ffd700;text-shadow:0 2px 10px rgba(0,0,0,0.5)}
.counter-piece{font-size:16px;color:rgba(255,255,255,0.7);font-weight:600}
.counter-bottom{margin-top:auto}
.counter-label{font-size:14px;color:rgba(255,255,255,0.6);text-transform:uppercase;letter-spacing:1px;margin-bottom:4px}
.counter-sub{font-size:13px;color:rgba(255,255,255,0.4)}
.back-btn{display:block;text-align:center;padding:16px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:16px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:14px;letter-spacing:2px;transition:all 0.3s;margin-top:10px}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
.reset-section{margin-top:20px;padding-top:20px;border-top:1px solid rgba(255,255,255,0.1);text-align:center}
.reset-label{font-size:12px;color:rgba(255,255,255,0.4);text-transform:uppercase;letter-spacing:2px;margin-bottom:12px}
.btn-reset-inv{width:100%;padding:14px;background:rgba(255,107,107,0.15);border:1px solid rgba(255,107,107,0.4);border-radius:14px;color:#ff6b6b;font-size:14px;font-weight:600;cursor:pointer;transition:all 0.3s;text-transform:uppercase;letter-spacing:1px}
.btn-reset-inv:hover{background:rgba(255,107,107,0.3);transform:translateY(-2px)}
.success-toast{background:rgba(0,255,136,0.1);border:1px solid rgba(0,255,136,0.3);border-radius:12px;padding:12px;margin-bottom:15px;font-size:14px;color:#00ff88;text-align:center;display:none}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="header"><h1>&#129689; Մետաղադրամներ</h1></div>
<div class="success-toast" id="resetSuccess">&#9989; Մետաղադրամների պահոցը զրոյացված է!</div>
<div class="counters-row">
<div class="counter-card" style="background:url('https://i.postimg.cc/XqBBtwRK/5379857257832584900.jpg')center/cover no-repeat"><div class="counter-top"><div class="counter-value">)rawliteral";
  page += String(totalCoins50);
  page += R"rawliteral(</div><div class="counter-piece">հատ</div></div><div class="counter-bottom"><div class="counter-label"></div><div class="counter-sub">)rawliteral";
  page += String(totalCoins50 * 50);
  page += R"rawliteral( դրամ</div></div></div>
<div class="counter-card" style="background:url('https://i.postimg.cc/MHffgyCb/5379857257832584899.jpg')center/cover no-repeat"><div class="counter-top"><div class="counter-value">)rawliteral";
  page += String(totalCoins100);
  page += R"rawliteral(</div><div class="counter-piece">հատ</div></div><div class="counter-bottom"><div class="counter-label"></div><div class="counter-sub">)rawliteral";
  page += String(totalCoins100 * 100);
  page += R"rawliteral( դրամ</div></div></div></div>
<div class="counters-row">
<div class="counter-card" style="background:url('https://i.postimg.cc/J0HHSbfN/5379857257832584901.jpg')center/cover no-repeat"><div class="counter-top"><div class="counter-value">)rawliteral";
  page += String(totalCoins200);
  page += R"rawliteral(</div><div class="counter-piece">հատ</div></div><div class="counter-bottom"><div class="counter-label"></div><div class="counter-sub">)rawliteral";
  page += String(totalCoins200 * 200);
  page += R"rawliteral( դրամ</div></div></div>
<div class="counter-card" style="background:url('https://i.postimg.cc/zvHH4nsn/5379857257832584902.jpg')center/cover no-repeat"><div class="counter-top"><div class="counter-value">)rawliteral";
  page += String(totalCoins500);
  page += R"rawliteral(</div><div class="counter-piece">հատ</div></div><div class="counter-bottom"><div class="counter-label"></div><div class="counter-sub">)rawliteral";
  page += String(totalCoins500 * 500);
  page += R"rawliteral( դրամ</div></div></div></div>
<div class="reset-section"><div class="reset-label"></div><button class="btn-reset-inv" onclick="resetInventory()">&#128260; Զրոյացնել մետաղադրամների պահոցը</button></div>
<a href="/menu" class="back-btn">&#9664; Վերադառնալ Գլխավոր էջ</a>
</div>
<script>
function resetInventory(){if(!confirm('Զրոյացնե՞լ մետաղադրամների պահոցը։ Ցուցանիշները կդառնան 0, բայց մնացորդը կպահպանվի։'))return;fetch('/api/reset/inventory',{method:'POST'}).then(r=>r.json()).then(d=>{if(d.success){document.getElementById('resetSuccess').style.display='block';setTimeout(()=>{location.reload()},1500)}}).catch(()=>{})}
</script></body></html>
)rawliteral";
  return page;
}
String getGoalHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Փոխել նպատակը - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:40px 20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:500px;display:flex;flex-direction:column;min-height:calc(100vh - 80px)}
.header{text-align:center;margin-bottom:20px}
.header h1{font-size:36px;font-weight:800;letter-spacing:4px;text-transform:uppercase;text-shadow:0 0 30px rgba(255,215,0,0.3)}
.card{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:20px;padding:30px;margin-bottom:20px;backdrop-filter:blur(10px);margin-top:auto}
.form-group{margin-bottom:20px}
.form-group label{display:block;font-size:13px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:10px}
input[type="text"],input[type="number"]{width:100%;padding:16px;background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.15);border-radius:14px;color:#fff;font-size:16px;outline:none;transition:all 0.3s}
input[type="text"]:focus,input[type="number"]:focus{border-color:rgba(255,215,0,0.5);background:rgba(255,255,255,0.12)}
input::placeholder{color:rgba(255,255,255,0.3)}
.current{background:rgba(255,215,0,0.08);border:1px solid rgba(255,215,0,0.2);border-radius:14px;padding:20px;margin-bottom:25px;text-align:center}
.current-label{font-size:12px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:8px}
.current-value{font-size:20px;font-weight:700;color:#ffd700}
.btn-save{width:100%;padding:18px;background:linear-gradient(135deg,#ffd700,#ffaa00);border:none;border-radius:14px;color:#000;font-size:16px;font-weight:700;letter-spacing:2px;cursor:pointer;transition:all 0.3s;text-transform:uppercase}
.btn-save:hover{transform:translateY(-2px);box-shadow:0 6px 25px rgba(255,215,0,0.4)}
.success-msg{background:rgba(0,255,136,0.1);border:1px solid rgba(0,255,136,0.3);border-radius:12px;padding:15px;margin-bottom:20px;font-size:15px;color:#00ff88;text-align:center;display:none}
.back-btn{display:block;text-align:center;padding:14px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:14px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:13px;letter-spacing:2px;transition:all 0.3s}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="header"><h1>Փոխել նպատակը</h1></div>
<div class="card">
<div class="success-msg" id="success">&#9989; Նպատակը պահպանված է!</div>
<div class="current"><div class="current-label">Ընթացիկ նպատակ</div><div class="current-value" id="currentGoal">)rawliteral";
  page += goalName + " - " + String(goalAmount) + " դրամ";
  page += R"rawliteral(</div></div>
<div class="form-group"><label>Նպատակի անվանում</label><input type="text" id="goalName" value=")rawliteral";
  page += goalName;
  page += R"rawliteral(" placeholder="Օրինակ՝ Այս ամիս ✅, Արձակուրդ..."></div>
<div class="form-group"><label>Նպատակային գումար (դրամ)</label><input type="number" id="goalAmount" value=")rawliteral";
  page += String(goalAmount);
  page += R"rawliteral(" placeholder="5000"></div>
<button class="btn-save" onclick="saveGoal()">Պահպանել նպատակը</button>
</div>
<a href="/menu" class="back-btn">&#9664; Վերադառնալ Գլխավոր էջ</a>
</div>
<script>
function saveGoal(){const name=document.getElementById('goalName').value.trim();const amount=parseInt(document.getElementById('goalAmount').value);if(!name||isNaN(amount)||amount<1){alert('Մուտքագրեք ճիշտ անվանում և գումար');return}fetch('/api/goal',{method:'POST',headers:{'Content-Type':'application/x-www-form-urlencoded'},body:'name='+encodeURIComponent(name)+'&amount='+amount}).then(r=>r.json()).then(d=>{if(d.success){document.getElementById('currentGoal').textContent=name+' - '+amount.toLocaleString()+' դրամ';document.getElementById('success').style.display='block';setTimeout(()=>{document.getElementById('success').style.display='none'},3000)}}).catch(()=>{})}
</script></body></html>
)rawliteral";
  return page;
}

String getSettingsHTML() {
  String page = R"rawliteral(
<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Կարգավորումներ - Խելացի Խնայատուփ</title><style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:url(')rawliteral";
  page += String(BG_MENU);
  page += R"rawliteral(')center/cover no-repeat fixed;color:#fff;font-family:'Segoe UI',Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:40px 20px}
.overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.75);z-index:0}
.content{position:relative;z-index:1;width:100%;max-width:500px}
.header{text-align:center;margin-bottom:30px}
.header h1{font-size:28px;font-weight:800;letter-spacing:3px;text-transform:uppercase}
.card{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:20px;padding:25px;margin-bottom:16px;backdrop-filter:blur(10px)}
.card-title{font-size:13px;color:rgba(255,255,255,0.5);text-transform:uppercase;letter-spacing:2px;margin-bottom:15px}
.btn-danger{width:100%;padding:16px;background:rgba(255,107,107,0.15);border:1px solid rgba(255,107,107,0.4);border-radius:14px;color:#ff6b6b;font-size:14px;font-weight:600;cursor:pointer;transition:all 0.3s;margin-bottom:10px}
.btn-danger:hover{background:rgba(255,107,107,0.3)}
.info-row{display:flex;justify-content:space-between;padding:10px 0;border-bottom:1px solid rgba(255,255,255,0.05);font-size:14px}
.info-row:last-child{border-bottom:none}
.info-label{color:rgba(255,255,255,0.5)}
.info-value{color:#fff;font-weight:600}
.back-btn{display:block;text-align:center;padding:14px;background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.1);border-radius:14px;color:rgba(255,255,255,0.6);text-decoration:none;font-size:13px;letter-spacing:2px;transition:all 0.3s;margin-top:10px}
.back-btn:hover{background:rgba(255,255,255,0.1);color:#fff}
</style></head><body>
<div class="overlay"></div><div class="content">
<div class="header"><h1>&#9881; Կարգավորումներ</h1></div>
<div class="card"><div class="card-title">Համակարգային տեղեկություն</div>
<div class="info-row"><span class="info-label">Սարք</span><span class="info-value">ESP32</span></div>
<div class="info-row"><span class="info-label">Կոդ</span><span class="info-value">Smart Bank Pro v2.0</span></div>
<div class="info-row"><span class="info-label">Մետաղադրամի պին</span><span class="info-value">GPIO 14</span></div></div>
<div class="card"><div class="card-title">Վտանգավոր գոտի</div>
<button class="btn-danger" onclick="resetStats()">&#128260; Զրոյացնել վիճակագրությունը</button>
<button class="btn-danger" onclick="resetAll()">&#128683; Զրոյացնել բոլոր տվյալները</button></div>
<a href="/menu" class="back-btn">&#9664; Վերադառնալ Գլխավոր էջ</a>
</div>
<script>
function resetStats(){if(!confirm('Զրոյացնե՞լ ամբողջ վիճակագրությունը։ Մետաղադրամների հաշվիչները և պատմությունը կմաքրվեն։'))return;fetch('/api/reset/stats',{method:'POST'}).then(()=>alert('Վիճակագրությունը զրոյացված է!')).catch(()=>{})}
function resetAll(){if(!confirm('ՈՒՇԱԴՐՈՒԹՅՈՒՆ. Սա կջնջի ԲՈԼՈՐ տվյալները, ներառյալ մնացորդը, նպատակը և Մետաղադրամների պահոցը։ Համոզված եք?'))return;fetch('/api/reset/all',{method:'POST'}).then(()=>{alert('Բոլոր տվյալները զրոյացված են!');location.reload()}).catch(()=>{})}
</script></body></html>
)rawliteral";
  return page;
}
void handleRoot(){server.send(200,"text/html",getScreensaverHTML());}
void handleMenu(){server.send(200,"text/html",getMenuHTML());}
void handleDeposit(){server.send(200,"text/html",getDepositHTML());}
void handleStats(){server.send(200,"text/html",getStatsHTML());}
void handleCollection(){server.send(200,"text/html",getCollectionHTML());}
void handleInventory(){server.send(200,"text/html",getInventoryHTML());}
void handleGoal(){server.send(200,"text/html",getGoalHTML());}
void handleSettings(){server.send(200,"text/html",getSettingsHTML());}

void handleApiCoin(){
  String json = "{\"coin\":\"" + lastCoin + "\",\"pulseId\":" + String(pulseId) + "}";
  server.send(200, "application/json", json);
}

void handleApi(){
  int pct = (total * 100) / goalAmount;
  if (pct > 100) pct = 100;
  String json = "{\"total\":" + String(total) + ",\"lastCoin\":\"" + lastCoin + "\",\"pct\":" + String(pct) + "}";
  server.send(200, "application/json", json);
}

void handleApiCollect() {
  if (total > 0) {
    lastCollectionAmount = total;
    String dateTime = getFormattedDateTime();
    lastCollectionDate = dateTime;
    String logEntry = "<div class=\"log-entry\">&#128235; " + dateTime + " — <b>" + String(total) + " դրամ</b></div>";
    if (collectionLog.length() > 0) {
      collectionLog = logEntry + collectionLog;
    } else {
      collectionLog = logEntry;
    }
    int entryCount = 0;
    int pos = 0;
    while ((pos = collectionLog.indexOf("log-entry", pos)) != -1) {
      entryCount++;
      pos++;
      if (entryCount >= 20) {
        int cutPos = collectionLog.indexOf("<div class=\"log-entry\">", pos);
        if (cutPos != -1) {
          collectionLog = collectionLog.substring(0, cutPos);
        }
        break;
      }
    }
    totalCollections++;
    totalCollectedAllTime += total;
    
    // Telegram notification — ДО обнуления total
    Serial.println("[TELEGRAM] Sending collection notification: " + String(lastCollectionAmount) + " dram");
    sendCollectionNotification(lastCollectionAmount);
    
    total = 0;
    saveData();
    String json = "{\"success\":true,\"amount\":" + String(lastCollectionAmount) + "}";
    server.send(200, "application/json", json);
  } else {
    server.send(200, "application/json", "{\"success\":false,\"error\":\"No money to collect\"}");
  }
}

void handleApiResetInventory() {
  totalCoins50 = 0;
  totalCoins100 = 0;
  totalCoins200 = 0;
  totalCoins500 = 0;
  totalCoinsUnknown = 0;
  saveData();
  server.send(200, "application/json", "{\"success\":true}");
}

void handleApiInventory() {
  String json = "{\"items\":[";
  for (int i = 0; i < inventoryCount; i++) {
    if (i > 0) json += ",";
    json += "{\"name\":\"" + inventory[i].name + "\",\"value\":" + String(inventory[i].value) + "}";
  }
  json += "]}";
  server.send(200, "application/json", json);
}

void handleApiInventoryAdd() {
  if (inventoryCount < 20 && server.hasArg("name") && server.hasArg("value")) {
    inventory[inventoryCount].name = server.arg("name");
    inventory[inventoryCount].value = server.arg("value").toInt();
    inventoryCount++;
    saveData();
  }
  server.send(200, "application/json", "{\"success\":true}");
}

void handleApiInventoryDelete() {
  if (server.hasArg("index")) {
    int idx = server.arg("index").toInt();
    if (idx >= 0 && idx < inventoryCount) {
      for (int i = idx; i < inventoryCount - 1; i++) {
        inventory[i] = inventory[i + 1];
      }
      inventoryCount--;
      saveData();
    }
  }
  server.send(200, "application/json", "{\"success\":true}");
}

void handleApiManual() {
  if (server.hasArg("amount") && server.hasArg("count")) {
    int amount = server.arg("amount").toInt();
    int count = server.arg("count").toInt();
    if (amount > 0 && count > 0) {
      int totalAdded = amount * count;
      sessionAmount += totalAdded;
      totalDeposits += count;
      if (amount == 50) totalCoins50 += count;
      else if (amount == 100) totalCoins100 += count;
      else if (amount == 200) totalCoins200 += count;
      else if (amount == 500) totalCoins500 += count;
      lastCoin = String(amount);
      saveData();
      String json = "{\"success\":true,\"amount\":" + String(totalAdded) + "}";
      server.send(200, "application/json", json);
      return;
    }
  }
  server.send(200, "application/json", "{\"success\":false}");
}

void handleApiDeposit() {
  checkAndResetDailyStats();
  if (sessionAmount > 0) {
    int deposited = sessionAmount;
    total += deposited;
    todayDeposits += 1;
    todayAmount += deposited;
    monthDeposits += 1;
    monthAmount += deposited;
    sessionAmount = 0;
    lastCoin = "-";
    lastPulses = 0;
    
    // Telegram УВЕДОМЛЕНИЕ — ДО saveData, пока данные актуальны
    Serial.println("[TELEGRAM] Sending deposit notification: " + String(deposited) + " dram");
    sendDepositNotification(deposited, total);
    
    // Check if goal reached
    if (total >= goalAmount && (total - deposited) < goalAmount) {
      Serial.println("[TELEGRAM] Goal reached!");
      sendGoalReachedNotification();
    }
    
    saveData();
    String json = "{\"success\":true,\"amount\":" + String(deposited) + "}";
    server.send(200, "application/json", json);
    return;
  }
  server.send(200, "application/json", "{\"success\":false}");
}

void handleApiGoal() {
  if (server.hasArg("name") && server.hasArg("amount")) {
    goalName = server.arg("name");
    goalAmount = server.arg("amount").toInt();
    saveData();
    server.send(200, "application/json", "{\"success\":true}");
  } else {
    server.send(200, "application/json", "{\"success\":false}");
  }
}

void handleApiResetStats() {
  totalCoins50 = 0;
  totalCoins100 = 0;
  totalCoins200 = 0;
  totalCoins500 = 0;
  totalCoinsUnknown = 0;
  totalDeposits = 0;
  totalCollections = 0;
  lastCollectionAmount = 0;
  lastCollectionDate = "Never";
  todayDeposits = 0;
  todayAmount = 0;
  monthDeposits = 0;
  monthAmount = 0;
  collectionLog = "";
  saveData();
  server.send(200, "application/json", "{\"success\":true}");
}

void handleApiResetAll() {
  total = 0;
  goalAmount = 5000;
  goalName = "This month";
  sessionAmount = 0;
  totalCoins50 = 0;
  totalCoins100 = 0;
  totalCoins200 = 0;
  totalCoins500 = 0;
  totalCoinsUnknown = 0;
  totalDeposits = 0;
  totalCollections = 0;
  lastCollectionAmount = 0;
  lastCollectionDate = "Never";
  todayDate = "";
  todayDeposits = 0;
  todayAmount = 0;
  monthDeposits = 0;
  monthAmount = 0;
  collectionLog = "";
  totalCollectedAllTime = 0;
  inventoryCount = 0;
  saveData();
  server.send(200, "application/json", "{\"success\":true}");
}
void setup() {
  Serial.begin(115200);
  delay(500);
  loadData();

  pinMode(COIN_PIN, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(COIN_PIN), coinISR, FALLING);

  WiFi.begin(ssid, password);
  Serial.print("Connecting");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println();
  Serial.print("IP: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.on("/menu", handleMenu);
  server.on("/deposit", handleDeposit);
  server.on("/stats", handleStats);
  server.on("/collection", handleCollection);
  server.on("/inventory", handleInventory);
  server.on("/goal", handleGoal);
  server.on("/settings", handleSettings);
  server.on("/api", handleApi);
  server.on("/api/coin", handleApiCoin);
  server.on("/api/collect", HTTP_POST, handleApiCollect);
  server.on("/api/inventory", handleApiInventory);
  server.on("/api/inventory/add", HTTP_POST, handleApiInventoryAdd);
  server.on("/api/inventory/delete", HTTP_POST, handleApiInventoryDelete);
  server.on("/api/manual", HTTP_POST, handleApiManual);
  server.on("/api/deposit", HTTP_POST, handleApiDeposit);
  server.on("/api/goal", HTTP_POST, handleApiGoal);
  server.on("/api/reset/stats", HTTP_POST, handleApiResetStats);
  server.on("/api/reset/all", HTTP_POST, handleApiResetAll);
  server.on("/api/reset/inventory", HTTP_POST, handleApiResetInventory);
  server.begin();

  Serial.println("Smart Piggy Bank PRO started!");
  Serial.println("Routes: /, /menu, /deposit, /stats, /collection, /inventory, /goal, /settings");

  // ============ TELEGRAM BOT INITIALIZATION ============
  secured_client.setCACert(TELEGRAM_CERTIFICATE_ROOT);
  bot_initialized = true;
  Serial.println("Telegram Bot initialized");

  // Отправляем стартовое сообщение для проверки
  String startupMsg = "🤖 *Smart Piggy Bank PRO* Համակարգը գործարկված է!\n";
  startupMsg += "📡 IP: " + WiFi.localIP().toString();
  bot.sendMessage(CHAT_ID, startupMsg, "Markdown");
}

void loop() {
  server.handleClient();
  handleTelegramMessages();

  if (pulseCount > 0 && millis() - lastPulseTime > 300) {
    noInterrupts();
    int count = pulseCount;
    pulseCount = 0;
    interrupts();

    lastPulses = count;
    pulseId++;

    Serial.print("Pulses: ");
    Serial.println(count);

    if (count == 10) {
      lastCoin = "100";
      sessionAmount += 100;
      totalCoins100++;
      totalDeposits++;
    }
    else if (count == 15) {
      lastCoin = "200";
      sessionAmount += 200;
      totalCoins200++;
      totalDeposits++;
    }
    else if (count == 20) {
      lastCoin = "500";
      sessionAmount += 500;
      totalCoins500++;
      totalDeposits++;
    }
    else {
      lastCoin = "Unknown";
      totalCoinsUnknown++;
    }
    saveData();
    Serial.println(lastCoin + " dram");
    Serial.print("Total: ");
    Serial.println(total);
    Serial.println("----------------");
  }
}
