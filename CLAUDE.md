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
