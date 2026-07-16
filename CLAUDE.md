# CLAUDE.md — warrior3d(3D 武士勇者比武=jousting3d 的無馬徒步版,德義武鬥館)

> 2026-07-16 全新(使用者拍板:「另外開發武士勇者比武版,沒有馬的」)。
> base=jousting3d 07-16 自由馬戰版(武器/戰鬥/AI 核心同源)。帳號 summer09201017-cloud。
> ★尚未上架:公開 repo/Netlify prod 站名等使用者逐字點名(上架鐵則)。

## 引擎要點

- 徒步自由走位:fighter={pos,heading,speed};W/S 前進後退(倒退 MAX_BACK=1.9)、A/D 轉向;
  場地 ARENA_HALF=15,開放無阻擋,邊界柔性擋(速度×0.5)。
- 血量制:對決各 100 血/大戰三百回合各 300 血(roundCap=300 戰滿以血量判定)/練習場 AI 不出手。
  「回合」=雙方出手總次數。KO=溫柔跪地演出(koT 單膝跪 1.2s,無流血)→ endT 1.6s 結算。
- 八般武器 WEAPONS 資料驅動(與騎士比武同表,徒步 reach 縮短、BODY_REACH=0.55):
  長槍(chargeBonus 衝刺加成)/長矛/青龍大刀/騎士劍/彎刀/西洋劍/弓箭(鈍頭箭)/雙綠鋼球
  (volley=2+stun 1.1s)。1-8 直選、Q/Tab 循環、戰鬥中換=0.35s 硬直;模型全掛右手 visible 切換。
- 大揮擊(07-16 二修):WEAPONS.swing 三型——chop=180°舉過頭直劈(劍/彎刀)、spin=360°上身
  迴旋橫掃(大刀)、lunge=回拉+整枝前刺(長槍/長矛/西洋劍);傷害延到 CONTACT_AT 接觸瞬間
  結算(_pendingStrikes 佇列)。
- 蓄力大招:長按出手 ≥CHARGE_MIN(0.6s)放開=發「刀光/劍光/武器波動」旋轉新月斬擊波
  (白心+五色暈,dmg 1.4-2.5x、冷卻 2.2x);蓄力=金圈+武器高舉發抖+移速減半,被打中斷;
  AI 大招腦 superT 定時蓄力(預告+減速=閃避窗)。衝刺 boost=玩家限定逃跑鍵。
- 突進技(07-16):衝刺 ≥0.35s 後按出手——遠距帶(5-12m)=跳殺(拋物線躍撲 airY,落地斬 1.6x)、近距帶(2-5m)=飛殺(1.8x 衝刺速爆發突進,接觸 1.8m 斬 1.5x);TECH_CD 3.5s 共用冷卻,玩家限定。
- 自動面向(07-16):AUTO_FACE_RANGE=8 內未按 A/D、未衝刺時自動轉向對手(高速背對不硬拉);出手/大招瞬間攻距內直接轉身面對再判定(玩家限定輔助)。
- 格擋(07-16):按住 K/C/舉盾鈕=舉盾——正面 ±60°;箭/鋼球=無傷、近戰/大招波動=×0.3;剛舉盾 ≤0.35s 被近戰打=完美盾反(無傷+對手彈開硬直 1.1s);舉盾移速×0.35 不能出招;AI 舉盾腦。
- 角色皮(07-16,移植 equestrian3d):CHARACTERS 傑洛/喬尼(同 jousting3d);選其一→對手自動變另一位;角色皮覆蓋 outfit 配色;setCharacter=_buildFighters 整組重蓋。
- 判定=畫面(鐵則4):近戰=距離+朝向幾何判定;AI 另過 aiSkill+0.18 命中門檻(擬人失手)。
- AI 三腦(npc-ai-kit):走位(近戰追擊/遠程保持 5-11m 繞圈/35% 得手後退開)+換武器
  (switchT 6-11s 依距離挑)+出手;快撞牆自動轉向場中央。
- 人物:makePerson 臉部鐵則(徒步版曝露 shirtMat/pantsMat 供 OUTFIT_COLORS 換戰袍七色);
  heroUp()=武士頭帶(不戴全罩盔,看得見臉)+胸甲+隊色盾+八般武器庫;
  武鬥系待機=格鬥架式(交戰低速時微蹲);走路循環=腿擺動幅度隨速度。
- 命中=hitFlash(鋼球=綠閃)+慢動作 0.42x+側面特寫;被擊=後仰苦臉;暈眩=搖晃。
- 殼層契約與 jousting3d 同(main.js HUD/事件/選單),差異:outfitSelect(戰袍)取代
  horseCoatSelect(馬色);storage 鍵 warrior3d-*;hook window.__warrior3d。

## 驗證

`npm run build && npx vite preview --port 4190` → `node scripts/verify-warrior.mjs http://localhost:4190 <outDir>`
①kids 對決自走 bot 應勝 ②normal 被動局 AI 應能 KO 玩家 ③八武器全出手+投射物 ④0 pageerror。

## 部署與同步(上架後)

Netlify 手動站(deyi- 前綴,站名待點名);同步=deyi-arena 武鬥館入口卡+奧運頁示範賽區
+portfolio+gamefleet(德義武鬥館分類)+本機 ~/.claude/gamefleet/sites.json。
